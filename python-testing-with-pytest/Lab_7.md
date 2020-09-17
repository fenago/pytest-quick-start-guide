
CHAPTER 7

Using pytest with Other Tools

You don’t usually use pytest on its own, but rather in a testing environment

with other tools. This chapter looks at other tools that are often used in

combination with pytest for effective and efficient testing. While this is by no

means an exhaustive list, the tools discussed here give you a taste of the

power of mixing pytest with other tools.

### pdb: Debugging Test Failures

The pdb module is the Python debugger in the standard library. You use --pdb

to have pytest start a debugging session at the point of failure. Let’s look at

pdb in action in the context of the Tasks project.

In Parametrizing Fixtures, on page 66, we left the Tasks project with a few

failures:

```
**$ cd /path/to/code/ch3/c/tasks_proj
$ pytest--tb=no-q**
.........................................FF.FFFFFFF[ 53%]
FFFFFFFFFFFFFFFFFFFFFFFFFFFFFF.FFF........... [100%]
42 failed,54 passedin 5.51seconds

Before we look at how pdb can help us debug this test, let’s take a look at the

pytest options available to help speed up debugging test failures, which we

first looked at in Using Options, on page 10:

- _--tb=[auto/long/short/line/native/no]_ : Controls the traceback style.
- _-v / --verbose_ : Displays all the test names, passing or failing.
- _-l / --showlocals_ : Displays local variables alongside the stacktrace.
- _-lf / --last-failed_ : Runs just the tests that failed last.
- _-x / --exitfirst_ : Stops the tests session with the first failure.
- _--pdb_ : Starts an interactive debugging session at the point of failure.


```
Installing MongoDB
As mentioned in Chapter 3, pytest Fixtures, on page 51, running
the MongoDB tests requires installing MongoDB and pymongo.
I’ve been testing with the Community Server edition found at
https://www.mongodb.com/download-center. pymongo is installed with pip:
pipinstallpymongo. However, this is the last example in the book that
uses MongoDB. To try out the debugger without using MongoDB,
you could run the pytest commands from code/ch2/, as this directory
also contains a few failing tests.
```
We just ran the tests from code/ch3/c to see that some of them were failing. We

didn’t see the tracebacks or the test names because --tb=no turns off trace-

backs, and we didn’t have --verbose turned on. Let’s re-run the failures (at most

three of them) with verbose:

```
**$ pytest--tb=no--verbose--lf--maxfail=3**
===================testsessionstarts===================
plugins:cov-2.5.1
collected96 items/ 54 deselected
run-last-failure:rerunprevious42 failures

tests/func/test_add.py::test_add_returns_valid_id[mongo]FAILED[ 2%]
tests/func/test_add.py::test_added_task_has_id_set[mongo]FAILED[ 4%]
tests/func/test_add_variety.py::test_add_1[mongo]FAILED[ 7%]

=========3 failed,54 deselectedin 3.12seconds=========

Now we know which tests are failing. Let’s look at just one of them by using

-x, including the traceback by not using --tb=no, and showing the local vari-

ables with -l:

```
**$ pytest-v --lf-l -x**
===================testsessionstarts===================
plugins:cov-2.5.1
collected96 items/ 54 deselected
run-last-failure:rerunprevious42 failures

tests/func/test_add.py::test_add_returns_valid_id[mongo]FAILED[ 2%]

========================FAILURES=========================
____________test_add_returns_valid_id[mongo]_____________

tasks_db= None

```
def test_add_returns_valid_id(tasks_db):
"""tasks.add(<validtask>)shouldreturnan integer."""
# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint
new_task= Task('dosomething')
task_id= tasks.add(new_task)
```
Chapter 7. Using pytest with Other Tools • 128


**> assertisinstance(task_id,int)**
E AssertionError:assertFalse
E + whereFalse= isinstance(ObjectId('5b8c12dccb02981dc226d897'),int)

new_task = Task(summary='dosomething',owner=None,done=False,id=None)
task_id = ObjectId('5b8c12dccb02981dc226d897')
tasks_db = None

tests/func/test_add.py:16:AssertionError
=========1 failed,54 deselectedin 2.91seconds=========

Quite often this is enough to understand the test failure. In this particular

case, it’s pretty clear that task_id is not an integer—it’s an instance of ObjectId.

ObjectId is a type used by MongoDB for object identifiers within the database.

My intention with the tasksdb_pymongo.py layer was to hide particular details of

the MongoDB implementation from the rest of the system. Clearly, in this

case, it didn’t work.

However, we want to see how to use pdb with pytest, so let’s pretend that it

wasn’t obvious why this test failed. We can have pytest start a debugging

session and start us right at the point of failure with --pdb:

```
**$ pytest-v --lf-x --pdb**
===================testsessionstarts===================
plugins:cov-2.5.1
collected96 items/ 54 deselected
run-last-failure:rerunprevious42 failures

tests/func/test_add.py::test_add_returns_valid_id[mongo]FAILED[ 2%]
**>>>>>>>>>>>>>>>>>>>>>>>>traceback>>>>>>>>>>>>>>>>>>>>>>>>**

tasks_db= None

def test_add_returns_valid_id(tasks_db):
"""tasks.add(<validtask>)shouldreturnan integer."""
_# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint_
new_task= Task('dosomething')
task_id= tasks.add(new_task)
**> assertisinstance(task_id,int)**
E AssertionError:assertFalse
E + whereFalse= isinstance(ObjectId('5b8c1316cb02981dc91fccd1'),int)

tests/func/test_add.py:16:AssertionError
**>>>>>>>>>>>>>>>>>>>>>>enteringPDB >>>>>>>>>>>>>>>>>>>>>>>
> /path/to/code/ch3/c/tasks_proj/tests/func/test_add.py(16)
> test_add_returns_valid_id()
-> assertisinstance(task_id,int)**
(Pdb)

pdb: Debugging Test Failures • 129


