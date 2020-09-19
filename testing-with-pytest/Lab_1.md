<img align="right" src="../logo.png">


### Getting Started with pytest

#### Pre-reqs:
- Google Chrome (Recommended)

#### Lab Environment
Al labs are ready to run. All packages have been installed. There is no requirement for any setup.

All exercises are present in `work/testing-with-pytest/code` folder.



This is a test:

```
ch1/test_one.py

def test_passing():
    assert (1, 2, 3) == (1, 2, 3)

```

This is what it looks like when it’s run:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest test_one.py

=====================testsessionstarts======================
collected1 item

test_one.py.

===================1 passedin 0.01seconds===================
```

The dot after test_one.py means that one test was run and it passed. The [100%]
is a percentage inticator showing how much of the test suite is done so far.
Since there is just one test in the test session, one test is 100% of the tests.
If you have two tests, each would be 50%. If you need more information, you
can use -v or --verbose:

```
$ pytest -v test_one.py
=====================testsessionstarts======================
collected1 item

test_one.py::test_passing PASSED            [100%]

===================1 passedin 0.01seconds===================
```

If you have a color terminal, the PASSED and bottom line are green. It’s nice.
This is a failing test:

```
ch1/test_two.py

def test_passing():
    assert (1, 2, 3) == (1, 2, 3)

```

The way pytest shows you test failures is one of the many reasons developers
love pytest. Let’s watch this fail:

```
$ pytest test_two.py


===================== test session starts ======================
collected 1 item
test_two.py F [100%]
=========================== FAILURES ===========================
_________________________ test_failing _________________________
def test_failing():
> assert (1, 2, 3) == (3, 2, 1)
E    assert (1, 2, 3) == (3, 2, 1)
E    At index 0 diff: 1 != 3
E    Use -v to get the full diff
test_two.py:2: AssertionError
=================== 1 failed in 0.10 seconds ===================
```

Cool. The failing test, test_failing, gets its own section to show us why it failed.
And pytest tells us exactly what the first failure is: index 0 is a mismatch.
Much of this is in red to make it really stand out (if you’ve got a color terminal).
That’s already a lot of information, but there’s a line that says Use -v to get the
full diff. Let’s do that:

```
$ pytest -v test_two.py
=====================testsessionstarts======================
collected1 item

test_two.py::test_failingFAILED [100%]

===========================FAILURES===========================
_________________________test_failing_________________________

def test_failing():
> assert(1,2, 3) == (3,2, 1)**
E       assert(1, 2, 3) == (3, 2, 1)
E       At index0 diff:1 != 3
E       Fulldiff:
E       - (1, 2, 3)
E? ^ ^
E       + (3, 2, 1)
E? ^ ^

