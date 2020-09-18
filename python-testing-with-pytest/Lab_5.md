<img align="right" src="../logo.png">


### Plugins

As powerful as pytest is right out of the box, it gets even better when you add
plugins to the mix. The pytest code base is structured with customization
and extensions, and there are hooks available to allow modifications and
improvements through plugins.

It might surprise you to know that you’ve already written some plugins if
you’ve worked through the previous labs in this book. Any time you put
fixtures and/or hook functions into a project’s top-level conftest.py file, you
created a local conftest plugin. It’s just a little bit of extra work to convert
these conftest.py files into installable plugins that you can share between
projects, with other people, or with the world.

We will start this lab looking at where to look for third-party plugins.
Quite a few plugins are available, so there’s a decent chance someone has
already written the change you want to make to pytest. Since we will be
looking at open source plugins, if a plugin does almost what you want to
do but not quite, you can fork it, or use it as a reference for creating your
own plugin. While this lab is about creating your own plugins, Appendix
3, Plugin Sampler Pack, on page 163 is included to give you a taste of what’s
possible.

In this lab, you’ll learn how to create plugins, and I’ll point you in the
right direction to test, package, and distribute them. The full topic of Python
packaging and distribution is probably a book of its own, so we won’t cover
everything. But you’ll get far enough to be able to share plugins with your
team. I’ll also discuss some shortcuts to getting PyPI–distributed plugins up
with the least amount of work.


### Finding Plugins

You can find third-party pytest plugins in several places. The plugins listed
in Appendix 3, Plugin Sampler Pack, on page 163 are all available for download
from PyPI. However, that’s not the only place to look for great pytest plugins.

```
_https://docs.pytest.org/en/latest/plugins.html_

```

The main pytest documentation site has a page that talks about installing
and using pytest plugins, and lists a few common plugins.

```
_https://pypi.python.org_
```
The Python Package Index (PyPI) is a great place to get lots of Python
packages, but it is also a great place to find pytest plugins. When looking
for pytest plugins, it should work pretty well to enter “pytest,” “pytest-,”
or “-pytest” into the search box, since most pytest plugins either start
with “pytest-,” or end in “-pytest.”

```
_https://github.com/pytest-dev_
```
The “pytest-dev” group on GitHub is where the pytest source code is kept.
It’s also where you can find some popular pytest plugins that are intended
to be maintained long-term by the pytest core team.


### Installing Plugins

pytest plugins are installed with pip, just like other Python packages. However,
you can use pip in several different ways to install plugins.

**Install from PyPI**

As PyPI is the default location for pip, installing plugins from PyPI is the easiest
method. Let’s install the pytest-cov plugin:

```
$ pip installpytest-cov**
```

This installs the latest stable version from PyPI.

**Install a Particular Version from PyPI**

If you want a particular version of a plugin, you can specify the version
after ‘==‘:

```
$ pip installpytest-cov==2.5.1**
```

**Install from a .tar.gz or .whl File**

Packages on PyPI are distributed as zipped files with the extensions .tar.gz
and/or .whl. These are often referred to as “tar balls” and “wheels.” If you’re



having trouble getting pip to work with PyPI directly (which can happen with
firewalls and other network complications), you can download either the .tar.gz
or the .whl and install from that.

You don’t have to unzip or anything; just point pip at it:

```
$ pip installpytest-cov-2.5.1.tar.gz**
_# or_

$ pip installpytest_cov-2.5.1-py2.py3-none-any.whl**
```

**Install from a Local Directory**

You can keep a local stash of plugins (and other Python packages) in a local
or shared directory in .tar.gz or .whl format and use that instead of PyPI for
installing plugins:

```
$ mkdirsome_plugins
$ cp pytest_cov-2.5.1-py2.py3-none-any.whlsome_plugins/
$ pip install--no-index--find-links=./some_plugins/pytest-cov**
```

