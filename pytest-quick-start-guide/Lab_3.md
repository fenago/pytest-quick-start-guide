

[]{#ch03}Chapter 3. Markers and Parametrization {#chapter-3.-markers-and-parametrization .title}
-----------------------------------------------

</div>

</div>
:::

After learning the basics of writing and running tests, we will delve
into two important pytest features: [**marks**]{.strong} and
[**parametrization**]{.strong}.

Firstly, we will learn[]{#id325162619 .indexterm} about marks, which
allow[]{#id325162628 .indexterm} us to selectively run tests based on
applied marks, and to attach general data to test functions, which can
be used by fixtures and plugins. In the same topic, we will take a look
at built-in marks and what they have to offer.

Secondly, we will learn[]{#id325162651 .indexterm} about [**test
parametrization**]{.strong}, which allows us to easily apply the same
test function to a set of input values. This greatly avoids duplicating
test code and makes it easy to add new test cases that may appear as our
software evolves.

In summary, here is what we will be covering in this chapter:

::: {.itemizedlist}
-   Mark basics
-   Built-in marks
-   Parametrization



[]{#ch03lvl1sec20}Mark basics {#mark-basics .title style="clear: both"}
-----------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Pytest allows you to mark[]{#id325091737 .indexterm} functions and
classes with metadata. This metadata can be used to selectively run
tests, and is also available for fixtures and plugins, to perform
different tasks. Let\'s take a look at how to create and apply marks to
test functions, and later on jump into built-in pytest marks.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec24}Creating marks {#creating-marks .title}

</div>

</div>
:::

Marks are created with the `@pytest.mark`{.literal} decorator. It works
as a factory, so any access to it will automatically create a new
mark[]{#id325092069 .indexterm} and apply it to a function. This is
easier to understand with an example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.slow
def test_long_computation():
    ...
```
:::

By using the `@pytest.mark.slow`{.literal} decorator, you are applying a
mark named `slow`{.literal} to `test_long_computation`{.literal}.

Marks can also receive [**parameters**]{.strong}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.timeout(10, method="thread")
def test_topology_sort():
    ...
```
:::

The `@pytest.mark.timeout`{.literal} used in the last example was taken
from the pytest-timeout plugin; for more details go
to <https://pypi.org/project/pytest-timeout/>. With this, we define that
`test_topology_sort`{.literal} should not take more than 10 seconds, in
which case it should be terminated using the `thread`{.literal} method.
Assigning arguments to marks is a very powerful feature, providing
plugins and fixtures with a lot of flexibility. We will explore those
capabilities and the `pytest-timeout`{.literal} plugin in the next
chapters.

You can add more than one mark to a test by applying the
`@pytest.mark`{.literal} decorator multiple times--- for example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.slow
@pytest.mark.timeout(10, method="thread")
def test_topology_sort():
    ...
```
:::

If you are applying the same mark over and over, you can avoid repeating
yourself by assigning it to a variable once and applying it over tests
as needed:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
timeout10 = pytest.mark.timeout(10, method="thread")

@timeout10
def test_topology_sort():
    ...

@timeout10
def test_remove_duplicate_points():
    ...
```
:::

If this mark is used in several tests, you can move it to a testing
utility module and import it as needed:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
from mylib.testing import timeout10

@timeout10
def test_topology_sort():
    ...

@timeout10
def test_remove_duplicate_points():
    ...
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec25}Running tests based on marks {#running-tests-based-on-marks .title}

</div>

</div>
:::

You can run tests by using marks[]{#id325092242 .indexterm} as selection
factors with the `-m`{.literal} flag. For example, to run all tests with
the `slow`{.literal} mark:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -m slow
```
:::

The `-m`{.literal} flag also accepts expressions, so you can do a more
advanced selection. To run all tests with the `slow`{.literal} mark but
not the tests with the `serial`{.literal} mark you can use::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -m "slow and not serial"
```
:::

The expression is limited to the `and`{.literal}, `not`{.literal}, and
`or`{.literal} operators.

Custom marks can be useful for optimizing test runs on your CI system.
Oftentimes, environment problems, missing dependencies, or even some
code committed by mistake might cause the entire test suite to fail. By
using marks, you can choose a few tests that are fast and/or broad
enough to detect problems in a good portion of the code and then run
those first, before all the other tests. If any of those tests fail, we
abort the job and avoid wasting potentially a lot of time by running all
tests that are doomed to fail anyway.

We start by applying a custom mark to those tests. Any name will do, but
a common name used is `smoke`{.literal}, as in [*smoke
detector*]{.emphasis}, to detect problems before everything bursts into
flames.

You then run smoke tests first, and only after they pass do, you run the
complete test suite: 

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -m "smoke"
...
λ pytest -m "not smoke"
```
:::

If any smoke test fails, you don\'t have to wait for the entire suite to
finish to obtain this feedback. 

You can add to this technique by creating layers of tests, from simplest
to slowest, creating a test hierarchy of sorts. For example:

::: {.itemizedlist}
-   `smoke`{.literal}
-   `unittest`{.literal}
-   `integration`{.literal}
-   `<all the rest>`{.literal}
:::

This would then be executed like this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -m "smoke"
...
λ pytest -m "unittest"
...
λ pytest -m "integration"
...
λ pytest -m "not smoke and not unittest and not integration"
```
:::

Make sure to include the fourth step; otherwise, tests without marks
will never run.

Using marks to differentiate tests in different pytest runs can also be
used in other scenarios. For instance, when using the
`pytest-xdist`{.literal} plugin to run tests in parallel, we have a
parallel session that executes most test suites in parallel but might
decide to run a few tests in a separate pytest session serially because
they are sensitive or problematic when executed together.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec26}Applying marks to classes {#applying-marks-to-classes .title}

</div>

</div>
:::

You can apply the `@pytest.mark`{.literal} decorator to a class. This
will apply that same mark to all tests methods[]{#id325022662
.indexterm} in that class, avoiding have to copy and paste the mark code
over all test methods:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.timeout(10)
class TestCore:

    def test_simple_simulation(self):
        ...

    def test_compute_tracers(self):
        ...
```
:::

The previous code is essentially the same as the following code:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class TestCore:

    @pytest.mark.timeout(10)
    def test_simple_simulation(self):
        ...

@pytest.mark.timeout(10)
    def test_compute_tracers(self):
        ...
```
:::

However, there is one difference: applying the `@pytest.mark`{.literal}
decorator to a class means that all its subclasses will inherit the
mark. Subclassing test classes is not common, but it is sometimes a
useful technique to avoid repeating test code, or to ensure that
implementations comply with a certain interface. We will see more
examples of this later in this chapter and in [Chapter
4](https://subscription.packtpub.com/book/web_development/9781789347562/4){.link},
[*Fixtures*]{.emphasis}.

Like test functions, decorators can be applied multiple times:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.slow
@pytest.mark.timeout(10)
class TestCore:

    def test_simple_simulation(self):
        ...

    def test_compute_tracers(self):
        ...
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec27}Applying marks to modules {#applying-marks-to-modules .title}

</div>

</div>
:::

We can also apply a mark to all test[]{#id325022726 .indexterm}
functions and test[]{#id325022734 .indexterm} classes in a module. Just
declare a [**global variable**]{.strong} named `pytestmark`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import pytest

pytestmark = pytest.mark.timeout(10)


class TestCore:

    def test_simple_simulation(self):
        ...


def test_compute_tracers():
    ...
```
:::

The following is the equivalent to this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import pytest


@pytest.mark.timeout(10)
class TestCore:

    def test_simple_simulation(self):
        ...


@pytest.mark.timeout(10)
def test_compute_tracers():
    ...
```
:::

You can use a `tuple`{.literal} or `list`{.literal} of marks to apply
multiple marks as well:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import pytest

pytestmark = [pytest.mark.slow, pytest.mark.timeout(10)]
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec28}Custom marks and pytest.ini {#custom-marks-and-pytest.ini .title}

</div>

</div>
:::

Being able to declare new marks[]{#id325022829 .indexterm} on the fly
just by applying the `pytest.mark`{.literal} decorator is convenient. It
makes it a breeze[]{#id325022841 .indexterm} to quickly start enjoying
the benefits of using marks.

This convenience comes at a price: it is possible for a user to make a
typo in the mark name, for example `@pytest.mark.solw`{.literal},
instead of `@pytest.mark.slow`{.literal}. Depending on the project under
testing, this typo might be a mere annoyance or a more serious problem.

So, let\'s go back to our previous example, where a test suite is
executed in layers on CI based on marked tests:

::: {.itemizedlist}
-   `smoke`{.literal}
-   `unittest`{.literal}
-   `integration`{.literal}
-   `<all the rest>`{.literal}
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -m "smoke"
...
λ pytest -m "unittest"
...
λ pytest -m "integration"
...
λ pytest -m "not smoke and not unittest and not integration"
```
:::

A developer could make a typo while applying a mark to one of the tests:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.smoek
def test_simulation_setup():
    ...
```
:::

This means the test will execute at the last step, instead of on the
first step with the other `smoke`{.literal} tests. Again, this might
vary from a small nuisance to a terrible problem, depending on the test
suite.

Mature test suites that have a fixed set of marks might declare them in
the `pytest.ini`{.literal} file:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
markers =
    slow
    serial
    smoke: quick tests that cover a good portion of the code
    unittest: unit tests for basic functionality
    integration: cover to cover functionality testing    
```
:::

The `markers`{.literal} option accepts a list of markers in the form
of `<name>: description`{.literal}, with the description part being
optional (`slow`{.literal} and `serial`{.literal} in the last example
don\'t have a description).

A full list of marks can be displayed by using the `--markers`{.literal}
flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --markers
@pytest.mark.slow:

@pytest.mark.serial:

@pytest.mark.smoke: quick tests that cover a good portion of the code

@pytest.mark.unittest: unit tests for basic functionality

@pytest.mark.integration: cover to cover functionality testing

...
```
:::

The `--strict`{.literal} flag makes it an error to use marks not
declared in the `pytest.ini`{.literal} file. Using our previous example
with a typo, we now obtain an error, instead of pytest silently creating
the mark when running with `--strict`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --strict tests\test_wrong_mark.py
...
collected 0 items / 1 errors

============================== ERRORS ===============================
_____________ ERROR collecting tests/test_wrong_mark.py _____________
tests\test_wrong_mark.py:4: in <module>
    @pytest.mark.smoek
..\..\.env36\lib\site-packages\_pytest\mark\structures.py:311: in __getattr__
    self._check(name)
..\..\.env36\lib\site-packages\_pytest\mark\structures.py:327: in _check
    raise AttributeError("%r not a registered marker" % (name,))
E AttributeError: 'smoek' not a registered marker
!!!!!!!!!!!!!! Interrupted: 1 errors during collection !!!!!!!!!!!!!!
====================== 1 error in 0.09 seconds ======================
```
:::

Test suites that want to ensure that all marks are registered in
`pytest.ini`{.literal} should also use `addopts`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
addopts = --strict
markers =
    slow
    serial
    smoke: quick tests that cover a good portion of the code
    unittest: unit tests for basic functionality
    integration: cover to cover functionality testing
```



[]{#ch03lvl1sec21}Built-in marks {#built-in-marks .title style="clear: both"}
--------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Having learned the basics of marks[]{#id324673415 .indexterm} and how to
use them, let\'s now take a look at some built-in pytest marks. This is
not an exhaustive list of all built-in marks, but the more commonly used
ones. Also, keep in mind that many plugins introduce other marks as
well.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec29}\@pytest.mark.skipif {#pytest.mark.skipif .title}

</div>

</div>
:::

You might have tests that[]{#id325023324 .indexterm} should not be
executed unless some condition is met. For example, some tests might
depend on certain libraries that are not always installed, or a local
database that might not be online, or are executed only on certain
platforms. 

Pytest provides a built-in mark, `skipif`{.literal}, that can be used to
[*skip*]{.emphasis} tests based on specific conditions. Skipped tests
are not executed if the condition is true, and are not counted towards
test suite failures.

For example, you can use the `skipif`{.literal} mark to always skip a
test when executing on Windows:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import sys
import pytest

@pytest.mark.skipif(
    sys.platform.startswith("win"),
    reason="fork not available on Windows",
)
def test_spawn_server_using_fork():
    ...
```
:::

The first argument to `@pytest.mark.skipif`{.literal} is the condition:
in this example, we are telling pytest to skip this test in Windows. The
`reason=`{.literal} keyword argument is mandatory and is used to display
why the test was skipped when using the `-ra`{.literal} flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
 tests\test_skipif.py s                                        [100%]
====================== short test summary info ======================
SKIP [1] tests\test_skipif.py:6: fork not available on Windows
===================== 1 skipped in 0.02 seconds =====================
```
:::

It is good style to always write descriptive messages, including ticket
numbers when applicable.

Alternatively, we can write the same condition as follows:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import os
import pytest

@pytest.mark.skipif(
    not hasattr(os, 'fork'), reason="os.fork not available"
)
def test_spawn_server_using_fork2():
    ...
```
:::

The latter version checks whether the actual feature is available,
instead of making assumptions based on the platform (Windows currently
does not have an `os.fork`{.literal} function, but perhaps in the future
Windows might support the function). The same thing is common when
testing features of libraries based on their version, instead of
checking whether some functions exist. I suggest that so this
reads: some functions exist. I suggest that when possible, prefer to
check whether a function actually exists, instead of checking for a
specific version of the library.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip9}Note {#note .title}

Checking capabilities and features is usually a better approach, instead
of checking platforms and library version numbers.
:::

The following is the full `@pytest.mark.skipif`{.literal} signature:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.skipif(condition, *, reason=None)
```
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch03lvl3sec13}pytest.skip {#pytest.skip .title}

</div>

</div>
:::

The `@pytest.mark.skipif`{.literal} decorator is very[]{#id325092109
.indexterm} handy, but the mark must evaluate the condition at
`import`{.literal}/`collection`{.literal} time, to determine whether the
test should be skipped. We want to minimize test collection time,
because, after all, we might end up not even executing all tests if
the `-k`{.literal} or `--lf`{.literal} flags are being used, for
example.

Sometimes, it might even be almost impossible (without some gruesome
hack) to check whether a test should be skipped during import time. For
example, you can make the decision to skip a test based on the
capabilities of the graphics driver only after initializing the
underlying graphics library, and initializing the graphics subsystem is
definitely not something you want to do at import time.

For those cases, pytest lets you skip tests imperatively in the test
body by using the `pytest.skip`{.literal} function:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_shaders():
    initialize_graphics()
    if not supports_shaders():
        pytest.skip("shades not supported in this driver")
# rest of the test code
    ...
```
:::

`pytest.skip`{.literal} works by raising an internal exception, so it
follows normal Python exception semantics, and nothing else needs to be
done for the test to be skipped properly.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch03lvl3sec14}pytest.importorskip {#pytest.importorskip .title}

</div>

</div>
:::

It is common for libraries to have[]{#id325533308 .indexterm} tests that
depend on a certain library being installed. For example, pytest\'s own
test suite has some tests for `numpy`{.literal} arrays, which should be
skipped if `numpy`{.literal} is not installed.

One way to handle this would be to manually try to import the library
and skip the test if it is not present:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_tracers_as_arrays_manual():
    try:
        import numpy
    except ImportError:
        pytest.skip("requires numpy")
    ...
```
:::

This can get old quickly, so for this reason, pytest provides the handy
`pytest.importorskip`{.literal} function:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_tracers_as_arrays():
    numpy = pytest.importorskip("numpy")
    ...
```
:::

`pytest.importorskip`{.literal} will import the module and return the
module object, or skip the test entirely if the module could not be
imported. 

If your test requires a minimum version of the library,
`pytest.importorskip`{.literal} also supports a `minversion`{.literal}
argument:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_tracers_as_arrays_114():
    numpy = pytest.importorskip("numpy", minversion="1.14")
    ...
```
:::
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec30}\@pytest.mark.xfail {#pytest.mark.xfail .title}

</div>

</div>
:::

You can use `@pytest.mark.xfail`{.literal} decorator to indicate that a
test is [*`expected to fail`{.literal}*]{.emphasis}. As usual, we apply
the[]{#id325534003 .indexterm} mark decorator to the test function or
method:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.xfail
def test_simulation_34():
    ...
```
:::

This mark supports some parameters, all of which we will see later in
this section; but one in particular warrants discussion now:
the `strict`{.literal} parameter. This parameter defines two distinct
behaviors for the mark:

::: {.itemizedlist}
-   With `strict=False`{.literal} (the default), the test will be
    counted separately as an [**XPASS**]{.strong}(if it passes)
    or an [**XFAIL**]{.strong}(if it fails), and will[**not fail the
    test suite**]{.strong}
-   With `strict=True`{.literal}, the test will be marked as
    [**XFAIL**]{.strong} if it fails, but if it unexpectedly passes, it
    will [**fail the test suite**]{.strong}, as a normal failing test
    would
:::

But why would you want to write a test that you expect to fail anyway,
and in which occasions is this useful? This might seem weird at first,
but there are a few situations where this comes in handy.

The first situation is when a test always fails, and you want to be told
(loudly) if it suddenly starts passing. This can happen when:

::: {.itemizedlist}
-   You found out that the cause of a bug in your code is due to a
    problem in a third-party library. In this situation, you can write a
    failing test that demonstrates the problem, and mark it with
    `@pytest.mark.xfail(strict=True)`{.literal}. If the test fails, the
    test will be marked as [**XFAIL**]{.strong} in the test session
    summary, but if the test [**passes**]{.strong}, it will [**fail the
    test suite**]{.strong}. This test might start to pass when you
    upgrade the library that was causing the problem, and this will
    alert you that the issue has been fixed and needs your attention.
-   You have thought about a new feature, and design one or more test
    cases that exercise it even before your start implementing it. You
    can commit the tests with the
    `@pytest.mark.xfail(strict=True)`{.literal} mark applied, and remove
    the mark from the tests as you code the new feature. This is very
    useful in a collaborative environment, where one person supplies
    tests on how they envision a new feature/API, and another person
    implements it based on the test cases. 
-   You discover a bug in your application and write a test case
    demonstrating the problem. You might not have the time to tackle it
    right now or another person would be better suited to work in that
    part of the code. In this situation, marking the test
    as `@pytest.mark.xfail(strict=True)`{.literal} would be a good
    approach.
:::

All the cases above share one characteristic: you have a failing test
and want to know whether it suddenly starts passing. In this case, the
fact that the test passes warns you about a fact that requires your
attention: a new version of a library with a bug fix has been released,
part of a feature is now working as intended, or a known bug has been
fixed. 

The other situation where the `xfail`{.literal} mark is useful is when
you have tests that fail [*sometimes*]{.emphasis}, also called
[**flaky**]{.strong}[**tests**]{.strong}. Flaky tests are tests that
fail on occasion, even if the underlying code has not changed. There are
many reasons why tests fail in a way that appears to be random; the
following are a few:

::: {.itemizedlist}
-   Timing issues in multi threaded code
-   Intermittent network connectivity problems
-   Tests that don\'t deal with asynchronous events correctly
-   Relying on non-deterministic behavior
:::

That is just to list a few possible causes. This non-determinism usually
happens in tests with broader scopes, such as integration or UI. The
fact is that you will almost always have to deal with flaky tests in
large test suites.

Flaky tests are a serious problem, because the test suite is supposed to
be an indicator that the code is working as intended and that it can
detect real problems when they happen. Flaky tests destroy that image,
because often developers will see flaky tests failing that don\'t have
anything to do with recent code changes. When this becomes commonplace,
people begin to just run the test suite again in the hope that this time
the flaky test passes (and it often does), but this erodes the trust in
the test suite as a whole, and brings frustration to the development
team. You should treat flaky tests as a menace that should be contained
and dealt with.

Here are some suggestions regarding how to deal with flaky tests within
a development team:

::: {.orderedlist}
1.  First, you need to be able to correctly identify flaky tests. If a
    test fails that apparently doesn\'t have anything to do with the
    recent changes, run the tests again. If the test that failed
    previously now `passes`{.literal}, it means the test is flaky.
2.  Create an issue to deal with that particular flaky test in your
    ticket system. Use a naming convention or other means to label that
    issue as related to a flaky test (for example GitHub or JIRA
    labels).
3.  Apply the
    `@pytest.mark.xfail(reason="flaky test #123", strict=False)`{.literal}
    mark, making sure to include the issue ticket number or identifier.
    Feel free to add more information to the description, if you like.
4.  Make sure to periodically assign issues about flaky tests to
    yourself or other team members (for example, during sprint
    planning). The idea is to take care of flaky tests at a comfortable
    pace, eventually reducing or eliminating them altogether.
:::

These practices take care of two[]{#id325541495 .indexterm} major
problems: they allow you to avoid eroding trust in the test suite, by
getting flaky tests out of the way of the development team, and they put
a policy in place to deal with flaky tests in their due time.

Having covered the situations where the `xfail`{.literal} mark is
useful, let\'s take a look at the full signature:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.xfail(condition=None, *, reason=None, raises=None, run=True, strict=False)
```
:::

::: {.itemizedlist}
-   `condition`{.literal}: the first parameter, if given, is a
    `True`{.literal}/`False`{.literal} condition, similar to the one
    used by `@pytest.mark.skipif`{.literal}: if `False`{.literal}, the
    `xfail`{.literal} mark is ignored. It is useful to mark a test as
    `xfail`{.literal} based on an external condition, such as the
    platform, Python version, library version, and so on.
    ::: {.informalexample}
    ::: {.toolbar .clearfix}
    Copy
    :::

    ``` {.programlisting .language-markup}
    @pytest.mark.xfail(
        sys.platform.startswith("win"),
        reason="flaky on Windows #42", strict=False
    )
    def test_login_dialog():
        ...
    ```
    :::
-   `reason`{.literal}: a string that will be shown in the short test
    summary when the `-ra`{.literal} flag is used. It is highly
    recommended to always use this parameter to explain the reason why
    the test has been marked as `xfail`{.literal} and/or include a
    ticket number.
    ::: {.informalexample}
    ::: {.toolbar .clearfix}
    Copy
    :::

    ``` {.programlisting .language-markup}
    @pytest.mark.xfail(
        sys.platform.startswith("win"), 
    reason="flaky on Windows #42", strict=False
    )
    def test_login_dialog():
        ...
    ```
    :::
-   `raises`{.literal}: given an exception type, it declares that we
    expect the test to raise an instance of that exception. If the test
    raises another type of exception (even `AssertionError`{.literal}),
    the test will `fail`{.literal} normally. It is especially useful for
    missing functionality or to test for known bugs.
    ::: {.informalexample}
    ::: {.toolbar .clearfix}
    Copy
    :::

    ``` {.programlisting .language-markup}
    @pytest.mark.xfail(raises=NotImplementedError,
                       reason='will be implemented in #987')
    def test_credential_check():
        check_credentials('Hawkwood') # not implemented yet
    ```
    :::
-   `run`{.literal}: if `False`{.literal}, the test will not even be
    executed and will fail as XFAIL. This is particularly useful for
    tests that run code that might crash the test-suite process (for
    example, C/C++ extensions causing a segmentation
    fault[]{#id325091808 .indexterm} due to a known problem).
    ::: {.informalexample}
    ::: {.toolbar .clearfix}
    Copy
    :::

    ``` {.programlisting .language-markup}
    @pytest.mark.xfail(
    run=False, reason="undefined particles cause a crash #625"
    )
    def test_undefined_particle_collision_crash():
        collide(Particle(), Particle())
    ```
    :::
-   `strict`{.literal}: if `True`{.literal}, a passing test will fail
    the test suite. If `False`{.literal}, the test will not fail the
    test suite regardless of the outcome (the default is
    `False`{.literal}). This was discussed in detail at the start of
    this section.
:::

The configuration variable `xfail_strict`{.literal} controls the default
value of the `strict`{.literal} parameter of `xfail`{.literal} marks:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
xfail_strict = True
```
:::

Setting it to `True`{.literal} means that all xfail-marked tests without
an explicit `strict`{.literal} parameter are considered an actual
failure expectation instead of a flaky test. Any `xfail`{.literal} mark
that explicitly passes the `strict`{.literal} parameter overrides the
configuration value.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch03lvl3sec15}pytest.xfail {#pytest.xfail .title}

</div>

</div>
:::

Finally, you can imperatively trigger[]{#id325091911 .indexterm} an
XFAIL result within a test by calling the `pytest.xfail`{.literal}
function:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_particle_splitting():
    initialize_physics()
    import numpy
    if numpy.__version__ < "1.13":
pytest.xfail("split computation fails with numpy < 1.13")
    ...
```
:::

Similar to `pytest.skip`{.literal}, this is useful when you can only
determine whether you need to mark a test as `xfail`{.literal} at
runtime.




[]{#ch03lvl1sec22}Parametrization {#parametrization .title style="clear: both"}
---------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

A common testing activity is passing[]{#id325023275 .indexterm} multiple
values to the same test function and asserting the outcome.

Suppose we have an application that allows the user to define custom
mathematical formulas, which will be parsed and evaluated at runtime.
The formulas are given as strings, and can make use of mathematical
functions such as `sin`{.literal}, `cos`{.literal}, `log`{.literal}, and
so on. A very simple way to implement this in Python would be to use the
`eval`{.literal} built-in
(<https://docs.python.org/3/library/functions.html#eval>), but because
it can execute arbitrary code, we opt to use a custom tokenizer and
evaluator for safety, instead.. 

Let\'s not delve into the implementation details but rather focus on a
test:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_formula_parsing():
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string("C0 * x + 10", tokenizer)
    assert formula.eval(x=1.0, C0=2.0) == pytest.approx(12.0)
```
:::

Here, we create a `Tokenizer`{.literal} class, which is used by our
implementation to break the formula string into internal tokens for
later processing. Then, we pass the formula string and tokenizer
to `Formula.from_string`{.literal}, to obtain a formula object. With the
formula object on our hands, we pass the input values
to `formula.eval`{.literal} and check that the returned value matches
our expectation. 

But we also want to test other math operations, to ensure we are
covering all the features of our `Formula`{.literal} class.

One approach is to expand our test by using multiple assertions to check
other formulas and input values:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_formula_parsing():
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string("C0 * x + 10", tokenizer)
assert formula.eval(x=1.0, C0=2.0) == pytest.approx(12.0)

    formula = Formula.from_string("sin(x) + 2 * cos(x)", tokenizer)
    assert formula.eval(x=0.7) == pytest.approx(2.1739021)

    formula = Formula.from_string("log(x) + 3", tokenizer)
assert formula.eval(x=2.71828182846) == pytest.approx(4.0)
```
:::

This works, but if one of the assertions fails, the following assertions
within the test function will not be executed. If there are multiple
failures, we will have to run the test multiple times to see all of them
and eventually fix all the issues.

To see multiple failures per test run, we might decide to explicitly
write a separate test for each assertion:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_formula_linear():
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string("C0 * x + 10", tokenizer)
    assert formula.eval(x=1.0, C0=2.0) == pytest.approx(12.0)

def test_formula_sin_cos():
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string("sin(x) + 2 * cos(x)", tokenizer)
    assert formula.eval(x=0.7) == pytest.approx(2.1739021)

def test_formula_log():
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string("log(x) + 3", tokenizer)
    assert formula.eval(x=2.71828182846) == pytest.approx(4.0)
```
:::

But now we are duplicating code all over the place, which will make
maintenance more difficult. Suppose in the future
`FormulaTokenizer`{.literal} is updated to explicitly receive a list of
functions that can be used in formulas. This means that we would have to
update the creation of `FormulaTokenzier`{.literal} in several places. 

To avoid repeating ourselves, we might decide to write this instead:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_formula_parsing2():
values = [
        ("C0 * x + 10", dict(x=1.0, C0=2.0), 12.0),
        ("sin(x) + 2 * cos(x)", dict(x=0.7), 2.1739021),
        ("log(x) + 3", dict(x=2.71828182846), 4.0),
    ]
    tokenizer = FormulaTokenizer()
for formula, inputs, result in values:
        formula = Formula.from_string(formula, tokenizer)
        assert formula.eval(**inputs) == pytest.approx(result)
```
:::

This solves the problem of duplicating code, but now we are back to the
initial problem of seeing only one failure at a time.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec31}Enter \@pytest.mark.parametrize {#enter-pytest.mark.parametrize .title}

</div>

</div>
:::

To solve all the above problems, pytest provides the much-loved
`@pytest.mark.parametrize`{.literal} mark. With this mark, you are able
to provide a list of input values[]{#id325092153 .indexterm} to the
test, and pytest automatically generates multiple test functions for
each input value.

The following shows this in action:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
        ("C0 * x + 10", dict(x=1.0, C0=2.0), 12.0),
        ("sin(x) + 2 * cos(x)", dict(x=0.7), 2.1739021),
        ("log(x) + 3", dict(x=2.71828182846), 4.0),
    ],
)
def test_formula_parsing(formula, inputs, result):
    tokenizer = FormulaTokenizer()
    formula = Formula.from_string(formula, tokenizer)
    assert formula.eval(**inputs) == pytest.approx(result)
```
:::

The `@pytest.mark.parametrize`{.literal} mark automatically generates
multiple test functions, parametrizing them with the arguments given to
the mark. The call receives two parameters:

::: {.itemizedlist}
-   `argnames`{.literal}: a comma-separated string of argument names
    that will be passed to the test function.
-   `argvalues`{.literal}: a sequence of tuples, with each tuple
    generating a new test invocation. Each item in the tuple corresponds
    to an argument name, so the first tuple
    `("C0 * x + 10", dict(x=1.0, C0=2.0), 12.0)`{.literal} will generate
    a call to the test function with the arguments:
    ::: {.itemizedlist}
    -   `formula`{.literal} = `"C0 * x + 10"`{.literal}
    -   `inputs`{.literal} = `dict(x=1.0, C0=2.0)`{.literal}
    -   `expected`{.literal} = `12.0`{.literal}
    :::
:::

Using this mark, pytest will run `test_formula_parsing`{.literal} three
times, passing one set of arguments given by the `argvalues`{.literal}
parameter each time. It will also automatically generate a different
node ID for each test, making it easy to distinguish between them:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
======================== test session starts ========================
...
collected 8 items / 5 deselected

test_formula.py::test_formula[C0 * x + 10-inputs0-12.0]
test_formula.py::test_formula[sin(x) + 2 * cos(x)-inputs1-2.1739021]
test_formula.py::test_formula[log(x) + 3-inputs2-4.0] 
============== 3 passed, 5 deselected in 0.05 seconds ===============
```
:::

It is also important to note that the body of the function is just as
compact as our starting test at the beginning of this section, but now
we have multiple tests which allows us to see multiple failures when
they happen.

Test parametrization not only avoids repeating test code and makes
maintenance easier, it also invites you and the next developer who comes
along to add more input values as the code matures. It encourages
developers to cover more cases, because people are more eager to add a
single line to the `argvalues`{.literal} of a parametrized test than to
copy and paste an entire new test to cover another input value.

In summary, `@pytest.mark.parametrize`{.literal} will make you cover
more input cases, with very little overhead. It is definitely a very
useful feature and should be used whenever you have multiple input
values to be tested in the same way.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec32}Applying marks to value sets {#applying-marks-to-value-sets .title}

</div>

</div>
:::

Often, in parametrized tests, you find the need[]{#id325023565
.indexterm} to apply one or more marks to a set of parameters as you
would to a normal test function. For example, you want to apply a
`timeout`{.literal} mark to one set of parameters because it takes too
long to run, or `xfail`{.literal} a set of parameters, because it has
not been implemented yet.

In those cases, use `pytest.param`{.literal} to wrap the set of values
and apply the marks you want:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
        ...
        ("log(x) + 3", dict(x=2.71828182846), 4.0),
pytest.param(
            "hypot(x, y)", dict(x=3, y=4), 5.0,
            marks=pytest.mark.xfail(reason="not implemented: #102"),
        ),
    ],
)
```
:::

The signature of `pytest.param`{.literal} is this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
pytest.param(*values, **kw)
```
:::

Where:

::: {.itemizedlist}
-   `*values`{.literal} is the parameter
    set: `"hypot(x, y)", dict(x=3, y=4), 5.0`{.literal}.
-   `**kw`{.literal} are options as keyword
    arguments: `marks=pytest.mark.xfail(reason="not implemented: #102")`{.literal}.
    It accepts a single mark or a sequence of marks. There is another
    option, `ids`{.literal}, which will be shown in the next section.
:::

Behind the scenes, every tuple of parameters passed to
`@pytest.mark.parametrize`{.literal} is converted to a
`pytest.param`{.literal} without extra options, so, for example,in the
following the first code snippet is equivalent to the second code
snippet:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
        ("C0 * x + 10", dict(x=1.0, C0=2.0), 12.0),
        ("sin(x) + 2 * cos(x)", dict(x=0.7), 2.1739021),
    ]
)
```
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
pytest.param("C0 * x + 10", dict(x=1.0, C0=2.0), 12.0),
pytest.param("sin(x) + 2 * cos(x)", dict(x=0.7), 2.1739021),
    ]
)
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec33}Customizing test IDs {#customizing-test-ids .title}

</div>

</div>
:::

Consider this example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
        ("x + 3", dict(x=1.0), 4.0,),
        ("x - 1", dict(x=6.0), 5.0,),
    ],
)
def test_formula_simple(formula, inputs, result):
    ...
```
:::

As we have seen, pytest automatically creates[]{#id325023069 .indexterm}
custom test IDs, based on the parameters used in a parametrized call.
Running `pytest -v`{.literal} will generate these test IDs:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
======================== test session starts ========================
...
tests/test_formula.py::test_formula_simple[x + 3-inputs0-4.0]
tests/test_formula.py::test_formula_simple[x - 1-inputs1-5.0]
```
:::

If you don\'t like the automatically generated IDs, you can use
`pytest.param`{.literal} and the `id`{.literal} option to customize it:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "formula, inputs, result",
    [
pytest.param("x + 3", dict(x=1.0), 4.0, id='add'),
pytest.param("x - 1", dict(x=6.0), 5.0, id='sub'),
    ],
)
def test_formula_simple(formula, inputs, result):
    ...
```
:::

This produces the following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
======================== test session starts ========================
...
tests/test_formula.py::test_formula_simple[add]
tests/test_formula.py::test_formula_simple[sub]
```
:::

This is also useful because it makes selecting tests significantly
easier when using the `-k `{.literal}flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -k "x + 3-inputs0-4.0"
```
:::

 

versus:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -k "add"
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch03lvl2sec34}Testing multiple implementations {#testing-multiple-implementations .title}

</div>

</div>
:::

Well-designed systems usually make[]{#id325092272 .indexterm} use of
abstractions provided by interfaces, instead of being tied to specific
implementations. This makes a system more resilient to future changes
because, to extend it, you need to implement a new extension that
complies with the expected interface, and integrate it into the system.

One challenge that often comes up is how to make sure existing
implementations comply with all the details of a specific interface.

For example, suppose our system needs to be able to serialize some
internal classes into a text format to save and load to disk. The
following are some of the internal classes in our system:

::: {.itemizedlist}
-   `Quantity`{.literal}: represents a value and a unit of measure. For
    example `Quantity(10, "m")`{.literal} means [*10
    meters*]{.emphasis}. `Quantity`{.literal} objects have addition,
    subtraction, and multiplication---basically, all the operators that
    you would expect from a native `float`{.literal}, but taking the
    unit of measure into account.
-   `Pipe`{.literal}: represents a duct where some fluid can flow
    through. It has a `length`{.literal} and `diameter`{.literal}, both
    `Quantity`{.literal} instances.
:::

Initially, in our development, we only need to save those objects in
[**JSON**]{.strong} format, so we go ahead and implement a
straightforward serializer class, that is able to serialize and
de-serialize our classes:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class JSONSerializer:

    def serialize_quantity(self, quantity: Quantity) -> str:
        ...

    def deserialize_quantity(self, data: str) -> Quantity:
        ...

    def serialize_pipe(self, pipe: Pipe) -> str:
        ...

    def deserialize_pipe(self, data: str) -> Pipe:
        ...
```
:::

Now we should write some tests to ensure everything is working:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test:

    def test_quantity(self):
        serializer = JSONSerializer()
        quantity = Quantity(10, "m")
        data = serializer.serialize(quantity)
        new_quantity = serializer.deserialize(data)
        assert new_quantity == quantity

    def test_pipe(self):
        serializer = JSONSerializer()
        pipe = Pipe(
            length=Quantity(1000, "m"), diameter=Quantity(35, "cm")
        )
        data = serializer.serialize(pipe)
        new_pipe = serializer.deserialize(data)
        assert new_pipe == pipe
```
:::

This works well and is a perfectly valid approach, given our
requirements.

Some time passes, and new requirements arrive: now we need to serialize
our objects into other formats, namely `XML`{.literal} and
[`YAML`{.literal}(](http://yaml.org/){.ulink}<http://yaml.org/>[)](http://yaml.org/){.ulink}.
To keep things simple, we create two new classes,
`XMLSerializer`{.literal} and `YAMLSerializer`{.literal}, which
implement the same `serialize`{.literal}/`deserialize`{.literal}
methods. Because they comply with the same interface as
`JSONSerializer`{.literal}, we can use the new classes interchangeably
in our system, which is great.

But how do we test the different implementations?

A naive approach would be to loop over the different implementations
inside each test:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class Test:

    def test_quantity(self):
for serializer in [
            JSONSerializer(), XMLSerializer(), YAMLSerializer()
        ]:
            quantity = Quantity(10, "m")
            data = serializer.serialize(quantity)
            new_quantity = serializer.deserialize(data)
            assert new_quantity == quantity

    def test_pipe(self):
for serializer in [
            JSONSerializer(), XMLSerializer(), YAMLSerializer()
        ]:
            pipe = Pipe(
                length=Quantity(1000, "m"),
                diameter=Quantity(35, "cm"),
            )
            data = serializer.serialize(pipe)
            new_pipe = serializer.deserialize(data)
            assert new_pipe == pipe
```
:::

This works, but it is not ideal, because we have to copy and paste the
loop definition in each test, making it harder to maintain. Also, if one
of the serializers fails, the next ones in the list will never be
executed.

Another, horrible approach, would be to copy and paste the entire test
functions, replacing the serializer class each time, but we won\'t be
showing that here.

A much better solution is to use `@pytest.mark.parametrize`{.literal} at
class level. Observe:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.parametrize(
    "serializer_class",
    [JSONSerializer, XMLSerializer, YAMLSerializer],
)
class Test:

    def test_quantity(self, serializer_class):
        serializer = serializer_class()
        quantity = Quantity(10, "m")
        data = serializer.serialize(quantity)
        new_quantity = serializer.deserialize(data)
        assert new_quantity == quantity

    def test_pipe(self, serializer_class):
        serializer = serializer_class()
        pipe = Pipe(
            length=Quantity(1000, "m"), diameter=Quantity(35, "cm")
        )
        data = serializer.serialize(pipe)
        new_pipe = serializer.deserialize(data)
        assert new_pipe == pipe
```
:::

With a small change, we have[]{#id325022682 .indexterm} multiplied our
existing tests to cover all the new implementations:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
test_parametrization.py::Test::test_quantity[JSONSerializer] PASSED
test_parametrization.py::Test::test_quantity[XMLSerializer] PASSED
test_parametrization.py::Test::test_quantity[YAMLSerializer] PASSED
test_parametrization.py::Test::test_pipe[JSONSerializer] PASSED
test_parametrization.py::Test::test_pipe[XMLSerializer] PASSED
test_parametrization.py::Test::test_pipe[YAMLSerializer] PASSED
```
:::

The `@pytest.mark.parametrize`{.literal} decorator also makes it very
clear that new implementations should be added to the list and that all
existing tests must pass. New tests added to the class also need to pass
for all implementations.

In summary, `@pytest.mark.parametrize`{.literal} can be a very powerful
tool to ensure that different implementations comply with the
specifications of an interface. 



[]{#ch03lvl1sec23}Summary {#summary .title style="clear: both"}
-------------------------

</div>

</div>

------------------------------------------------------------------------
:::

In this chapter, we learned how to use marks to organize our code and
help us run the test suite in flexible ways. We then looked at how to
use the `@pytest.mark.skipif`{.literal} to conditionally skip tests, and
how to use the `@pytest.mark.xfail`{.literal} mark to deal with expected
failures and flaky tests. Then we discussed ways of dealing with flaky
tests in a collaborative environment. Finally, we discussed the benefits
of using `@pytest.mark.parametrize`{.literal} to avoid repeating our
testing code and to make it easy for ourselves and others to add new
input cases to existing tests.

In the next chapter, we will finally get to one of pytest\'s most loved
and powerful features: [**fixtures**]{.strong}.