Now that we are at the (Pdb) prompt, we have access to all of the interactive

debugging features of pdb. When looking at failures, I regularly use these

commands:

- p/printexpr: Prints the value of exp.
- pp expr: Pretty prints the value of expr.
- l/list: Lists the point of failure and five lines of code above and below.
- l/list begin,end: Lists specific line numbers.
- a/args: Prints the arguments of the current function with their values. (This
    is helpful when in a test helper function.)
- u/up: Moves up one level in the stack trace.
- d/down: Moves down one level in the stack trace.
- q/quit: Quits the debugging session.

Other navigation commands like step and next aren’t that useful since we are

sitting right at an assert statement. You can also just type variable names and

get the values.

You can use p/printexpr similar to the -l/--showlocals option to see values within

the function:

(Pdb)p new_task
Task(summary='dosomething',owner=None, done=False,id=None)
(Pdb)p task_id
ObjectId('5b8c1316cb02981dc91fccd1')
(Pdb)

Now you can quit the debugger and continue on with testing.

(Pdb)q

========1 failed,54 deselectedin 87.51seconds=========

If we hadn’t used -x, pytest would have opened pdb again at the next failed

test. More information about using the pdb module is available in the Python

documentation.^1

### Coverage.py: Determining How Much Code Is Tested

Code coverage is a measurement of what percentage of the code under test

is being tested by a test suite. When you run the tests for the Tasks project,

1. https://docs.python.org/3/library/pdb.html

Chapter 7. Using pytest with Other Tools • 130


some of the Tasks functionality is executed with every test, but not all of it.

Code coverage tools are great for telling you which parts of the system are

being completely missed by tests.

Coverage.py is the preferred Python coverage tool that measures code coverage.

You’ll use it to check the Tasks project code under test with pytest.

Before you use coverage.py, you need to install it. I’m also going to have you

install a plugin called pytest-cov that will allow you to call coverage.py from pytest

with some extra pytest options. Since coverage is one of the dependencies of

pytest-cov, it is sufficient to install pytest-cov, as it will pull in coverage.py:

```
**$ pip installpytest-cov
...**
Installingcollectedpackages:coverage,pytest-cov
Successfullyinstalledcoverage-4.5.1 pytest-cov-2.5.1

Let’s run the coverage report on version 2 of Tasks. If you still have the first

version of the Tasks project installed, uninstall it and install version 2:

```
**$ pip uninstalltasks**
Uninstallingtasks-0.1.0:
**...**
Proceed(y/n)?y
Successfullyuninstalledtasks-0.1.0
```
**$ cd /path/to/code/ch7/tasks_proj_v2
$ pip install-e.**
Obtainingfile:///path/to/code/ch7/tasks_proj_v2
**...**
Installingcollectedpackages:tasks
Runningsetup.pydevelopfor tasks
Successfullyinstalledtasks
```
**$ pip list
...**
tasks 0.1.1 /path/to/code/tasks_proj_v2/src
**...**

Coverage.py: Determining How Much Code Is Tested • 131


Now that the next version of Tasks is installed, we can run our baseline cov-

erage report:

```
**$ cd /path/to/code/ch7/tasks_proj_v2
$ pytest--cov=src**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected62 items

tests/func/test_add.py... [ 4%]
tests/func/test_add_variety.py....................[ 37%]
........ [ 50%]
tests/func/test_add_variety2.py............ [ 69%]
tests/func/test_api_exceptions.py......... [ 83%]
tests/func/test_unique_id.py. [ 85%]
tests/unit/test_cli.py..... [ 93%]
tests/unit/test_task.py.... [100%]

----------coverage:platformdarwin,python3.7.0-final-0-----------
Name Stmts Miss Cover
--------------------------------------------------
src/tasks/__init__.py 2 0 100%
src/tasks/api.py 79 22 72%
src/tasks/cli.py 45 14 69%
src/tasks/config.py 18 12 33%
src/tasks/tasksdb_pymongo.py 74 74 0%
src/tasks/tasksdb_tinydb.py 32 4 88%
--------------------------------------------------
TOTAL 250 126 50%

================62 passedin 0.62seconds================

Since the current directory is tasks_proj_v2 and the source code under test is

all within src, adding the option --cov=src generates a coverage report for that

specific directory under test only.

As you can see, some of the files have pretty low, to even 0%, coverage. These

are good reminders: tasksdb_pymongo.py is at 0% because we’ve turned off testing

for MongoDB in this version. Some of the others are pretty low. The project

will definitely have to put tests in place for all of these areas before it’s ready

for prime time.

A couple of files I thought would have a higher coverage percentage are api.py

and tasksdb_tinydb.py. Let’s look at tasksdb_tinydb.py and see what’s missing. I find

the best way to do that is to use the HTML reports.

If you run coverage.py again with --cov-report=html, an HTML report is generated:

```
**$ pytest--cov=src--cov-report=html**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1

Chapter 7. Using pytest with Other Tools • 132


collected62 items

tests/func/test_add.py... [ 4%]
tests/func/test_add_variety.py....................[ 37%]
........ [ 50%]
tests/func/test_add_variety2.py............ [ 69%]
tests/func/test_api_exceptions.py......... [ 83%]
tests/func/test_unique_id.py. [ 85%]
tests/unit/test_cli.py..... [ 93%]
tests/unit/test_task.py.... [100%]

----------coverage:platformdarwin,python3.7.0-final-0-----------
CoverageHTMLwrittento dir htmlcov

================62 passedin 0.70seconds================

You can then open htmlcov/index.html in a browser, which shows the output in

the following screen:

Clicking on tasksdb_tinydb.py shows a report for the one file. The top of the report

shows the percentage of lines covered, plus how many lines were covered and

how many are missing, as shown in the following screen:

Coverage.py: Determining How Much Code Is Tested • 133


Chapter 7. Using pytest with Other Tools • 134


