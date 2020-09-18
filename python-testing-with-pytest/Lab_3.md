<img align="right" src="../logo.png">


### pytest Fixtures

Now that you’ve seen the basics of pytest, let’s turn our attention to fixtures,
which are essential to structuring test code for almost any non-trivial software
system. Fixtures are functions that are run by pytest before (and sometimes
after) the actual test functions. The code in the fixture can do whatever you
want it to. You can use fixtures to get a data set for the tests to work on. You
can use fixtures to get a system into a known state before running a test.
Fixtures are also used to get data ready for multiple tests.

Here’s a simple fixture that returns a number:

```
**ch3/test_fixtures.py
import pytest

@pytest.fixture()
def some_data ():
_"""Returnanswerto ultimatequestion."""_
    return 42

def test_some_data (some_data):
_"""Usefixturereturnvaluein a test."""_
**assert** some_data== 42
```

The @pytest.fixture() decorator is used to tell pytest that a function is a fixture.
When you include the fixture name in the parameter list of a test function,
pytest knows to run it before running the test. Fixtures can do work, and can
also return data to the test function.

The test test_some_data() has the name of the fixture, some_data, as a parameter.
pytest will see this and look for a fixture with this name. Naming is significant
in pytest. pytest will look in the module of the test for a fixture of that name.
It will also look in conftest.py files if it doesn’t find it in this file.


Before we start our exploration of fixtures (and the conftest.py file), I need to
address the fact that the term _fixture_ has many meanings in the programming
and test community, and even in the Python community. I use “fixture,”
“fixture function,” and “fixture method” interchangeably to refer to the
@pytest.fixture() decorated functions discussed in this lab. _Fixture_ can also
be used to refer to the resource that is being set up by the fixture functions.
Fixture functions often set up or retrieve some data that the test can work
with. Sometimes this data is considered a fixture. For example, the Django
community often uses _fixture_ to mean some initial data that gets loaded into
a database at the start of an application.

Regardless of other meanings, in pytest and in this book, test fixtures refer
to the mechanism pytest provides to allow the separation of “getting ready
for” and “cleaning up after” code from your test functions.

pytest fixtures are one of the unique core features that make pytest stand
out above other test frameworks, and are the reason why many people switch
to and stay with pytest. However, fixtures in pytest are different than fixtures
in Django and different than the setup and teardown procedures found in
unittest and nose. There are a lot of features and nuances about fixtures.
Once you get a good mental model of how they work, they will seem easy to
you. However, you have to play with them a while to get there, so let’s get
started.

### Sharing Fixtures Through conftest.py

You can put fixtures into individual test files, but to share fixtures among
multiple test files, you need to use a conftest.py file somewhere centrally located
for all of the tests. For the Tasks project, all of the fixtures will be in
tasks_proj/tests/conftest.py.

From there, the fixtures can be shared by any test. You can put fixtures in
individual test files if you want the fixture to only be used by tests in that
file. Likewise, you can have other conftest.py files in subdirectories of the top
tests directory. If you do, fixtures defined in these lower-level conftest.py files
will be available to tests in that directory and subdirectories. So far, however,
the fixtures in the Tasks project are intended to be available to any test.
Therefore, putting all of our fixtures in the conftest.py file at the test root,
tasks_proj/tests, makes the most sense.

Although conftest.py is a Python module, it should not be imported by test files.
Don’t importconftest from anywhere. The conftest.py file gets read by pytest, and
is considered a local _plugin_ , which will make sense once we start talking about
plugins in Lab 5, Plugins, on page 97. For now, think of tests/conftest.py as
a place where we can put fixtures used by all tests under the tests directory.
Next, let’s rework some of our tests for tasks_proj to properly use fixtures.

### Using Fixtures for Setup and Teardown

Most of the tests in the Tasks project will assume that the Tasks database is
already set up and running and ready. And we should clean things up at the
end if there is any cleanup needed. And maybe also disconnect from the
database. Luckily, most of this is taken care of within the tasks code with
tasks.start_tasks_db(<directoryto store db>, 'tiny' or 'mongo') and tasks.stop_tasks_db(); we
just need to call them at the right time, and we need a temporary directory.

Fortunately, pytest includes a cool fixture called tmpdir that we can use for
testing and don’t have to worry about cleaning up. It’s not magic, just good
coding by the pytest folks. (Don’t worry; we look at tmpdir and it’s session-
scoped relative tmpdir_factory in more depth in Using tmpdir and tmpdir_factory,
on page 73.)

Given those pieces, this fixture works nicely:

```
**ch3/a/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture()
def tasks_db (tmpdir):
_"""Connectto db beforetests,disconnectafter."""
# Setup: startdb_
tasks.start_tasks_db(str(tmpdir), _'tiny'_ )
**yield** _# thisis wherethe testinghappens_

# Teardown: stopdb
tasks.stop_tasks_db()
```