The --no-index tells pip to not connect to PyPI. The --find-links=./some_plugins/ tells
pip to look in the directory called some_plugins. This technique is especially
useful if you have both third-party and your own custom plugins stored
locally, and also if you’re creating new virtual environments for continuous
integration or with tox. (We’ll talk about both tox and continuous integration
in Lab 7, Using pytest with Other Tools, on page 127 .)

Note that with the local directory install method, you can install multiple
versions and specify which version you want by adding == and the version
number:

```
$ pip install--no-index--find-links=./some_plugins/pytest-cov==2.5.1**
```

**Install from a Git Repository**

You can install plugins directly from a Git repository—in this case, GitHub:

```
$ pip installgit+https://github.com/pytest-dev/pytest-cov**
```

You can also specify a version tag:

```
$ pip installgit+https://github.com/pytest-dev/pytest-cov@v2.5.1**
```

Or you can specify a branch:

```
$ pip installgit+https://github.com/pytest-dev/pytest-cov@master**
```

Installing from a Git repository is especially useful if you’re storing your own
work within Git, or if the plugin or plugin version you want isn’t on PyPI.
Installing Plugins • 99


### Writing Your Own Plugins

Many third-party plugins contain quite a bit of code. That’s one of the reasons
we use them—to save us the time to develop all of that code ourselves.
However, for your specific coding domain, you’ll undoubtedly come up with
special fixtures and modifications that help you test. Even a handful of fix-
tures that you want to share between a couple of projects can be shared
easily by creating a plugin. You can share those changes with multiple
projects—and possibly the rest of the world—by developing and distributing
your own plugins. It’s pretty easy to do so. In this section, we’ll develop a
small modification to pytest behavior, package it as a plugin, test it, and look
into how to distribute it.

Plugins can include hook functions that alter pytest’s behavior. Because
pytest was developed with the intent to allow plugins to change quite a bit
about the way pytest behaves, a lot of hook functions are available. The hook
functions for pytest are specified on the pytest documentation site.^1

For our example, we’ll create a plugin that changes the way the test status
looks. We’ll also include a command-line option to turn on this new behavior.
We’re also going to add some text to the output header. Specifically, we’ll
change all of the FAILED status indicators to “OPPORTUNITY for improve-
ment,” change F to O, and add “Thanks for running the tests” to the header.
We’ll use the --nice option to turn the behavior on.

To keep the behavior changes separate from the discussion of plugin
mechanics, we’ll make our changes in conftest.py before turning it into a dis-
tributable plugin. You don’t have to start plugins this way. But frequently,
changes you only intended to use on one project will become useful enough
to share and grow into a plugin. Therefore, we’ll start by adding functionality
to a conftest.py file, then, after we get things working in conftest.py, we’ll move
the code to a package.

Let’s go back to the Tasks project. In Expecting Exceptions, on page 30, we
wrote some tests that made sure exceptions were raised if someone called an
API function incorrectly. Looks like we missed at least a few possible error
conditions.

1. https://docs.pytest.org/en/latest/reference.html#hooks



Here are a couple more tests:

```
**ch5/a/tasks_proj/tests/func/test_api_exceptions.py
importpytest
importtasks
fromtasksimport** Task

@pytest.mark.usefixtures( _'tasks_db'_ )
**class** TestAdd():
_"""Testsrelatedto tasks.add()."""_
def test_missing_summary (self):
_"""Shouldraisean exceptionif summarymissing."""_
**with** pytest.raises(ValueError):
tasks.add(Task(owner= _'bob'_ ))
def test_done_not_bool (self):
_"""Shouldraisean exceptionif doneis not a bool."""_
**with** pytest.raises(ValueError):
tasks.add(Task(summary= _'summary'_ , done= _'True'_ ))
```

Let’s run them to see if they pass:

```
$ cd /path/to/code/ch5/a/tasks_proj
$ pytest
===================testsessionstarts===================
plugins:cov-2.5.1
collected57 items

tests/func/test_add.py... [ 5%]
tests/func/test_add_variety.py....................[ 40%]
........ [ 54%]
tests/func/test_add_variety2.py............ [ 75%]
tests/func/test_api_exceptions.py.F....... [ 91%]
tests/func/test_unique_id.py. [ 92%]
tests/unit/test_task.py.... [100%]

========================FAILURES=========================
_______________TestAdd.test_done_not_bool________________

self= <func.test_api_exceptions.TestAddobjectat 0x103d82860>

def test_done_not_bool(self):
"""Shouldraisean exceptionif doneis not a bool."""
withpytest.raises(ValueError):
**> tasks.add(Task(summary=** _'summary'_ **, done=** _'True'_ **))**
E Failed:DID NOT RAISE<class'ValueError'>

tests/func/test_api_exceptions.py:20:Failed
===========1 failed,56 passedin 0.48seconds===========
```

Let’s run it again with -v for verbose. Since you’ve already seen the traceback,
you can turn that off with --tb=no.


And now let’s focus on the new tests with -k TestAdd, which works because
there aren’t any other tests with names that contain “TestAdd.”

```
$ cd /path/to/code/ch5/a/tasks_proj/tests/func
$ pytest-v --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py::TestAdd::test_done_not_boolFAILED[100%]

====1 failed,1 passed,7 deselectedin 0.10seconds=====
```

We could go off and try to fix this test (and we should later), but now we are
focused on trying to make failures more pleasant for developers.

Let’s start by adding the “thank you” message to the header, which you can
do with a pytest hook called pytest_report_header().

```
**ch5/b/tasks_proj/tests/conftest.py
def pytest_report_header ():
_"""Thanktesterfor runningtests."""_
    return _"Thanksfor runningthe tests."_
```

Obviously, printing a thank-you message is rather silly. However, the ability
to add information to the header can be extended to add a username and
specify hardware used and versions under test. Really, anything you can
convert to a string, you can stuff into the test header.

Next, we’ll change the status reporting for tests to change F to O and FAILED to
OPPORTUNITYfor improvement. There’s a hook function that allows for this type of
shenanigans: pytest_report_teststatus():

```
**ch5/b/tasks_proj/tests/conftest.py
def pytest_report_teststatus (report):
_"""Turnfailuresintoopportunities."""_
**if** report.when== _'call'_ **and** report.failed:
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )
```

And now we have just the output we were looking for. A test session with no
--verbose flag shows an O for failures, er, improvement opportunities:

```
$ cd /path/to/code/ch5/b/tasks_proj/tests/func
$ pytest--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.O
```


```
====`1 failed,1 passed,7 deselectedin 0.10seconds=====
```



And the -v or --verbose flag will be nicer also:

```
$ pytest-v --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py
::TestAdd::test_done_not_boolOPPORTUNITYfor improvement[100%]

====1 failed,1 passed,7 deselectedin 0.11seconds=====
```

The last modification we’ll make is to add a command-line option, --nice, to
only have our status modifications occur if --nice is passed in:

```
**ch5/c/tasks_proj/tests/conftest.py
def pytest_addoption (parser):
_"""Turnnicefeatureson with--niceoption."""_
group= parser.getgroup( _'nice'_ )
group.addoption( _"--nice"_ , action= _"store_true"_ ,
help= _"nice:turnfailuresintoopportunities"_ )

def pytest_report_header (config):
_"""Thanktesterfor runningtests."""_
**if** config.getoption( _'nice'_ ):
    return _"Thanksfor running the tests."_

def pytest_report_teststatus (report,config):
_"""Turnfailuresintoopportunities."""_
**if** report.when== _'call'_ :
**if** report.failed **and** config.getoption( _'nice'_ ):
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )
```

This is a good place to note that for this plugin, we are using just a couple of
hook functions. There are many more, which can be found on the main pytest
documentation site.^2

We can manually test our plugin just by running it against our example file.
First, with no --nice option, to make sure just the username shows up:

```
$ cd /path/to/code/ch5/c/tasks_proj/tests/func
$ pytest--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.F [100%]
```

2. https://docs.pytest.org/en/latest/writing_plugins.html



```
====1 failed,1 passed,7 deselectedin 0.11seconds=====