Scrolling down, you can see the missing lines, as shown in the next screen:

Even though this screen isn’t the complete page for this file, it’s enough to

tell us that:

1. We’re not testing list_tasks() with owner set.
2. We’re not testing update() or delete().
3. We may not be testing unique_id() thoroughly enough.

Great. We can put those on our testing to-do list, along with testing the config

system.

While code coverage tools are extremely useful, striving for 100% coverage

can be dangerous. When you see code that isn’t tested, it might mean a test

is needed. But it also might mean that there’s some functionality of the system

that isn’t needed and could be removed. Like all software development tools,

code coverage analysis does not replace thinking.

Quite a few more options and features of both coverage.py and pytest-cov are

available. More information can be found in the coverage.py^2 and pytest-cov^3

documentation.

2. https://coverage.readthedocs.io
3. https://pytest-cov.readthedocs.io

Coverage.py: Determining How Much Code Is Tested • 135


### mock: Swapping Out Part of the System

The mock package is used to swap out pieces of the system to isolate bits of

our code under test from the rest of the system. Mock objects are sometimes

called test doubles, spies, fakes, or stubs. Between pytest’s own monkeypatch

fixture (covered in Using monkeypatch, on page 88) and mock, you should

have all the test double functionality you need.

```
Mocks Are Weird
If this is the first time you’ve encountered test doubles like mocks,
stubs, and spies, it’s gonna get real weird real fast. It’s fun though,
and quite powerful.
```
The mock package is shipped as part of the Python standard library as

unittest.mock as of Python 3.3. In earlier versions, it’s available as a separate

PyPI-installable package as a rolling backport. What that means is that you

can use the PyPI version of mock with Python 2.6 through the latest Python

version and get the same functionality as the latest Python mock. However,

for use with pytest, a plugin called pytest-mock has some conveniences that

make it my preferred interface to the mock system.

For the Tasks project, we’ll use mock to help us test the command-line interface.

In Coverage.py: Determining How Much Code Is Tested, on page 130 , you saw

that our cli.py file wasn’t being tested at all. We’ll start to fix that now. But

let’s first talk about strategy.

An early decision in the Tasks project was to do most of the functionality

testing through api.py. Therefore, it’s a reasonable decision that the command-

line testing doesn’t have to be complete functionality testing. We can have a

fair amount of confidence that the system will work through the CLI if we

mock the API layer during CLI testing. It’s also a convenient decision, allowing

us to look at mocks in this section.

The implementation of the Tasks CLI uses the Click third-party command-

line interface package.^4 There are many alternatives for implementing a CLI,

including Python’s builtin argparse module. One of the reasons I chose Click

is because it includes a test runner to help us test Click applications. However,

the code in cli.py, although hopefully typical of Click applications, is not

obvious.

4. [http://click.pocoo.org](http://click.pocoo.org)

Chapter 7. Using pytest with Other Tools • 136


Let’s pause and install version 2 of Tasks:

```
**$ cd /path/to/code/
$ pip install-e ch7/tasks_proj_v2
...**
Successfullyinstalledtasks

In the rest of this section, you’ll develop some tests for the “list” functionality.

Let’s see it in action to understand what we’re going to test:

```
**$ taskslist**
ID owner donesummary
-- ----- -----------
```
**$ tasksadd** _"do somethinggreat"_
```
**$ tasksadd** _"repeat"_ **-o Brian
$ tasksadd** _"againand again"_ **--ownerOkken
$ taskslist**
ID owner donesummary
-- ----- -----------
1 Falsedo somethinggreat
2 BrianFalserepeat
3 OkkenFalseagainand again
```
**$ taskslist-o Brian**
ID owner donesummary
-- ----- -----------
2 BrianFalserepeat
```
**$ taskslist--ownerBrian**
ID owner donesummary
-- ----- -----------
2 BrianFalserepeat

Looks pretty simple. The tasks list command lists all the tasks with a header.

It prints the header even if the list is empty. It prints just the things from one

owner if -o or --owner are used. How do we test it? Lots of ways are possible,

but we’re going to use mocks.

Tests that use mocks are necessarily white-box tests, and we have to look

into the code to decide what to mock and where. The main entry point is here:

**ch7/tasks_proj_v2/src/tasks/cli.py
if** __name__== _'__main__'_ :
tasks_cli()

That’s just a call to tasks_cli():

**ch7/tasks_proj_v2/src/tasks/cli.py**
@click.group(context_settings={ _'help_option_names'_ : [ _'-h'_ , _'--help'_ ]})
@click.version_option(version= _'0.1.1'_ )
**def tasks_cli** ():
_"""Runthe tasksapplication."""_
**pass**

mock: Swapping Out Part of the System • 137


Obvious? No. But hold on, it gets better (or worse, depending on your perspec-

tive). Here’s one of the commands—the list command:

**ch7/tasks_proj_v2/src/tasks/cli.py**
@tasks_cli.command(name= _"list"_ , help= _"listtasks"_ )
@click.option( _'-o'_ , _'--owner'_ , default=None,
help= _'listtaskswiththisowner'_ )
**def list_tasks** (owner):
_"""
Listtasksin db.
If ownergiven,onlylisttaskswiththatowner.
"""_
formatstr= _"{: >4} {: >10}{: >5} {}"_
**print** (formatstr.format( _'ID'_ , _'owner'_ , _'done'_ , _'summary'_ ))
**print** (formatstr.format( _'--'_ , _'-----'_ , _'----'_ , _'-------'_ ))
**with** _tasks_db():
**for** t **in** tasks.list_tasks(owner):
done= _'True'_ **if** t.done **else** _'False'_
owner= _''_ **if** t.owner **is** None **else** t.owner
**print** (formatstr.format(
t.id,owner,done,t.summary))

Once you get used to writing Click code, it’s not that bad. I’m not going to

explain all of this here, as developing command-line code isn’t the focus of

the book; however, even though I’m pretty sure I have this code right, there’s

lots of room for human error. That’s why a good set of automated tests to

make sure this works correctly is important.

This list_tasks(owner) function depends on a couple of other functions: _tasks_db(),

which is a context manager, and tasks.list_tasks(owner), which is the API function.

We’re going to use mock to put fake functions in place for _tasks_db() and

tasks.list_tasks(). Then we can call the list_tasks method through the command-

line interface and make sure it calls the tasks.list_tasks() function correctly and

deals with the return value correctly.

To stub _tasks_db(), let’s look at the real implementation:

**ch7/tasks_proj_v2/src/tasks/cli.py**
@contextmanager
**def _tasks_db** ():
config= tasks.config.get_config()
tasks.start_tasks_db(config.db_path,config.db_type)
**yield**
tasks.stop_tasks_db()

The _tasks_db() function is a context manager that retrieves the configuration

from tasks.config.get_config(), another external dependency, and uses the config-

uration to start a connection with the database. The yield releases control to

Chapter 7. Using pytest with Other Tools • 138


the with block of list_tasks(), and after everything is done, the database connection

is stopped.

For the purpose of just testing the CLI behavior up to the point of calling API

functions, we don’t need a connection to an actual database. Therefore, we

can replace the context manager with a simple stub:

**ch7/tasks_proj_v2/tests/unit/test_cli.py**
@contextmanager
**def stub_tasks_db** ():
**yield**

Because this is the first time we’ve looked at our test code for test_cli,py, let’s

look at this with all of the import statements:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
fromclick.testingimport** CliRunner
**fromcontextlibimport** contextmanager
**importpytest
fromtasks.apiimport** Task
**importtasks.cli
importtasks.config**

@contextmanager
**def stub_tasks_db** ():
**yield**

Those imports are for the tests. The only import needed for the stub is from

contextlib importcontextmanager.

We’ll use mock to replace the real context manager with our stub. Actually,

we’ll use mocker, which is a fixture provided by the pytest-mock plugin. Let’s look

at an actual test. Here’s a test that calls tasks list:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
def test_list_no_args** (mocker):
mocker.patch.object(tasks.cli, _'_tasks_db'_ , new=stub_tasks_db)
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ , return_value=[])
runner= CliRunner()
runner.invoke(tasks.cli.tasks_cli,[ _'list'_ ])
tasks.cli.tasks.list_tasks.assert_called_once_with(None)