The value of tmpdir isn’t a string—it’s an object that represents a directory.
However, it implements __str__, so we can use str() to get a string to pass to
start_tasks_db(). We’re still using 'tiny' for TinyDB, for now.

A fixture function runs before the tests that use it. However, if there is a yield
in the function, it stops there, passes control to the tests, and picks up on
next line after the tests are done. Therefore, think of the code above the
yield as “setup” and the code after yield as “teardown.” The code after the yield,
the “teardown,” is guaranteed to run regardless of what happens during the
tests. We’re not returning any data with the yield in this fixture. But you can.


Let’s change one of our tasks.add() tests to use this fixture:

```
**ch3/a/tasks_proj/tests/func/test_add.py
importpytest
importtasks
fromtasksimport** Task

def test_add_returns_valid_id (tasks_db):
_"""tasks.add(<validtask>)shouldreturnan integer."""
# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint_
new_task= Task( _'do something'_ )
task_id= tasks.add(new_task)
**assert** isinstance(task_id,int)
```

The main change here is that the extra fixture in the file has been removed,
and we’ve added tasks_db to the parameter list of the test. I like to structure
tests in a GIVEN/WHEN/THEN format using comments, especially when it
isn’t obvious from the code what’s going on. I think it’s helpful in this case.
Hopefully, GIVENan initializedtasks db helps to clarify why tasks_db is used as a fix-
ture for the test.

```
Make Sure Tasks Is Installed
We’re still writing tests to be run against the Tasks project in this
lab, which was first installed in Lab 2. If you skipped that
lab, be sure to install tasks with cd code; pip install ./tasks_proj/.
```
### Tracing Fixture Execution with –setup-show

If you run the test from the last section, you don’t get to see what fixtures
are run:

```
$ cd /path/to/code/
$ pip install ./tasks_proj/** _# if not installedyet_
$ cd /path/to/code/ch3/a/tasks_proj/tests/func
$ pytest-v test_add.py-k valid_id**
===================testsessionstarts===================
collected3 items/ 2 deselected

test_add.py::test_add_returns_valid_idPASSED [100%]

=========1 passed,2 deselectedin 0.04seconds==========
```

When I’m developing fixtures, I like to see what’s running and when. Fortu-
nately, pytest provides a command-line flag, --setup-show, that does just that:


```
$ pytest--setup-showtest_add.py-k valid_id**
===================testsessionstarts===================
collected3 items/ 2 deselected

test_add.py
SETUP S tmpdir_factory
SETUP F tmpdir(fixturesused:tmpdir_factory)
SETUP F tasks_db(fixturesused:tmpdir)
func/test_add.py::test_add_returns_valid_id
(fixturesused:tasks_db,tmpdir,tmpdir_factory).
TEARDOWNF tasks_db
TEARDOWNF tmpdir
TEARDOWNS tmpdir_factory

=========1 passed,2 deselectedin 0.03seconds==========
```

Our test is in the middle, and pytest designates a SETUP and TEARDOWN portion
to each fixture. Going from test_add_returns_valid_id up, you see that tmpdir ran
before the test. And before that, tmpdir_factory. Apparently, tmpdir uses it as a
fixture.

The F and S in front of the fixture names indicate scope. F for function scope,
and S for session scope. I’ll talk about scope in Specifying Fixture Scope, on
page 58.

### Using Fixtures for Test Data

Fixtures are a great place to store data to use for testing. You can return
anything. Here’s a fixture returning a tuple of mixed type:

```
**ch3/test_fixtures.py
@pytest.fixture()
def a_tuple ():
_"""Returnsomethingmoreinteresting."""_
**return (1, _'foo'_ , None,{ _'bar'_ : 23})

def test_a_tuple (a_tuple):
_"""Demothe a_tuplefixture."""_
**assert** a_tuple[3][ _'bar'_ ] == 32
```

Since test_a_tuple() should fail (23 != 32), we can see what happens when a test
with a fixture fails:

```
$ cd /path/to/code/ch3
$ pytesttest_fixtures.py::test_a_tuple**
===================testsessionstarts===================
collected1 item

test_fixtures.pyF [100%]

========================FAILURES=========================
______________________test_a_tuple_______________________
```


```
a_tuple= (1, 'foo',None,{'bar':23})

def test_a_tuple(a_tuple):
"""Demothe a_tuplefixture."""
**> asserta_tuple[3][** _'bar'_ **] == 32**
E assert23 == 32

test_fixtures.py:43:AssertionError
================1 failedin 0.07seconds=================
```

Along with the stack trace section, pytest reports the value parameters of the
function that raised the exception or failed an assert. In the case of tests, the
fixtures are parameters to the test, and are therefore reported with the stack
trace.

What happens if the assert (or any exception) happens in the fixture?