test_two.py:2:AssertionError
===================1 failedin 0.05seconds===================
```

Wow. pytest adds little carets (^) to show us exactly what’s different.
If you’re already impressed with how easy it is to write, read, and run tests
with pytest, and how easy it is to read the output to see where the tests fail,
well, you ain’t seen nothing yet. There’s lots more where that came from. Stick

around and let me show you why I think pytest is the absolute best test
framework available.

In the rest of this lab, you’ll install pytest, look at different ways to run
it, and run through some of the most often used command-line options. In
future labs, you’ll learn how to write test functions that maximize the
power of pytest, how to pull setup code into setup and teardown sections
called fixtures, and how to use fixtures and plugins to really supercharge
your software testing.

But first, I have an apology. I’m sorry that the test, assert(1, 2, 3) == (3, 2, 1), is
so boring. Snore. No one would write a test like that in real life. Software tests
are comprised of code that tests other software that you aren’t always positive
will work. And (1, 2, 3) == (1, 2, 3) will always work. That’s why we won’t use
overly silly tests like this in the rest of the book. We’ll look at tests for a real
software project. We’ll use an example project called Tasks that needs some
test code. Hopefully it’s simple enough to be easy to understand, but not so
simple as to be boring.

Another great use of software tests is to test your assumptions about how
the software under test works, which can include testing your understanding
of third-party modules and packages, and even builtin Python data structures.
The Tasks project uses a structure called Task, which is based on the named-
tuple factory method, which is part of the standard library. The Task structure
is used as a data structure to pass information between the UI and the API.
For the rest of this lab, I’ll use Task to demonstrate running pytest and
using some frequently used command-line options.

Here’s Task:

```
**fromcollectionsimport** namedtuple
Task= namedtuple( 'Task' , [ 'summary' , 'owner' , 'done' , 'id' ])
```

The namedtuple() factory function has been around since Python 2.6, but I still
find that many Python developers don’t know how cool it is. At the very least,
using Task for test examples will be more interesting than (1, 2, 3) == (1, 2, 3)
or add(1,2) == 3.

Before we jump into the examples, let’s take a step back and talk about how
to get pytest and install it.


### Getting pytest

The headquarters for pytest is https://docs.pytest.org. That’s the official documen-
tation. But it’s distributed through PyPI (the Python Package Index) at
https://pypi.python.org/pypi/pytest.

Like other Python packages distributed through PyPI, use pip to install pytest
into the virtual environment you’re using for testing:

```
python3 -m venv venv
source venv/bin/activate
pip install pytest
```

If you are not familiar with virtualenv or pip, I have got you covered. Check
out Appendix 1, Virtual Environmentsand Appendix 2, pip,
on page 159.

**What About Windows, Python 2, and virtualenv?**

The example for venv and pip should work on many POSIX systems, such as Linux
and macOS, and many versions of Python, including Python 3.4 and later. With
Python 2.7 and with some distributions of Linux, you will need to use virtualenv:

```
$ python -m pip install virtualenv
$ python -m virtualenv venv
$ source venv/bin/activate
$ pip install pytest
```

The sourcevenv/bin/activate line won’t work for Windows, use venv\Scripts\activate.bat instead.
Do this:

```
C:\> python3 -m venv venv
C:\> venv\Scripts\activate.bat
C:\> pip install pytest
```
### Running pytest

```
$ pytest --help
usage:pytest[options][file_or_dir][file_or_dir][...]
...

```

Given no arguments, pytest looks at your current directory and all subdirec-
tories for test files and runs the test code it finds. If you give pytest a filename,
a directory name, or a list of those, it looks there instead of the current
directory. Each directory listed on the command line is recursively traversed
to look for test code.

For example, let’s create a subdirectory called tasks, and start with this test file:

```
ch1/tasks/test_three.py

"""Test the Task data type."""

from collections import namedtuple

Task = namedtuple('Task', ['summary', 'owner', 'done', 'id'])
Task.__new__.__defaults__ = (None, None, False, None)


def test_defaults():
    """Using no parameters should invoke defaults."""
    t1 = Task()
    t2 = Task(None, None, False, None)
    assert t1 == t2

def test_member_access():
    """Check .field functionality of namedtuple."""
    t = Task('buy milk', 'brian')
    assert t.summary == 'buy milk'
    assert t.owner == 'brian'
    assert (t.done, t.id) == (False, None)


```

You can use __new__.__defaults__ to create Task objects without having to specify
all the fields. The test_defaults() test is there to demonstrate and validate how
the defaults work.

The test_member_access() test is to demonstrate how to access members by name
and not by index, which is one of the main reasons to use namedtuples.

Let’s put a couple more tests into a second file to demonstrate the _asdict() and
_replace() functionality:

```
ch1/tasks/test_four.py

"""Test the Task data type."""

from collections import namedtuple

Task = namedtuple('Task', ['summary', 'owner', 'done', 'id'])
Task.__new__.__defaults__ = (None, None, False, None)

def test_asdict():
    """_asdict() should return a dictionary."""
    t_task = Task('do something', 'okken', True, 21)
    t_dict = t_task._asdict()
    expected = {'summary': 'do something',
                'owner': 'okken',
                'done': True,
                'id': 21}
    assert t_dict == expected


def test_replace():
    """replace() should change passed in fields."""
    t_before = Task('finish book', 'brian', False)
    t_after = t_before._replace(id=10, done=True)
    t_expected = Task('finish book', 'brian', True, 10)
    assert t_after == t_expected


