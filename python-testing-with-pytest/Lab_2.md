

CHAPTER 2

Writing Test Functions

In the last chapter, you got pytest up and running. You saw how to run it

against files and directories and how many of the options worked. In this

chapter, you’ll learn how to write test functions in the context of testing a

Python package. If you’re using pytest to test something other than a Python

package, most of this chapter still applies.

We’re going to write tests for the Tasks package. Before we do that, I’ll talk

about the structure of a distributable Python package and the tests for it,

and how to get the tests able to see the package under test. Then I’ll show

you how to use assert in tests, how tests handle unexpected exceptions, and

testing for expected exceptions.

Eventually, we’ll have a lot of tests. Therefore, you’ll learn how to organize

tests into classes, modules, and directories. I’ll then show you how to use

markers to mark which tests you want to run and discuss how builtin markers

can help you skip tests and mark tests as expecting to fail. Finally, I’ll cover

parametrizing tests, which allows tests to get called with different data.

### Testing a Package

We’ll use the sample project, Tasks, as discussed in The Tasks Project, on

page xii, to see how to write test functions for a Python package. Tasks is a

Python package that includes a command-line tool of the same name, tasks.

Appendix 4, Packaging and Distributing Python Projects, on page 175 includes

an explanation of how to distribute your projects locally within a small team

or globally through PyPI, so I won’t go into detail of how to do that here;

however, let’s take a quick look at what’s in the Tasks project and how the

different files fit into the story of testing this project.


Following is the file structure for the Tasks project:

tasks_proj/
├──CHANGELOG.rst
├──LICENSE
├──MANIFEST.in
├──README.rst
├──setup.py
├──src
│ └──tasks
│ ├──__init__.py
│ ├──api.py
│ ├──cli.py
│ ├──config.py
│ ├──tasksdb_pymongo.py
│ └──tasksdb_tinydb.py
└──tests
├──conftest.py
├──pytest.ini
├──func
│ ├──__init__.py
│ ├──test_add.py
│ └──...
└──unit
├──__init__.py
├──test_task.py
└──...

I included the complete listing of the project (with the exception of the full

list of test files) to point out how the tests fit in with the rest of the project,

and to point out a few files that are of key importance to testing, namely con-

ftest.py, pytest.ini, the various __init__.py files, and setup.py.

All of the tests are kept in tests and separate from the package source files in

src. This isn’t a requirement of pytest, but it’s a best practice.

All of the top-level files, CHANGELOG.rst, LICENSE, README.rst, MANIFEST.in, and setup.py,

are discussed in more detail in Appendix 4, Packaging and Distributing Python

Projects, on page 175. Although setup.py is important for building a distribution

out of a package, it’s also crucial for being able to install a package locally so

that the package is available for import.

Functional and unit tests are separated into their own directories. This is an

arbitrary decision and not required. However, organizing test files into multiple

directories allows you to easily run a subset of tests. I like to keep functional

and unit tests separate because functional tests should only break if we are

intentionally changing functionality of the system, whereas unit tests could

break during a refactoring or an implementation change.

Chapter 2. Writing Test Functions • 24


The project contains two types of __init__.py files: those found under the src/

directory and those found under tests/. The src/tasks/__init__.py file tells Python

that the directory is a package. It also acts as the main interface to the

package when someone uses importtasks. It contains code to import specific

functions from api.py so that cli.py and our test files can access package func-

tionality like tasks.add() instead of having to do tasks.api.add().

The tests/func/__init__.py and tests/unit/__init__.py files are empty. They are optional,

but do allow you to have duplicate test file names in multiple test directories.

This is discussed in full in Avoiding Filename Collisions, on page 123.

The pytest.ini file is optional. It contains project-wide pytest configuration. There

should be at most only one of these in your project. It can contain directives

that change the behavior of pytest, such as setting up a list of options that

will always be used. You’ll learn all about pytest.ini in Chapter 6, Configuration,

on page 115.

The conftest.py file is also optional. It is considered by pytest as a “local plugin”

and can contain hook functions and fixtures. Hook functions are a way to

insert code into part of the pytest execution process to alter how pytest works.

Fixtures are setup and teardown functions that run before and after test

functions, and can be used to represent resources and data used by the

tests. (Fixtures are discussed in Chapter 3, pytest Fixtures, on page 51 and

Chapter 4, Builtin Fixtures, on page 73, and hook functions are discussed

in Chapter 5, Plugins, on page 97.) Hook functions and fixtures that are used

by tests in multiple subdirectories should be contained in tests/conftest.py. You

can have multiple conftest.py files; for example, you can have one at tests and

one for each subdirectory under tests.

If you haven’t already done so, you can download a copy of the source code

for this project on the book’s website.^1 Alternatively, you can work on your

own project with a similar structure.

**Installing a Package Locally**

The test file, tests/unit/test_task.py, contains the tests we worked on in Running

pytest, on page 4, in files test_three.py and test_four.py. I’ve just renamed it here

to something that makes more sense for what it’s testing and copied everything

into one file. I also removed the definition of the Task data structure, because

that really belongs in api.py.

1. https://pragprog.com/titles/bopytest/source_code

Testing a Package • 25


Here is test_task.py:

**ch2/tasks_proj/tests/unit/test_task.py**
_"""Testthe Taskdatatype."""_
**fromtasksimport** Task

**def test_asdict** ():
_"""_asdict()shouldreturna dictionary."""_
t_task= Task( _'do something'_ , _'okken'_ , True,21)
t_dict= t_task._asdict()
expected= { _'summary'_ : _'do something'_ ,
_'owner'_ : _'okken'_ ,
_'done'_ : True,
_'id'_ : 21}
**assert** t_dict== expected

**def test_replace** ():
_"""replace()shouldchangepassedin fields."""_
t_before= Task( _'finishbook'_ , _'brian'_ , False)
t_after= t_before._replace(id=10,done=True)
t_expected= Task( _'finishbook'_ , _'brian'_ , True,10)
**assert** t_after== t_expected