```
$ pytest-v test_fixtures.py::test_other_data**
===================testsessionstarts===================
collected1 item

test_fixtures.py::test_other_dataERROR [100%]

=========================ERRORS==========================
____________ERRORat setupof test_other_data____________

@pytest.fixture()
def some_other_data():
"""Raisean exceptionfromfixture."""
x = 43
**> assertx == 42**
E assert43 == 42

test_fixtures.py:24:AssertionError
=================1 errorin 0.06seconds=================
```

A couple of things happen. The stack trace shows correctly that the assert
happened in the fixture function. Also, test_other_data is reported not as FAIL,
but as ERROR. This distinction is great. If a test ever fails, you know the failure
happened in the test proper, and not in any fixture it depends on.

But what about the Tasks project? For the Tasks project, we could probably
use some data fixtures, perhaps different lists of tasks with various properties:

```
**ch3/a/tasks_proj/tests/conftest.py
_# Reminderof Taskconstructorinterface
# Task(summary=None,owner=None,done=False,id=None)
# summaryis required
# ownerand doneare optional
# id is set by database_

@pytest.fixture()
def tasks_just_a_few ():
```


```
"""Allsummariesand ownersare unique."""
return (
Task( 'Writesomecode' , 'Brian' , True),
Task( "CodereviewBrian'scode" , 'Katie' , False),
Task( 'FixwhatBriandid' , 'Michelle' , False))

@pytest.fixture()
def tasks_mult_per_owner ():
_"""Severalownerswithseveraltaskseach."""_
**return (
Task( _'Makea cookie'_ , _'Raphael'_ ),
Task( _'Usean emoji'_ , _'Raphael'_ ),
Task( _'Moveto Berlin'_ , _'Raphael'_ ),
Task( _'Create'_ , _'Michelle'_ ),
Task( _'Inspire'_ , _'Michelle'_ ),
Task( _'Encourage'_ , _'Michelle'_ ),
Task( _'Do a handstand'_ , _'Daniel'_ ),
Task( _'Writesomebooks'_ , _'Daniel'_ ),
Task( _'Eatice cream'_ , _'Daniel'_ ))
```

You can use these directly from tests, or you can use them from other fixtures.
Let’s use them to build up some non-empty databases to use for testing.

### Using Multiple Fixtures

You’ve already seen that tmpdir uses tmpdir_factory. And you used tmpdir in our
tasks_db fixture. Let’s keep the chain going and add some specialized fixtures
for non-empty tasks databases:

```
**ch3/a/tasks_proj/tests/conftest.py
@pytest.fixture()
def db_with_3_tasks (tasks_db,tasks_just_a_few):
_"""Connecteddb with3 tasks,all unique."""_
**for** t **in** tasks_just_a_few:
tasks.add(t)

@pytest.fixture()
def db_with_multi_per_owner (tasks_db,tasks_mult_per_owner):
_"""Connecteddb with9 tasks,3 owners,all with3 tasks."""_
**for** t **in** tasks_mult_per_owner:
tasks.add(t)
```

These fixtures all include two fixtures each in their parameter list: tasks_db
and a data set. The data set is used to add tasks to the database. Now tests
can use these when you want the test to start from a non-empty database,
like this:

```
**ch3/a/tasks_proj/tests/func/test_add.py
def test_add_increases_count (db_with_3_tasks):
```


```
"""Testtasks.add()affecton tasks.count()."""
# GIVENa db with3 tasks
# WHENanothertaskis added
tasks.add(Task( 'throwa party' ))
# THENthe countincreasesby 1
assert tasks.count()== 4
```

This also demonstrates one of the great reasons to use fixtures: to focus the
test on what you’re actually testing, not on what you had to do to get ready
for the test. I like using comments for GIVEN/WHEN/THEN and trying to
push as much GIVEN into fixtures for two reasons. First, it makes the test
more readable and, therefore, more maintainable. Second, an assert or exception
in the fixture results in an ERROR, while an assert or exception in a test
function results in a FAIL. I don’t want test_add_increases_count() to FAIL if
database initialization failed. That would just be confusing. I want a FAIL for
test_add_increases_count() to only be possible if add() really failed to alter the count.

Let’s trace it and see all the fixtures run:

```
$ cd /path/to/code/ch3/a/tasks_proj/tests/func
$ pytest--setup-showtest_add.py::test_add_increases_count**
===================testsessionstarts===================
collected1 item

test_add.py
SETUP S tmpdir_factory
SETUP F tmpdir(fixturesused:tmpdir_factory)
SETUP F tasks_db(fixturesused:tmpdir)
SETUP F tasks_just_a_few
SETUP F db_with_3_tasks (fixturesused:tasks_db,tasks_just_a_few)
test_add.py::test_add_increases_count(fixturesused:db_with_3_tasks,
tasks_db,tasks_just_a_few,tmpdir,tmpdir_factory).
TEARDOWNF db_with_3_tasks
TEARDOWNF tasks_just_a_few
TEARDOWNF tasks_db
TEARDOWNF tmpdir
TEARDOWNS tmpdir_factory

================1 passedin 0.05seconds=================
```