```



```
t_expected= Task( 'finishbook' , 'brian' , True,10)
assert t_after== t_expected
```

To run pytest, you have the option to specify files and directories. If you don’t
specify any files or directories, pytest will look for tests in the current working
directory and subdirectories. It looks for files starting with test_ or ending with
_test. From the ch1 directory, if you run pytest with no commands, you’ll run
four files’ worth of tests:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest
=====================testsessionstarts======================
collected6 items

test_one.py. [ 16%]
test_two.pyF [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

===========================FAILURES===========================
_________________________test_failing_________________________

def test_failing():
> assert(1,2, 3) == (3,2, 1)**
E       assert(1, 2, 3) == (3, 2, 1)
E       At index0 diff:1 != 3
E       Use -v to get the fulldiff

test_two.py:2:AssertionError
==============1 failed,5 passedin 0.17seconds==============
```

To get just our new task tests to run, you can give pytest all the filenames
you want run, or the directory, or call pytest from the directory where our
tests are:

```
$ pytest tasks/test_three.py tasks/test_four.py
=====================testsessionstarts======================
collected4 items

tasks/test_three.py.. [ 50%]
tasks/test_four.py.. [100%]

===================4 passedin 0.02seconds===================

$ pytest tasks**
=====================testsessionstarts======================
collected4 items

tasks/test_four.py.. [ 50%]
tasks/test_three.py.. [100%]

===================4 passedin 0.02seconds===================

$ cd tasks
$ pytest
=====================testsessionstarts======================
```

```
collected4 items

test_four.py.. [ 50%]
test_three.py.. [100%]

===================4 passedin 0.02seconds===================
```

The part of pytest execution where pytest goes off and finds which tests to
run is called _test discovery_. pytest was able to find all the tests we wanted it
to run because we named them according to the pytest naming conventions.
Here’s a brief overview of the naming conventions to keep your test code dis-
coverable by pytest:

- Test files should be named test_<something>.py or <something>_test.py.
- Test methods and functions should be named test_<something>.
- Test classes should be named Test<Something>.

Since our test files and functions start with test_, we’re good. There are ways
to alter these discovery rules if you have a bunch of tests named differently.
I’ll cover that in Lab 6, Configuration.

Let’s take a closer look at the output of running just one file:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1/tasks
$ pytest test_three.py


===================== test session starts ======================
platform darwin -- Python 3.x.y, pytest-3.x.y, py-1.x.y, pluggy-0.x.y
rootdir: /home/jovyan/work/testing-with-pytest/code/ch1, inifile:
collected 2 items
test_three.py .. [100%]
=================== 2 passed in 0.01 seconds ===================
```

pytest provides a nice delimiter for the start of the test session. A session
is one invocation of pytest, including all of the tests run on possibly
multiple directories. This definition of session becomes important when
I talk about session scope in relation to pytest fixtures in Specifying Fixture
Scope

```
_platformdarwin-- Python3.x.y, pytest -3.x.y, py-1.x.y, pluggy-0.x.y_
```
platform darwin is a Mac thing. This is different on a Windows machine. The
Python and pytest versions are listed, as well as the packages pytest
depends on. Both py and pluggy are packages developed by the pytest team
to help with the implementation of pytest.

```
_rootdir:/home/jovyan/work/testing-with-pytest/code/ch1/tasks,inifile:_

```
The rootdir is the topmost common directory to all of the directories being
searched for test code. The inifile (blank here) lists the configuration file being
used. Configuration files could be pytest.ini, tox.ini, or setup.cfg. You’ll look at
configuration files in more detail in Lab 6, Configuration

```
_collected2 items_
```

These are the two test functions in the file.

```
_test_three.py.. [100%]_