Now with --nice:

$ pytest--nice--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.O [100%]

====1 failed,1 passed,7 deselectedin 0.12seconds=====

And with --nice and --verbose:

$ pytest-v --nice--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py
::TestAdd::test_done_not_boolOPPORTUNITYfor improvement[100%]

====1 failed,1 passed,7 deselectedin 0.10seconds=====
```

Great! All of the changes we wanted are done with about a dozen lines of code
in a conftest.py file. Next, we’ll move this code into a plugin structure.

### Creating an Installable Plugin

The process for sharing plugins with others is well-defined. Even if you never
put your own plugin up on PyPI, by walking through the process, you’ll have
an easier time reading the code from open source plugins and be better
equipped to judge if they will help you or not.

It would be overkill to fully cover Python packaging and distribution in this
book, as the topic is well documented elsewhere.3,4 However, it’s a small task
to go from the local config plugin we created in the previous section to some-
thing pip-installable.

First, we need to create a new directory to put our plugin code. It does not
matter what you call it, but since we are making a plugin for the “nice” flag,
let’s call it pytest-nice. We will have two files in this new directory: pytest_nice.py
and setup.py. (The tests directory will be discussed in Testing Plugins, on
page 109 .)

3. [http://python-packaging.readthedocs.io](http://python-packaging.readthedocs.io)
4. https://www.pypa.io


```
pytest-nice
├──LICENCE
├──README.rst
├──pytest_nice.py
├──setup.py
└──tests
├──conftest.py
└──test_nice.py
```

In pytest_nice.py, we’ll put the exact contents of our conftest.py that were related
to this feature (and take it out of the tasks_proj/tests/conftest.py):

```
**ch5/pytest-nice/pytest_nice.py
_"""Codefor pytest-niceplugin."""_

import pytest

def pytest_addoption (parser):
_"""Turnnicefeatureson with--niceoption."""_
group= parser.getgroup( _'nice'_ )
group.addoption( _"--nice"_ , action= _"store_true"_ ,
help= _"nice:turnFAILEDintoOPPORTUNITYfor improvement"_ )

def pytest_report_header (config):
_"""Thanktesterfor runningtests."""_
**if** config.getoption( _'nice'_ ):
    return _"Thanksfor running the tests."_

def pytest_report_teststatus (report,config):
_"""Turnfailuresintoopportunities."""_
**if** report.when== _'call'_ :
**if** report.failed **and** config.getoption( _'nice'_ ):
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )

In setup.py, we need a very minimal call to setup():

**ch5/pytest-nice/setup.py
_"""Setupfor pytest-niceplugin."""_

**fromsetuptoolsimport** setup

setup(
name= _'pytest-nice'_ ,
version= _'0.1.0'_ ,
description= _'A pytestpluginto turnFAILUREintoOPPORTUNITY'_ ,
url= _'https://wherever/you/have/info/on/this/package'_ ,
author= _'YourName'_ ,
author_email= _'your_email@somewhere.com'_ ,
license= _'proprietary'_ ,
py_modules=[ _'pytest_nice'_ ],
install_requires=[ _'pytest'_ ],
entry_points={ _'pytest11'_ : [ _'nice= pytest_nice'_ , ], },
)
```

You’ll want more information in your setup if you’re going to distribute to a
wide audience or online. However, for a small team or just for yourself, this
will suffice.

You can include many more parameters to setup(); we only have the required
fields. The version field is the version of this plugin. And it’s up to you when
you bump the version. The url field is required. You can leave it out, but you
get a warning if you do. The author and author_email fields can be replaced with
maintainer and maintainer_email, but one of those pairs needs to be there. The
license field is a short text field. It can be one of the many open source licenses,
your name or company, or whatever is appropriate for you. The py_modules
entry lists pytest_nice as our one and only module for this plugin. Although it’s
a list and you could include more than one module, if I had more than one,
I’d use packages instead and put all the modules inside a directory.

So far, all of the parameters to setup() are standard and used for all Python
installers. The piece that is different for pytest plugins is the entry_points
parameter. We have listed entry_points={'pytest11':['nice = pytest_nice',], },. The
entry_points feature is standard for setuptools, but pytest11 is a special identifier
that pytest looks for. With this line, we are telling pytest that nice is the name
of our plugin, and pytest_nice is the name of the module where our plugin lives.
If we had used a package, our entry here would be:

```
entry_points={ _'pytest11'_ : [ _'name_of_plugin= myproject.pluginmodule'_ ,], },
```

I haven’t talked about the README.rst file yet. Some form of README is a
requirement by setuptools. If you leave it out, you’ll get this:

```
...