There are those F’s and S’s for function and session scope again. Let’s learn
about those next.

### Specifying Fixture Scope

Fixtures include an optional parameter called scope, which controls how often
a fixture gets set up and torn down. The scope parameter to @pytest.fixture() can
have the values of function, class, module, or session. The default scope is function.


The tasks_db fixture and all of the fixtures so far don’t specify a scope. Therefore,
they are function scope fixtures.

Here’s a rundown of each scope value:

```
_scope='function'_

```

Run once per test function. The setup portion is run before each test using
the fixture. The teardown portion is run after each test using the fixture.
This is the default scope used when no scope parameter is specified.

```
_scope='class'_
```

Run once per test class, regardless of how many test methods are in the class.

```
_scope='module'_
```

Run once per module, regardless of how many test functions or methods
or other fixtures in the module use it.

```
_scope='session'_
```

Run once per session. All test methods and functions using a fixture of
session scope share one setup and teardown call.

Here’s how the scope values look in action:

```
**ch3/test_scope.py
_"""Demofixturescope."""_

import pytest

@pytest.fixture(scope= _'function'_ )
def func_scope ():
_"""Afunctionscopefixture."""_

@pytest.fixture(scope= _'module'_ )
def mod_scope ():
_"""Amodulescopefixture."""_

@pytest.fixture(scope= _'session'_ )
def sess_scope ():
_"""Asessionscopefixture."""_

@pytest.fixture(scope= _'class'_ )
def class_scope ():
_"""Aclassscopefixture."""_

def test_1 (sess_scope,mod_scope,func_scope):
_"""Testusingsession,module,and functionscopefixtures."""_

def test_2 (sess_scope,mod_scope,func_scope):
_"""Demois morefun withmultiple tests."""_

@pytest.mark.usefixtures( _'class_scope'_ )
```

```
**class** TestSomething():
_"""Democlassscopefixtures."""_

def test_3 (self):
"""Testusinga classscopefixture."""

def test_4 (self):
"""Again,multipletestsare morefun."""
```

Let’s use --setup-show to demonstrate that the number of times a fixture is called
and when the setup and teardown are run depend on the scope:

```
$ cd /path/to/code/ch3
$ pytest--setup-showtest_scope.py
===================testsessionstarts===================
collected4 items

test_scope.py
SETUP S sess_scope
SETUP M mod_scope
SETUP F func_scope
test_scope.py::test_1(fixturesused:func_scope,mod_scope,sess_scope).
TEARDOWNF func_scope
SETUP F func_scope
test_scope.py::test_2(fixturesused:func_scope,mod_scope,sess_scope).
TEARDOWNF func_scope
SETUP C class_scope
test_scope.py::TestSomething::()::test_3(fixturesused:class_scope).
test_scope.py::TestSomething::()::test_4(fixturesused:class_scope).
TEARDOWNC class_scope
TEARDOWNM mod_scope
TEARDOWNS sess_scope

================4 passedin 0.02seconds=================
``` 

Now you get to see not just F and S for function and session, but also C and
M for class and module.

Scope is defined with the fixture. I know this is obvious from the code, but
it’s an important point to make sure you fully grok. The scope is set at the
definition of a fixture, and not at the place where it’s called. The test functions
that use a fixture don’t control how often a fixture is set up and torn down.

Fixtures can only depend on other fixtures of their same scope or wider. So
a function scope fixture can depend on other function scope fixtures (the
default, and used in the Tasks project so far). A function scope fixture can
also depend on class, module, and session scope fixtures, but you can’t go
in the reverse order.


**Changing Scope for Tasks Project Fixtures**

With this knowledge of scope, let’s now change the scope of some of the Task
project fixtures.

So far, we haven’t had a problem with test times. But it seems like a waste
to set up a temporary directory and new connection to a database for every
test. As long as we can ensure an empty database when needed, that should
be sufficient.

To have something like tasks_db be session scope, you need to use tmpdir_factory,
since tmpdir is function scope and tmpdir_factory is session scope. Luckily, this
is just a one-line code change (well, two if you count tmpdir -> tmpdir_factory in
the parameter list):

```
**ch3/b/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture(scope= _'session'_ )
def tasks_db_session (tmpdir_factory):
_"""Connectto db beforetests,disconnectafter."""_
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )
**yield**
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
_"""Anemptytasksdb."""_
tasks.delete_all()
```

Here we changed tasks_db to depend on tasks_db_session, and we deleted all the
entries to make sure it’s empty. Because we didn’t change its name, none of
the fixtures or tests that already include it have to change.

The data fixtures just return a value, so there really is no reason to have them
run all the time. Once per session is sufficient:

```
**ch3/b/tasks_proj/tests/conftest.py
_# Reminderof Taskconstructorinterface
# Task(summary=None,owner=None,done=False,id=None)
# summaryis required
# ownerand doneare optional
# id is set by database_
```