```

The test_three.py shows the file being tested. There is one line for each test
file. The two dots denote that the tests passed—one dot for each test
function or method. Dots are only for passing tests. Failures, errors, skips,
xfails, and xpasses are denoted with F, E, s, x, and X, respectively. If you
want to see more than dots for passing tests, use the -v or --verbose option.
The percentage of completed tests is reported after each test file and based
on percentage of the number of test cases collected and selected to run.

```
_===================2 passedin 0.01 seconds===================_
```
This refers to the number of passing tests and how long the entire test
session took. If non-passing tests were present, the number of each cate-
gory would be listed here as well.

The outcome of a test is the primary way the person running a test or looking
at the results understands what happened in the test run. In pytest, test
functions may have several different outcomes, not just pass or fail.

Here are the possible outcomes of a test function:

- PASSED (.): The test ran successfully.
- FAILED (F): The test did not run successfully (or XPASS + strict).
- SKIPPED (s): The test was skipped. You can tell pytest to skip a test by
    using either the @pytest.mark.skip() or pytest.mark.skipif() decorators, discussed
    in Skipping Tests
- xfail (x): The test was not supposed to pass, ran, and failed. You can tell
    pytest that a test is expected to fail by using the @pytest.mark.xfail() decorator,
    discussed in Marking Tests as Expecting to Fail
- XPASS (X): The test was not supposed to pass, ran, and passed.
- ERROR (E): An exception happened outside of the test function, in either
    a fixture, discussed in Lab 3, pytest Fixtures, or in a
    hook function, discussed in Lab 5, Plugins


### Running Only One Test

One of the first things you’ll want to do once you’ve started writing tests is to
run just one. Specify the file directly, and add a ::test_name, like this:



```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -v tasks/test_four.py::test_asdict**
=====================testsessionstarts======================
collected1 item

tasks/test_four.py::test_asdict PASSED            [100%]

===================1 passedin 0.01seconds===================
```

Now, let’s take a look at some of the options.

### Using Options

We’ve used the verbose option, -v or --verbose, a couple of times already, but
there are many more options worth knowing about. We’re not going to use
all of the options in this course, but quite a few. You can see all of them with
pytest --help.

The following are a handful of options that are quite useful when starting out
with pytest. This is by no means a complete list, but these options in partic-
ular address some common early desires for controlling how pytest runs when
you’re first getting started.

```
$ pytest --help
... subsetof the list...**
-k EXPRESSION onlyrun tests/classes whichmatchthe given
substringexpression.
Example:-k 'test_methodor test_other'matches
all testfunctionsand classeswhosename
contains'test_method'or 'test_other'.
-m MARKEXPR onlyrun testsmatchinggivenmarkexpression.
example:-m 'mark1and not mark2'.
-x, --exitfirst exitinstantlyon firsterroror failedtest.
--maxfail=num exitafter firstnum failuresor errors.
--capture=method per-testcapturingmethod:one of fd|sys|no.
-s shortcutfor --capture=no.
--lf,--last-failed rerunonlythe teststhatfailedlasttime
(or all if nonefailed)
--ff,--failed-first run all testsbut run the lastfailuresfirst.
-v, --verbose increaseverbosity.
-q, --quiet decreaseverbosity.
-l, --showlocals showlocalsin tracebacks(disabledby default).
--tb=style tracebackprintmode(auto/long/short/line/native/no).
--durations=N showN slowestsetup/testdurations(N=0for all).
--collect-only onlycollecttests,don'texecutethem.
--version displaypytestlib versionand importinformation.
-h, --help showhelpmessageand configurationinfo
```

**--collect-only**

The --collect-only option shows you which tests will be run with the given options
and configuration. It’s convenient to show this option first so that the output
can be used as a reference for the rest of the examples. If you start in the ch1
directory, you should see all of the test functions you’ve looked at so far in
this lab:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --collect-only**
===================testsessionstarts===================
collected6 items
<Module'test_one.py'>
<Function'test_passing'>
<Module'test_two.py'>
<Function'test_failing'>
<Module'tasks/test_four.py'>
<Function'test_asdict'>
<Function'test_replace'>
<Module'tasks/test_three.py'>
<Function'test_defaults'>
<Function'test_member_access'>

==============no testsran in 0.02seconds===============
```

The --collect-only option is helpful to check if other options that select tests are correct
before running the tests. We’ll use it again with -k to show how that works.

**-k EXPRESSION**