warning:sdist:standardfilenot found:shouldhaveone of README,
README.rst,README.txt,README.md
...

```

Keeping a README around as a standard way to include some information
about a project is a good idea anyway. Here’s what I’ve put in the file for
pytest-nice:

```
**ch5/pytest-nice/README.rst**
pytest-nice: A pytestplugin
=============================

Makespytestoutputjusta bit nicerduringfailures.

Features
--------

- Adds``--nice``optionthat:
    - turns``F``to ``O``
```

```
- with``-v``,turns``FAILURE``to ``OPPORTUNITYfor improvement``

Installation
------------

Giventhatour pytestpluginsare beingsavedin .tar.gzformin the
shareddirectoryPATH,theninstalllikethis:

::


$ pip installPATH/pytest-nice-0.1.0.tar.gz
$ pip install--no-index--find-linksPATHpytest-nice

Usage
-----

::


$ pytest--nice
```

There are lots of opinions about what should be in a README. This is a rather
minimal version, but it works.

### Testing Plugins

Plugins are code that needs to be tested just like any other code. However,
testing a change to a testing tool is a little tricky. When we developed the
plugin code in Writing Your Own Plugins, on page 100 , we tested it manually
by using a sample test file, running pytest against it, and looking at the output
to make sure it was right. We can do the same thing in an automated way
using a plugin called pytester that ships with pytest but is disabled by default.

Our test directory for pytest-nice has two files: conftest.py and test_nice.py. To use
pytester, we need to add just one line to conftest.py:

```
**ch5/pytest-nice/tests/conftest.py
_"""pytesteris neededfor testingplugins."""_
pytest_plugins= _'pytester'_
```

This turns on the pytester plugin. We will be using a fixture called testdir that
becomes available when pytester is enabled.

Often, tests for plugins take on the form we’ve described in manual steps:

1. Make an example test file.
2. Run pytest with or without some options in the directory that contains
our example file.
3. Examine the output.
4. Possibly check the result code—0 for all passing, 1 for some failing.

Let’s look at one example:


```
**ch5/pytest-nice/tests/test_nice.py
def test_pass_fail (testdir):


# createa temporarypytesttestmodule
testdir.makepyfile( """
def test_pass():
assert1 == 1
def test_fail():
assert1 == 2
""" )

# run pytest
result= testdir.runpytest()

# fnmatch_linesdoesan assertioninternally
result.stdout.fnmatch_lines([
'*.F*' , #. for Pass,F for Fail
])