The mocker fixture is provided by pytest-mock as a convenience interface to

unittest.mock. The first line, mocker.patch.object(tasks.cli,'_tasks_db',new=stub_tasks_db),

replaces the _tasks_db() context manager with our stub that does nothing.

mock: Swapping Out Part of the System • 139


The second line, mocker.patch.object(tasks.cli.tasks,'list_tasks',return_value=[]), replaces

any calls to tasks.list_tasks() from within tasks.cli to a default MagicMock object with

a return value of an empty list. We can use this object later to see if it was

called correctly. The MagicMock class is a flexible subclass of unittest.Mock with

reasonable default behavior and the ability to specify a return value, which

is what we are using in this example. The Mock and MagicMock classes (and

others) are used to mimic the interface of other code with introspection

methods built in to allow you to ask them how they were called.

The third and fourth lines of test_list_no_args() use the Click CliRunner to do the

same thing as calling tasks list on the command line.

The final line uses the mock object to make sure the API call was called cor-

rectly. The assert_called_once_with() method is part of unittest.mock.Mock objects,

which are all listed in the Python documentation.^5

Let’s look at an almost identical test function that checks the output:

**ch7/tasks_proj_v2/tests/unit/test_cli.py**
@pytest.fixture()
**def no_db** (mocker):
mocker.patch.object(tasks.cli, _'_tasks_db'_ , new=stub_tasks_db)

**def test_list_print_empty** (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ , return_value=[])
runner= CliRunner()
result= runner.invoke(tasks.cli.tasks_cli,[ _'list'_ ])
expected_output= ( _" ID owner donesummary\n"
" -- ----- -----------\n"_ )
**assert** result.output== expected_output

This time we put the mock stubbing of tasks_db into a no_db fixture so we

can reuse it more easily in future tests. The mocking of tasks.list_tasks() is

the same as before. This time, however, we are also checking the output

of the command-line action through result.output and asserting equality to

expected_output.

This assert could have been put in the first test, test_list_no_args, and we could

have eliminated the need for two tests. However, I have less faith in my ability

to get CLI code correct than other code, so separating the questions of “Is the

API getting called correctly?” and “Is the action printing the right thing?” into

two tests seems appropriate.

5. https://docs.python.org/dev/library/unittest.mock.html

Chapter 7. Using pytest with Other Tools • 140


The rest of the tests for the tasks list functionality don’t add any new concepts,

but perhaps looking at several of these makes the code easier to understand:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
def test_list_print_many_items** (no_db,mocker):
many_tasks= (
Task( _'writechapter'_ , _'Brian'_ , True,1),
Task( _'editchapter'_ , _'Katie'_ , False,2),
Task( _'modifychapter'_ , _'Brian'_ , False,3),
Task( _'finalizechapter'_ , _'Katie'_ , False,4),
)
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ ,
return_value=many_tasks)
runner= CliRunner()
result= runner.invoke(tasks.cli.tasks_cli,[ _'list'_ ])
expected_output= ( _" ID owner donesummary\n"
" -- ----- -----------\n"
" 1 Brian Truewritechapter\n"
" 2 KatieFalseeditchapter\n"
" 3 BrianFalsemodifychapter\n"
" 4 KatieFalsefinalizechapter\n"_ )
**assert** result.output== expected_output

**def test_list_dash_o** (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ )
runner= CliRunner()
runner.invoke(tasks.cli.tasks_cli,[ _'list'_ , _'-o'_ , _'brian'_ ])
tasks.cli.tasks.list_tasks.assert_called_once_with( _'brian'_ )