The -k option lets you use an expression to find what test functions to run.
Pretty powerful. It can be used as a shortcut to running an individual test if
its name is unique, or running a set of tests that have a common prefix or
suffix in their names. Let’s say you want to run the test_asdict() and test_defaults()
tests. You can test out the filter with --collect-only:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -k** _"asdictor defaults"_ **--collect-only**
===================testsessionstarts===================
collected6 items/ 4 deselected
<Module'tasks/test_four.py'>
<Function'test_asdict'>
<Module'tasks/test_three.py'>
<Function'test_defaults'>

==============4 deselectedin 0.02seconds===============
```

Yep. That looks like what we want. Now you can run them by removing the
--collect-only:


```
$ pytest -k** _"asdictor defaults"_
===================testsessionstarts===================
collected6 items/ 4 deselected

tasks/test_four.py. [ 50%]
tasks/test_three.py. [100%]

=========2 passed,4 deselectedin 0.03seconds==========
```

Hmm. Just dots. So they passed. But were they the right tests? One way to
find out is to use -v or --verbose:

```
$ pytest -v -k** _"asdictor defaults"_
===================testsessionstarts===================
collected6 items/ 4 deselected

tasks/test_four.py::test_asdict PASSED            [ 50%]
tasks/test_three.py::test_defaults PASSED            [100%]

=========2 passed,4 deselectedin 0.02seconds==========
```

Yep. They were the correct tests.

**-m MARKEXPR**

Markers are one of the best ways to mark a subset of your test functions so
that they can be run together. As an example, one way to run test_replace() and
test_member_access(), even though they are in separate files, is to mark them.

You can use any marker name. Let’s say you want to use run_these_please. You’d
mark a test using the decorator @pytest.mark.run_these_please, like so:

```
import pytest

...
@pytest.mark.run_these_please
def test_member_access ():
...
```

Then you’d do the same for test_replace(). You can then run all the tests with
the same marker with pytest -m run_these_please:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1/tasks
$ pytest -m run_these_please

=================== test session starts ===================
collected 4 items / 2 deselected
test_four.py . [ 50%]
test_three.py . [100%]
========= 2 passed, 2 deselected in 0.02 seconds ==========
```

The marker expression doesn’t have to be a single marker. You can say things
like -m "mark1and mark2" for tests with both markers, -m "mark1and not mark2" for

tests that have mark1 but not mark2, -m "mark1or mark2" for tests with either,
and so on. I’ll discuss markers more completely in Marking Test Functions,
on page 31.

**-x, --exitfirst**

Normal pytest behavior is to run every test it finds. If a test function
encounters a failing assert or an exception, the execution for that test stops
there and the test fails. And then pytest runs the next test. Most of the time,
this is what you want. However, especially when debugging a problem, stop-
ping the entire test session immediately when a test fails is the right thing to
do. That’s what the -x option does.

Let’s try it on the six tests we have so far:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -x**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF

========================FAILURES=========================
______________________test_failing_______________________

def test_failing():
> assert(1,2, 3) == (3,2, 1)**
E       assert(1, 2, 3) == (3, 2, 1)
E       At index0 diff:1 != 3
E       Use -v to get the fulldiff

test_two.py:2:AssertionError
===========1 failed,1 passedin 0.13seconds============
```

Near the top of the output you see that all six tests (or “items”) were collected,
and in the bottom line you see that one test failed and one passed.

Without -x, all six tests would have run. Let’s run it again without the -x. Let’s
also use --tb=no to turn off the stack trace, since you’ve already seen it and
don’t need to see it again:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --tb=no 
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

===========1 failed,5 passedin 0.07seconds============
```

This demonstrates that without the -x, pytest notes failure in test_two.py and
continues on with further testing.

**--maxfail=num**

The -x option stops after one test failure. If you want to let some failures
happen, but not a ton, use the --maxfail option to specify how many failures to
allow before stopping the test session.

It’s hard to really show this with only one failing test in our system so far,
but let’s take a look anyway. Since there is only one failure, if we set --maxfail=2,
all of the tests should run, and --maxfail=1 should act just like -x:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --maxfail=2 --tb=no 

=================== test session starts ===================
collected 6 items
test_one.py . [ 16%]
test_two.py F [ 33%]
tasks/test_four.py .. [ 66%]
tasks/test_three.py .. [100%]
=========== 1 failed, 5 passed in 0.07 seconds ============
$ pytest --maxfail=1 --tb=no 
=================== test session starts ===================
collected 6 items
test_one.py . [ 16%]
test_two.py F
=========== 1 failed, 1 passed in 0.07 seconds ============