**def test_defaults** ():
_"""Usingno parametersshouldinvokedefaults."""_
t1 = Task()
t2 = Task(None,None,False,None)
**assert** t1 == t2

**def test_member_access** ():
_"""Check.fieldfunctionalityof namedtuple."""_
t = Task( _'buymilk'_ , _'brian'_ )
**assert** t.summary== _'buymilk'_
**assert** t.owner== _'brian'_
**assert** (t.done,t.id)== (False,None)

The test_task.py file has this import statement:

**fromtasksimport** Task

The best way to allow the tests to be able to importtasks or fromtasksimportsomething

is to install tasks locally using pip. This is possible because there’s a setup.py

file present to direct pip.

Install tasks either by running pip install. or pip install -e. from the tasks_proj direc-

tory. Or you can run pip install -e tasks_proj from one directory up:

**$ cd /path/to/code
$ pip install./tasks_proj/**
Processing./tasks_proj
Collectingclick(fromtasks==0.1.0)

Chapter 2. Writing Test Functions • 26


Downloading... click-6.7-py2.py3-none-any.whl
Collectingtinydb(fromtasks==0.1.0)
Downloading... tinydb-3.11.0-py2.py3-none-any.whl
Requirementalreadysatisfied:six in
./venv/lib/python3.7/site-packages(fromtasks==0.1.0)(1.11.0)
Installingcollectedpackages:click,tinydb,tasks
Runningsetup.pyinstallfor tasks... done
Successfullyinstalledclick-6.7tasks-0.1.0tinydb-3.11.0

If you only want to run tests against tasks, this command is fine. If you want

to be able to modify the source code while tasks is installed, you need to install

it with the -e option (for “editable”):

**$ pip install-e ./tasks_proj/**
Obtainingfile:///path/to/code/tasks_proj
Requirementalreadysatisfied:clickin
/path/to/venv/lib/python3.7/site-packages(fromtasks==0.1.0)
Requirementalreadysatisfied:tinydbin
/path/to/venv/lib/python3.7/site-packages(fromtasks==0.1.0)
Requirementalreadysatisfied:six in
/path/to/venv/lib/python3.7/site-packages(fromtasks==0.1.0)
Installingcollectedpackages:tasks
Runningsetup.pydevelopfor tasks
Successfullyinstalledtasks

We also need to install pytest (if you haven’t already done so):

**$ pip installpytest**

Now let’s try running tests:

**$ cd /path/to/code/ch2/tasks_proj/tests/unit
$ pytesttest_task.py**
===================testsessionstarts===================
collected4 items

test_task.py.... [100%]

================4 passedin 0.02seconds=================

The import worked! The rest of our tests can now safely use importtasks. Now

let’s write some tests.

### Using assert Statements

When you write test functions, the normal Python assert statement is your

primary tool to communicate test failure. The simplicity of this within pytest

is brilliant. It’s what drives a lot of developers to use pytest over other

frameworks.

Using assert Statements • 27


If you’ve used any other testing framework, you’ve probably seen various assert

helper functions. For example, the following is a list of a few of the assert forms

and assert helper functions:

```
pytest unittest
```
assertsomething assertTrue(something)

asserta == b assertEqual(a,b)

asserta <= b assertLessEqual(a,b)

... ...

With pytest, you can use assert<expression> with any expression. If the expres-

sion would evaluate to False if converted to a bool, the test would fail.

pytest includes a feature called assert rewriting that intercepts assert calls and

replaces them with something that can tell you more about why your asser-

tions failed. Let’s see how helpful this rewriting is by looking at a few assertion

failures:

**ch2/tasks_proj/tests/unit/test_task_fail.py**
_"""Usethe Tasktypeto showtestfailures."""_
**fromtasksimport** Task

**def test_task_equality** ():
_"""Differenttasksshouldnot be equal."""_
t1 = Task( _'sitthere'_ , _'brian'_ )
t2 = Task( _'do something'_ , _'okken'_ )
**assert** t1 == t2

**def test_dict_equality** ():
_"""Differenttaskscomparedas dicts shouldnot be equal."""_
t1_dict= Task( _'makesandwich'_ , _'okken'_ )._asdict()
t2_dict= Task( _'makesandwich'_ , _'okkem'_ )._asdict()
**assert** t1_dict== t2_dict

All of these tests fail, but what’s interesting is the traceback information:

**$ cd /path/to/code/ch2/tasks_proj/tests/unit**
venv)$ pytesttest_task_fail.py
===================testsessionstarts===================
collected2 items

test_task_fail.pyFF [100%]

========================FAILURES=========================
___________________test_task_equality____________________

```
def test_task_equality():
"""Differenttasksshouldnot be equal."""
t1 = Task('sitthere','brian')
t2 = Task('dosomething','okken')
```
Chapter 2. Writing Test Functions • 28


**> assertt1 == t2**
E AssertionError:assertTask(summary=...alse,id=None)==
Task(summary='...alse,id=None)
E At index0 diff:'sitthere'!= 'do something'
E Use -v to get the fulldiff

test_task_fail.py:9:AssertionError
___________________test_dict_equality____________________

def test_dict_equality():
"""Differenttaskscomparedas dictsshouldnot be equal."""
t1_dict= Task('makesandwich','okken')._asdict()
t2_dict= Task('makesandwich','okkem')._asdict()
**> assertt1_dict== t2_dict**
E AssertionError:assertOrderedDict([...('id',None)])==
OrderedDict([(...('id',None)])
E Omitting3 identicalitems,use -vv to show
E Differingitems:
E {'owner':'okken'}!= {'owner':'okkem'}
E Use -v to get the fulldiff

test_task_fail.py:16:AssertionError
================2 failedin 0.07seconds=================

Wow. That’s a lot of information. For each failing test, the exact line of failure

is shown with a > pointing to the failure. The E lines show you extra informa-

tion about the assert failure to help you figure out what went wrong.

I intentionally put two mismatches in test_task_equality(), but only the first was

shown in the previous code. Let’s try it again with the -v flag, as suggested in

the error message:

**$ pytest-v test_task_fail.py::test_task_equality**
===================testsessionstarts===================
collected1 item

test_task_fail.py::test_task_equalityFAILED [100%]

========================FAILURES=========================
___________________test_task_equality____________________

def test_task_equality():
"""Differenttasksshouldnot be equal."""
t1 = Task('sitthere','brian')
t2 = Task('dosomething','okken')
**> assertt1 == t2**
E AssertionError:assertTask(summary=...alse,id=None)==
Task(summary='...alse,id=None)
E At index0 diff:'sitthere'!= 'do something'
E Fulldiff:
E - Task(summary='sitthere',owner='brian',done=False,id=None)
E? ^^^ ^^^ ^^^^
E + Task(summary='dosomething', owner='okken',done=False,id=None)

Using assert Statements • 29


#### E? +++ ^^^ ^^^ ^^^^

test_task_fail.py:9:AssertionError
================1 failedin 0.07seconds=================

Well, I think that’s pretty darned cool. pytest not only found both differences,

but it also showed us exactly where the differences are.

This example only used equality assert; many more varieties of assert statements

with awesome trace debug information are found on the pytest.org website.^2

### Expecting Exceptions

Exceptions may be raised in a few places in the Tasks API. Let’s take a quick

peek at the functions found in tasks/api.py:

**def add** (task): _# type:(Task)-> int_
**def get** (task_id): _# type:(int)-> Task_
**def list_tasks** (owner=None): _# type:(str|None)-> listof Task_
**def count** (): _# type:(None)-> int_
**def update** (task_id,task): _# type:(int,Task)-> None_
**def delete** (task_id): _# type:(int)-> None_
**def delete_all** (): _# type:() -> None_
**def unique_id** (): _# type:() -> int_
**def start_tasks_db** (db_path,db_type): _# type:(str,str)-> None_
**def stop_tasks_db** (): _# type:() -> None_

There’s an agreement between the CLI code in cli.py and the API code in api.py

as to what types will be sent to the API functions. These API calls are a place

where I’d expect exceptions to be raised if the type is wrong.

To make sure these functions raise exceptions if called incorrectly, let’s use

the wrong type in a test function to intentionally cause TypeError exceptions,

and use with pytest.raises(<expectedexception>), like this:

**ch2/tasks_proj/tests/func/test_api_exceptions.py
importpytest
importtasks**

**def test_add_raises** ():
_"""add()shouldraisean exceptionwithwrongtypeparam."""_
**with** pytest.raises(TypeError):
tasks.add(task= _'nota Taskobject'_ )

In test_add_raises(), the with pytest.raises(TypeError): statement says that whatever is

in the next block of code should raise a TypeError exception. If no exception is

raised, the test fails. If the test raises a different exception, it fails.

2. [http://doc.pytest.org/en/latest/example/reportingdemo.html](http://doc.pytest.org/en/latest/example/reportingdemo.html)

Chapter 2. Writing Test Functions • 30


We just checked for the type of exception in test_add_raises(). You can also check

the parameters to the exception. For start_tasks_db(db_path,db_type), not only does

db_type need to be a string, it really has to be either 'tiny' or 'mongo'. You can

check to make sure the exception message is correct by adding as excinfo:

**ch2/tasks_proj/tests/func/test_api_exceptions.py
def test_start_tasks_db_raises** ():
_"""Makesureunsupporteddb raisesan exception."""_
**with** pytest.raises(ValueError) **as** excinfo:
tasks.start_tasks_db( _'some/great/path'_ , _'mysql'_ )
exception_msg= excinfo.value.args[0]
**assert** exception_msg== _"db_typemustbe a 'tiny'or 'mongo'"_

This allows us to look at the exception more closely. The variable name you

put after as (excinfo in this case) is filled with information about the exception,

and is of type ExceptionInfo.

In our case, we want to make sure the first (and only) parameter to the

exception matches a string.

### Marking Test Functions

pytest provides a cool mechanism to let you put markers on test functions.

A test can have more than one marker, and a marker can be on multiple

tests.

Markers make sense after you see them in action. Let’s say we want to run

a subset of our tests as a quick “smoke test” to get a sense for whether or not

there is some major break in the system. Smoke tests are by convention not

all-inclusive, thorough test suites, but a select subset that can be run

quickly and give a developer a decent idea of the health of all parts of the

system.

To add a smoke test suite to the Tasks project, we can add @pytest.mark.smoke

to some of the tests. Let’s add it to a couple of tests in test_api_exceptions.py (note

that the markers smoke and get aren’t built into pytest; I just made them up):

**ch2/tasks_proj/tests/func/test_api_exceptions.py**
@pytest.mark.smoke
**def test_list_raises** ():
_"""list()shouldraisean exceptionwithwrongtypeparam."""_
**with** pytest.raises(TypeError):
tasks.list_tasks(owner=123)

@pytest.mark.get
@pytest.mark.smoke
**def test_get_raises** ():
_"""get()shouldraisean exceptionwithwrongtypeparam."""_

Marking Test Functions • 31


```
with pytest.raises(TypeError):
tasks.get(task_id= '123' )
```
Now, let’s run just those tests that are marked with -m marker_name:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v -m smoketest_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 5 deselected

test_api_exceptions.py::test_list_raisesPASSED [ 50%]
test_api_exceptions.py::test_get_raisesPASSED [100%]

=========2 passed,5 deselectedin 0.03seconds==========
**$ pytest-v -m get test_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 6 deselected

test_api_exceptions.py::test_get_raisesPASSED [100%]

=========1 passed,6 deselectedin 0.02seconds==========

Remember that -v is short for --verbose and lets us see the names of the tests

that are run. Using -m 'smoke' runs both tests marked with @pytest.mark.smoke.

Using -m 'get' runs the one test marked with @pytest.mark.get. Pretty straight-

forward.

It gets better. The expression after -m can use and, or, and not to combine

multiple markers:

**$ pytest-v -m** _"smokeand get"_ **test_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 6 deselected

test_api_exceptions.py::test_get_raisesPASSED [100%]

=========1 passed,6 deselectedin 0.02seconds==========

That time we only ran the test that had both smoke and get markers. We can

use not as well:

**$ pytest-v -m** _"smokeand not get"_ **test_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 6 deselected

test_api_exceptions.py::test_list_raisesPASSED [100%]

=========1 passed,6 deselectedin 0.02seconds==========

The addition of -m "smoke and not get" selected the test that was marked with

@pytest.mark.smoke but not @pytest.mark.get.

Chapter 2. Writing Test Functions • 32


**Filling Out the Smoke Test**

The previous tests don’t seem like a reasonable smoke test suite yet. We

haven’t actually touched the database or added any tasks. Surely a smoke

test would do that.

Let’s add a couple of tests that look at adding a task, and use one of them as

part of our smoke test suite:

**ch2/tasks_proj/tests/func/test_add.py
importpytest
importtasks
fromtasksimport** Task

**def test_add_returns_valid_id** ():
_"""tasks.add(<validtask>)shouldreturnan integer."""
# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint_
new_task= Task( _'do something'_ )
task_id= tasks.add(new_task)
**assert** isinstance(task_id,int)

@pytest.mark.smoke
**def test_added_task_has_id_set** ():
_"""Makesurethe task_idfield is set by tasks.add()."""
# GIVENan initializedtasksdb
# AND a new taskis added_
new_task= Task( _'sitin chair'_ , owner= _'me'_ , done=True)
task_id= tasks.add(new_task)

```
# WHENtaskis retrieved
task_from_db= tasks.get(task_id)
# THENtask_idmatchesid field
assert task_from_db.id== task_id
```
Both of these tests have the comment GIVENan initializedtasks db, and yet there

is no database initialized in the test. We can define a fixture to get the database

initialized before the test and cleaned up after the test:

**ch2/tasks_proj/tests/func/test_add.py**
@pytest.fixture(autouse=True)
**def initialized_tasks_db** (tmpdir):
_"""Connectto db beforetesting,disconnectafter."""
# Setup: startdb_
tasks.start_tasks_db(str(tmpdir), _'tiny'_ )
**yield** _# thisis wherethe testinghappens
# Teardown: stopdb_
tasks.stop_tasks_db()

Marking Test Functions • 33


The fixture, tmpdir, used in this example is a builtin fixture. You’ll learn all

about builtin fixtures in Chapter 4, Builtin Fixtures, on page 73, and you’ll

learn about writing your own fixtures and how they work in Chapter 3, pytest

Fixtures, on page 51, including the autouse parameter used here.

autouse as used in our test indicates that all tests in this file will use the fixture.

The code before the yield runs before each test; the code after the yield runs

after the test. The yield can return data to the test if desired. You’ll look at all

that and more in later chapters, but here we need some way to set up the

database for testing, so I couldn’t wait any longer to show you a fixture. (pytest

also supports old-fashioned setup and teardown functions, like what is used

in unittest and nose, but they are not nearly as fun. However, if you are

curious, they are described in Appendix 5, xUnit Fixtures, on page 183 .)

Let’s set aside fixture discussion for now and go to the top of the project and

run our smoke test suite:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest-v -m smoke**
===================testsessionstarts===================
collected56 items/ 53 deselected

tests/func/test_add.py::test_added_task_has_id_setPASSED[ 33%]
tests/func/test_api_exceptions.py::test_list_raisesPASSED[ 66%]
tests/func/test_api_exceptions.py::test_get_raisesPASSED[100%]

=========3 passed,53 deselectedin 0.14seconds=========

This shows that marked tests from different files can all run together.

### Skipping Tests

While the markers discussed in Marking Test Functions, on page 31 were

names of your own choosing, pytest includes a few helpful builtin markers:

skip, skipif, and xfail. I’ll discuss skip and skipif in this section, and xfail in the next.

The skip and skipif markers enable you to skip tests you don’t want to run. For

example, let’s say we weren’t sure how tasks.unique_id() was supposed to work.

Does each call to it return a different number? Or is it just a number that

doesn’t exist in the database already?

First, let’s write a test (note that the initialized_tasks_db fixture is in this file, too;

it’s just not shown here):

**ch2/tasks_proj/tests/func/test_unique_id_1.py
importpytest
importtasks**

**def test_unique_id** ():

Chapter 2. Writing Test Functions • 34


```
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
assert id_1!= id_2
```
Then give it a run:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytesttest_unique_id_1.py**
===================testsessionstarts===================
collected1 item

test_unique_id_1.pyF [100%]

========================FAILURES=========================
_____________________test_unique_id______________________

def test_unique_id():
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**> assertid_1!= id_2**
E assert1 != 1

test_unique_id_1.py:12:AssertionError
================1 failedin 0.10seconds=================

Hmm. Maybe we got that wrong. After looking at the API a bit more, we see

that the docstring says """Return an integerthat does not exist in the db.""".

We could just change the test. But instead, let’s just mark the first one to get

skipped for now:

**ch2/tasks_proj/tests/func/test_unique_id_2.py**
@pytest.mark.skip(reason= _'misunderstoodthe API'_ )
**def test_unique_id_1** ():
_"""Callingunique_id()twiceshouldreturndifferentnumbers."""_
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

**def test_unique_id_2** ():
_"""unique_id()shouldreturnan unusedid."""_
ids = []
ids.append(tasks.add(Task( _'one'_ )))
ids.append(tasks.add(Task( _'two'_ )))
ids.append(tasks.add(Task( _'three'_ )))
_# graba uniqueid_
uid = tasks.unique_id()
_# makesureit isn'tin the listof existingids_
**assert** uid **not in** ids

Skipping Tests • 35


Marking a test to be skipped is as simple as adding @pytest.mark.skip() just above

the test function.

Let’s run again:

**$ pytest-v test_unique_id_2.py**
===================testsessionstarts===================
collected2 items

test_unique_id_2.py::test_unique_id_1SKIPPED [ 50%]
test_unique_id_2.py::test_unique_id_2PASSED [100%]

===========1 passed,1 skippedin 0.03seconds===========

Now, let’s say that for some reason we decide the first test should be valid

also, and we intend to make that work in version 0.2.0 of the package. We

can leave the test in place and use skipif instead:

**ch2/tasks_proj/tests/func/test_unique_id_3.py**
@pytest.mark.skipif(tasks.__version__< _'0.2.0'_ ,
reason= _'notsupporteduntilversion0.2.0'_ )
**def test_unique_id_1** ():
_"""Callingunique_id()twiceshouldreturndifferentnumbers."""_
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

The expression we pass into skipif() can be any valid Python expression. In this

case, we’re checking the package version.

We included reasons in both skip and skipif. It’s not required in skip, but it is

required in skipif. I like to include a reason for every skip, skipif, or xfail.

Here’s the output of the changed code:

**$ pytesttest_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.pys. [100%]

===========1 passed,1 skippedin 0.03seconds===========

The s. shows that one test was skipped and one test passed.

We can see which one with -v:

**$ pytest-v test_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.py::test_unique_id_1SKIPPED [ 50%]
test_unique_id_3.py::test_unique_id_2PASSED [100%]

===========1 passed,1 skippedin 0.03seconds===========

Chapter 2. Writing Test Functions • 36


But we still don’t know why. We can see those reasons with -rs:

**$ pytest-rs test_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.pys. [100%]
=================shorttestsummaryinfo=================
SKIP[1] test_unique_id_3.py:9:not supporteduntilversion0.2.0

===========1 passed,1 skippedin 0.04seconds===========

The -r chars option has this help text:

**$ pytest--help**

```
-r chars
```
```
showextratestsummaryinfoas specifiedby chars
(f)ailed,(E)error,(s)skipped,(x)failed,(X)passed,
(p)passed,(P)passedwithoutput,(a)allexceptpP.
```
It’s not only helpful for understanding test skips, but also you can use it for

other test outcomes as well.

### Marking Tests as Expecting to Fail

With the skip and skipif markers, a test isn’t even attempted if skipped. With

the xfail marker, we are telling pytest to run a test function, but that we expect

it to fail. Let’s modify our unique_id() test again to use xfail:

**ch2/tasks_proj/tests/func/test_unique_id_4.py**
@pytest.mark.xfail(tasks.__version__< _'0.2.0'_ ,
reason= _'notsupporteduntilversion0.2.0'_ )
**def test_unique_id_1** ():
_"""Callingunique_id()twiceshouldreturndifferentnumbers."""_
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

@pytest.mark.xfail()
**def test_unique_id_is_a_duck** ():
_"""Demonstratexfail."""_
uid = tasks.unique_id()
**assert** uid == _'a duck'_

@pytest.mark.xfail()
**def test_unique_id_not_a_duck** ():
_"""Demonstratexpass."""_
uid = tasks.unique_id()
**assert** uid != _'a duck'_

Marking Tests as Expecting to Fail • 37


The first test is the same as before, but with xfail. The next two tests are listed

as xfail, and differ only by == vs. !=. So one of them is bound to pass.

Running this shows:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytesttest_unique_id_4.py**
===================testsessionstarts===================
collected4 items

test_unique_id_4.pyxxX. [100%]

=====1 passed,2 xfailed,1 xpassedin 0.10seconds======

The x is for XFAIL, which means “expected to fail.” The capital X is for XPASS or

“expected to fail but passed.”

--verbose lists longer descriptions:

**$ pytest-v test_unique_id_4.py**
===================testsessionstarts===================
collected4 items

test_unique_id_4.py::test_unique_id_1xfail [ 25%]
test_unique_id_4.py::test_unique_id_is_a_duckxfail[ 50%]
test_unique_id_4.py::test_unique_id_not_a_duckXPASS[ 75%]
test_unique_id_4.py::test_unique_id_2PASSED [100%]

=====1 passed,2 xfailed,1 xpassedin 0.10seconds======

You can configure pytest to report the tests that pass but were marked with

xfail to be reported as FAIL. This is done in a pytest.ini file:

[pytest]
xfail_strict=true

I’ll discuss pytest.ini more in Chapter 6, Configuration, on page 115.

### Running a Subset of Tests

I’ve talked about how you can place markers on tests and run tests based on

markers. You can run a subset of tests in several other ways. You can run

all of the tests, or you can select a single directory, file, class within a file, or

an individual test in a file or class. You haven’t seen test classes used yet, so

you’ll look at one in this section. You can also use an expression to match

test names. Let’s take a look at these.

Chapter 2. Writing Test Functions • 38


**A Single Directory**

To run all the tests from one directory, use the directory as a parameter to

pytest:

**$ cd /path/to/code/ch2/tasks_proj
$ pytesttests/func--tb=no**
===================testsessionstarts===================
collected50 items

tests/func/test_add.py.. [ 4%]
tests/func/test_add_variety.py....................[ 44%]
............ [ 68%]
tests/func/test_api_exceptions.py....... [ 82%]
tests/func/test_unique_id_1.pyF [ 84%]
tests/func/test_unique_id_2.pys. [ 88%]
tests/func/test_unique_id_3.pys. [ 92%]
tests/func/test_unique_id_4.pyxxX. [100%]

```
1 failed,44 passed,2 skipped,2 xfailed,1 xpassedin 0.41seconds
```
An important trick to learn is that using -v gives you the syntax for how to

run a specific directory, class, and test.

**$ pytest-v tests/func--tb=no**
===================testsessionstarts===================
collected50 items

tests/func/test_add.py::test_add_returns_valid_idPASSED[ 2%]
tests/func/test_add.py::test_added_task_has_id_setPASSED[ 4%]
**...**
tests/func/test_api_exceptions.py::test_add_raisesPASSED[ 70%]
tests/func/test_api_exceptions.py::test_list_raisesPASSED[ 72%]
tests/func/test_api_exceptions.py::test_get_raisesPASSED[ 74%]
**...**
tests/func/test_unique_id_1.py::test_unique_idFAILED[ 84%]
tests/func/test_unique_id_2.py::test_unique_id_1SKIPPED[ 86%]
tests/func/test_unique_id_2.py::test_unique_id_2PASSED[ 88%]
**...**
tests/func/test_unique_id_4.py::test_unique_id_1xfail[ 94%]
tests/func/test_unique_id_4.py::test_unique_id_is_a_duckxfail[ 96%]
tests/func/test_unique_id_4.py::test_unique_id_not_a_duckXPASS[ 98%]
tests/func/test_unique_id_4.py::test_unique_id_2PASSED[100%]

```
1 failed,44 passed,2 skipped,2 xfailed,1 xpassedin 0.48seconds
```
You’ll see the syntax listed here in the next few examples.

Running a Subset of Tests • 39


**A Single Test File/Module**

To run a file full of tests, list the file with the relative path as a parameter to

pytest:

**$ cd /path/to/code/ch2/tasks_proj
$ pytesttests/func/test_add.py**
===================testsessionstarts===================
collected2 items

tests/func/test_add.py.. [100%]

================2 passedin 0.10seconds=================

We’ve been doing this for a while.

**A Single Test Function**

To run a single test function, add :: and the test function name:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest-v tests/func/test_add.py::test_add_returns_valid_id**
===================testsessionstarts===================
collected1 item

tests/func/test_add.py::test_add_returns_valid_idPASSED[100%]

================1 passedin 0.04seconds=================

Use -v so you can see which function was run.

**A Single Test Class**

Test classes are a way to group tests that make sense to be grouped together.

Here’s an example:

**ch2/tasks_proj/tests/func/test_api_exceptions.py
class** TestUpdate():
_"""Testexpectedexceptionswithtasks.update()."""_
**def test_bad_id** (self):
_"""Anon-intid shouldraisean excption."""_
**with** pytest.raises(TypeError):
tasks.update(task_id={ _'dictinstead'_ : 1},
task=tasks.Task())

```
def test_bad_task (self):
"""Anon-Tasktaskshouldraisean excption."""
with pytest.raises(TypeError):
tasks.update(task_id=1,task= 'nota task' )
```
Since these are two related tests that both test the update() function, it’s rea-

sonable to group them in a class. To run just this class, do like we did with

functions and add ::, then the class name to the file parameter:

Chapter 2. Writing Test Functions • 40


**$ cd /path/to/code/ch2/tasks_proj
$ pytest-v tests/func/test_api_exceptions.py::TestUpdate**
===================testsessionstarts===================
collected2 items

tests/func/test_api_exceptions.py::TestUpdate::test_bad_idPASSED[ 50%]
tests/func/test_api_exceptions.py::TestUpdate::test_bad_taskPASSED[100%]

================2 passedin 0.02seconds=================

**A Single Test Method of a Test Class**

If you don’t want to run all of a test class—just one method—just add

another :: and the method name:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest-v tests/func/test_api_exceptions.py::TestUpdate::test_bad_id**
===================testsessionstarts===================
collected1 item

tests/func/test_api_exceptions.py::TestUpdate::test_bad_idPASSED[100%]

================1 passedin 0.02seconds=================

```
Grouping Syntax Shown by Verbose Listing
Remember that the syntax for how to run a subset of tests by
directory, file, function, class, and method doesn’t have to be
memorized. The format is the same as the test function listing
when you run pytest -v.
```
**A Set of Tests Based on Test Name**

The -k option enables you to pass in an expression to run tests that have

certain names specified by the expression as a substring of the test name.

You can use and, or, and not in your expression to create complex expressions.

For example, we can run all of the functions that have _raises in their name:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest-v -k _raises**
===================testsessionstarts===================
collected56 items/ 51 deselected

tests/func/test_api_exceptions.py::test_add_raisesPASSED[ 20%]
tests/func/test_api_exceptions.py::test_list_raisesPASSED[ 40%]
tests/func/test_api_exceptions.py::test_get_raisesPASSED[ 60%]
tests/func/test_api_exceptions.py::test_delete_raisesPASSED[ 80%]
tests/func/test_api_exceptions.py::test_start_tasks_db_raisesPASSED[100%]

=========5 passed,51 deselectedin 0.13seconds=========

We can use and and not to get rid of the test_delete_raises() from the session:

Running a Subset of Tests • 41


**$ pytest-v -k** _"_raisesand not delete"_
===================testsessionstarts===================
collected56 items/ 52 deselected

tests/func/test_api_exceptions.py::test_add_raisesPASSED[ 25%]
tests/func/test_api_exceptions.py::test_list_raisesPASSED[ 50%]
tests/func/test_api_exceptions.py::test_get_raisesPASSED[ 75%]
tests/func/test_api_exceptions.py::test_start_tasks_db_raisesPASSED[100%]

=========4 passed,52 deselectedin 0.12seconds=========

In this section, you learned how to run specific test files, directories, classes,

and functions, and how to use expressions with -k to run specific sets of tests.

In the next section, you’ll learn how one test function can turn into many test

cases by allowing the test to run multiple times with different test data.

### Parametrized Testing

Sending some values through a function and checking the output to make sure

it’s correct is a common pattern in software testing. However, calling a function

once with one set of values and one check for correctness isn’t enough to fully

test most functions. Parametrized testing is a way to send multiple sets of data

through the same test and have pytest report if any of the sets failed.

To help understand the problem parametrized testing is trying to solve, let’s

take a simple test for add():

**ch2/tasks_proj/tests/func/test_add_variety.py
importpytest
importtasks
fromtasksimport** Task

**def test_add_1** ():
_"""tasks.get()usingid returnedfromadd()works."""_
task= Task( _'breathe'_ , _'BRIAN'_ , True)
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
_# everythingbut the id should be the same_
**assert** equivalent(t_from_db,task)

**def equivalent** (t1,t2):
_"""Checktwo tasksfor equivalence."""
# Compareeverythingbut the id field_
**return** ((t1.summary== t2.summary) **and**
(t1.owner== t2.owner) **and**
(t1.done== t2.done))

@pytest.fixture(autouse=True)
**def initialized_tasks_db** (tmpdir):
_"""Connectto db beforetesting,disconnectafter."""_

Chapter 2. Writing Test Functions • 42


```
tasks.start_tasks_db(str(tmpdir), 'tiny' )
yield
tasks.stop_tasks_db()
```
When a Task object is created, its id field is set to None. After it’s added and

retrieved from the database, the id field will be set. Therefore, we can’t just

use == to check to see if our task was added and retrieved correctly. The

equivalent() helper function checks all but the id field. The autouse fixture is

included to make sure the database is accessible. Let’s make sure the test

passes:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_1**
===================testsessionstarts===================
collected1 item

test_add_variety.py::test_add_1PASSED [100%]

================1 passedin 0.05seconds=================

The test seems reasonable. However, it’s just testing one example task. What

if we want to test lots of variations of a task? No problem. We can use

@pytest.mark.parametrize(argnames,argvalues) to pass lots of data through the same

test, like this:

**ch2/tasks_proj/tests/func/test_add_variety.py**
@pytest.mark.parametrize( _'task'_ ,
[Task( _'sleep'_ , done=True),
Task( _'wake'_ , _'brian'_ ),
Task( _'breathe'_ , _'BRIAN'_ , True),
Task( _'exercise'_ , _'BrIaN'_ , False)])
**def test_add_2** (task):
_"""Demonstrateparametrizewithone parameter."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

The first argument to parametrize() is a string with a comma-separated list of

names—'task', in our case. The second argument is a list of values, which in

our case is a list of Task objects. pytest will run this test once for each task

and report each as a separate test:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_2**
===================testsessionstarts===================
collected4 items

test_add_variety.py::test_add_2[task0]PASSED [ 25%]
test_add_variety.py::test_add_2[task1]PASSED [ 50%]
test_add_variety.py::test_add_2[task2]PASSED [ 75%]
test_add_variety.py::test_add_2[task3]PASSED [100%]

Parametrized Testing • 43


================4 passedin 0.05seconds=================

This use of parametrize() works for our purposes. However, let’s pass in the

tasks as tuples to see how multiple test parameters would work:

**ch2/tasks_proj/tests/func/test_add_variety.py**
@pytest.mark.parametrize( _'summary,owner,done'_ ,
[( _'sleep'_ , None,False),
( _'wake'_ , _'brian'_ , False),
( _'breathe'_ , _'BRIAN'_ , True),
( _'eateggs'_ , _'BrIaN'_ , False),
])
**def test_add_3** (summary,owner,done):
_"""Demonstrateparametrizewithmultipleparameters."""_
task= Task(summary,owner,done)
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

When you use types that are easy for pytest to convert into strings, the test

identifier uses the parameter values in the report to make it readable:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_3**
===================testsessionstarts===================
collected4 items

test_add_variety.py::test_add_3[sleep-None-False]PASSED[ 25%]
test_add_variety.py::test_add_3[wake-brian-False]PASSED[ 50%]
test_add_variety.py::test_add_3[breathe-BRIAN-True]PASSED[ 75%]
test_add_variety.py::test_add_3[eateggs-BrIaN-False]PASSED[100%]

================4 passedin 0.05seconds=================

You can use that whole test identifier—called a node in pytest terminology—to

re-run the test if you want:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_3[sleep-None-False]**
===================testsessionstarts===================
collected1 item

test_add_variety.py::test_add_3[sleep-None-False]PASSED[100%]

================1 passedin 0.03seconds=================

Be sure to use quotes if there are spaces in the identifier:

Chapter 2. Writing Test Functions • 44


**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v** _"test_add_variety.py::test_add_3[eateggs-BrIaN-False]"_
===================testsessionstarts===================
collected1 item

test_add_variety.py::test_add_3[eateggs-BrIaN-False]PASSED[100%]

================1 passedin 0.03seconds=================

Now let’s go back to the list of tasks version, but move the task list to a vari-

able outside the function:

**ch2/tasks_proj/tests/func/test_add_variety.py**
tasks_to_try= (Task( _'sleep'_ , done=True),
Task( _'wake'_ , _'brian'_ ),
Task( _'wake'_ , _'brian'_ ),
Task( _'breathe'_ , _'BRIAN'_ , True),
Task( _'exercise'_ , _'BrIaN'_ , False))

@pytest.mark.parametrize( _'task'_ , tasks_to_try)
**def test_add_4** (task):
_"""Slightlydifferenttake."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

It’s convenient and the code looks nice. But the readability of the output is

hard to interpret:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_4**
===================testsessionstarts===================
collected5 items

test_add_variety.py::test_add_4[task0]PASSED [ 20%]
test_add_variety.py::test_add_4[task1]PASSED [ 40%]
test_add_variety.py::test_add_4[task2]PASSED [ 60%]
test_add_variety.py::test_add_4[task3]PASSED [ 80%]
test_add_variety.py::test_add_4[task4]PASSED [100%]

================5 passedin 0.06seconds=================

The readability of the multiple parameter version is nice, but so is the list of

Task objects. To compromise, we can use the ids optional parameter to

parametrize() to make our own identifiers for each task data set. The ids param-

eter needs to be a list of strings the same length as the number of data sets.

However, because we assigned our data set to a variable name, tasks_to_try, we

can use it to generate ids:

Parametrized Testing • 45


**ch2/tasks_proj/tests/func/test_add_variety.py**
task_ids= [ _'Task({},{},{})'_ .format(t.summary,t.owner,t.done)
**for** t **in** tasks_to_try]

@pytest.mark.parametrize( _'task'_ , tasks_to_try,ids=task_ids)
**def test_add_5** (task):
_"""Demonstrateids."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

Let’s run that and see how it looks:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_5**
===================testsessionstarts===================
collected5 items

test_add_variety.py::test_add_5[Task(sleep,None,True)]PASSED[ 20%]
test_add_variety.py::test_add_5[Task(wake,brian,False)0]PASSED[ 40%]
test_add_variety.py::test_add_5[Task(wake,brian,False)1]PASSED[ 60%]
test_add_variety.py::test_add_5[Task(breathe,BRIAN,True)]PASSED[ 80%]
test_add_variety.py::test_add_5[Task(exercise,BrIaN,False)]PASSED[100%]

================5 passedin 0.06seconds=================

Note that the second and third tasks are actually duplicates of eachother and

generate the same task id. To be able to tell them apart, pytest added a unique

index to each, 0 and 1. The custom test identifiers can be used to run tests:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v** _"test_add_variety.py::test_add_5[Task(exercise,BrIaN,False)]"_
===================testsessionstarts===================
collected1 item

test_add_variety.py::test_add_5[Task(exercise,BrIaN,False)]PASSED[100%]

================1 passedin 0.05seconds=================

We definitely need quotes for these identifiers; otherwise, the brackets and

parentheses will confuse the shell.

You can apply parametrize() to classes as well. When you do that, the same data

sets will be sent to all test methods in the class:

**ch2/tasks_proj/tests/func/test_add_variety.py**
@pytest.mark.parametrize( _'task'_ , tasks_to_try,ids=task_ids)
**class** TestAdd():
_"""Demonstrateparametrizeand testclasses."""_
**def test_equivalent** (self,task):
_"""Similartest,justwithina class."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)

Chapter 2. Writing Test Functions • 46


**assert** equivalent(t_from_db,task)

Parametrized Testing • 47


**def test_valid_id** (self,task):
_"""Wecan use the samedatafor multipletests."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** t_from_db.id== task_id

Here it is in action:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::TestAdd**
===================testsessionstarts===================
collected10 items

test_add_variety.py::TestAdd::test_equivalent[Task(sleep,None,True)]PASSED
test_add_variety.py::TestAdd::test_equivalent[Task(wake,brian,False)0]PASSED
test_add_variety.py::TestAdd::test_equivalent[Task(wake,brian,False)1]PASSED
test_add_variety.py::TestAdd::test_equivalent[Task(breathe,BRIAN,True)]PASSED
test_add_variety.py::TestAdd::test_equivalent[Task(exercise,BrIaN,False)]PASSED
test_add_variety.py::TestAdd::test_valid_id[Task(sleep,None,True)]PASSED
test_add_variety.py::TestAdd::test_valid_id[Task(wake,brian,False)0]PASSED
test_add_variety.py::TestAdd::test_valid_id[Task(wake,brian,False)1]PASSED
test_add_variety.py::TestAdd::test_valid_id[Task(breathe,BRIAN,True)]PASSED
test_add_variety.py::TestAdd::test_valid_id[Task(exercise,BrIaN,False)]PASSED

================10 passedin 0.10seconds================

You can also identify parameters by including an id right alongside the

parameter value when passing in a list within the @pytest.mark.parametrize()

decorator. You do this with pytest.param(<value>,id="something") syntax:

**ch2/tasks_proj/tests/func/test_add_variety.py**
@pytest.mark.parametrize( _'task'_ , [
pytest.param(Task( _'create'_ ), id= _'justsummary'_ ),
pytest.param(Task( _'inspire'_ , _'Michelle'_ ), id= _'summary/owner'_ ),
pytest.param(Task( _'encourage'_ , _'Michelle'_ , True),id= _'summary/owner/done'_ )])
**def test_add_6** (task):
_"""Demonstratepytest.paramand id."""_
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

In action:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest-v test_add_variety.py::test_add_6**
===================testsessionstarts===================
collected3 items

test_add_variety.py::test_add_6[justsummary]PASSED[ 33%]
test_add_variety.py::test_add_6[summary/owner]PASSED[ 66%]
test_add_variety.py::test_add_6[summary/owner/done]PASSED[100%]

================3 passedin 0.06seconds=================

Chapter 2. Writing Test Functions • 48


This is useful when the id cannot be derived from the parameter value.

### Exercises

1. Download the project for this chapter, tasks_proj, from the book’s webpage^3

and make sure you can install it locally with pip install /path/to/tasks_proj.

2. Explore the tests directory.
3. Run pytest with a single file.
4. Run pytest against a single directory, such as tasks_proj/tests/func. Use pytest

```
to run tests individually as well as a directory full at a time. There are
some failing tests there. Do you understand why they fail?
```
5. Add xfail or skip markers to the failing tests until you can run pytest from

the tests directory with no arguments and no failures.

6. We don’t have any tests for tasks.count() yet, among other functions. Pick

```
an untested API function and think of which test cases we need to have
to make sure it works correctly.
```
7. What happens if you try to add a task with the id already set? There are

```
some missing exception tests in test_api_exceptions.py. See if you can fill in
the missing exceptions. (It’s okay to look at api.py for this exercise.)
```
### What’s Next

You’ve run through a lot of the power of pytest in this chapter. Even with just

what’s covered here, you can start supercharging your test suites. In many

of the examples, you used a fixture called initialized_tasks_db. Fixtures can sepa-

rate retrieving and/or generating test data from the real guts of a test function.

They can also separate common code so that multiple test functions can use

the same setup. In the next chapter, you’ll take a deep dive into the wonderful

world of pytest fixtures.

3. https://pragprog.com/titles/bopytest/source_code

Exercises • 49