**def test_list_dash_dash_owner** (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ )
runner= CliRunner()
runner.invoke(tasks.cli.tasks_cli,[ _'list'_ , _'--owner'_ , _'okken'_ ])
tasks.cli.tasks.list_tasks.assert_called_once_with( _'okken'_ )

Let’s make sure they all work:

```
**$ cd /path/to/code/ch7/tasks_proj_v2
$ pytest-v tests/unit/test_cli.py**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected5 items

tests/unit/test_cli.py::test_list_no_argsPASSED [ 20%]
tests/unit/test_cli.py::test_list_print_emptyPASSED[ 40%]
tests/unit/test_cli.py::test_list_print_many_itemsPASSED[ 60%]
tests/unit/test_cli.py::test_list_dash_oPASSED [ 80%]
tests/unit/test_cli.py::test_list_dash_dash_ownerPASSED[100%]

================5 passedin 0.07seconds=================

Yay! They pass.

mock: Swapping Out Part of the System • 141


This was an extremely fast fly-through of using test doubles and mocks. If

you want to use mocks in your testing, I encourage you to read up on

unittest.mock in the standard library documentation,^6 and about pytest-mock at

pypi.python.org.^7

### tox: Testing Multiple Configurations

tox is a command-line tool that allows you to run your complete suite of tests

in multiple environments. We’re going to use it to test the Tasks project in

multiple versions of Python. However, tox is not limited to just Python versions.

You can use it to test with different dependency configurations and different

configurations for different operating systems.

In gross generalities, here’s a mental model for how tox works:

tox uses the setup.py file for the package under test to create an installable

source distribution of your package. It looks in tox.ini for a list of environments

and then for each environment...

1. tox creates a virtual environment in a .tox directory.
2. tox pip installs some dependencies.
3. tox pip installs your package from the sdist in step 1.
4. tox runs your tests.

After all of the environments are tested, tox reports a summary of how they

all did.

This makes a lot more sense when you see it in action, so let’s look at how

to modify the Tasks project to use tox to test Python 2.7 and 3.6. I chose

versions 2.7 and 3.6 because they are both already installed on my system.

If you have different versions installed, go ahead and change the envlist line

to match whichever version you have or are willing to install.

The first thing we need to do to the Tasks project is add a tox.ini file at the

same level as setup.py—the top project directory. I’m also going to move anything

that’s in pytest.ini into tox.ini.

Here’s the abbreviated code layout:

6. https://docs.python.org/dev/library/unittest.mock.html
7. https://pypi.python.org/pypi/pytest-mock

Chapter 7. Using pytest with Other Tools • 142


tasks_proj_v2/
├──...
├──setup.py
├──tox.ini
├──src
│ └──tasks
│ ├──__init__.py
│ ├──api.py
│ └──...
└──tests
├──conftest.py
├──func
│ ├──__init__.py
│ ├──test_add.py
│ └──...
└──unit
├──__init__.py
├──test_task.py
└──...

Now, here’s what the tox.ini file looks like:

**ch7/tasks_proj_v2/tox.ini**
_# tox.ini, put in samedir as setup.py_

**[tox]**
envlist= _py27,py37_

**[testenv]**
deps= _pytest_
commands= _pytest_

**[pytest]**
addopts= _-rsxX-l --tb=short--strict_
markers=
**smoke:Run the smoketesttestfunctions
get:Run the testfunctionsthattesttasks.get()**

Under [tox], we have envlist= py27,py37. This is a shorthand to tell tox to run

our tests using python2.7, python3.7

Under [testenv], the deps=pytest line tells tox to make sure pytest is installed. If

you have multiple test dependencies, you can put them on separate lines.

You can also specify which version to use.

The commands=pytest line tells tox to run pytest in each environment.

Under [pytest], we can put whatever we normally would want to put into pytest.ini

to configure pytest, as discussed in Chapter 6, Configuration, on page 115. In

this case, addopts is used to turn on extra summary information for skips,

xfails, and xpasses (-rsxX) and turn on showing local variables in stack traces

tox: Testing Multiple Configurations • 143


(-l). It also defaults to shortened stack traces (--tb=short) and makes sure all

markers used in tests are declared first (--strict). The markers section is where

the markers are declared.

Before running tox, you have to make sure you install it:

```
**$ pip installtox**

This can be done within a virtual environment.

Then to run tox, just run, well, tox:

```
**$ cd /path/to/code/ch7/tasks_proj_v2
$ tox**
GLOBsdist-make:/path/to/code/ch7/tasks_proj_v2/setup.py
py27inst-nodeps:/path/to/code/ch7/tasks_proj_v2/.tox/dist/tasks-0.1.1.zip
py27installed:atomicwrites==1.2.1,attrs==18.2.0,Click==7.0,funcsigs==1.0.2,
mock==2.0.0,more-itertools==4.3.0,pathlib2==2.3.2,pbr==5.0.0,pluggy==0.8.0,
py==1.7.0,pytest==3.9.1,pytest-mock==1.10.0,scandir==1.9.0,six==1.11.0,
tasks==0.1.1,tinydb==3.11.1

py27runtests:PYTHONHASHSEED='2121166562'
py27runtests:commands[0]| pytest
========================testsessionstarts========================
plugins:mock-1.10.0
collected62 items

tests/func/test_add.py... [ 4%]
tests/func/test_add_variety.py............................ [ 50%]
tests/func/test_add_variety2.py............ [ 69%]
tests/func/test_api_exceptions.py......... [ 83%]
tests/func/test_unique_id.py. [ 85%]
tests/unit/test_cli.py..... [ 93%]
tests/unit/test_task.py.... [100%]

=====================62 passedin 0.51seconds=====================
py37inst-nodeps:/path/to/code/ch7/tasks_proj_v2/.tox/dist/tasks-0.1.1.zip
py37installed:atomicwrites==1.2.1,attrs==18.2.0,Click==7.0,
more-itertools==4.3.0,pluggy==0.8.0,py==1.7.0,pytest==3.9.1,
pytest-mock==1.10.0,six==1.11.0,tasks==0.1.1,tinydb==3.11.1
py37runtests:PYTHONHASHSEED='2121166562'
py37runtests:commands[0]| pytest
========================testsessionstarts========================
plugins:mock-1.10.0
collected62 items

tests/func/test_add.py... [ 4%]
tests/func/test_add_variety.py............................ [ 50%]
tests/func/test_add_variety2.py............ [ 69%]
tests/func/test_api_exceptions.py......... [ 83%]
tests/func/test_unique_id.py. [ 85%]
tests/unit/test_cli.py..... [ 93%]
tests/unit/test_task.py.... [100%]

Chapter 7. Using pytest with Other Tools • 144


=====================62 passedin 0.46seconds=====================
______________________________summary______________________________
py27:commandssucceeded
py37:commandssucceeded
congratulations:)

At the end, we have a nice summary of all the test environments and their

outcomes:

_________________________summary_________________________
py27:commandssucceeded
py37:commandssucceeded
congratulations:)

Doesn’t that give you a nice, warm, happy feeling? We got a “congratulations”

_and_ a smiley face.

tox is much more powerful than what I’m showing here and deserves your

attention if you are using pytest to test packages intended to be run in multiple

environments. For more detailed information, check out the tox documentation.^8

### Jenkins CI: Automating Your Automated Tests

Jenkins^9 is an open source automation server that is frequently used for

continuous integration. Continuous integration (CI) systems such as Jenkins

are frequently used to launch test suites after each code commit. pytest

includes options to generate junit.xml-formatted files required by Jenkins and

other CI systems to display test results.

In this section, you’ll take a look at how the Tasks project might be set up in

Jenkins. I’m not going to walk through the Jenkins installation. It’s different

for every operating system, and instructions are available on the Jenkins

website.

When using Jenkins for running pytest suites, there are a few Jenkins plugins

that you may find useful. Here’s one that I find especially useful, and will be

used in the example:

- _Test Results Analyzer plugin_ : This plugin shows the history of test execu-
    tion results in a tabular or graphical format.

You can install plugins by going to the top-level Jenkins page, which is local-

host:8080/manage for me as I’m running it locally, then clicking Manage Jenkins

-> Manage Plugins -> Available. Search for the plugin you want with the filter

8. https://tox.readthedocs.io
9. https://jenkins.io

Jenkins CI: Automating Your Automated Tests • 145


box. Check the box for the plugin you want. I usually select “Install without

Restart,” and then on the Installing Plugins/Upgrades page, I select the box

that says, “Restart Jenkins when installation is complete and no jobs are

running.”

We’ll look at a complete configuration in case you’d like to follow along for

the Tasks project. The Jenkins project/item is a “Freestyle Project” named

“tasks,” and the only configuration I’ve changed from the default settings are

one build step and one post-build action.

After creating the Jenkins project, scroll down to the build steps. On a Mac

or Unix-like systems, select Add build step->Execute shell. On Windows, select Add

buildstep->ExecuteWindowsbatchcommand. Since I’m on a Mac, I used an Executeshell

block, as shown here:

The content of the text box is:

_# yourpathswillbe different_
venv=/Users/okken/code/venv
code_path=/Users/okken/code/ch7/tasks_proj_v2

_# run pytestin projectusingvirtualenvironmentinstalledpytest_
cd ${code_path}
${venv}/bin/pytest--junitxml=${WORKSPACE}/results.xml

The bottom line has ${venv}/bin/pytest--junit-xml=${WORKSPACE}/results.xml. The --junit-

xml flag is the only thing needed to produce the junit.xml format results file

Jenkins needs.

Next, we’ll add a post-build action: Add post-buildaction->PublishJunit test result report.

Fill in the Test report XMLs with results.xml, as shown in the next screen.

Chapter 7. Using pytest with Other Tools • 146


That’s it! Now we can run tests through Jenkins. Here are the steps:

1. Click Save.
2. Click “Build Now”
3. When it’s done, hover over the number next to the ball in Build History

```
and select Console Output jrom the drop-down menu that appears. (Or
click the build number and select Console Output.)
```
4. If the build failed, try to figure out what went wrong from the console

output.

Before running the tests a second time, I’ve added a failure to one of the tests.

After a refresh of the top project view, we see the following overview:

Jenkins CI: Automating Your Automated Tests • 147


You can zoom in to the failure information by clicking inside the graph or on

the “Latest Test Result” link to see an overview of the test session. Click on

“+” icons to expand for test failures.

Clicking on any of the failing test names shows you the individual test failure

information.

Going back to Jenkins > tasks, you can click on Test Results Analyzer to see

a table view of test results:

You’ve seen how to run pytest suites with virtual environments from Jenkins,

but there are quite a few other topics to explore around using pytest and

Jenkins together. You can test multiple environments with Jenkins by either

Chapter 7. Using pytest with Other Tools • 148


setting up separate Jenkins jobs for each environment, or by having Jenkins

call tox directly. There’s also a nice plugin called Cobertura that is able to

display coverage data from coverage.py. Check out the Jenkins documentation^10

for more information.

### unittest: Running Legacy Tests with pytest

unittest is the test framework built into the Python standard library. Its

purpose is to test Python itself, but it is often used for project testing, too.

pytest works as a unittest runner, and can run both pytest and unittest tests

in the same session.

Let’s pretend that when the Tasks project started, it used unittest instead of

pytest for testing. And perhaps there are a lot of tests already written. Fortunate-

ly, you can use pytest to run unittest-based tests. This might be a reasonable

option if you are migrating your testing effort from unittest to pytest. You can

leave all the old tests as unittest, and write new ones in pytest. You can also

gradually migrate older tests as you have time, or as changes are needed. There

are a couple of issues that might trip you up in the migration, however, and I’ll

address some of those here. First, let’s look at a test written for unittest:

**ch7/unittest/test_delete_unittest.py
importunittest
importshutil
importtempfile
importtasks
fromtasksimport** Task

**def setUpModule** ():
_"""Maketempdir,initializeDB."""_
**global** temp_dir
temp_dir= tempfile.mkdtemp()
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )

**def tearDownModule** ():
_"""Cleanup DB, removetempdir."""_
tasks.stop_tasks_db()
shutil.rmtree(temp_dir)

**class** TestNonEmpty(unittest.TestCase):

```
def setUp (self):
tasks.delete_all() # startempty
# add a few items,savingids
self.ids= []
self.ids.append(tasks.add(Task( 'One' , 'Brian' , True)))
```
10.https://wiki.jenkins-ci.org/display/JENKINS/Cobertura+Plugin

unittest: Running Legacy Tests with pytest • 149


```
self.ids.append(tasks.add(Task( 'Two' , 'StillBrian' , False)))
self.ids.append(tasks.add(Task( 'Three' , 'NotBrian' , False)))
```
```
def test_delete_decreases_count (self):
# GIVEN3 items
self.assertEqual(tasks.count(),3)
# WHENwe deleteone
tasks.delete(self.ids[0])
# THENcountdecreasesby 1
self.assertEqual(tasks.count(),2)
```
The actual test is at the bottom, test_delete_decreases_count(). The rest of the code

is there for setup and teardown. This test runs fine in unittest:

```
**$ cd /path/to/code/ch7/unittest
$ python-m unittest-v test_delete_unittest.py**
test_delete_decreases_count(test_delete_unittest.TestNonEmpty)... ok