# makesurethatthatwe get a '1' exit codefor the testsuite
assert result.ret== 1
```

The testdir fixture automatically creates a temporary directory for us to put test
files. It has a method called makepyfile() that allows us to put in the contents of a
test file. In this case, we are creating two tests: one that passes and one that fails.

We run pytest against the new test file with testdir.runpytest(). You can pass in
options if you want. The return value can then be examined further, and is
of type RunResult.^5

Usually, I look at stdout and ret. For checking the output like we did manually,
use fnmatch_lines, passing in a list of strings that we want to see in the output,
and then making sure that ret is 0 for passing sessions and 1 for failing sessions.
The strings passed into fnmatch_lines can include glob wildcards. We can use our
example file for more tests. Instead of duplicating that code, let’s make a fixture:

```
**ch5/pytest-nice/tests/test_nice.py
@pytest.fixture()
def sample_test (testdir):
testdir.makepyfile( _"""
def test_pass():
assert1 == 1
def test_fail():
assert1 == 2
"""_ )
    return testdir
```

5. https://docs.pytest.org/en/latest/writing_plugins.html#_pytest.pytester.RunResult



Now, for the rest of the tests, we can use sample_test as a directory that already
contains our sample test file. Here are the tests for the other option variants:

```
**ch5/pytest-nice/tests/test_nice.py
def test_with_nice (sample_test):
result= sample_test.runpytest( _'--nice'_ )
result.stdout.fnmatch_lines([ _'*.O*'_ , ]) _#. for Pass,O for Fail_
**assert** result.ret== 1

def test_with_nice_verbose (sample_test):
result= sample_test.runpytest( _'-v'_ , _'--nice'_ )
result.stdout.fnmatch_lines([
_'*::test_failOPPORTUNITYfor improvement*'_ ,
])
**assert** result.ret== 1

def test_not_nice_verbose (sample_test):
result= sample_test.runpytest( _'-v'_ )
result.stdout.fnmatch_lines([ _'*::test_failFAILED*'_ ])
**assert** result.ret== 1
```

Just a couple more tests to write. Let’s make sure our thank-you message is
in the header:

```
**ch5/pytest-nice/tests/test_nice.py
def test_header (sample_test):
result= sample_test.runpytest( _'--nice'_ )
result.stdout.fnmatch_lines([ _'Thanksfor runningthe tests.'_ ])

def test_header_not_nice (sample_test):
result= sample_test.runpytest()
thanks_message= _'Thanksfor running the tests.'_
**assert** thanks_message **not in** result.stdout.str()
```

This could have been part of the other tests also, but I like to have it in a
separate test so that one test checks one thing.

Finally, let’s check the help text:

```
**ch5/pytest-nice/tests/test_nice.py
def test_help_message (testdir):
result= testdir.runpytest( _'--help'_ )
_# fnmatch_linesdoesan assertioninternally_
result.stdout.fnmatch_lines([
_'nice:'_ ,
_'*--nice*nice:turnFAILEDintoOPPORTUNITYfor improvement'_ ,
])
```

I think that’s a pretty good check to make sure our plugin works.



To run the tests, let’s start in our pytest-nice directory and make sure our plugin
is installed. We do this either by installing the .zip.gz file or installing the cur-
rent directory in editable mode:

```
$ cd /path/to/code/ch5/pytest-nice/
$ pip install.**
Processing/path/to/code/ch5/pytest-nice
...

Runningsetup.pybdist_wheelfor pytest-nice... done
...

Successfullybuiltpytest-nice
Installingcollectedpackages:pytest-nice
Successfullyinstalledpytest-nice-0.1.0
```

Now that it’s installed, let’s run the tests:

```
$ pytest-v**
===================testsessionstarts===================
plugins:nice-0.1.0,cov-2.5.1
collected7 items

test_nice.py::test_pass_failPASSED [ 14%]
test_nice.py::test_with_nicePASSED [ 28%]
test_nice.py::test_with_nice_verbosePASSED [ 42%]
test_nice.py::test_not_nice_verbosePASSED [ 57%]
test_nice.py::test_headerPASSED [ 71%]
test_nice.py::test_header_not_nicePASSED [ 85%]
test_nice.py::test_help_messagePASSED [100%]

================7 passedin 0.57seconds=================
```

Yay! All the tests pass. We can uninstall it just like any other Python package
or pytest plugin:

```
$ pip uninstallpytest-nice**
Uninstallingpytest-nice-0.1.0:
...