```
@pytest.fixture(scope= _'session'_ )
def tasks_just_a_few ():
_"""Allsummariesand ownersare unique."""_
**return (
Task( _'Writesomecode'_ , _'Brian'_ , True),
Task( _"CodereviewBrian'scode"_ , _'Katie'_ , False),
Task( _'FixwhatBriandid'_ , _'Michelle'_ , False))

@pytest.fixture(scope= _'session'_ )
def tasks_mult_per_owner ():
_"""Severalownerswithseveraltaskseach."""_
**return (
Task( _'Makea cookie'_ , _'Raphael'_ ),
Task( _'Usean emoji'_ , _'Raphael'_ ),
Task( _'Moveto Berlin'_ , _'Raphael'_ ),
Task( _'Create'_ , _'Michelle'_ ),
Task( _'Inspire'_ , _'Michelle'_ ),
Task( _'Encourage'_ , _'Michelle'_ ),
Task( _'Do a handstand'_ , _'Daniel'_ ),
Task( _'Writesomebooks'_ , _'Daniel'_ ),
Task( _'Eatice cream'_ , _'Daniel'_ ))
```

Now, let’s see if all of these changes work with our tests:

```
$ cd /path/to/code/ch3/b/tasks_proj
$ pytest
===================testsessionstarts===================
collected55 items

tests/func/test_add.py... [ 5%]
tests/func/test_add_variety.py....................[ 41%]
........ [ 56%]
tests/func/test_add_variety2.py............ [ 78%]
tests/func/test_api_exceptions.py....... [ 90%]
tests/func/test_unique_id.py. [ 92%]
tests/unit/test_task.py.... [100%]

================55 passedin 0.33seconds================
```

Looks like it’s all good. Let’s trace the fixtures for one test file to see if the
different scoping worked as expected:

```
$ pytest--setup-showtests/func/test_add.py
===================testsessionstarts===================
collected3 items

tests/func/test_add.py
SETUP S tmpdir_factory
SETUP S tasks_db_session(fixturesused:tmpdir_factory)
SETUP F tasks_db(fixturesused:tasks_db_session)
func/test_add.py::test_add_returns_valid_id(fixturesused:tasks_db,
tasks_db_session,tmpdir_factory).
```

```
TEARDOWNF tasks_db
SETUP F tasks_db(fixturesused:tasks_db_session)
func/test_add.py::test_added_task_has_id_set(fixturesused:tasks_db,
tasks_db_session,tmpdir_factory).
TEARDOWNF tasks_db
SETUP S tasks_just_a_few
SETUP F tasks_db(fixturesused:tasks_db_session)
SETUP F db_with_3_tasks (fixturesused:tasks_db,tasks_just_a_few)
func/test_add.py::test_add_increases_count(fixturesused:db_with_3_tasks,
tasks_db,tasks_db_session,tasks_just_a_few,tmpdir_factory).
TEARDOWNF db_with_3_tasks
TEARDOWNF tasks_db
TEARDOWNS tasks_db_session
TEARDOWNS tmpdir_factory
TEARDOWNS tasks_just_a_few

================3 passedin 0.04seconds=================
```

Yep. Looks right. tasks_db_session is called once per session, and the quicker
tasks_db now just cleans out the database before each test.

### Specifying Fixtures with usefixtures

So far, if you wanted a test to use a fixture, you put it in the parameter list.
You can also mark a test or a class with @pytest.mark.usefixtures('fixture1', 'fixture2').
usefixtures takes a comma separated list of strings representing fixture names.
It doesn’t make sense to do this with test functions—it’s just more typing.
But it does work well for test classes:

```
**ch3/test_scope.py
@pytest.mark.usefixtures( _'class_scope'_ )
**class** TestSomething():
_"""Democlassscopefixtures."""_

def test_3 (self):
"""Testusinga classscopefixture."""
def test_4 (self):
"""Again,multipletestsare morefun."""
```

Using usefixtures is almost the same as specifying the fixture name in the test
method parameter list. The one difference is that the test can use the return
value of a fixture only if it’s specified in the parameter list. A test using a fix-
ture due to usefixtures cannot use the fixture’s return value.

### Using autouse for Fixtures That Always Get Used

So far in this lab, all of the fixtures used by tests were named by the
tests (or used usefixtures for that one class example). However, you can use
autouse=True to get a fixture to run all of the time. This works well for code you


want to run at certain times, but tests don’t really depend on any system
state or data from the fixture. Here’s a rather contrived example:

```
**ch3/test_autouse.py
_"""Demonstrateautousefixtures."""_

import pytest
importtime**

@pytest.fixture(autouse=True,scope= _'session'_ )
def footer_session_scope ():
_"""Reportthe timeat the end of a session."""_
**yield**
now = time.time()
**print ( _'--'_ )
**print ( _'finished: {}'_ .format(time.strftime( _'%d %b %X'_ , time.localtime(now))))
**print ( _'-----------------'_ )

@pytest.fixture(autouse=True)
def footer_function_scope ():
_"""Reporttestdurationsafter eachfunction."""_
start= time.time()
**yield**
stop= time.time()
delta= stop- start
**print ( _'\ntestduration: {:0.3}seconds'_ .format(delta))

def test_1 ():
_"""Simulatelong-ishrunningtest."""_
time.sleep(1)

def test_2 ():
_"""Simulateslightlylongertest."""_
time.sleep(1.23)
```

We want to add test times after each test, and the date and current time at
the end of the session. Here’s what these look like:

```
$ cd /path/to/code/ch3
$ pytest-v -s test_autouse.py
=====================testsessionstarts======================
collected2 items

test_autouse.py::test_1PASSED
testduration: 1.0 seconds

test_autouse.py::test_2PASSED
testduration: 1.24seconds
--
finished: 25 Jul 16:18:27
-----------------
===================2 passedin 2.25seconds===================
```

The autouse feature is good to have around. But it’s more of an exception than
a rule. Opt for named fixtures unless you have a really great reason not to.

Now that you’ve seen autouse in action, you may be wondering why we didn’t
use it for tasks_db in this lab. In the Tasks project, I felt it was important
to keep the ability to test what happens if we try to use an API function before
db initialization. It should raise an appropriate exception. But we can’t test
this if we force good initialization on every test.

### Renaming Fixtures

The name of a fixture, listed in the parameter list of tests and other fixtures
using it, is usually the same as the function name of the fixture. However,
pytest allows you to rename fixtures with a name parameter to @pytest.fixture():

```
**ch3/test_rename_fixture.py
_"""Demonstratefixturerenaming."""_

import pytest

@pytest.fixture(name= _'lue'_ )
def ultimate_answer_to_life_the_universe_and_everything ():
_"""Returnultimateanswer."""_
    return 42

def test_everything (lue):
_"""Usethe shortername."""_
**assert** lue == 42
```

Here, lue is now the fixture name, instead of ultimate_answer_to_life_the_uni-
verse_and_everything. That name even shows up if we run it with --setup-show:

```
$ pytest--setup-showtest_rename_fixture.py
===================testsessionstarts===================
collected1 item

test_rename_fixture.py
SETUP F lue
test_rename_fixture.py::test_everything(fixturesused:lue).
TEARDOWNF lue

================1 passedin 0.01seconds=================
```

If you need to find out where lue is defined, you can add the pytest option
--fixtures and give it the filename for the test. It lists all the fixtures available
for the test, including ones that have been renamed:

```
$ pytest--fixturestest_rename_fixture.py
===================testsessionstarts===================
...

```


```
--------fixturesdefinedfromtest_rename_fixture--------
lue
Returnultimateanswer.

==============no testsran in 0.01seconds===============
```

Most of the output is omitted—there’s a lot there. Luckily, the fixtures we
defined are at the bottom, along with where they are defined. We can use this
to look up the definition of lue. Let’s use that in the Tasks project:

```
$ cd /path/to/code/ch3/b/tasks_proj
$ pytest--fixturestests/func/test_add.py
========================testsessionstarts========================
...

tmpdir_factory
Returna TempdirFactoryinstancefor the testsession.
tmpdir
Returna temporarydirectorypathobjectwhichis
uniqueto eachtestfunctioninvocation,createdas
a sub directoryof the basetemporarydirectory.
The returnedobjectis a `py.path.local`_pathobject.

------------------fixturesdefinedfromconftest-------------------
tasks_db_session
Connectto db beforetests,disconnectafter.
tasks_db
An emptytasksdb.
tasks_just_a_few
All summariesand ownersare unique.
tasks_mult_per_owner
Severalownerswithseveraltasks each.
db_with_3_tasks
Connecteddb with3 tasks,all unique.
db_with_multi_per_owner
Connecteddb with9 tasks,3 owners,all with3 tasks.

===================no testsran in 0.01 seconds====================
```

Cool. All of our conftest.py fixtures are there. And at the bottom of the builtin
list is the tmpdir and tmpdir_factory that we used also.

### Parametrizing Fixtures

In Parametrized Testing, on page 42, we parametrized tests. We can also
parametrize fixtures. We still use our list of tasks, list of task identifiers, and
an equivalence function, just as before:

```
**ch3/b/tasks_proj/tests/func/test_add_variety2.py
importpytest
importtasks
fromtasksimport** Task
```

```
tasks_to_try= (Task( _'sleep'_ , done=True),
Task( _'wake'_ , _'brian'_ ),
Task( _'breathe'_ , _'BRIAN'_ , True),
Task( _'exercise'_ , _'BrIaN'_ , False))

task_ids= [ _'Task({},{},{})'_ .format(t.summary,t.owner,t.done)
**for** t **in** tasks_to_try]

def equivalent (t1,t2):
_"""Checktwo tasksfor equivalence."""_
**return ((t1.summary== t2.summary) **and**
(t1.owner== t2.owner) **and**
(t1.done== t2.done))
```