----------------------------------------------------------------------
Ran 1 testin 0.029s

OK

It also runs fine in pytest:

```
**$ pytest-v test_delete_unittest.py**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected1 item

test_delete_unittest.py::TestNonEmpty::test_delete_decreases_countPASSED[100%]

================1 passedin 0.02seconds=================

This is great if you just want to use pytest as a test runner for unittest.

However, our premise is that the Tasks project is migrating to pytest. Let’s

say we want to migrate tests one at a time and run both unittest and pytest

versions at the same time until we are confident in the pytest versions. Let’s

look at a rewrite for this test and then try running them both:

**ch7/unittest/test_delete_pytest.py
importtasks**

**def test_delete_decreases_count** (db_with_3_tasks):
ids = [t.id **for** t **in** tasks.list_tasks()]
_# GIVEN3 items_
**assert** tasks.count()== 3
_# WHENwe deleteone_
tasks.delete(ids[0])
_# THENcountdecreasesby 1_
**assert** tasks.count()== 2

Chapter 7. Using pytest with Other Tools • 150


The fixtures we’ve been using for the Tasks project, including db_with_3_tasks

introduced in Using Multiple Fixtures, on page 57, help set up the database

before the test. It’s a much smaller file, even though the test function itself

is almost identical.

Both tests pass individually:

```
**$ pytest-q test_delete_pytest.py**

. [100%]
1 passedin 0.02seconds
```
**$ pytest-q test_delete_unittest.py**
. [100%]
1 passedin 0.02seconds

You can even run them together if—and only if—you make sure the unittest

version runs first:

```
**$ pytest-v test_delete_unittest.pytest_delete_pytest.py**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected2 items

test_delete_unittest.py::TestNonEmpty::test_delete_decreases_countPASSED[ 50%]
test_delete_pytest.py::test_delete_decreases_countPASSED[100%]

================2 passedin 0.03seconds=================

If you run the pytest version first, something goes haywire:

```
**$ pytest-v test_delete_pytest.pytest_delete_unittest.py**
===================testsessionstarts===================
collected2 items

test_delete_pytest.py::test_delete_decreases_countPASSED[ 50%]
test_delete_unittest.py::TestNonEmpty::test_delete_decreases_countPASSED[100%]
test_delete_unittest.py::TestNonEmpty::test_delete_decreases_countERROR[100%]

=========================ERRORS==========================
ERRORat teardownof TestNonEmpty.test_delete_decreases_count
**...**
conftest.py:12:in tasks_db_session
tasks.stop_tasks_db()
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

def stop_tasks_db(): _# type:() -> None_
"""DisconnectAPI functionsfromdb."""
global_tasksdb
**> _tasksdb.stop_tasks_db()**
E AttributeError:'NoneType'objecthas no attribute'stop_tasks_db'

../tasks_proj_v2/src/tasks/api.py:128:AttributeError
============2 passed,1 errorin 0.14seconds============

unittest: Running Legacy Tests with pytest • 151


You can see that something goes wrong at the end, after both tests have run

and passed.

Let’s use --setup-show to investigate further:

```
**$ pytest-q --tb=no--setup-showtest_delete_pytest.pytest_delete_unittest.py**

SETUP S tmpdir_factory
SETUP S tasks_db_session(fixturesused:tmpdir_factory)
SETUP F tasks_db(fixturesused:tasks_db_session)
SETUP F tasks_just_a_few
SETUP F db_with_3_tasks (fixturesused:tasks_db,tasks_just_a_few)
test_delete_pytest.py::test_delete_decreases_count
(fixturesused:db_with_3_tasks,tasks_db,tasks_db_session,
tasks_just_a_few,tmpdir_factory).
TEARDOWNF db_with_3_tasks
TEARDOWNF tasks_just_a_few
TEARDOWNF tasks_db
test_delete_unittest.py::TestNonEmpty::test_delete_decreases_count.
TEARDOWNS tasks_db_session
TEARDOWNS tmpdir_factoryE
2 passed,1 errorin 0.15seconds

The session scope teardown fixtures are run after all the tests, including the

unittest tests. This stumped me for a bit until I realized that the tearDownModule()

in the unittest module was shutting down the connection to the database.