```

Again, we used --tb=no to turn off the traceback.

**-s and --capture=method**

The -s flag allows print statements—or really any output that normally would
be printed to stdout—to actually be printed to stdout while the tests are running.
It is a shortcut for --capture=no. This makes sense once you understand that
normally the output is captured on all tests. Failing tests will have the output
reported after the test runs on the assumption that the output will help you
understand what went wrong. The -s or --capture=no option turns off output
capture. When developing tests, I find it useful to add several print() statements
so that I can watch the flow of the test.

Another option that may help you to not need print statements in your code
is -l/--showlocals, which prints out the local variables in a test if the test fails.


Other options for capture method are --capture=fd and --capture=sys. The --capture=sys
option replaces sys.stdout/stderr with in-mem files. The --capture=fd option points
file descriptors 1 and 2 to a temp file.

I’m including descriptions of sys and fd for completeness. But to be honest,
I’ve never needed or used either. I frequently use -s. And to fully describe how
-s works, I needed to touch on capture methods.

We don’t have any print statements in our tests yet; a demo would be point-
less. However, I encourage you to play with this a bit so you see it in action.

**--lf, --last-failed**

When one or more tests fails, having a convenient way to run just the failing
tests is helpful for debugging. Just use --lf and you’re ready to debug:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --lf


=================== test session starts ===================
collected 6 items / 5 deselected
run-last-failure: rerun previous 1 failure
test_two.py F [100%]
======================== FAILURES =========================
______________________ test_failing _______________________
def test_failing():
> assert (1, 2, 3) == (3, 2, 1)
E       assert (1, 2, 3) == (3, 2, 1)
E       At index 0 diff: 1 != 3
E       Use -v to get the full diff
test_two.py:2: AssertionError
========= 1 failed, 5 deselected in 0.07 seconds ==========
```

This is great if you’ve been using a --tb option that hides some information
and you want to re-run the failures with a different traceback option.

**--ff, --failed-first**

The --ff/--failed-first option will do the same as --last-failed, and then run the rest
of the tests that passed last time:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --ff --tb=no 
$ pytest --ff --tb=no 


=================== test session starts ===================
collected 6 items
run-last-failure: rerun previous 1 failure first
test_two.py F [ 16%]
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --ff --tb=no 
$ pytest --ff --tb=no 
=================== test session starts ===================
collected 6 items
run-last-failure: rerun previous 1 failure first
test_two.py F [ 16%]

```

Usually, test_failing() from test_two.py is run after test_one.py. However, because
test_failing() failed last time, --ff causes it to be run first.

**-v, --verbose**

The -v/--verbose option reports more information than without it. The most
obvious difference is that each test gets its own line, and the name of the test
and the outcome are spelled out instead of indicated with just a dot.

We’ve used it quite a bit already, but let’s run it again for fun in conjunction
with --ff and --tb=no :

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -v --ff --tb=no 

=================== test session starts ===================
collected 6 items
run-last-failure: rerun previous 1 failure first
test_two.py::test_failing FAILED [ 16%]
test_one.py::test_passing PASSED            [ 33%]
tasks/test_four.py::test_asdict PASSED            [ 50%]
tasks/test_four.py::test_replace PASSED            [ 66%]
tasks/test_three.py::test_defaults PASSED            [ 83%]
tasks/test_three.py::test_member_access PASSED            [100%]
=========== 1 failed, 5 passed in 0.08 seconds ============
```

With color terminals, you’d see red FAILED and green PASSED outcomes in the
report as well.

**-q, --quiet**

The -q/--quiet option is the opposite of -v/--verbose; it decreases the information
reported. I like to use it in conjunction with --tb=line, which reports just the
failing line of any failing tests.




Let’s try -q by itself:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -q


