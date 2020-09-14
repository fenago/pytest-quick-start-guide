

[]{#ch06}Chapter 6. Converting unittest suites to pytest {#chapter-6.-converting-unittest-suites-to-pytest .title}
--------------------------------------------------------

</div>

</div>
:::

In the previous chapter, we have seen how the flexible pytest
architecture has created a rich plugin ecosystem, with hundreds of
plugins available. We learned how easy it is to find and install
plugins, and had an overview of a number of interesting plugins.

Now that you are proficient with pytest, you might be in a situation
where you have one or more `unittest`{.literal}-based test suites and
want to start using pytest with them. In this chapter, we will discuss
the best approaches to start doing just that, ranging from simple test
suites that might require little to no modification, to large
in-house-grown test suites that contain all kinds of customizations
grown organically over the years. Most of the tips and advice in this
chapter come from my own experience when migrating our[]{#id324673405
.indexterm} massive `unittest`{.literal}-style test suite at ESSS
([https://wwww.esss.co](https://www.esss.co/){.ulink}), where I work.

Here is what we will cover in this chapter:

::: {.itemizedlist}
-   Using pytest as a test runner
-   Converting asserts with `unittest2pytest`{.literal}
-   Handling setup and teardown
-   Managing test hierarchies
-   Refactoring test utilities
-   Migration strategy



[]{#ch06lvl1sec37}Using pytest as a test runner {#using-pytest-as-a-test-runner .title style="clear: both"}
-----------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

One thing that surprisingly many people don\'t know[]{#id324673415
.indexterm} is that pytest can run `unittest`{.literal} suites out of
the box, without any modifications.

For example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        cls.temp_dir = Path(tempfile.mkdtemp())
        cls.filepath = cls.temp_dir / "data.csv"
        cls.filepath.write_text(DATA.strip())

    @classmethod
    def tearDownClass(cls):
        shutil.rmtree(cls.temp_dir)

    def setUp(self):
        self.grids = list(iter_grids_from_csv(self.filepath))

    def test_read_properties(self):
        self.assertEqual(self.grids[0], GridData("Main Grid", 48, 44))
        self.assertEqual(self.grids[1], GridData("2nd Grid", 24, 21))
        self.assertEqual(self.grids[2], GridData("3rd Grid", 24, 48))

    def test_invalid_path(self):
        with self.assertRaises(IOError):
            list(iter_grids_from_csv(Path("invalid file")))

    @unittest.expectedFailure
    def test_write_properties(self):
        self.fail("not implemented yet")
```
:::

We can run this using the `unittest`{.literal} runner:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
..x
----------------------------------------------------------------------
Ran 3 tests in 0.005s

OK (expected failures=1)
```
:::

But the cool thing is that pytest also runs this test without any
modifications:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest test_simple.py
======================== test session starts ========================
...
collected 3 items

test_simple.py ..x                                             [100%]

================ 2 passed, 1 xfailed in 0.11 seconds ================
```
:::

This makes it really easy to just start using pytest as a test runner,
which brings several benefits:

::: {.itemizedlist}
-   You can use plugins---for example, `pytest-xdist`{.literal}, to
    speed up the test suite.
-   You have several command-line options at your
    disposal: `-k`{.literal} for selecting tests, `--pdb`{.literal} to
    jump into the debugger on errors, `--lf`{.literal} to run last
    failed tests only, and so on.
-   You can stop writing `self.assert*`{.literal} methods and go with
    plain `assert`{.literal}s. pytest will happily provide rich failure
    information, even for `unittest`{.literal}-based subclasses.
:::

For completeness, here are the `unittest`{.literal} idioms and features
supported out of the box:

::: {.itemizedlist}
-   `setUp`{.literal} and `tearDown`{.literal} for function-level
    `setup`{.literal}/`teardown`{.literal}
-   `setUpClass`{.literal} and `tearDownClass`{.literal} for class-level
    `setup`{.literal}/`teardown`{.literal}
-   `setUpModule`{.literal} and `tearDownModule`{.literal} for
    module-level `setup`{.literal}/`teardown`{.literal}
-   `skip`{.literal}, `skipIf`{.literal}, `skipUnless`{.literal},
    and `expectedFailure`{.literal} decorators, for functions and
    classes
-   `TestCase.skipTest`{.literal} to imperatively skip inside tests
:::

The following idioms are currently not supported:

::: {.itemizedlist}
-   `load_tests protocol`{.literal}: This protocol allows users to
    completely customize which tests are loaded from a module
    (<https://docs.python.org/3/library/unittest.html#load-tests-protocol>).
    The collection concept used by pytest is not compatible with how
    `load_tests`{.literal} protocol works, so no work is planned by the
    pytest core team to support this feature (see
    the `#992`{.literal} (<https://github.com/pytest-dev/pytest/issues/992>)
    issue if you are interested in the details).
-   `subtests`{.literal}: Tests using this feature can report multiple
    failures inside the same test method
    (<https://docs.python.org/3/library/unittest.html#distinguishing-test-iterations-using-subtests>).
    This feature is similar to pytest\'s own parametrization support,
    with the difference that the test results can be determined at
    runtime instead of collection time. In theory, this can be supported
    by pytest, and the feature is currently[]{#id325091885 .indexterm}
    being tracked by issue
    `#1367`{.literal} (<https://github.com/pytest-dev/pytest/issues/1367>).
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note13}Note {#note .title}

[**Surprises with**]{.strong}`pytest-xdist`{.literal} If you decide to
use `pytest-xdist`{.literal} in your test suite, be aware that it runs
tests in an arbitrary order: each worker will run tests as they finish
other tests, so the order the in which the tests are executed is not
predictable. Because the default `unittest`{.literal} runner runs tests
sequentially and always in the same order, often this will bring to
light concurrency problems to your test suite---for example, tests
trying to create a temporary directory with the same name. You should
see this as an opportunity to fix the underlying concurrency problems,
as they should not be part of the test suite anyway.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch06lvl2sec71}Pytest features in unittest subclasses {#pytest-features-in-unittest-subclasses .title}

</div>

</div>
:::

Although not designed to support[]{#id325091925 .indexterm} all its
features when running `unittest`{.literal}-based tests, a few of the
pytest idioms are supported:

::: {.itemizedlist}
-   [**Plain asserts**]{.strong}: pytest assertion introspect works just
    as well when subclassing `unittest.TestCase`{.literal}
-   [**Marks**]{.strong}: marks can be applied normally to
    `unittest`{.literal} test methods and classes. Plugins that deal
    with marks should work normally in most cases (for
    example, `pytest-timeout`{.literal} marks)
-   [**Autouse**]{.strong} fixtures: autouse fixtures defined in modules
    or `conftest.py`{.literal} files will be created/destroyed when
    executing `unittest`{.literal} test methods normally,
    including  `unittest`{.literal} subclasses in the case of
    class-scoped autouse fixtures
-   [**Test selection**]{.strong}: `-k`{.literal} and `-m`{.literal} in
    the command line should work as normal
:::

Other pytest features do not work with `unittest`{.literal}, especially:

::: {.itemizedlist}
-   [**Fixtures**]{.strong}: `unittest`{.literal} test methods cannot
    request fixtures. Pytest uses `unittest`{.literal}\'s own result
    collector to execute the tests, which doesn\'t support passing
    arguments to test functions
-   [**Parametrization**]{.strong}: this is not supported, for similar
    reasons as for fixtures: we need to pass the parametrized values,
    and this is not currently possible
:::

Plugins that don\'t rely on fixtures may work normally, for
example `pytest-timeout`{.literal}or `pytest-randomly`{.literal}.



[]{#ch06lvl1sec38}Converting asserts with unitest2pytest {#converting-asserts-with-unitest2pytest .title style="clear: both"}
--------------------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Once you have changed the test runner[]{#id324673427 .indexterm} to
pytest, you can take advantage[]{#id325091706 .indexterm} of writing
plain assert statements instead of `self.assert*`{.literal} methods.

Converting all the method calls is boring and error-prone, that\'s why
the
[`unittest2pytest`{.literal}](https://github.com/pytest-dev/unittest2pytest){.ulink} tool
exists. It converts all `self.assert*`{.literal} method calls to plain
asserts, and also converts `self.assertRaises`{.literal} calls to the
appropriate pytest idiom.

Install it using `pip`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pip install unittest2pytest
```
:::

Once installed, you can now execute it on the files you want:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ unittest2pytest test_simple2.py
RefactoringTool: Refactored test_simple2.py
--- test_simple2.py (original)
+++ test_simple2.py (refactored)
@@ -5,6 +5,7 @@
 import unittest
 from collections import namedtuple
 from pathlib import Path
+import pytest

 DATA = """
 Main Grid,48,44
@@ -49,12 +50,12 @@
         self.grids = list(iter_grids_from_csv(self.filepath))

     def test_read_properties(self):
-        self.assertEqual(self.grids[0], GridData("Main Grid", 48, 44))
-        self.assertEqual(self.grids[1], GridData("2nd Grid", 24, 21))
-        self.assertEqual(self.grids[2], GridData("3rd Grid", 24, 48))
+        assert self.grids[0] == GridData("Main Grid", 48, 44)
+        assert self.grids[1] == GridData("2nd Grid", 24, 21)
+        assert self.grids[2] == GridData("3rd Grid", 24, 48)

     def test_invalid_path(self):
-        with self.assertRaises(IOError):
+        with pytest.raises(IOError):
             list(iter_grids_from_csv(Path("invalid file")))

     @unittest.expectedFailure
RefactoringTool: Files that need to be modified:
RefactoringTool: test_simple2.py
```
:::

By default, it won\'t touch the files and will only show the difference
in the changes it could apply. To actually apply the changes, pass
`-wn`{.literal} (`--write`{.literal} and `--nobackups`{.literal}).

Note that in the previous example, it correctly replaced the
`self.assert*`{.literal} calls, `self.assertRaises`{.literal}, and added
the `pytest`{.literal} import. It did not change the subclass of our
test class, as this could have other consequences, depending on the
actual subclass you are using, so `unittest2pytest`{.literal} leaves
that alone.

The updated file runs just as before:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest test_simple2.py
======================== test session starts ========================
...
collected 3 items

test_simple2.py ..x                                            [100%]

================ 2 passed, 1 xfailed in 0.10 seconds ================
```
:::

Adopting pytest as a runner and being able to use plain assert
statements is a great win that is often underestimated: it is liberating
to not have to type `self.assert...`{.literal} all the time any more. 

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note14}Note {#note .title}

At the time of writing, `unittest2pytest`{.literal} doesn\'t handle the
`self.fail("not implemented yet")`{.literal} statement of the last test
yet. So, we need to replace it manually with
`assert 0, "not implemented yet"`{.literal}. Perhaps you would like
to submit a PR to improve the project?
(<https://github.com/pytest-dev/unittest2pytest>).



[]{#ch06lvl1sec39}Handling setup/teardown {#handling-setupteardown .title style="clear: both"}
-----------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

To fully convert a `TestCase`{.literal} subclass to pytest style, we
need to replace `unittest`{.literal} with pytest\'s idioms. We have
already seen how to do that with `self.assert*`{.literal} methods in the
previous section, by using `unittest2pytest`{.literal}. But what can we
do do about `setUp`{.literal} and `tearDown`{.literal} methods?

As we learned[]{#id325091956 .indexterm} previously, autouse fixtures
work[]{#id325533900 .indexterm} just fine in `TestCase`{.literal}
subclasses, so they are a natural way to replace `setUp`{.literal} and
`tearDown`{.literal} methods. Let\'s use the example from the previous
section.

 

After converting the `assert`{.literal} statements, the first thing to
do is to remove the `unittest.TestCase`{.literal} subclassing:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test(unittest.TestCase):
    ...
```
:::

This becomes the following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test:
    ...
```
:::

Next, we need to transform the `setup`{.literal}/`teardown`{.literal}
methods into fixture equivalents:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    @classmethod
    def setUpClass(cls):
        cls.temp_dir = Path(tempfile.mkdtemp())
        cls.filepath = cls.temp_dir / "data.csv"
        cls.filepath.write_text(DATA.strip())

    @classmethod
    def tearDownClass(cls):
        shutil.rmtree(cls.temp_dir)
```
:::

So, the class-scoped `setUpClass`{.literal} and
`tearDownClass`{.literal} methods will become a single class-scoped
fixture:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    @classmethod
@pytest.fixture(scope='class', autouse=True)
    def _setup_class(cls):
        temp_dir = Path(tempfile.mkdtemp())
        cls.filepath = temp_dir / "data.csv"
        cls.filepath.write_text(DATA.strip())
yield
        shutil.rmtree(temp_dir)
```
:::

Thanks to the `yield`{.literal} statement, we can easily write the
teardown code in the fixture itself, as we\'ve already learned.

Here are some observations:

::: {.itemizedlist}
-   Pytest doesn\'t care what we call our fixture, so we could just as
    well keep the old `setUpClass`{.literal} name. We chose to change it
    to `setup_class`{.literal} instead, with two objectives: avoid
    confusing readers of this code, because it might seem that it is
    still a `TestCase`{.literal} subclass, and using a
    `_`{.literal} prefix denotes that this fixture should not be used as
    a normal pytest fixture.
:::

::: {.itemizedlist}
-   We change `temp_dir`{.literal} to a local variable because we don\'t
    need to keep it around in `cls`{.literal} any more. Previously, we
    had to do that because we needed to access `cls.temp_dir`{.literal}
    during `tearDownClass`{.literal}, but now we can keep it as a local
    variable instead and access it after the `yield`{.literal}
    statement. That\'s one of the beautiful things about using
    `yield`{.literal} to separate setup and teardown code: you don\'t
    need to keep context variables around; they are kept naturally as
    the local variables of the function.
:::

We follow the same approach with the `setUp`{.literal} method:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    def setUp(self):
        self.grids = list(iter_grids_from_csv(self.filepath))
```
:::

This becomes the following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.fixture(autouse=True)
    def _setup(self):
        self.grids = list(iter_grids_from_csv(self.filepath))
```
:::

This technique is very useful because you can get a pure pytest class
from a minimal set of changes. Also, using a naming
convention[]{#id325541520 .indexterm} for fixtures, as we
did[]{#id325541528 .indexterm} previously, helps to convey to readers
that the fixtures are converting the old
`setup`{.literal}/`teardown`{.literal} idioms.

Now that this class is a proper pytest class, you are free to use
fixtures and parametrization.



[]{#ch06lvl1sec40}Managing test hierarchies {#managing-test-hierarchies .title style="clear: both"}
-------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

As we have seen, it is common[]{#id324673427 .indexterm}to need to share
functionality in large test suites. Because `unittest`{.literal} is
based on subclassing `TestCase`{.literal}, it is common to put extra
functionality in your `TestCase`{.literal} subclass itself. For example,
if we need to test application logic that requires a database, we might
initially add functionality to the start and connect to a database in
our `TestCase`{.literal} subclass directly:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test(unittest.TestCase):

    def setUp(self):
        self.db_file = self.create_temporary_db()
        self.session = self.connect_db(self.db_file)

    def tearDown(self):
        self.session.close()
        os.remove(self.db_file)

    def create_temporary_db(self):
        ...

    def connect_db(self, db_file):
        ...

    def create_table(self, table_name, **fields):
        ...

    def check_row(self, table_name, **query):
        ...

    def test1(self):
        self.create_table("weapons", name=str, type=str, dmg=int)
        ...
```
:::

This works well for a single test module, but often it is the case that
we need this functionality in another test module sometime later. The
`unittest`{.literal} module does not contain built-in provisions to
share common `setup`{.literal}/`teardown`{.literal} code, so what comes
naturally for most people is to extract the required functionality in a
superclass, and then a subclass from that, where needed:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
# content of testing.py
class DataBaseTesting(unittest.TestCase):

    def setUp(self):
        self.db_file = self.create_temporary_db()
        self.session = self.connect_db(self.db_file)

    def tearDown(self):
        self.session.close()
        os.remove(self.db_file)

    def create_temporary_db(self):
        ...

    def connect_db(self, db_file):
        ...

    def create_table(self, table_name, **fields):
        ...

    def check_row(self, table_name, **query):
        ...

# content of test_database2.py
from . import testing

class Test(testing.DataBaseTesting):

    def test1(self):
        self.create_table("weapons", name=str, type=str, dmg=int)
        ...
```
:::

The superclass usually not only contains
`setup`{.literal}/`teardown`{.literal} code, but it often also includes
utility functions that call `self.assert*`{.literal} to perform common
checks (such as `check_row`{.literal} in the previous example).

Continuing with our example: some time later, we need completely
different functionality in another test module, for example, to test a
GUI application. We are now wiser and suspect we will need GUI-related
functionality in several other test modules, so we start by creating a
superclass with the functionality we need directly:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class GUITesting(unittest.TestCase):

    def setUp(self):
        self.app = self.create_app()

    def tearDown(self):
        self.app.close_all_windows()

    def mouse_click(self, window, button):
        ...

    def enter_text(self, window, text):
        ...
```
:::

The approach of moving `setup`{.literal}/`teardown`{.literal} and test
functionality to superclasses works `OK`{.literal} and is easy to
understand.

The problem comes when we get to the point where we need two unrelated
functionalities in the same test module. In that case, we have no other
choice than to resort to multiple inheritance. Suppose we need to test a
dialog that connects to the database; we will need to write code such as
this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
from . import testing

class Test(testing.DataBaseTesting, testing.GUITesting):

    def setUp(self):
        testing.DataBaseTesting.setUp(self)
        testing.GUITesting.setUp(self)

    def tearDown(self):
        testing.GUITesting.setUp(self)
        testing.DataBaseTesting.setUp(self)
```
:::

Multiple inheritance in general[]{#id325092078 .indexterm} tends to make
the code less readable and harder to follow. Here, it also has the
additional aggravation that we need to call `setUp`{.literal} and
`tearDown`{.literal} in the correct order, explicitly. 

Another point to be aware of is that `setUp`{.literal} and
`tearDown`{.literal} are optional in the `unittest`{.literal} framework,
so it is common for a certain class to not declare a
`tearDown`{.literal} method at all if it doesn\'t need any teardown
code. If this class contains functionality that is later moved to a
superclass, many subclasses probably will not declare a
`tearDown`{.literal} method as well. The problem comes when, later on in
a multiple inheritance scenario, you improve the super class and need to
add a `tearDown`{.literal} method, because you will now have to go over
all subclasses and ensure that they call the `tearDown`{.literal} method
of the super class.

So, let\'s say we find ourselves in the previous situation and we want
to start to use pytest functionality that is incompatible with
`TestCase`{.literal} tests. How can we refactor our utility classes so
we can use them naturally from pytest and also keep existing
`unittest`{.literal}-based tests working?

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch06lvl2sec72}Reusing test code with fixtures {#reusing-test-code-with-fixtures .title}

</div>

</div>
:::

The first thing we should do is to extract[]{#id325162657 .indexterm}
the desired functionality[]{#id325533303 .indexterm} into well-defined
fixtures and put them into a `conftest.py`{.literal} file. Continuing
with our example, we can create `db_testing`{.literal} and
`gui_testing`{.literal} fixtures:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class DataBaseFixture:

    def __init__(self):
        self.db_file = self.create_temporary_db()
        self.session = self.connect_db(self.db_file)

    def teardown(self):
        self.session.close()
        os.remove(self.db_file)

    def create_temporary_db(self):
        ...

    def connect_db(self, db_file):
        ...

    ...

@pytest.fixture
def db_testing():
    fixture = DataBaseFixture()
    yield fixture
    fixture.teardown()


class GUIFixture:

    def __init__(self):
        self.app = self.create_app()

    def teardown(self):
        self.app.close_all_windows()

    def mouse_click(self, window, button):
        ...

    def enter_text(self, window, text):
        ...


@pytest.fixture
def gui_testing():
    fixture = GUIFixture()
    yield fixture
    fixture.teardown()
```
:::

Now, you can start to write new tests using plain pytest style and use
the `db_testing`{.literal} and `gui_testing`{.literal} fixtures, which
is great because it opens the door to use pytest features in new tests.
But the cool thing here is that we can now change
`DataBaseTesting`{.literal} and `GUITesting`{.literal} to reuse the
functionality provided by the fixtures, in a way that we don\'t break
existing code:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class DataBaseTesting(unittest.TestCase):

@pytest.fixture(autouse=True)
def _setup(self, db_testing):
        self._db_testing = db_testing

    def create_temporary_db(self):
        return self._db_testing.create_temporary_db()

    def connect_db(self, db_file):
        return self._db_testing.connect_db(db_file)

    ...


class GUITesting(unittest.TestCase):

@pytest.fixture(autouse=True)
    def _setup(self, gui_testing):
        self._gui_testing = gui_testing

    def mouse_click(self, window, button):
        return self._gui_testing.mouse_click(window, button)

    ...
```
:::

Our `DatabaseTesting`{.literal} and `GUITesting`{.literal} classes
obtain the fixture values by declaring an autouse `_setup`{.literal}
fixture, a trick we have learned early[]{#id325092200 .indexterm} in
this chapter. We can[]{#id325092209 .indexterm} get rid of the
`tearDown`{.literal} method because the fixture will take care of
cleaning up after itself after each test, and the utility methods become
simple proxies for the methods implemented in the fixture.

As bonus points, `GUIFixture`{.literal} and `DataBaseFixture`{.literal}
can also use other pytest fixtures. For example, we can probably remove
`DataBaseTesting.create_temporary_db()`{.literal} and use the built-in
`tmpdir`{.literal} fixture to create the temporary database file for us:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class DataBaseFixture:

    def __init__(self, tmpdir):
        self.db_file = str(tmpdir / "file.db")
        self.session = self.connect_db(self.db_file)

    def teardown(self):
        self.session.close()

    ...

@pytest.fixture
def db_testing(tmpdir):
    fixture = DataBaseFixture(tmpdir)
    yield fixture
    fixture.teardown()
```
:::

Using other fixtures can then greatly simplify the existing testing
utilities code.

It is worth emphasizing that this refactoring will require zero
changes to the existing tests. Here, again, one of the benefits of
fixtures becomes evident: changes in requirements of a fixture do not
affect tests that use the fixture.



[]{#ch06lvl1sec41}Refactoring test utilities {#refactoring-test-utilities .title style="clear: both"}
--------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

In the previous section, we saw how[]{#id324673415 .indexterm} test
suites might make use of subclasses to share test functionality and how
to refactor them into fixtures while keeping existing tests working.

An alternative to sharing test functionality through superclasses in
`unittest`{.literal} suites is to write separate utility classes and use
them inside the tests. Getting back to our example, where we need to
have database-related facilities, here is a way to implement that in a
`unittest`{.literal}-friendly way, without using superclasses:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
# content of testing.py
class DataBaseTesting:

    def __init__(self, test_case):        
        self.db_file = self.create_temporary_db()
        self.session = self.connect_db(self.db_file)
self.test_case = test_case
test_case.addCleanup(self.teardown)

    def teardown(self):
        self.session.close()
        os.remove(self.db_file)

    ...

    def check_row(self, table_name, **query):
        row = self.session.find(table_name, **query)
self.test_case.assertIsNotNone(row)
        ...

# content of test_1.py
from testing import DataBaseTesting

class Test(unittest.TestCase):

    def test_1(self):
        db_testing = DataBaseTesting(self)
        db_testing.create_table("weapons", name=str, type=str, dmg=int)
        db_testing.check_row("weapons", name="zweihander")
        ...
```
:::

In this approach, we separate our testing functionality in a class that
receives the current `TestCase`{.literal} instance as its first
argument, followed by any other arguments, as required.

The `TestCase`{.literal} instance serves two purposes: to provide the
class access to the various `self.assert*`{.literal} functions, and as a
way to register clean-up functions with
`TestCase.addCleanup`{.literal} (<https://docs.python.org/3/library/unittest.html#unittest.TestCase.addCleanup>).
`TestCase.addCleanup`{.literal} registers functions that will be called
after each test is done, regardless of whether they have been
successful. I consider them a superior alternative to the
`setUp`{.literal}/`tearDown`{.literal} functions because they allow
resources to be created and immediately registered for cleaning up.
Creating all resources during `setUp`{.literal} and releasing them
during `tearDown`{.literal} has the disadvantage that if any exception
is raised during the `setUp`{.literal} method, then `tearDown`{.literal}
will not be called at all, leaking resources and state, which might
affect the tests that follow.

If your `unittest`{.literal} suite uses this approach for testing
facilities, then the good news is that you are in for an easy ride to
convert/reuse this functionality for pytest.

Because this approach is very similar to how fixtures work, it is simple
to change the classes slightly to work as fixtures:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
# content of testing.py
class DataBaseFixture:

    def __init__(self):
        self.db_file = self.create_temporary_db()
        self.session = self.connect_db(self.db_file)

    ...

    def check_row(self, table_name, **query):
        row = self.session.find(table_name, **query)
assert row is not None

# content of conftest.py
@pytest.fixture
def db_testing():
    from .testing import DataBaseFixture
    result = DataBaseFixture()
    yield result
    result.teardown()
```
:::

We get rid of the dependency to the `TestCase`{.literal} instance
because our fixture now takes care of calling `teardown()`{.literal},
and we are free to use plain asserts instead of `Test.assert*`{.literal}
methods.

To keep the existing suite working, we just need to make a thin subclass
to handle cleanup when it is used with `TestCase`{.literal} subclasses:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
# content of testing.py
class DataBaseTesting(DataBaseFixture):

    def __init__(self, test_case):
        super().__init__()
test_case.addCleanup(self.teardown)
```
:::

With this small refactoring, we can now use native pytest fixtures in
new tests, while keeping the existing tests working exactly as before.

While this approach works well, one problem[]{#id325533924 .indexterm}
is that unfortunately, we cannot use other pytest fixtures (such
as `tmpdir`{.literal}) in our `DataBaseFixture`{.literal} class without
breaking compatibility with `DataBaseTesting`{.literal} usage in
`TestCase`{.literal} subclasses.



[]{#ch06lvl1sec42}Migration strategy {#migration-strategy .title style="clear: both"}
------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Being able to run `unittest`{.literal}-based tests out[]{#id325091700
.indexterm} of the box is definitely a very powerful feature, because it
allows you to start using pytest right away as a runner.

Eventually, you need to decide what to do with the existing
`unittest`{.literal}-based tests. There are a few approaches you can
choose:

::: {.itemizedlist}
-   [**Convert everything**]{.strong}: if your test suite is relatively
    small, you might decide to convert all tests at once. This has the
    advantage that you don\'t have to make compromises to keep existing
    `unittest`{.literal} suites working, and being simpler to be
    reviewed by others because your pull request will have a single
    theme.
-   [**Convert as you go**]{.strong}: you might decide to convert tests
    and functionality as needed. When you need to add new tests or
    change existing tests, you take the opportunity to convert tests
    and/or refactor functionality to fixtures using the techniques from
    the previous sections. This is a good approach if you don\'t want to
    spend time upfront converting everything, while slowly but surely
    paving the way to have a pytest-only test suite.
-   [**New tests only**]{.strong}: you might decide to never touch the
    existing `unittest`{.literal} suite, only writing new tests in
    pytest-style. This approach is reasonable if you have thousands of
    tests that might never need to undergo maintenance, but you will
    have to keep the hybrid approaches shown in the previous sections
    working indefinitely.
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip15}Note {#note .title}

Choose which migration strategy to use based on the time budget you have
and the test suite\'s size. 



[]{#ch06lvl1sec43}Summary {#summary .title style="clear: both"}
-------------------------

</div>

</div>

------------------------------------------------------------------------
:::

We have discussed a few strategies and tips on how to use pytest in
`unittest`{.literal}-based suites of various sizes. We started with a
discussion about using pytest as a test runner, and which features work
with `TestCase`{.literal} tests. We looked at how to use
the `unittest2pytest`{.literal} tool to convert `self.assert*`{.literal}
methods to plain assert statements and take full advantage of pytest
introspection features. Then, we learned a few techniques on how to
migrate `unittest`{.literal}-based
`setUp`{.literal}/`tearDown`{.literal} code to pytest-style in test
classes, manage functionality spread in test hierarchies, and general
utilities. Finally, we wrapped up the chapter with an overview of the
possible migration strategies that you can take for test suites of
various sizes.

In the next chapter, we will see a quick summary of what we have learned
in this book, and discuss what else might be in store for us.