The tasks_db_session() teardown from pytest was then trying to do the same thing

afterward.

Fix the problem by using the pytest session scope fixture with the unittest

tests. This is possible by adding @pytest.mark.usefixtures() decorators at the class

or method level:

**ch7/unittest/test_delete_unittest_fix.py
importpytest
importunittest
importtasks
fromtasksimport** Task

@pytest.mark.usefixtures( _'tasks_db_session'_ )
**class** TestNonEmpty(unittest.TestCase):

```
def setUp (self):
tasks.delete_all() # startempty
# add a few items,savingids
self.ids= []
self.ids.append(tasks.add(Task( 'One' , 'Brian' , True)))
self.ids.append(tasks.add(Task( 'Two' , 'StillBrian' , False)))
self.ids.append(tasks.add(Task( 'Three' , 'NotBrian' , False)))
```
Chapter 7. Using pytest with Other Tools • 152


**def test_delete_decreases_count** (self):
_# GIVEN3 items_
self.assertEqual(tasks.count(),3)
_# WHENwe deleteone_
tasks.delete(self.ids[0])
_# THENcountdecreasesby 1_
self.assertEqual(tasks.count(),2)

Now both unittest and pytest can cooperate and not run into each other:

```
**$ pytest-v test_delete_pytest.pytest_delete_unittest_fix.py**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected2 items

test_delete_pytest.py::test_delete_decreases_countPASSED[ 50%]
test_delete_unittest_fix.py::TestNonEmpty::test_delete_decreases_countPASSED[100%]

================2 passedin 0.03seconds=================

Note that this is only necessary for session scope resources shared by unittest

and pytest. As discussed earlier in Marking Test Functions, on page 31, you

can also use pytest markers on unittest tests, such as @pytest.mark.skip() and

@pytest.mark.xfail(), and user markers like @pytest.mark.foo().

Going back to the unittest example, we still used setUp() to save the ids of the

tasks. Aside from highlighting that getting a list of ids from tasks is obviously

an overlooked API method, it also points to a slight issue with using

pytst.mark.usefixtures with unittest: we can’t pass data from a fixture to a unittest

function directly.

However, you can pass it through the cls object that is part of the request object.

In the next example, setUp() code has been moved into a function scope fixture

that passes the ids through request.cls.ids:

**ch7/unittest/test_delete_unittest_fix2.py
importpytest
importunittest
importtasks
fromtasksimport** Task

@pytest.fixture()
**def tasks_db_non_empty** (tasks_db_session,request):
tasks.delete_all() _# startempty
# add a few items,savingids_
ids = []
ids.append(tasks.add(Task( _'One'_ , _'Brian'_ , True)))
ids.append(tasks.add(Task( _'Two'_ , _'StillBrian'_ , False)))
ids.append(tasks.add(Task( _'Three'_ , _'NotBrian'_ , False)))
request.cls.ids= ids

unittest: Running Legacy Tests with pytest • 153


@pytest.mark.usefixtures( _'tasks_db_non_empty'_ )
**class** TestNonEmpty(unittest.TestCase):

```
def test_delete_decreases_count (self):
# GIVEN3 items
self.assertEqual(tasks.count(),3)
# WHENwe deleteone
tasks.delete(self.ids[0])
# THENcountdecreasesby 1
self.assertEqual(tasks.count(),2)
```
The test accesses the ids list through self.ids, just like before.

The ability to use marks has a limitation: you cannot use parametrized fixtures

with unittest-based tests. However, when looking at the last example with

unittest using pytest fixtures, it’s not that far from rewriting it in pytest form.

Remove the unittest.TestCase base class and change the self.assertEqual() calls to

straight assert calls, and you’d be there.

Another limitation with running unittest with pytest is that unittest subtests

will stop at the first failure, while unittest will run each subtest, regardless

of failures. When all subtests pass, pytest runs all of them. Because you won’t

see any false-positive results because of this limitation, I consider this a minor

difference.

### Exercises

1. The test code in ch2 has a few intentionally failing tests. Use --pdb while

```
running these tests. Try it without the -x option and the debugger will
open multiple times, once for each failure.
```
2. Try fixing the code and rerunning tests with --lf --pdb to just run the failed

```
tests and use the debugger. Trying out debugging tools in a casual envi-
ronment where you can play around and not be worried about deadlines
and fixes is important.
```
3. We noticed lots of missing tests during our coverage exploration. One

```
topic missing is to test tasks.update(). Write some tests of that in the func
directory.
```
4. Run coverage.py. What other tests are missing? If you covered api.py, do you

think it would be fully tested?

5. Add some tests to test_cli.py to check the command-line interface for tasks

update using mock.

6. Run your new tests (along with all the old ones) against at least two Python

versions with tox.

7. Try using Jenkins to graph all the different tasks_proj versions and test

permutations in the chapters.

Chapter 7. Using pytest with Other Tools • 154


### What’s Next

You are definitely ready to go out and try pytest with your own projects. And

check out the appendixes that follow. If you’ve made it this far, I’ll assume

you no longer need help with pip or virtual environments. However, you may

not have looked at Appendix 3, Plugin Sampler Pack, on page 163. If you

enjoyed this chapter, it deserves your time to at least skim through it. Then,

Appendix 4, Packaging and Distributing Python Projects, on page 175 provides

a quick look at how to share code through various levels of packaging, and

Appendix 5, xUnit Fixtures, on page 183 covers an alternative style of pytest

fixtures that closer resembles traditional xUnit testing tools.

Also, keep in touch! Check out the book’s webpage^11 and use the discussion

forum^12 and errata^13 pages to help me keep the book lean, relevant, and easy

to follow. This book is intended to be a living document. I want to keep it up

to date and relevant for every wave of new pytest users.

11.https://pragprog.com/titles/bopytest
12.https://forums.pragprog.com/forums/438
13.https://pragprog.com/titles/bopytest/errata

What’s Next • 155