.F.... [100%]
======================== FAILURES =========================
______________________ test_failing _______________________
def test_failing():
> assert (1, 2, 3) == (3, 2, 1)
E       assert (1, 2, 3) == (3, 2, 1)
E       At index 0 diff: 1 != 3
E       Full diff:
E       - (1, 2, 3)
E       ? ^ ^
E       + (3, 2, 1)
E       ? ^ ^
test_two.py:2: AssertionError
1 failed, 5 passed in 0.08 seconds
```

The -q option makes the output pretty terse, but it’s usually enough. We’ll
use the -q option frequently in the rest of the book (as well as --tb=no ) to limit
the output to what we are specifically trying to understand at the time.

**-l, --showlocals**

If you use the -l/--showlocals option, local variables and their values are displayed
with tracebacks for failing tests.

So far, we don’t have any failing tests that have local variables. If I take the
test_replace() test and change

```
t_expected= Task( 'finishbook' , 'brian' , True,10)

to

t_expected= Task( 'finishbook' , 'brian' , True,11)
```

the 10 and 11 should cause a failure. Any change to the expected value will
cause a failure. But this is enough to demonstrate the command-line option
--l/--showlocals:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest -l tasks


=================== test session starts ===================
collected 4 items
tasks/test_four.py .F [ 50%]
tasks/test_three.py .. [100%]
======================== FAILURES =========================
______________________ test_replace _______________________

def test_replace():
    """replace() should change passed in fields."""
    t_before = Task('finish book', 'brian', False)
    t_after = t_before._replace(id=10, done=True)
    t_expected = Task('finish book', 'brian', True, 10)
    assert t_after == t_expected


E       AssertionError: assert Task(summary=...e=True, id=10) ==
Task(summary='...e=True, id=11)
E       At index 3 diff: 10 != 11
E       Use -v to get the full diff

t_after = Task(summary='finish book', owner='brian', done=True, id=10)
t_before = Task(summary='finish book', owner='brian', done=False, id=None)
t_expected = Task(summary='finish book', owner='brian', done=True, id=11)
tasks/test_four.py:24: AssertionError
=========== 1 failed, 3 passed in 0.07 seconds ============

```

The local variables t_after, t_before, and t_expected are shown after the code
snippet, with the value they contained at the time of the failed assert.

**--tb=style**

The --tb=style option modifies the way tracebacks for failures are output. When
a test fails, pytest lists the failures and what’s called a _traceback_ , which shows
you the exact line where the failure occurred. Although tracebacks are helpful
most of time, there may be times when they get annoying. That’s where the
--tb=style option comes in handy. The styles I find useful are short, line, and no.
short prints just the assert line and the E       evaluated line with no context; line
keeps the failure to one line; no removes the traceback entirely.

Let’s leave the modification to test_replace() to make it fail and run it with differ-
ent traceback styles.

--tb=no removes the traceback entirely:

```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --tb=no tasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

===========1 failed,3 passedin 0.06seconds============
```

--tb=line in many cases is enough to tell what’s wrong. If you have a ton of
failing tests, this option can help to show a pattern in the failures:


```
$ pytest --tb=linetasks
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

========================FAILURES=========================
/home/jovyan/work/testing-with-pytest/code/ch1/tasks/test_four.py:24:
AssertionError:assertTask(summary=...e=True, id=10)==
Task(summary='...e=True,id=11)
===========1 failed,3 passedin 0.07seconds============

The next step up in verbose tracebacks is --tb=short:

$ pytest --tb=shorttasks
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

========================FAILURES=========================
______________________test_replace_______________________
tasks/test_four.py:24:in test_replace
assertt_after== t_expected
E       AssertionError:assertTask(summary=...e=True,id=10)==
Task(summary='...e=True,id=11)
E       At index3 diff:10 != 11
E       Use -v to get the fulldiff
===========1 failed,3 passedin 0.07seconds============
```

That’s definitely enough to tell you what’s going on.

There are three remaining traceback choices that we haven’t covered so far.
pytest --tb=long will show you the most exhaustive, informative traceback possi-
ble. pytest --tb=auto will show you the long version for the first and last trace-
backs, if you have multiple failures. This is the default behavior. pytest --tb=native
will show you the standard library traceback without any extra information.

**--durations=N**