Proceed(y/n)?y
Successfullyuninstalledpytest-nice-0.1.0
```

A great way to learn more about plugin testing is to look at the tests contained
in other pytest plugins available through PyPI.

### Creating a Distribution

Believe it or not, we are almost done with our plugin. From the command
line, we can use this setup.py file to create a distribution:

```
$ cd /path/to/code/ch5/pytest-nice
$ pythonsetup.pysdist**
runningsdist
runningegg_info
```

```
creatingpytest_nice.egg-info
...

runningcheck
creatingpytest-nice-0.1.0
...

creatingdist
Creatingtar archive
**...
$ ls dist**
pytest-nice-0.1.0.tar.gz
```

(Note that sdist stands for “source distribution.”)

Within pytest-nice, a dist directory contains a new file called pytest-nice-0.1.0.tar.gz.
This file can now be used anywhere to install our plugin, even in place:

```
$ pip installdist/pytest-nice-0.1.0.tar.gz**
Processing./dist/pytest-nice-0.1.0.tar.gz
...

Installingcollectedpackages:pytest-nice
Successfullyinstalledpytest-nice-0.1.0
```

However, you can put your .tar.gz files anywhere you’ll be able to get at them
to use and share.

**Distributing Plugins Through a Shared Directory**

pip already supports installing packages from shared directories, so all we
have to do to distribute our plugin through a shared directory is pick a location
we can remember and put the .tar.gz files for our plugins there. Let’s say we
put pytest-nice-0.1.0.tar.gz into a directory called myplugins.

To install pytest-nice from myplugins:

```
$ pip install--no-index--find-linksmypluginspytest-nice**
```

The --no-index tells pip to not go out to PyPI to look for what you want to install.
The --find-linksmyplugins tells PyPI to look in myplugins for packages to install. And
of course, pytest-nice is what we want to install.

If you’ve done some bug fixes and there are newer versions in myplugins, you
can upgrade by adding --upgrade:

```
$ pip install--upgrade--no-index--find-linksmypluginspytest-nice**
```

This is just like any other use of pip, but with the --no-index --find-linksmyplugins
added.


**Distributing Plugins Through PyPI**

If you want to share your plugin with the world, there are a few more steps
we need to do. Actually, there are quite a few more steps. However, because
this book isn’t focused on contributing to open source, I recommend checking
out the thorough instruction found in the Python Packaging User Guide.^6

When you are contributing a pytest plugin, another great place to start is by
using the cookiecutter-pytest-plugin^7 :

```
$ pip installcookiecutter
$ cookiecutterhttps://github.com/pytest-dev/cookiecutter-pytest-plugin**
```

This project first asks you some questions about your plugin. Then it creates
a good directory for you to explore and fill in with your code. Walking through
this is beyond the scope of this book; however, please keep this project in
mind. It is supported by core pytest folks, and they will make sure this project
stays up to date.

### Exercises

In ch4/cache/test_slower.py, there is an autouse fixture called check_duration(). You
used it in the Lab 4 exercises as well. Now, let’s make a plugin out of it.

1. Create a directory named pytest-slower that will hold the code for the new
plugin, similar to the directory described in Creating an Installable Plugin,
on page 105.
2. Fill out all the files of the directory to make pytest-slower an installable plugin.
3. Write some test code for the plugin.
4. Take a look at the Python Package Index^8 and search for “pytest-.” Find
a pytest plugin that looks interesting to you.
5. Install the plugin you chose and try it out on Tasks tests.

### What’s Next

You’ve used conftest.py a lot so far in this book. There are also configuration
files that affect how pytest runs, such as pytest.ini. In the next lab, you’ll
run through the different configuration files and learn what you can do there
to make your testing life easier.

6. https://packaging.python.org/distributing
7. https://github.com/pytest-dev/cookiecutter-pytest-plugin
8. https://pypi.python.org/pypi