But now, instead of parametrizing the test, we will parametrize a fixture
called a_task:

```
**ch3/b/tasks_proj/tests/func/test_add_variety2.py
@pytest.fixture(params=tasks_to_try)
def a_task (request):
_"""Usingno ids."""_
    return request.param

def test_add_a (tasks_db,a_task):
_"""Usinga_taskfixture(no ids)."""_
task_id= tasks.add(a_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,a_task)
```

The request listed in the fixture parameter is another builtin fixture that repre-
sents the calling state of the fixture. You’ll explore it more in the next lab.
It has a field param that is filled in with one element from the list assigned to
params in @pytest.fixture(params=tasks_to_try).

The a_task fixture is pretty simple—it just returns the request.param as its value
to the test using it. Since our task list has four tasks, the fixture will be called
four times, and then the test will get called four times:

```
$ cd /path/to/code/ch3/b/tasks_proj/tests/func
$ pytest-v test_add_variety2.py::test_add_a**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_a[a_task0]PASSED [ 25%]
test_add_variety2.py::test_add_a[a_task1]PASSED [ 50%]
test_add_variety2.py::test_add_a[a_task2]PASSED [ 75%]
test_add_variety2.py::test_add_a[a_task3]PASSED [100%]

================4 passedin 0.05seconds=================
```


We didn’t provide ids, so pytest just made up some names by appending a
number to the name of the fixture. However, we can use the same string list
we used when we parametrized our tests:

```
**ch3/b/tasks_proj/tests/func/test_add_variety2.py
@pytest.fixture(params=tasks_to_try,ids=task_ids)
def b_task (request):
_"""Usinga listof ids."""_
    return request.param

def test_add_b (tasks_db,b_task):
_"""Usingb_taskfixture,withids."""_
task_id= tasks.add(b_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,b_task)
```

This gives us better identifiers:

```
$ pytest-v test_add_variety2.py::test_add_b**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_b[Task(sleep,None,True)]PASSED[ 25%]
test_add_variety2.py::test_add_b[Task(wake,brian,False)]PASSED[ 50%]
test_add_variety2.py::test_add_b[Task(breathe,BRIAN,True)]PASSED[ 75%]
test_add_variety2.py::test_add_b[Task(exercise,BrIaN,False)]PASSED[100%]

================4 passedin 0.04seconds=================
```

We can also set the ids parameter to a function we write that provides the
identifiers. Here’s what it looks like when we use a function to generate the
identifiers:

```
**ch3/b/tasks_proj/tests/func/test_add_variety2.py
def id_func (fixture_value):
_"""Afunctionfor generatingids."""_
t = fixture_value
    return _'Task({},{},{})'_ .format(t.summary,t.owner,t.done)

@pytest.fixture(params=tasks_to_try,ids=id_func)
def c_task (request):
_"""Usinga function(id_func)to generateids."""_
    return request.param

def test_add_c (tasks_db,c_task):
_"""Usefixturewithgeneratedids."""_
task_id= tasks.add(c_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,c_task)
```


The function will be called from the value of each item from the parametrization.
Since the parametrization is a list of Task objects, id_func() will be called with a
Task object, which allows us to use the namedtuple accessor methods to access a
single Task object to generate the identifier for one Task object at a time. It’s a bit
cleaner than generating a full list ahead of time, and looks the same:

```
$ pytest-v test_add_variety2.py::test_add_c**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_c[Task(sleep,None,True)]PASSED[ 25%]
test_add_variety2.py::test_add_c[Task(wake,brian,False)]PASSED[ 50%]
test_add_variety2.py::test_add_c[Task(breathe,BRIAN,True)]PASSED[ 75%]
test_add_variety2.py::test_add_c[Task(exercise,BrIaN,False)]PASSED[100%]

================4 passedin 0.05seconds=================
```

With parametrized functions, you get to run that function multiple times. But
with parametrized fixtures, every test function that uses that fixture will be
called multiple times. Very powerful.

**Parametrizing Fixtures in the Tasks Project**

Now, let’s see how we can use parametrized fixtures in the Tasks project. So
far, we used TinyDB for all of the testing. But we want to keep our options
open until later in the project. Therefore, any code we write, and any tests
we write, should work with both TinyDB and with MongoDB.

The decision (in the code) of which database to use is isolated to the
start_tasks_db() call in the tasks_db_session fixture:

```
**ch3/b/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture(scope= _'session'_ )
def tasks_db_session (tmpdir_factory):
_"""Connectto db beforetests,disconnectafter."""_
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )
**yield**
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
_"""Anemptytasksdb."""_
tasks.delete_all()
```



The db_type parameter in the call to start_tasks_db() isn’t magic. It just ends up
switching which subsystem gets to be responsible for the rest of the database
interactions:

```
**tasks_proj/src/tasks/api.py
def start_tasks_db (db_path,db_type): _# type:(str,str)-> None
"""ConnectAPI functionsto a db."""_
**if not** isinstance(db_path,string_types):
**raise** TypeError( _'db_pathmustbe a string'_ )
**global** _tasksdb
**if** db_type== _'tiny'_ :
import tasks.tasksdb_tinydb**
_tasksdb= tasks.tasksdb_tinydb.start_tasks_db(db_path)
**elif** db_type== _'mongo'_ :
import tasks.tasksdb_pymongo**
_tasksdb= tasks.tasksdb_pymongo.start_tasks_db(db_path)
**else** :
**raise** ValueError( _"db_typemustbe a 'tiny'or 'mongo'"_ )
```

To test MongoDB, we need to run all the tests with db_type set to mongo. A small
change does the trick:

```
**ch3/c/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

_#@pytest.fixture(scope='session',params=['tiny',])_
@pytest.fixture(scope= _'session'_ , params=[ _'tiny'_ , _'mongo'_ ])
def tasks_db_session (tmpdir_factory,request):
_"""Connectto db beforetests,disconnectafter."""_
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir),request.param)
**yield** _# thisis wherethe testinghappens_
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
_"""Anemptytasksdb."""_
tasks.delete_all()
```

Here I added params=['tiny','mongo'] to the fixture decorator. I added request to the
parameter list of temp_db, and I set db_type to request.param instead of just picking
'tiny' or 'mongo'.

When you set the --verbose or -v flag with pytest running parametrized tests
or parametrized fixtures, pytest labels the different runs based on the value
of the parametrization. And because the values are already strings, that
works great.


```
Installing MongoDB
To follow along with MongoDB testing, make sure MongoDB and
pymongo are installed. I’ve been testing with the community edition
of MongoDB, found at https://www.mongodb.com/download-center. pymongo
is installed with pip—pip install pymongo. However, using MongoDB is
not necessary to follow along with the rest of the book; it’s used in
this example and in a debugger example in Lab 7.
```

Here’s what we have so far:

```
$ cd /path/to/code/ch3/c/tasks_proj
$ pip install pymongo
$ pytest-v --tb=no**
===================testsessionstarts===================
collected96 items

tests/func/test_add.py::test_add_returns_valid_id[tiny]PASSED[ 1%]
tests/func/test_add.py::test_added_task_has_id_set[tiny]PASSED[ 2%]
tests/func/test_add.py::test_add_increases_count[tiny]PASSED[ 3%]
tests/func/test_add_variety.py::test_add_1[tiny]PASSED[ 4%]
tests/func/test_add_variety.py::test_add_2[tiny-task0]PASSED[ 5%]
tests/func/test_add_variety.py::test_add_2[tiny-task1]PASSED[ 6%]
...

tests/func/test_add.py::test_add_returns_valid_id[mongo]FAILED[ 43%]
tests/func/test_add.py::test_added_task_has_id_set[mongo]FAILED[ 44%]
tests/func/test_add.py::test_add_increases_count[mongo]PASSED[ 45%]
tests/func/test_add_variety.py::test_add_1[mongo]FAILED[ 46%]
tests/func/test_add_variety.py::test_add_2[mongo-task0]FAILED[ 47%]
tests/func/test_add_variety.py::test_add_2[mongo-task1]FAILED[ 48%]
...

==========42 failed,54 passedin 4.85seconds===========
```

Hmm. Bummer. Looks like we’ll need to do some debugging before we let
anyone use the Mongo version. You’ll take a look at how to debug this in pdb:
Debugging Test Failures, on page 127. Until then, we’ll use the TinyDB version.

### Exercises

1. Create a test file called test_fixtures.py.
2. Write a few data fixtures—functions with the @pytest.fixture() decorator—that
return some data. Perhaps a list, or a dictionary, or a tuple.
3. For each fixture, write at least one test function that uses it.
4. Write two tests that use the same fixture.
5. Run pytest --setup-showtest_fixtures.py. Are all the fixtures run before every test?
6. Add scope='module' to the fixture from Exercise 4.
7. Re-run pytest--setup-showtest_fixtures.py. What changed?
8. For the fixture from Exercise 6, change return <data> to yield <data>.
9. Add print statements before and after the yield.
10. Run pytest-s -v test_fixtures.py. Does the output make sense?

### What’s Next

The pytest fixture implementation is flexible enough to use fixtures like
building blocks to build up test setup and teardown, and to swap in and out
different chunks of the system (like swapping in Mongo for TinyDB). Because
fixtures are so flexible, I use them heavily to push as much of the setup of
my tests into fixtures as I can.

In this lab, you looked at pytest fixtures you write yourself, as well as a
couple of builtin fixtures, tmpdir and tmpdir_factory. You’ll take a closer look at
the builtin fixtures in the next lab.