The --durations=N option is incredibly helpful when you’re trying to speed up
your test suite. It doesn’t change how your tests are run; it reports the slowest
N number of tests/setups/teardowns after the tests run. If you pass in
--durations=0, it reports everything in order of slowest to fastest.

None of our tests are long, so I’ll add a time.sleep(0.1) to one of the tests. Guess
which one:




```
$ cd /home/jovyan/work/testing-with-pytest/code/ch1
$ pytest --durations=3tasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.. [ 50%]
tasks/test_three.py.. [100%]

================slowest3 testdurations=================
0.10scall tasks/test_four.py::test_replace
0.00ssetup tasks/test_three.py::test_defaults
0.00steardowntasks/test_four.py::test_asdict
================4 passedin 0.13seconds=================
```

The slow test with the extra sleep shows up right away with the label call,
followed by setup and teardown. Every test essentially has three phases: call,
setup, and teardown. Setup and teardown are also called _fixtures_ and are a
chance for you to add code to get data or the software system under test into
a precondition state before the test runs, as well as clean up afterwards if
necessary. I cover fixtures in depth in Lab 3, pytest Fixtures

**-- version

The --version option shows the version of pytest and the directory where it’s
installed:

```
$ pytest -- version
Thisis pytestversion3.x.y,importedfrom
/path/to/venv/lib/python3.x/site-packages/pytest.py
```

Since we installed pytest into a virtual environment, pytest will be located in
the site-packages directory of that virtual environment.

**-h, --help**

The -h/--help option is quite helpful, even after you get used to pytest. Not only
does it show you how to use stock pytest, but it also expands as you install
plugins to show options and configuration variables added by plugins.

The -h option shows:

- usage:pytest[options][file_or_dir][file_or_dir][...]

- Command-line options and a short description, including options added
    via plugins

- A list of options available to ini style configuration files, which I’ll discuss
    more in Lab 6, Configuration

- A list of environmental variables that can affect pytest behavior (also
    discussed in Lab 6, Configuration)

- A reminder that pytest --markers can be used to see available markers,
    discussed in Lab 2, Writing Test Functions

- A reminder that pytest --fixtures can be used to see available fixtures, dis-
    cussed in Lab 3, pytest Fixtures

The last bit of information the help text displays is this note:

```
(shownaccordingto specifiedfile_or_diror currentdir if not specified;
fixtureswithleading'' are onlyshownwiththe '-v'option)
```

This note is important because the options, markers, and fixtures can change
based on which directory or test file you’re running. This is because along
the path to a specified file or directory, pytest may find conftest.py files that can
include hook functions that create new options, fixture definitions, and
marker definitions.

The ability to customize the behavior of pytest in conftest.py files and test files
allows customized behavior local to a project or even a subset of the tests for
a project. You’ll learn about conftest.py and ini files such as pytest.ini in Lab
6, Configuration

### Exercises

1. Create a new virtual environment using python-m virtualenv or python-m venv.
Even if you know you don’t need virtual environments for the project
you’re working on, humor me and learn enough about them to create one
for trying out things in this course. I resisted using them for a very long
time, and now I always use them. Read Appendix 1, Virtual Environments,
on page 157 if you’re having any difficulty.

2. Practice activating and deactivating your virtual environment a few times.

    - $ source venv/bin/activate

    - $ deactivate

On Windows:

- C:\Users\okken\sandbox>venv\scripts\activate.bat
- C:\Users\okken\sandbox>deactivate

3. Install pytest in your new virtual environment. See Appendix 2, pip, on
page 159 if you have any trouble. Even if you thought you already had
pytest installed, you’ll need to install it into the virtual environment you
just created.

4. Create a few test files. You can use the ones we used in this lab or
make up your own. Practice running pytest against these files.

5. Change the assert statements. Don’t just use assertsomething==something_else;
try things like:

- assert1 in [2, 3, 4]
- asserta < b
- assert'fizz' not in 'fizzbuzz'

### What’s Next

In this lab, we looked at where to get pytest and the various ways to run
it. However, we didn’t discuss what goes into test functions. In the next
lab, we’ll look at writing test functions, parametrizing them so they get
called with different data, and grouping tests into classes, modules, and
packages.


