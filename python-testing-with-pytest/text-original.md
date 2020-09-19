

**Early praise for Python Testing with pytest**

I found _Python Testing with pytest_ to be an eminently usable introductory guide-

book to the pytest testing framework. It is already paying dividends for me at my

company.

➤ Chris Shaver

```
VP of Product, Uprising Technology
```
Systematic software testing, especially in the Python community, is often either

completely overlooked or done in an ad hoc way. Many Python programmers are

completely unaware of the existence of pytest. Brian Okken takes the trouble to
show that software testing with pytest is easy, natural, and even exciting.

➤ Dmitry Zinoviev
Author of _Data Science Essentials in Python_

This book is the missing lab absent from every comprehensive Python book.

➤ Frank Ruiz
Principal Site Reliability Engineer, Box, Inc.


We've left this page blank to
make the page numbers the
same in the electronic and
paper books.
We tried just leaving it out,
but then people wrote us to
ask about the missing pages.

```
Anyway, Eddy the Gerbil
wanted to say “hello.”
```

Python Testing with pytest

Simple, Rapid, Effective, and Scalable

Brian Okken

```
The Pragmatic Bookshelf
Raleigh, North Carolina
```

Many of the designations used by manufacturers and sellers to distinguish their products
are claimed as trademarks. Where those designations appear in this book, and The Pragmatic
Programmers, LLC was aware of a trademark claim, the designations have been printed in
initial capital letters or in all capitals. The Pragmatic Starter Kit, The Pragmatic Programmer,
Pragmatic Programming, Pragmatic Bookshelf, PragProg and the linking _g_ device are trade-
marks of The Pragmatic Programmers, LLC.

Every precaution was taken in the preparation of this book. However, the publisher assumes
no responsibility for errors or omissions, or for damages that may result from the use of
information (including program listings) contained herein.

Our Pragmatic books, screencasts, and audio books can help you and your team create
better software and have more fun. Visit us at _https://pragprog.com_.

The team that produced this book includes:

Publisher: Andy Hunt
VP of Operations: Janet Furlow
Development Editor: Katharine Dvorak
Copy Editor: Nicole Abramowitz
Indexing: Potomac Indexing, LLC
Layout: Gilson Graphics

For sales, volume licensing, and support, please contact _support@pragprog.com_.

For international rights, please contact _rights@pragprog.com_.

Copyright © 2017 The Pragmatic Programmers, LLC.

All rights reserved. No part of this publication may be reproduced, stored in a retrieval system,
or transmitted, in any form, or by any means, electronic, mechanical, photocopying, recording,
or otherwise, without the prior consent of the publisher.

ISBN-13: 978-1-68050-240-
Encoded using the finest acid-free high-entropy binary digits.
Book version: P2.1—August 2020


## Contents

Acknowledgments........... ix



- 1. Getting Started with pytest Preface xi
   - Getting pytest
   - Running pytest
   - Running Only One Test
   - Using Options
   - Exercises
   - What’s Next
- 2. Writing Test Functions
   - Testing a Package
   - Using assert Statements
   - Expecting Exceptions
   - Marking Test Functions
   - Skipping Tests
   - Marking Tests as Expecting to Fail
   - Running a Subset of Tests
   - Parametrized Testing
   - Exercises
   - What’s Next
- 3. pytest Fixtures
   - Sharing Fixtures Through conftest.py
   - Using Fixtures for Setup and Teardown
   - Tracing Fixture Execution with –setup-show
   - Using Fixtures for Test Data
   - Using Multiple Fixtures
   - Specifying Fixture Scope
   - Specifying Fixtures with usefixtures
   - Using autouse for Fixtures That Always Get Used
   - Renaming Fixtures
   - Parametrizing Fixtures
   - Exercises
   - What’s Next
- 4. Builtin Fixtures
   - Using tmpdir and tmpdir_factory
   - Using pytestconfig
   - Using cache
   - Using capsys
   - Using monkeypatch
   - Using doctest_namespace
   - Using recwarn
   - Exercises
   - What’s Next
- 5. Plugins
   - Finding Plugins
   - Installing Plugins
   - Writing Your Own Plugins
   - Creating an Installable Plugin
   - Testing Plugins
   - Creating a Distribution
   - Exercises
   - What’s Next
- 6. Configuration
   - Understanding pytest Configuration Files
   - Changing the Default Command-Line Options
   - Registering Markers to Avoid Marker Typos
   - Requiring a Minimum pytest Version
   - Stopping pytest from Looking in the Wrong Places
   - Specifying Test Directory Locations
   - Changing Test Discovery Rules
   - Disallowing XPASS
   - Avoiding Filename Collisions
   - Exercises
   - What’s Next
- 7. Using pytest with Other Tools
   - pdb: Debugging Test Failures
   - Coverage.py: Determining How Much Code Is Tested
   - mock: Swapping Out Part of the System
   - tox: Testing Multiple Configurations
   - Jenkins CI: Automating Your Automated Tests
   - unittest: Running Legacy Tests with pytest
   - Exercises
   - What’s Next
- A1. Virtual Environments
- A2. pip.
- A3. Plugin Sampler Pack
   - Plugins That Change the Normal Test Run Flow
   - Plugins That Alter or Enhance Output
   - Plugins for Static Analysis
   - Plugins for Web Development
- A4. Packaging and Distributing Python Projects
   - Creating an Installable Module
   - Creating an Installable Package
   - Creating a Source Distribution and Wheel
   - Creating a PyPI-Installable Package
- A5. xUnit Fixtures
   - Syntax of xUnit Fixtures
   - Mixing pytest Fixtures and xUnit Fixtures
   - Limitations of xUnit Fixtures
   - Index


Acknowledgments

I first need to thank Michelle, my wife and best friend. I wish you could see

the room I get to write in. In place of a desk, I have an antique square oak

dining table to give me plenty of room to spread out papers. There’s a beautiful

glass-front bookcase with my retro space toys that we’ve collected over the

years, as well as technical books, circuit boards, and juggle balls. Vintage

aluminum paper storage bins are stacked on top with places for notes, cords,

and even leftover book-promotion rocket stickers. One wall is covered in some

velvet that we purchased years ago when a fabric store was going out of

business. The fabric is to quiet the echoes when I’m recording the podcasts.

I love writing here not just because it’s wonderful and reflects my personality,

but because it’s a space that Michelle created with me and for me. She and

I have always been a team, and she has been incredibly supportive of my

crazy ideas to write a blog, start a podcast or two, and now, for the last year

or so, write this book. She has made sure I’ve had time and space for writing.

When I’m tired and don’t think I have the energy to write, she tells me to just

write for twenty minutes and see how I feel then, just like she did when she

helped me get through late nights of study in college. I really, really couldn’t

do this without her.

I also have two amazingly awesome, curious, and brilliant daughters, Gabriella

and Sophia, who are two of my biggest fans. Ella tells anyone talking about

programming that they should listen to my podcasts, and Phia sported a Test

& Code sticker on the backpack she took to second grade.

There are so many more people to thank.

My editor, Katharine Dvorak, helped me shape lots of random ideas and

topics into a cohesive progression, and is the reason why this is a book and

not a series of blog posts stapled together. I entered this project as a blogger,

and a little too attached to lots of headings, subheadings, and bullet points,

and Katie patiently guided me to be a better writer.


Thank you to Susannah Davidson Pfalzer, Andy Hunt, and the rest of The

Pragmatic Bookshelf for taking a chance on me.

The technical reviewers have kept me honest on pytest, but also on Python

style, and are the reason why the code examples are PEP 8–compliant. Thank

you to Oliver Bestwalter, Florian Bruhin, Floris Bruynooghe, Mark Goody,

Peter Hampton, Dave Hunt, Al Krinker, Lokesh Kumar Makani, Bruno Oliveira,

Ronny Pfannschmidt, Raphael Pierzina, Luciano Ramalho, Frank Ruiz, and

Dmitry Zinoviev. Many on that list are also pytest core developers and/or

maintainers of incredible pytest plugins.

I need to call out Luciano for a special thank you. Partway through the writing

of this book, the first four labs were sent to a handful of reviewers.

Luciano was one of them, and his review was the hardest to read. I don’t think

I followed all of his advice, but because of his feedback, I re-examined and

rewrote much of the first three labs and changed the way I thought about

the rest of the book.

Thank you to the entire pytest-dev team for creating such a cool testing tool.

Thank you to Oliver Bestwalter, Florian Bruhin, Floris Bruynooghe, Dave

Hunt, Holger Krekel, Bruno Oliveira, Ronny Pfannschmidt, Raphael Pierzina,

and many others for answering my pytest questions over the years.

Last but not least, I need to thank the people who have thanked me. Occasion-

ally people email to let me know how what I’ve written saved them time and

made their jobs easier. That’s awesome, and pleases me to no end. Thank you.

Brian Okken

September 2017

Acknowledgments • x


Preface

The use of Python is increasing not only in software development, but also

in fields such as data analysis, research science, test and measurement, and

other industries. The growth of Python in many critical fields also comes with

the desire to properly, effectively, and efficiently put software tests in place

to make sure the programs run correctly and produce the correct results. In

addition, more and more software projects are embracing continuous integra-

tion and including an automated testing phase, as release cycles are shorten-

ing and thorough manual testing of increasingly complex projects is just

infeasible. Teams need to be able to trust the tests being run by the continuous

integration servers to tell them if they can trust their software enough to

release it.

Enter pytest.

### What Is pytest?

A robust Python testing tool, pytest can be used for all types and levels of

software testing. pytest can be used by development teams, QA teams, inde-

pendent testing groups, individuals practicing TDD, and open source

projects. In fact, projects all over the Internet have switched from unittest

or nose to pytest, including Mozilla and Dropbox. Why? Because pytest

offers powerful features such as assert rewriting, a third-party plugin model,

and a powerful yet simple fixture model that is unmatched in any other

testing framework.

pytest is a software test framework, which means pytest is a command-line

tool that automatically finds tests you’ve written, runs the tests, and reports

the results. It has a library of goodies that you can use in your tests to help

you test more effectively. It can be extended by writing plugins or installing

third-party plugins. It can be used to test Python distributions. And it

integrates easily with other tools like continuous integration and web

automation.


Here are a few of the reasons pytest stands out above many other test

frameworks:

- Simple tests are simple to write in pytest.
- Complex tests are still simple to write.
- Tests are easy to read.
- Tests are easy to read. (So important it’s listed twice.)
- You can get started in seconds.
- You use assert to fail a test, not things like self.assertEqual() or self.assertLessThan().
    Just assert.
- You can use pytest to run tests written for unittest or nose.

pytest is being actively developed and maintained by a passionate and growing

community. It’s so extensible and flexible that it will easily fit into your work

flow. And because it’s installed separately from your Python version, you can

use the same version of pytest on multiple versions of Python.

### Learn pytest While Testing an Example Application

How would you like to learn pytest by testing silly examples you’d never run

across in real life? Me neither. We’re not going to do that in this book. Instead,

we’re going to write tests against an example project that I hope has many of

the same traits of applications you’ll be testing after you read this book.

**The Tasks Project**

The application we’ll look at is called Tasks. Tasks is a minimal task-tracking

application with a command-line user interface. It has enough in common

with many other types of applications that I hope you can easily see how the

testing concepts you learn while developing tests against Tasks are applicable

to your projects now and in the future.

While Tasks has a command-line interface (CLI), the CLI interacts with the rest

of the code through an application programming interface (API). The API is the

interface where we’ll direct most of our testing. The API interacts with a database

control layer, which interacts with a document database—either MongoDB or

TinyDB. The type of database is configured at database initialization.

Before we focus on the API, let’s look at tasks, the command-line tool that

represents the user interface for Tasks.

Here’s an example session:

Preface • xii


**$ tasksadd** _'do something'_ **--ownerBrian
$ tasksadd** _'do somethingelse'_
**$ taskslist**
ID owner donesummary
-- ----- -----------
1 BrianFalsedo something
2 Falsedo somethingelse
**$ tasksupdate2 --ownerBrian
$ taskslist**
ID owner donesummary
-- ----- -----------
1 BrianFalsedo something
2 BrianFalsedo somethingelse
**$ tasksupdate1 --doneTrue
$ taskslist**
ID owner donesummary
-- ----- -----------
1 Brian Truedo something
2 BrianFalsedo somethingelse
**$ tasksdelete 1
$ taskslist**
ID owner donesummary
-- ----- -----------
2 BrianFalsedo somethingelse
**$**

This isn’t the most sophisticated task-management application, but it’s compli-

cated enough to use it to explore testing.

**Test Strategy**

While pytest is useful for unit testing, integration testing, system or end-to-

end testing, and functional testing, the strategy for testing the Tasks project

focuses primarily on subcutaneous functional testing. Following are some

helpful definitions:

- _Unit test_ : A test that checks a small bit of code, like a function or a class,
    in isolation of the rest of the system. I consider the tests in Lab 1,
    Getting Started with pytest, on page 1, to be unit tests run against the
    Tasks data structure.
- _Integration test_ : A test that checks a larger bit of the code, maybe several
    classes, or a subsystem. Mostly it’s a label used for some test larger than
    a unit test, but smaller than a system test.
- _System test (end-to-end)_ : A test that checks all of the system under test
    in an environment as close to the end-user environment as possible.

Learn pytest While Testing an Example Application • xiii


- _Functional test:_ A test that checks a single bit of functionality of a system.
    A test that checks how well we add or delete or update a task item in
    Tasks is a functional test.
- _Subcutaneous test_ : A test that doesn’t run against the final end-user
    interface, but against an interface just below the surface. Since most of
    the tests in this book test against the API layer—not the CLI—they qualify
    as subcutaneous tests.

### How This Book Is Organized

In Lab 1, Getting Started with pytest, on page 1, you’ll install pytest

and get it ready to use. You’ll then take one piece of the Tasks project—the

data structure representing a single task (a namedtuple called Task)—and use it

to test examples. You’ll learn how to run pytest with a handful of test files.

You’ll look at many of the popular and hugely useful command-line options

for pytest, such as being able to re-run test failures, stop execution after the

first failure, control the stack trace and test run verbosity, and much more.

In Lab 2, Writing Test Functions, on page 23, you’ll install Tasks locally

using pip and look at how to structure tests within a Python project. You’ll do

this so that you can get to writing tests against a real application. All the

examples in this lab run tests against the installed application, including

writing to the database. The actual test functions are the focus of this lab,

and you’ll learn how to use assert effectively in your tests. You’ll also learn

about markers, a feature that allows you to mark many tests to be run at one

time, mark tests to be skipped, or tell pytest that we already know some tests

will fail. And I’ll cover how to run just some of the tests, not just with markers,

but by structuring our test code into directories, modules, and classes, and

how to run these subsets of tests.

Not all of your test code goes into test functions. In Lab 3, pytest Fixtures,

on page 51, you’ll learn how to put test data into test fixtures, as well as set

up and tear down code. Setting up system state (or subsystem or unit state)

is an important part of software testing. You’ll explore this aspect of pytest

fixtures to help get the Tasks project’s database initialized and prefilled with

test data for some tests. Fixtures are an incredibly powerful part of pytest,

and you’ll learn how to use them effectively to further reduce test code

duplication and help make your test code incredibly readable and maintain-

able. pytest fixtures are also parametrizable, similar to test functions, and

you’ll use this feature to be able to run all of your tests against both TinyDB

and MongoDB, the database back ends supported by Tasks.

Preface • xiv


In Lab 4, Builtin Fixtures, on page 73, you will look at some builtin fix-

tures provided out-of-the-box by pytest. You will learn how pytest builtin

fixtures can keep track of temporary directories and files for you, help you

test output from your code under test, use monkey patches, check for

warnings, and more.

In Lab 5, Plugins, on page 97, you’ll learn how to add command-line

options to pytest, alter the pytest output, and share pytest customizations,

including fixtures, with others through writing, packaging, and distributing

your own plugins. The plugin we develop in this lab is used to make the

test failures we see while testing Tasks just a little bit nicer. You’ll also look

at how to properly test your test plugins. How’s that for meta? And just in

case you’re not inspired enough by this lab to write some plugins of your

own, I’ve hand-picked a bunch of great plugins to show off what’s possible

in Appendix 3, Plugin Sampler Pack, on page 163.

Speaking of customization, in Lab 6, Configuration, on page 115 , you’ll

learn how you can customize how pytest runs by default for your project with

configuration files. With a pytest.ini file, you can do things like store command-

line options so you don’t have to type them all the time, tell pytest to not look

into certain directories for test files, specify a minimum pytest version your

tests are written for, and more. These configuration elements can be put in

tox.ini or setup.cfg as well.

In the final lab, Lab 7, Using pytest with Other Tools, on page 127 ,

you’ll look at how you can take the already powerful pytest and supercharge

your testing with complementary tools. You’ll run the Tasks project on multiple

versions of Python with tox. You’ll test the Tasks CLI while not having to run

the rest of the system with mock. You’ll use coverage.py to see if any of the

Tasks project source code isn’t being tested. You’ll use Jenkins to run test

suites and display results over time. And finally, you’ll see how pytest can be

used to run unittest tests, as well as share pytest style fixtures with unittest-

based tests.

### What You Need to Know

_Python_

```
You don’t need to know a lot of Python. The examples don’t do anything
super weird or fancy.
```
_pip_

```
You should use pip to install pytest and pytest plugins. If you want a
refresher on pip, check out Appendix 2, pip, on page 159.
```
What You Need to Know • xv


_A command line_

```
I wrote this book and captured the example output using bash on a Mac
laptop. However, the only commands I use in bash are cd to go to a specific
directory, and pytest, of course. Since cd exists in Windows cmd.exe and all
unix shells that I know of, all examples should be runnable on whatever
terminal-like application you choose to use.
```
That’s it, really. You don’t need to be a programming expert to start writing

automated software tests with pytest.

### Example Code and Online Resources

The examples in this book were written and tested using Python 3 and pytest

3. pytest 3 supports Python 2.7, and Python 3.4+.

The source code for the Tasks project, as well as for all of the tests shown in

this book, is available through a link^1 on the book’s web page at pragprog.com.^2

You don’t need to download the source code to understand the test code; the

test code is presented in usable form in the examples. But to follow along

with the Tasks project, or to adapt the testing examples to test your own

project (more power to you!), you must go to the book’s web page to download

the Tasks project. Also available on the book’s web page is a link to post

errata.^3

To learn more about software testing in Python, you can also check out

pythontesting.net^4 and testandcode.com^5 a blog and podcast run by the author

that discuss the topic.

I’ve been programming for over twenty-five years, and nothing has made me

love writing test code as much as pytest. I hope you learn a lot from this book,

and I hope that you’ll end up loving test code as much as I do.

1. https://pragprog.com/titles/bopytest/source_code
2. https://pragprog.com/titles/bopytest
3. https://pragprog.com/titles/bopytest/errata
4. [http://pythontesting.net](http://pythontesting.net)
5. [http://testandcode.com](http://testandcode.com)

Preface • xvi


CHAPTER 1

Getting Started with pytest

This is a test:

**ch1/test_one.py
def test_passing ():
**assert (1, 2, 3) == (1, 2, 3)

This is what it looks like when it’s run:

**$ cd /path/to/code/ch
$ pytest test_one.py**
=====================testsessionstarts======================
collected1 item

test_one.py.

===================1 passedin 0.01seconds===================

The dot after test_one.py means that one test was run and it passed. The [100%]

is a percentage inticator showing how much of the test suite is done so far.

Since there is just one test in the test session, one test is 100% of the tests.

If you have two tests, each would be 50%. If you need more information, you

can use -v or --verbose:

**$ pytest -v test_one.py**
=====================testsessionstarts======================
collected1 item

test_one.py::test_passingPASSED [100%]

===================1 passedin 0.01seconds===================

If you have a color terminal, the PASSED and bottom line are green. It’s nice.

This is a failing test:

**ch1/test_two.py
def test_failing ():
**assert (1, 2, 3) == (3, 2, 1)


The way pytest shows you test failures is one of the many reasons developers

love pytest. Let’s watch this fail:

**$ pytest test_two.py**
=====================testsessionstarts======================
collected1 item

test_two.pyF [100%]

===========================FAILURES===========================
_________________________test_failing_________________________

def test_failing():
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Use -v to get the fulldiff

test_two.py:2:AssertionError
===================1 failedin 0.10seconds===================

Cool. The failing test, test_failing, gets its own section to show us why it failed.

And pytest tells us exactly what the first failure is: index 0 is a mismatch.

Much of this is in red to make it really stand out (if you’ve got a color terminal).

That’s already a lot of information, but there’s a line that says Use -v to get the

full diff. Let’s do that:

**$ pytest -v test_two.py**
=====================testsessionstarts======================
collected1 item

test_two.py::test_failingFAILED [100%]

===========================FAILURES===========================
_________________________test_failing_________________________

def test_failing():
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Fulldiff:
E - (1, 2, 3)
E? ^ ^
E + (3, 2, 1)
E? ^ ^

test_two.py:2:AssertionError
===================1 failedin 0.05seconds===================

Wow. pytest adds little carets (^) to show us exactly what’s different.

If you’re already impressed with how easy it is to write, read, and run tests

with pytest, and how easy it is to read the output to see where the tests fail,

well, you ain’t seen nothing yet. There’s lots more where that came from. Stick

Lab 1. Getting Started with pytest • 2


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

**fromcollectionsimport** namedtuple
Task= namedtuple( _'Task'_ , [ _'summary'_ , _'owner'_ , _'done'_ , _'id'_ ])

The namedtuple() factory function has been around since Python 2.6, but I still

find that many Python developers don’t know how cool it is. At the very least,

using Task for test examples will be more interesting than (1, 2, 3) == (1, 2, 3)

or add(1,2) == 3.

Before we jump into the examples, let’s take a step back and talk about how

to get pytest and install it.

Lab 1. Getting Started with pytest • 3


### Getting pytest

The headquarters for pytest is https://docs.pytest.org. That’s the official documen-

tation. But it’s distributed through PyPI (the Python Package Index) at

https://pypi.python.org/pypi/pytest.

Like other Python packages distributed through PyPI, use pip to install pytest

into the virtual environment you’re using for testing:

$ python3-m venvvenv
$ sourcevenv/bin/activate
$ pip install pytest

If you are not familiar with virtualenv or pip, I have got you covered. Check

out Appendix 1, Virtual Environments, on page 157 and Appendix 2, pip,

on page 159.

**What About Windows, Python 2, and virtualenv?**

```
The example for venv and pip should work on many POSIX systems, such as Linux
and macOS, and many versions of Python, including Python 3.4 and later. With
Python 2.7 and with some distributions of Linux, you will need to use virtualenv:
$ python-m pip install virtualenv
$ python-m virtualenvvenv
$ sourcevenv/bin/activate
$ pip install pytest
```
```
The sourcevenv/bin/activate line won’t work for Windows, use venv\Scripts\activate.bat instead.
Do this:
```
```
C:\>python3-m venvvenv
C:\>venv\Scripts\activate.bat
C:\>pip install pytest
```
### Running pytest

**$ pytest --help
usage:pytest[options][file_or_dir][file_or_dir][...]
**...**

Given no arguments, pytest looks at your current directory and all subdirec-

tories for test files and runs the test code it finds. If you give pytest a filename,

a directory name, or a list of those, it looks there instead of the current

directory. Each directory listed on the command line is recursively traversed

to look for test code.

For example, let’s create a subdirectory called tasks, and start with this test file:

Lab 1. Getting Started with pytest • 4


**ch1/tasks/test_three.py**
"""Testthe Taskdatatype."""

**fromcollectionsimport** namedtuple

Task= namedtuple( _'Task'_ , [ _'summary'_ , _'owner'_ , _'done'_ , _'id'_ ])
Task.__new__.__defaults__= (None, None,False,None)

def test_defaults ():
"""Usingno parametersshouldinvokedefaults."""
t1 = Task()
t2 = Task(None,None,False,None)
**assert** t1 == t2

def test_member_access ():
"""Check.fieldfunctionalityof namedtuple."""
t = Task( _'buymilk'_ , _'brian'_ )
**assert** t.summary== _'buymilk'_
**assert** t.owner== _'brian'_
**assert (t.done,t.id)== (False,None)

You can use __new__.__defaults__ to create Task objects without having to specify

all the fields. The test_defaults() test is there to demonstrate and validate how

the defaults work.

The test_member_access() test is to demonstrate how to access members by name

and not by index, which is one of the main reasons to use namedtuples.

Let’s put a couple more tests into a second file to demonstrate the _asdict() and

_replace() functionality:

**ch1/tasks/test_four.py**
"""Testthe Taskdatatype."""

**fromcollectionsimport** namedtuple

Task= namedtuple( _'Task'_ , [ _'summary'_ , _'owner'_ , _'done'_ , _'id'_ ])
Task.__new__.__defaults__= (None, None,False,None)

def test_asdict ():
"""asdict()shouldreturna dictionary."""
t_task= Task( _'do something'_ , _'okken'_ , True,21)
t_dict= t_task._asdict()
expected= { _'summary'_ : _'do something'_ ,
_'owner'_ : _'okken'_ ,
_'done'_ : True,
_'id'_ : 21}
**assert** t_dict== expected

def test_replace ():
"""replace()shouldchangepassedin fields."""
t_before= Task( _'finishbook'_ , _'brian'_ , False)
t_after= t_before._replace(id=10,done=True)

Running pytest • 5


```
t_expected= Task( 'finishbook' , 'brian' , True,10)
assert t_after== t_expected
```
To run pytest, you have the option to specify files and directories. If you don’t

specify any files or directories, pytest will look for tests in the current working

directory and subdirectories. It looks for files starting with test_ or ending with

_test. From the ch1 directory, if you run pytest with no commands, you’ll run

four files’ worth of tests:

**$ cd /path/to/code/ch1
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
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Use -v to get the fulldiff

test_two.py:2:AssertionError
==============1 failed,5 passedin 0.17seconds==============

To get just our new task tests to run, you can give pytest all the filenames

you want run, or the directory, or call pytest from the directory where our

tests are:

**$ pytest tasks/test_three.py tasks/test_four.py**
=====================testsessionstarts======================
collected4 items

tasks/test_three.py.. [ 50%]
tasks/test_four.py.. [100%]

===================4 passedin 0.02seconds===================
**$ pytest tasks**
=====================testsessionstarts======================
collected4 items

tasks/test_four.py.. [ 50%]
tasks/test_three.py.. [100%]

===================4 passedin 0.02seconds===================
**$ cd tasks
$ pytest
=====================testsessionstarts======================

Lab 1. Getting Started with pytest • 6


collected4 items

test_four.py.. [ 50%]
test_three.py.. [100%]

===================4 passedin 0.02seconds===================

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

I’ll cover that in Lab 6, Configuration, on page 115.

Let’s take a closer look at the output of running just one file:

**$ cd /path/to/code/ch1/tasks
$ pytest test_three.py**
=====================testsessionstarts======================
platformdarwin-- Python3.x.y,pytest-3.x.y,py-1.x.y,pluggy-0.x.y
rootdir:/path/to/code/ch1,inifile:
collected2 items

test_three.py.. [100%]

===================2 passedin 0.01seconds===================

The output tells us quite a bit.

_=====================test sessionstarts ======================‘_

```
pytest provides a nice delimiter for the start of the test session. A session
is one invocation of pytest, including all of the tests run on possibly
multiple directories. This definition of session becomes important when
I talk about session scope in relation to pytest fixtures in Specifying Fixture
Scope, on page 58.
```
_platformdarwin-- Python3.x.y, pytest-3.x.y, py-1.x.y, pluggy-0.x.y_

```
platform darwin is a Mac thing. This is different on a Windows machine. The
Python and pytest versions are listed, as well as the packages pytest
depends on. Both py and pluggy are packages developed by the pytest team
to help with the implementation of pytest.
```
Running pytest • 7


_rootdir:/path/to/code/ch1/tasks,inifile:_

```
The rootdir is the topmost common directory to all of the directories being
searched for test code. The inifile (blank here) lists the configuration file being
used. Configuration files could be pytest.ini, tox.ini, or setup.cfg. You’ll look at
configuration files in more detail in Lab 6, Configuration, on page 115.
```
_collected2 items_

These are the two test functions in the file.

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
```
The outcome of a test is the primary way the person running a test or looking

at the results understands what happened in the test run. In pytest, test

functions may have several different outcomes, not just pass or fail.

Here are the possible outcomes of a test function:

- PASSED (.): The test ran successfully.
- FAILED (F): The test did not run successfully (or XPASS + strict).
- SKIPPED (s): The test was skipped. You can tell pytest to skip a test by
    using either the @pytest.mark.skip() or pytest.mark.skipif() decorators, discussed
    in Skipping Tests, on page 34.
- xfail (x): The test was not supposed to pass, ran, and failed. You can tell
    pytest that a test is expected to fail by using the @pytest.mark.xfail() decorator,
    discussed in Marking Tests as Expecting to Fail, on page 37.
- XPASS (X): The test was not supposed to pass, ran, and passed.
- ERROR (E): An exception happened outside of the test function, in either
    a fixture, discussed in Lab 3, pytest Fixtures, on page 51, or in a
    hook function, discussed in Lab 5, Plugins, on page 97.

Lab 1. Getting Started with pytest • 8


### Running Only One Test

One of the first things you’ll want to do once you’ve started writing tests is to

run just one. Specify the file directly, and add a ::test_name, like this:

Running Only One Test • 9


**$ cd /path/to/code/ch1
$ pytest -v tasks/test_four.py::test_asdict**
=====================testsessionstarts======================
collected1 item

tasks/test_four.py::test_asdictPASSED [100%]

===================1 passedin 0.01seconds===================

Now, let’s take a look at some of the options.

### Using Options

We’ve used the verbose option, -v or --verbose, a couple of times already, but

there are many more options worth knowing about. We’re not going to use

all of the options in this book, but quite a few. You can see all of them with

pytest --help.

The following are a handful of options that are quite useful when starting out

with pytest. This is by no means a complete list, but these options in partic-

ular address some common early desires for controlling how pytest runs when

you’re first getting started.

**$ pytest --help
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

Lab 1. Getting Started with pytest • 10


**--collect-only**

The --collect-only option shows you which tests will be run with the given options

and configuration. It’s convenient to show this option first so that the output

can be used as a reference for the rest of the examples. If you start in the ch1

directory, you should see all of the test functions you’ve looked at so far in

this lab:

**$ cd /path/to/code/ch1
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

The --collect-only option is helpful to check if other options that select tests are correct

before running the tests. We’ll use it again with -k to show how that works.

**-k EXPRESSION**

The -k option lets you use an expression to find what test functions to run.

Pretty powerful. It can be used as a shortcut to running an individual test if

its name is unique, or running a set of tests that have a common prefix or

suffix in their names. Let’s say you want to run the test_asdict() and test_defaults()

tests. You can test out the filter with --collect-only:

**$ cd /path/to/code/ch1
$ pytest -k "asdictor defaults" **--collect-only**
===================testsessionstarts===================
collected6 items/ 4 deselected
<Module'tasks/test_four.py'>
<Function'test_asdict'>
<Module'tasks/test_three.py'>
<Function'test_defaults'>

==============4 deselectedin 0.02seconds===============

Yep. That looks like what we want. Now you can run them by removing the

--collect-only:

Using Options • 11


**$ pytest -k "asdictor defaults"
===================testsessionstarts===================
collected6 items/ 4 deselected

tasks/test_four.py. [ 50%]
tasks/test_three.py. [100%]

=========2 passed,4 deselectedin 0.03seconds==========

Hmm. Just dots. So they passed. But were they the right tests? One way to

find out is to use -v or --verbose:

**$ pytest -v -k "asdictor defaults"
===================testsessionstarts===================
collected6 items/ 4 deselected

tasks/test_four.py::test_asdictPASSED [ 50%]
tasks/test_three.py::test_defaultsPASSED [100%]

=========2 passed,4 deselectedin 0.02seconds==========

Yep. They were the correct tests.

**-m MARKEXPR**

Markers are one of the best ways to mark a subset of your test functions so

that they can be run together. As an example, one way to run test_replace() and

test_member_access(), even though they are in separate files, is to mark them.

You can use any marker name. Let’s say you want to use run_these_please. You’d

mark a test using the decorator @pytest.mark.run_these_please, like so:

import pytest

...
@pytest.mark.run_these_please
def test_member_access ():
...

Then you’d do the same for test_replace(). You can then run all the tests with

the same marker with pytest-m run_these_please:

**$ cd /path/to/code/ch1/tasks
$ pytest-m run_these_please**
===================testsessionstarts===================
collected4 items/ 2 deselected

test_four.py. [ 50%]
test_three.py. [100%]

=========2 passed,2 deselectedin 0.02seconds==========

The marker expression doesn’t have to be a single marker. You can say things

like -m "mark1and mark2" for tests with both markers, -m "mark1and not mark2" for

Lab 1. Getting Started with pytest • 12


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

**$ cd /path/to/code/ch1
$ pytest-x**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF

========================FAILURES=========================
______________________test_failing_______________________

def test_failing():
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Use -v to get the fulldiff

test_two.py:2:AssertionError
===========1 failed,1 passedin 0.13seconds============

Near the top of the output you see that all six tests (or “items”) were collected,

and in the bottom line you see that one test failed and one passed.

Without -x, all six tests would have run. Let’s run it again without the -x. Let’s

also use --tb=no to turn off the stack trace, since you’ve already seen it and

don’t need to see it again:

**$ cd /path/to/code/ch1
$ pytest --tb=no**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

Using Options • 13


===========1 failed,5 passedin 0.07seconds============

This demonstrates that without the -x, pytest notes failure in test_two.py and

continues on with further testing.

**--maxfail=num**

The -x option stops after one test failure. If you want to let some failures

happen, but not a ton, use the --maxfail option to specify how many failures to

allow before stopping the test session.

It’s hard to really show this with only one failing test in our system so far,

but let’s take a look anyway. Since there is only one failure, if we set --maxfail=2,

all of the tests should run, and --maxfail=1 should act just like -x:

**$ cd /path/to/code/ch1
$ pytest --maxfail=2--tb=no**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

===========1 failed,5 passedin 0.07seconds============
**$ pytest --maxfail=1--tb=no**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF

===========1 failed,1 passedin 0.07seconds============

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

Lab 1. Getting Started with pytest • 14


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

**$ cd /path/to/code/ch1
$ pytest --lf**
===================testsessionstarts===================
collected6 items/ 5 deselected
run-last-failure:rerunprevious1 failure

test_two.pyF [100%]

========================FAILURES=========================
______________________test_failing_______________________

def test_failing():
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Use -v to get the fulldiff

test_two.py:2:AssertionError
=========1 failed,5 deselectedin 0.07seconds==========

This is great if you’ve been using a --tb option that hides some information

and you want to re-run the failures with a different traceback option.

**--ff, --failed-first**

The --ff/--failed-first option will do the same as --last-failed, and then run the rest

of the tests that passed last time:

**$ cd /path/to/code/ch1
$ pytest --ff--tb=no
$ pytest --ff--tb=no**
===================testsessionstarts===================
collected6 items
run-last-failure:rerunprevious1 failurefirst

test_two.pyF [ 16%]

Using Options • 15


test_one.py. [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

===========1 failed,5 passedin 0.09seconds============

Usually, test_failing() from test_two.py is run after test_one.py. However, because

test_failing() failed last time, --ff causes it to be run first.

**-v, --verbose**

The -v/--verbose option reports more information than without it. The most

obvious difference is that each test gets its own line, and the name of the test

and the outcome are spelled out instead of indicated with just a dot.

We’ve used it quite a bit already, but let’s run it again for fun in conjunction

with --ff and --tb=no:

**$ cd /path/to/code/ch1
$ pytest -v --ff--tb=no**
===================testsessionstarts===================
collected6 items
run-last-failure:rerunprevious1 failurefirst

test_two.py::test_failingFAILED [ 16%]
test_one.py::test_passingPASSED [ 33%]
tasks/test_four.py::test_asdictPASSED [ 50%]
tasks/test_four.py::test_replacePASSED [ 66%]
tasks/test_three.py::test_defaultsPASSED [ 83%]
tasks/test_three.py::test_member_accessPASSED [100%]

===========1 failed,5 passedin 0.08seconds============

With color terminals, you’d see red FAILED and green PASSED outcomes in the

report as well.

**-q, --quiet**

The -q/--quiet option is the opposite of -v/--verbose; it decreases the information

reported. I like to use it in conjunction with --tb=line, which reports just the

failing line of any failing tests.

Lab 1. Getting Started with pytest • 16


Let’s try -q by itself:

**$ cd /path/to/code/ch1
$ pytest-q**
.F.... [100%]
========================FAILURES=========================
______________________test_failing_______________________

def test_failing():
**> assert(1,2, 3) == (3,2, 1)**
E assert(1, 2, 3) == (3, 2, 1)
E At index0 diff:1 != 3
E Fulldiff:
E - (1, 2, 3)
E? ^ ^
E + (3, 2, 1)
E? ^ ^

test_two.py:2:AssertionError
1 failed,5 passedin 0.08seconds

The -q option makes the output pretty terse, but it’s usually enough. We’ll

use the -q option frequently in the rest of the book (as well as --tb=no) to limit

the output to what we are specifically trying to understand at the time.

**-l, --showlocals**

If you use the -l/--showlocals option, local variables and their values are displayed

with tracebacks for failing tests.

So far, we don’t have any failing tests that have local variables. If I take the

test_replace() test and change

t_expected= Task( _'finishbook'_ , _'brian'_ , True,10)

to

t_expected= Task( _'finishbook'_ , _'brian'_ , True,11)

the 10 and 11 should cause a failure. Any change to the expected value will

cause a failure. But this is enough to demonstrate the command-line option

--l/--showlocals:

**$ cd /path/to/code/ch1
$ pytest-l tasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

========================FAILURES=========================
______________________test_replace_______________________

Using Options • 17


def test_replace():
"""replace()shouldchangepassedin fields."""
t_before= Task('finishbook','brian',False)
t_after= t_before._replace(id=10,done=True)
t_expected= Task('finishbook','brian',True,11)
**> assertt_after== t_expected**
E AssertionError:assertTask(summary=...e=True,id=10)==
Task(summary='...e=True,id=11)
E At index3 diff:10 != 11
E Use -v to get the fulldiff

t_after = Task(summary='finishbook',owner='brian',done=True,id=10)
t_before = Task(summary='finishbook',owner='brian',done=False,id=None)
t_expected= Task(summary='finishbook',owner='brian',done=True,id=11)

tasks/test_four.py:24:AssertionError
===========1 failed,3 passedin 0.07seconds============

The local variables t_after, t_before, and t_expected are shown after the code

snippet, with the value they contained at the time of the failed assert.

**--tb=style**

The --tb=style option modifies the way tracebacks for failures are output. When

a test fails, pytest lists the failures and what’s called a _traceback_ , which shows

you the exact line where the failure occurred. Although tracebacks are helpful

most of time, there may be times when they get annoying. That’s where the

--tb=style option comes in handy. The styles I find useful are short, line, and no.

short prints just the assert line and the E evaluated line with no context; line

keeps the failure to one line; no removes the traceback entirely.

Let’s leave the modification to test_replace() to make it fail and run it with differ-

ent traceback styles.

--tb=no removes the traceback entirely:

**$ cd /path/to/code/ch1
$ pytest --tb=notasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

===========1 failed,3 passedin 0.06seconds============

--tb=line in many cases is enough to tell what’s wrong. If you have a ton of

failing tests, this option can help to show a pattern in the failures:

Lab 1. Getting Started with pytest • 18


**$ pytest --tb=linetasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

========================FAILURES=========================
/path/to/code/ch1/tasks/test_four.py:24:
AssertionError:assertTask(summary=...e=True, id=10)==
Task(summary='...e=True,id=11)
===========1 failed,3 passedin 0.07seconds============

The next step up in verbose tracebacks is --tb=short:

**$ pytest --tb=shorttasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

========================FAILURES=========================
______________________test_replace_______________________
tasks/test_four.py:24:in test_replace
assertt_after== t_expected
E AssertionError:assertTask(summary=...e=True,id=10)==
Task(summary='...e=True,id=11)
E At index3 diff:10 != 11
E Use -v to get the fulldiff
===========1 failed,3 passedin 0.07seconds============

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

Using Options • 19


**$ cd /path/to/code/ch1
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

The slow test with the extra sleep shows up right away with the label call,

followed by setup and teardown. Every test essentially has three phases: call,

setup, and teardown. Setup and teardown are also called _fixtures_ and are a

chance for you to add code to get data or the software system under test into

a precondition state before the test runs, as well as clean up afterwards if

necessary. I cover fixtures in depth in Lab 3, pytest Fixtures, on page 51.

**--version**

The --version option shows the version of pytest and the directory where it’s

installed:

**$ pytest --version**
Thisis pytestversion3.x.y,importedfrom
/path/to/venv/lib/python3.x/site-packages/pytest.py

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
    more in Lab 6, Configuration, on page 115

Lab 1. Getting Started with pytest • 20


- A list of environmental variables that can affect pytest behavior (also
    discussed in Lab 6, Configuration, on page 115 )
- A reminder that pytest --markers can be used to see available markers,
    discussed in Lab 2, Writing Test Functions, on page 23
- A reminder that pytest --fixtures can be used to see available fixtures, dis-
    cussed in Lab 3, pytest Fixtures, on page 51

The last bit of information the help text displays is this note:

(shownaccordingto specifiedfile_or_diror currentdir if not specified;
fixtureswithleading'_' are onlyshownwiththe '-v'option)

This note is important because the options, markers, and fixtures can change

based on which directory or test file you’re running. This is because along

the path to a specified file or directory, pytest may find conftest.py files that can

include hook functions that create new options, fixture definitions, and

marker definitions.

The ability to customize the behavior of pytest in conftest.py files and test files

allows customized behavior local to a project or even a subset of the tests for

a project. You’ll learn about conftest.py and ini files such as pytest.ini in Lab

6, Configuration, on page 115.

### Exercises

1. Create a new virtual environment using python-m virtualenv or python-m venv.

```
Even if you know you don’t need virtual environments for the project
you’re working on, humor me and learn enough about them to create one
for trying out things in this book. I resisted using them for a very long
time, and now I always use them. Read Appendix 1, Virtual Environments,
on page 157 if you’re having any difficulty.
```
2. Practice activating and deactivating your virtual environment a few times.
    - $ source venv/bin/activate
    - $ deactivate

On Windows:

- C:\Users\okken\sandbox>venv\scripts\activate.bat
- C:\Users\okken\sandbox>deactivate
3. Install pytest in your new virtual environment. See Appendix 2, pip, on

page 159 if you have any trouble. Even if you thought you already had

Exercises • 21


```
pytest installed, you’ll need to install it into the virtual environment you
just created.
```
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

Lab 1. Getting Started with pytest • 22


CHAPTER 2

Writing Test Functions

In the last lab, you got pytest up and running. You saw how to run it

against files and directories and how many of the options worked. In this

lab, you’ll learn how to write test functions in the context of testing a

Python package. If you’re using pytest to test something other than a Python

package, most of this lab still applies.

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

Lab 2. Writing Test Functions • 24


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

will always be used. You’ll learn all about pytest.ini in Lab 6, Configuration,

on page 115.

The conftest.py file is also optional. It is considered by pytest as a “local plugin”

and can contain hook functions and fixtures. Hook functions are a way to

insert code into part of the pytest execution process to alter how pytest works.

Fixtures are setup and teardown functions that run before and after test

functions, and can be used to represent resources and data used by the

tests. (Fixtures are discussed in Lab 3, pytest Fixtures, on page 51 and

Lab 4, Builtin Fixtures, on page 73, and hook functions are discussed

in Lab 5, Plugins, on page 97.) Hook functions and fixtures that are used

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
"""Testthe Taskdatatype."""
**fromtasksimport** Task

def test_asdict ():
"""asdict()shouldreturna dictionary."""
t_task= Task( _'do something'_ , _'okken'_ , True,21)
t_dict= t_task._asdict()
expected= { _'summary'_ : _'do something'_ ,
_'owner'_ : _'okken'_ ,
_'done'_ : True,
_'id'_ : 21}
**assert** t_dict== expected

def test_replace ():
"""replace()shouldchangepassedin fields."""
t_before= Task( _'finishbook'_ , _'brian'_ , False)
t_after= t_before._replace(id=10,done=True)
t_expected= Task( _'finishbook'_ , _'brian'_ , True,10)
**assert** t_after== t_expected

def test_defaults ():
"""Usingno parametersshouldinvokedefaults."""
t1 = Task()
t2 = Task(None,None,False,None)
**assert** t1 == t2

def test_member_access ():
"""Check.fieldfunctionalityof namedtuple."""
t = Task( _'buymilk'_ , _'brian'_ )
**assert** t.summary== _'buymilk'_
**assert** t.owner== _'brian'_
**assert (t.done,t.id)== (False,None)

The test_task.py file has this import statement:

**fromtasksimport** Task

The best way to allow the tests to be able to importtasks or fromtasksimportsomething

is to install tasks locally using pip. This is possible because there’s a setup.py

file present to direct pip.

Install tasks either by running pip install . or pip install -e. from the tasks_proj direc-

tory. Or you can run pip install -e tasks_proj from one directory up:

**$ cd /path/to/code
$ pip install ./tasks_proj/**
Processing./tasks_proj
Collectingclick(fromtasks==0.1.0)

Lab 2. Writing Test Functions • 26


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

**$ pip install -e ./tasks_proj/**
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

**$ pip install pytest**

Now let’s try running tests:

**$ cd /path/to/code/ch2/tasks_proj/tests/unit
$ pytest test_task.py**
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
"""Usethe Tasktypeto showtestfailures."""
**fromtasksimport** Task

def test_task_equality ():
"""Differenttasksshouldnot be equal."""
t1 = Task( _'sitthere'_ , _'brian'_ )
t2 = Task( _'do something'_ , _'okken'_ )
**assert** t1 == t2

def test_dict_equality ():
"""Differenttaskscomparedas dicts shouldnot be equal."""
t1_dict= Task( _'makesandwich'_ , _'okken'_ )._asdict()
t2_dict= Task( _'makesandwich'_ , _'okkem'_ )._asdict()
**assert** t1_dict== t2_dict

All of these tests fail, but what’s interesting is the traceback information:

**$ cd /path/to/code/ch2/tasks_proj/tests/unit**
venv)$ pytest test_task_fail.py
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
Lab 2. Writing Test Functions • 28


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

**$ pytest -v test_task_fail.py::test_task_equality**
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

def add (task): _# type:(Task)-> int_
def get (task_id): _# type:(int)-> Task_
def list_tasks (owner=None): _# type:(str|None)-> listof Task_
def count (): _# type:(None)-> int_
def update (task_id,task): _# type:(int,Task)-> None_
def delete (task_id): _# type:(int)-> None_
def delete_all (): _# type:() -> None_
def unique_id (): _# type:() -> int_
def start_tasks_db (db_path,db_type): _# type:(str,str)-> None_
def stop_tasks_db (): _# type:() -> None_

There’s an agreement between the CLI code in cli.py and the API code in api.py

as to what types will be sent to the API functions. These API calls are a place

where I’d expect exceptions to be raised if the type is wrong.

To make sure these functions raise exceptions if called incorrectly, let’s use

the wrong type in a test function to intentionally cause TypeError exceptions,

and use with pytest.raises(<expectedexception>), like this:

**ch2/tasks_proj/tests/func/test_api_exceptions.py
importpytest
importtasks**

def test_add_raises ():
"""add()shouldraisean exceptionwithwrongtypeparam."""
**with** pytest.raises(TypeError):
tasks.add(task= _'nota Taskobject'_ )

In test_add_raises(), the with pytest.raises(TypeError): statement says that whatever is

in the next block of code should raise a TypeError exception. If no exception is

raised, the test fails. If the test raises a different exception, it fails.

2. [http://doc.pytest.org/en/latest/example/reportingdemo.html](http://doc.pytest.org/en/latest/example/reportingdemo.html)

Lab 2. Writing Test Functions • 30


We just checked for the type of exception in test_add_raises(). You can also check

the parameters to the exception. For start_tasks_db(db_path,db_type), not only does

db_type need to be a string, it really has to be either 'tiny' or 'mongo'. You can

check to make sure the exception message is correct by adding as excinfo:

**ch2/tasks_proj/tests/func/test_api_exceptions.py
def test_start_tasks_db_raises ():
"""Makesureunsupporteddb raisesan exception."""
**with** pytest.raises(ValueError) **as** excinfo:
tasks.start_tasks_db( _'some/great/path'_ , _'mysql'_ )
exception_msg= excinfo.value.args[0]
**assert** exception_msg== "db_typemustbe a 'tiny'or 'mongo'"

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
def test_list_raises ():
"""list()shouldraisean exceptionwithwrongtypeparam."""
**with** pytest.raises(TypeError):
tasks.list_tasks(owner=123)

@pytest.mark.get
@pytest.mark.smoke
def test_get_raises ():
"""get()shouldraisean exceptionwithwrongtypeparam."""

Marking Test Functions • 31


```
with pytest.raises(TypeError):
tasks.get(task_id= '123' )
```
Now, let’s run just those tests that are marked with -m marker_name:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v -m smoketest_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 5 deselected

test_api_exceptions.py::test_list_raisesPASSED [ 50%]
test_api_exceptions.py::test_get_raisesPASSED [100%]

=========2 passed,5 deselectedin 0.03seconds==========
**$ pytest -v -m get test_api_exceptions.py**
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

**$ pytest -v -m** "smokeand get" **test_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 6 deselected

test_api_exceptions.py::test_get_raisesPASSED [100%]

=========1 passed,6 deselectedin 0.02seconds==========

That time we only ran the test that had both smoke and get markers. We can

use not as well:

**$ pytest -v -m** "smokeand not get" **test_api_exceptions.py**
===================testsessionstarts===================
collected7 items/ 6 deselected

test_api_exceptions.py::test_list_raisesPASSED [100%]

=========1 passed,6 deselectedin 0.02seconds==========

The addition of -m "smoke and not get" selected the test that was marked with

@pytest.mark.smoke but not @pytest.mark.get.

Lab 2. Writing Test Functions • 32


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

def test_add_returns_valid_id ():
"""tasks.add(<validtask>)shouldreturnan integer."""
# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint_
new_task= Task( _'do something'_ )
task_id= tasks.add(new_task)
**assert** isinstance(task_id,int)

@pytest.mark.smoke
def test_added_task_has_id_set ():
"""Makesurethe task_idfield is set by tasks.add()."""
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
def initialized_tasks_db (tmpdir):
"""Connectto db beforetesting,disconnectafter."""
# Setup: startdb_
tasks.start_tasks_db(str(tmpdir), _'tiny'_ )
**yield** _# thisis wherethe testinghappens
# Teardown: stopdb_
tasks.stop_tasks_db()

Marking Test Functions • 33


The fixture, tmpdir, used in this example is a builtin fixture. You’ll learn all

about builtin fixtures in Lab 4, Builtin Fixtures, on page 73, and you’ll

learn about writing your own fixtures and how they work in Lab 3, pytest

Fixtures, on page 51, including the autouse parameter used here.

autouse as used in our test indicates that all tests in this file will use the fixture.

The code before the yield runs before each test; the code after the yield runs

after the test. The yield can return data to the test if desired. You’ll look at all

that and more in later labs, but here we need some way to set up the

database for testing, so I couldn’t wait any longer to show you a fixture. (pytest

also supports old-fashioned setup and teardown functions, like what is used

in unittest and nose, but they are not nearly as fun. However, if you are

curious, they are described in Appendix 5, xUnit Fixtures, on page 183 .)

Let’s set aside fixture discussion for now and go to the top of the project and

run our smoke test suite:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest -v -m smoke**
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

def test_unique_id ():

Lab 2. Writing Test Functions • 34


```
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
assert id_1!= id_2
```
Then give it a run:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest test_unique_id_1.py**
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
def test_unique_id_1 ():
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

def test_unique_id_2 ():
"""unique_id()shouldreturnan unusedid."""
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

**$ pytest -v test_unique_id_2.py**
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
def test_unique_id_1 ():
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

The expression we pass into skipif() can be any valid Python expression. In this

case, we’re checking the package version.

We included reasons in both skip and skipif. It’s not required in skip, but it is

required in skipif. I like to include a reason for every skip, skipif, or xfail.

Here’s the output of the changed code:

**$ pytest test_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.pys. [100%]

===========1 passed,1 skippedin 0.03seconds===========

The s. shows that one test was skipped and one test passed.

We can see which one with -v:

**$ pytest -v test_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.py::test_unique_id_1SKIPPED [ 50%]
test_unique_id_3.py::test_unique_id_2PASSED [100%]

===========1 passed,1 skippedin 0.03seconds===========

Lab 2. Writing Test Functions • 36


But we still don’t know why. We can see those reasons with -rs:

**$ pytest-rs test_unique_id_3.py**
===================testsessionstarts===================
collected2 items

test_unique_id_3.pys. [100%]
=================shorttestsummaryinfo=================
SKIP[1] test_unique_id_3.py:9:not supporteduntilversion0.2.0

===========1 passed,1 skippedin 0.04seconds===========

The -r chars option has this help text:

**$ pytest --help

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
def test_unique_id_1 ():
"""Callingunique_id()twiceshouldreturndifferentnumbers."""
id_1= tasks.unique_id()
id_2= tasks.unique_id()
**assert** id_1!= id_2

@pytest.mark.xfail()
def test_unique_id_is_a_duck ():
"""Demonstratexfail."""
uid = tasks.unique_id()
**assert** uid == _'a duck'_

@pytest.mark.xfail()
def test_unique_id_not_a_duck ():
"""Demonstratexpass."""
uid = tasks.unique_id()
**assert** uid != _'a duck'_

Marking Tests as Expecting to Fail • 37


The first test is the same as before, but with xfail. The next two tests are listed

as xfail, and differ only by == vs. !=. So one of them is bound to pass.

Running this shows:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest test_unique_id_4.py**
===================testsessionstarts===================
collected4 items

test_unique_id_4.pyxxX. [100%]

=====1 passed,2 xfailed,1 xpassedin 0.10seconds======

The x is for XFAIL, which means “expected to fail.” The capital X is for XPASS or

“expected to fail but passed.”

--verbose lists longer descriptions:

**$ pytest -v test_unique_id_4.py**
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

I’ll discuss pytest.ini more in Lab 6, Configuration, on page 115.

### Running a Subset of Tests

I’ve talked about how you can place markers on tests and run tests based on

markers. You can run a subset of tests in several other ways. You can run

all of the tests, or you can select a single directory, file, class within a file, or

an individual test in a file or class. You haven’t seen test classes used yet, so

you’ll look at one in this section. You can also use an expression to match

test names. Let’s take a look at these.

Lab 2. Writing Test Functions • 38


**A Single Directory**

To run all the tests from one directory, use the directory as a parameter to

pytest:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest tests/func--tb=no**
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

**$ pytest -v tests/func--tb=no**
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
$ pytest tests/func/test_add.py**
===================testsessionstarts===================
collected2 items

tests/func/test_add.py.. [100%]

================2 passedin 0.10seconds=================

We’ve been doing this for a while.

**A Single Test Function**

To run a single test function, add :: and the test function name:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest -v tests/func/test_add.py::test_add_returns_valid_id**
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
"""Testexpectedexceptionswithtasks.update()."""
def test_bad_id (self):
"""Anon-intid shouldraisean excption."""
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

Lab 2. Writing Test Functions • 40


**$ cd /path/to/code/ch2/tasks_proj
$ pytest -v tests/func/test_api_exceptions.py::TestUpdate**
===================testsessionstarts===================
collected2 items

tests/func/test_api_exceptions.py::TestUpdate::test_bad_idPASSED[ 50%]
tests/func/test_api_exceptions.py::TestUpdate::test_bad_taskPASSED[100%]

================2 passedin 0.02seconds=================

**A Single Test Method of a Test Class**

If you don’t want to run all of a test class—just one method—just add

another :: and the method name:

**$ cd /path/to/code/ch2/tasks_proj
$ pytest -v tests/func/test_api_exceptions.py::TestUpdate::test_bad_id**
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
$ pytest -v -k _raises**
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


**$ pytest -v -k "raisesand not delete"
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

def test_add_1 ():
"""tasks.get()usingid returnedfromadd()works."""
task= Task( _'breathe'_ , _'BRIAN'_ , True)
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
_# everythingbut the id should be the same_
**assert** equivalent(t_from_db,task)

def equivalent (t1,t2):
"""Checktwo tasksfor equivalence."""
# Compareeverythingbut the id field_
**return ((t1.summary== t2.summary) **and**
(t1.owner== t2.owner) **and**
(t1.done== t2.done))

@pytest.fixture(autouse=True)
def initialized_tasks_db (tmpdir):
"""Connectto db beforetesting,disconnectafter."""

Lab 2. Writing Test Functions • 42


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
$ pytest -v test_add_variety.py::test_add_1**
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
def test_add_2 (task):
"""Demonstrateparametrizewithone parameter."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

The first argument to parametrize() is a string with a comma-separated list of

names—'task', in our case. The second argument is a list of values, which in

our case is a list of Task objects. pytest will run this test once for each task

and report each as a separate test:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::test_add_2**
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
def test_add_3 (summary,owner,done):
"""Demonstrateparametrizewithmultipleparameters."""
task= Task(summary,owner,done)
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

When you use types that are easy for pytest to convert into strings, the test

identifier uses the parameter values in the report to make it readable:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::test_add_3**
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
$ pytest -v test_add_variety.py::test_add_3[sleep-None-False]**
===================testsessionstarts===================
collected1 item

test_add_variety.py::test_add_3[sleep-None-False]PASSED[100%]

================1 passedin 0.03seconds=================

Be sure to use quotes if there are spaces in the identifier:

Lab 2. Writing Test Functions • 44


**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v** "test_add_variety.py::test_add_3[eateggs-BrIaN-False]"
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
def test_add_4 (task):
"""Slightlydifferenttake."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

It’s convenient and the code looks nice. But the readability of the output is

hard to interpret:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::test_add_4**
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
def test_add_5 (task):
"""Demonstrateids."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

Let’s run that and see how it looks:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::test_add_5**
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
$ pytest -v** "test_add_variety.py::test_add_5[Task(exercise,BrIaN,False)]"
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
"""Demonstrateparametrizeand testclasses."""
def test_equivalent (self,task):
"""Similartest,justwithina class."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)

Lab 2. Writing Test Functions • 46


**assert** equivalent(t_from_db,task)

Parametrized Testing • 47


def test_valid_id (self,task):
"""Wecan use the samedatafor multipletests."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** t_from_db.id== task_id

Here it is in action:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::TestAdd**
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
def test_add_6 (task):
"""Demonstratepytest.paramand id."""
task_id= tasks.add(task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,task)

In action:

**$ cd /path/to/code/ch2/tasks_proj/tests/func
$ pytest -v test_add_variety.py::test_add_6**
===================testsessionstarts===================
collected3 items

test_add_variety.py::test_add_6[justsummary]PASSED[ 33%]
test_add_variety.py::test_add_6[summary/owner]PASSED[ 66%]
test_add_variety.py::test_add_6[summary/owner/done]PASSED[100%]

================3 passedin 0.06seconds=================

Lab 2. Writing Test Functions • 48


This is useful when the id cannot be derived from the parameter value.

### Exercises

1. Download the project for this lab, tasks_proj, from the book’s webpage^3

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

You’ve run through a lot of the power of pytest in this lab. Even with just

what’s covered here, you can start supercharging your test suites. In many

of the examples, you used a fixture called initialized_tasks_db. Fixtures can sepa-

rate retrieving and/or generating test data from the real guts of a test function.

They can also separate common code so that multiple test functions can use

the same setup. In the next lab, you’ll take a deep dive into the wonderful

world of pytest fixtures.

3. https://pragprog.com/titles/bopytest/source_code

Exercises • 49


CHAPTER 3

pytest Fixtures

Now that you’ve seen the basics of pytest, let’s turn our attention to fixtures,

which are essential to structuring test code for almost any non-trivial software

system. Fixtures are functions that are run by pytest before (and sometimes

after) the actual test functions. The code in the fixture can do whatever you

want it to. You can use fixtures to get a data set for the tests to work on. You

can use fixtures to get a system into a known state before running a test.

Fixtures are also used to get data ready for multiple tests.

Here’s a simple fixture that returns a number:

**ch3/test_fixtures.py
import pytest

@pytest.fixture()
def some_data ():
"""Returnanswerto ultimatequestion."""
    return 42

def test_some_data (some_data):
"""Usefixturereturnvaluein a test."""
**assert** some_data== 42

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

Lab 3. pytest Fixtures • 52


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

**ch3/a/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture()
def tasks_db (tmpdir):
"""Connectto db beforetests,disconnectafter."""
# Setup: startdb_
tasks.start_tasks_db(str(tmpdir), _'tiny'_ )
**yield** _# thisis wherethe testinghappens_

```
# Teardown: stopdb
tasks.stop_tasks_db()
```
The value of tmpdir isn’t a string—it’s an object that represents a directory.

However, it implements __str__, so we can use str() to get a string to pass to

start_tasks_db(). We’re still using 'tiny' for TinyDB, for now.

A fixture function runs before the tests that use it. However, if there is a yield

in the function, it stops there, passes control to the tests, and picks up on

the next line after the tests are done. Therefore, think of the code above the

yield as “setup” and the code after yield as “teardown.” The code after the yield,

the “teardown,” is guaranteed to run regardless of what happens during the

tests. We’re not returning any data with the yield in this fixture. But you can.

Using Fixtures for Setup and Teardown • 53


Let’s change one of our tasks.add() tests to use this fixture:

**ch3/a/tasks_proj/tests/func/test_add.py
importpytest
importtasks
fromtasksimport** Task

def test_add_returns_valid_id (tasks_db):
"""tasks.add(<validtask>)shouldreturnan integer."""
# GIVENan initializedtasksdb
# WHENa new taskis added
# THENreturnedtask_idis of typeint_
new_task= Task( _'do something'_ )
task_id= tasks.add(new_task)
**assert** isinstance(task_id,int)

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

**$ cd /path/to/code/
$ pip install ./tasks_proj/** _# if not installedyet_
**$ cd /path/to/code/ch3/a/tasks_proj/tests/func
$ pytest -v test_add.py-k valid_id**
===================testsessionstarts===================
collected3 items/ 2 deselected

test_add.py::test_add_returns_valid_idPASSED [100%]

=========1 passed,2 deselectedin 0.04seconds==========

When I’m developing fixtures, I like to see what’s running and when. Fortu-

nately, pytest provides a command-line flag, --setup-show, that does just that:

Lab 3. pytest Fixtures • 54


**$ pytest --setup-showtest_add.py-k valid_id**
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

**ch3/test_fixtures.py**
@pytest.fixture()
def a_tuple ():
"""Returnsomethingmoreinteresting."""
**return (1, _'foo'_ , None,{ _'bar'_ : 23})

def test_a_tuple (a_tuple):
"""Demothe a_tuplefixture."""
**assert** a_tuple[3][ _'bar'_ ] == 32

Since test_a_tuple() should fail (23 != 32), we can see what happens when a test

with a fixture fails:

**$ cd /path/to/code/ch3
$ pytest test_fixtures.py::test_a_tuple**
===================testsessionstarts===================
collected1 item

test_fixtures.pyF [100%]

========================FAILURES=========================
______________________test_a_tuple_______________________

Using Fixtures for Test Data • 55


a_tuple= (1, 'foo',None,{'bar':23})

def test_a_tuple(a_tuple):
"""Demothe a_tuplefixture."""
**> asserta_tuple[3][** _'bar'_ **] == 32**
E assert23 == 32

test_fixtures.py:43:AssertionError
================1 failedin 0.07seconds=================

Along with the stack trace section, pytest reports the value parameters of the

function that raised the exception or failed an assert. In the case of tests, the

fixtures are parameters to the test, and are therefore reported with the stack

trace.

What happens if the assert (or any exception) happens in the fixture?

**$ pytest -v test_fixtures.py::test_other_data**
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

A couple of things happen. The stack trace shows correctly that the assert

happened in the fixture function. Also, test_other_data is reported not as FAIL,

but as ERROR. This distinction is great. If a test ever fails, you know the failure

happened in the test proper, and not in any fixture it depends on.

But what about the Tasks project? For the Tasks project, we could probably

use some data fixtures, perhaps different lists of tasks with various properties:

**ch3/a/tasks_proj/tests/conftest.py**
_# Reminderof Taskconstructorinterface
# Task(summary=None,owner=None,done=False,id=None)
# summaryis required
# ownerand doneare optional
# id is set by database_

@pytest.fixture()
def tasks_just_a_few ():

Lab 3. pytest Fixtures • 56


```
"""Allsummariesand ownersare unique."""
return (
Task( 'Writesomecode' , 'Brian' , True),
Task( "CodereviewBrian'scode" , 'Katie' , False),
Task( 'FixwhatBriandid' , 'Michelle' , False))
```
@pytest.fixture()
def tasks_mult_per_owner ():
"""Severalownerswithseveraltaskseach."""
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

You can use these directly from tests, or you can use them from other fixtures.

Let’s use them to build up some non-empty databases to use for testing.

### Using Multiple Fixtures

You’ve already seen that tmpdir uses tmpdir_factory. And you used tmpdir in our

tasks_db fixture. Let’s keep the chain going and add some specialized fixtures

for non-empty tasks databases:

**ch3/a/tasks_proj/tests/conftest.py**
@pytest.fixture()
def db_with_3_tasks (tasks_db,tasks_just_a_few):
"""Connecteddb with3 tasks,all unique."""
**for** t **in** tasks_just_a_few:
tasks.add(t)

@pytest.fixture()
def db_with_multi_per_owner (tasks_db,tasks_mult_per_owner):
"""Connecteddb with9 tasks,3 owners,all with3 tasks."""
**for** t **in** tasks_mult_per_owner:
tasks.add(t)

These fixtures all include two fixtures each in their parameter list: tasks_db

and a data set. The data set is used to add tasks to the database. Now tests

can use these when you want the test to start from a non-empty database,

like this:

**ch3/a/tasks_proj/tests/func/test_add.py
def test_add_increases_count (db_with_3_tasks):

Using Multiple Fixtures • 57


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

**$ cd /path/to/code/ch3/a/tasks_proj/tests/func
$ pytest --setup-showtest_add.py::test_add_increases_count**
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

There are those F’s and S’s for function and session scope again. Let’s learn

about those next.

### Specifying Fixture Scope

Fixtures include an optional parameter called scope, which controls how often

a fixture gets set up and torn down. The scope parameter to @pytest.fixture() can

have the values of function, class, module, or session. The default scope is function.

Lab 3. pytest Fixtures • 58


The tasks_db fixture and all of the fixtures so far don’t specify a scope. Therefore,

they are function scope fixtures.

Here’s a rundown of each scope value:

_scope='function'_

```
Run once per test function. The setup portion is run before each test using
the fixture. The teardown portion is run after each test using the fixture.
This is the default scope used when no scope parameter is specified.
```
_scope='class'_

Run once per test class, regardless of how many test methods are in the class.

_scope='module'_

```
Run once per module, regardless of how many test functions or methods
or other fixtures in the module use it.
```
_scope='session'_

```
Run once per session. All test methods and functions using a fixture of
session scope share one setup and teardown call.
```
Here’s how the scope values look in action:

**ch3/test_scope.py**
"""Demofixturescope."""

import pytest

@pytest.fixture(scope= _'function'_ )
def func_scope ():
"""Afunctionscopefixture."""

@pytest.fixture(scope= _'module'_ )
def mod_scope ():
"""Amodulescopefixture."""

@pytest.fixture(scope= _'session'_ )
def sess_scope ():
"""Asessionscopefixture."""

@pytest.fixture(scope= _'class'_ )
def class_scope ():
"""Aclassscopefixture."""

def test_1 (sess_scope,mod_scope,func_scope):
"""Testusingsession,module,and functionscopefixtures."""

def test_2 (sess_scope,mod_scope,func_scope):
"""Demois morefun withmultiple tests."""

@pytest.mark.usefixtures( _'class_scope'_ )

Specifying Fixture Scope • 59


**class** TestSomething():
"""Democlassscopefixtures."""

```
def test_3 (self):
"""Testusinga classscopefixture."""
```
```
def test_4 (self):
"""Again,multipletestsare morefun."""
```
Let’s use --setup-show to demonstrate that the number of times a fixture is called

and when the setup and teardown are run depend on the scope:

**$ cd /path/to/code/ch3
$ pytest --setup-showtest_scope.py**
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

Lab 3. pytest Fixtures • 60


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

**ch3/b/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture(scope= _'session'_ )
def tasks_db_session (tmpdir_factory):
"""Connectto db beforetests,disconnectafter."""
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )
**yield**
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
"""Anemptytasksdb."""
tasks.delete_all()

Here we changed tasks_db to depend on tasks_db_session, and we deleted all the

entries to make sure it’s empty. Because we didn’t change its name, none of

the fixtures or tests that already include it have to change.

The data fixtures just return a value, so there really is no reason to have them

run all the time. Once per session is sufficient:

**ch3/b/tasks_proj/tests/conftest.py**
_# Reminderof Taskconstructorinterface
# Task(summary=None,owner=None,done=False,id=None)
# summaryis required
# ownerand doneare optional
# id is set by database_

Specifying Fixture Scope • 61


@pytest.fixture(scope= _'session'_ )
def tasks_just_a_few ():
"""Allsummariesand ownersare unique."""
**return (
Task( _'Writesomecode'_ , _'Brian'_ , True),
Task( "CodereviewBrian'scode" , _'Katie'_ , False),
Task( _'FixwhatBriandid'_ , _'Michelle'_ , False))

@pytest.fixture(scope= _'session'_ )
def tasks_mult_per_owner ():
"""Severalownerswithseveraltaskseach."""
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

Now, let’s see if all of these changes work with our tests:

**$ cd /path/to/code/ch3/b/tasks_proj
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

Looks like it’s all good. Let’s trace the fixtures for one test file to see if the

different scoping worked as expected:

**$ pytest --setup-showtests/func/test_add.py**
===================testsessionstarts===================
collected3 items

tests/func/test_add.py
SETUP S tmpdir_factory
SETUP S tasks_db_session(fixturesused:tmpdir_factory)
SETUP F tasks_db(fixturesused:tasks_db_session)
func/test_add.py::test_add_returns_valid_id(fixturesused:tasks_db,
tasks_db_session,tmpdir_factory).

Lab 3. pytest Fixtures • 62


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

Yep. Looks right. tasks_db_session is called once per session, and the quicker

tasks_db now just cleans out the database before each test.

### Specifying Fixtures with usefixtures

So far, if you wanted a test to use a fixture, you put it in the parameter list.

You can also mark a test or a class with @pytest.mark.usefixtures('fixture1', 'fixture2').

usefixtures takes a comma separated list of strings representing fixture names.

It doesn’t make sense to do this with test functions—it’s just more typing.

But it does work well for test classes:

**ch3/test_scope.py**
@pytest.mark.usefixtures( _'class_scope'_ )
**class** TestSomething():
"""Democlassscopefixtures."""

```
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

Specifying Fixtures with usefixtures • 63


want to run at certain times, but tests don’t really depend on any system

state or data from the fixture. Here’s a rather contrived example:

**ch3/test_autouse.py**
"""Demonstrateautousefixtures."""

import pytest
importtime**

@pytest.fixture(autouse=True,scope= _'session'_ )
def footer_session_scope ():
"""Reportthe timeat the end of a session."""
**yield**
now = time.time()
**print ( _'--'_ )
**print ( _'finished: {}'_ .format(time.strftime( _'%d %b %X'_ , time.localtime(now))))
**print ( _'-----------------'_ )

@pytest.fixture(autouse=True)
def footer_function_scope ():
"""Reporttestdurationsafter eachfunction."""
start= time.time()
**yield**
stop= time.time()
delta= stop- start
**print ( _'\ntestduration: {:0.3}seconds'_ .format(delta))

def test_1 ():
"""Simulatelong-ishrunningtest."""
time.sleep(1)

def test_2 ():
"""Simulateslightlylongertest."""
time.sleep(1.23)

We want to add test times after each test, and the date and current time at

the end of the session. Here’s what these look like:

**$ cd /path/to/code/ch3
$ pytest -v -s test_autouse.py**
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

Lab 3. pytest Fixtures • 64


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

**ch3/test_rename_fixture.py**
"""Demonstratefixturerenaming."""

import pytest

@pytest.fixture(name= _'lue'_ )
def ultimate_answer_to_life_the_universe_and_everything ():
"""Returnultimateanswer."""
    return 42

def test_everything (lue):
"""Usethe shortername."""
**assert** lue == 42

Here, lue is now the fixture name, instead of ultimate_answer_to_life_the_uni-

verse_and_everything. That name even shows up if we run it with --setup-show:

**$ pytest --setup-showtest_rename_fixture.py**
===================testsessionstarts===================
collected1 item

test_rename_fixture.py
SETUP F lue
test_rename_fixture.py::test_everything(fixturesused:lue).
TEARDOWNF lue

================1 passedin 0.01seconds=================

If you need to find out where lue is defined, you can add the pytest option

--fixtures and give it the filename for the test. It lists all the fixtures available

for the test, including ones that have been renamed:

**$ pytest --fixturestest_rename_fixture.py**
===================testsessionstarts===================
**...**

Renaming Fixtures • 65


--------fixturesdefinedfromtest_rename_fixture--------
lue
Returnultimateanswer.

==============no testsran in 0.01seconds===============

Most of the output is omitted—there’s a lot there. Luckily, the fixtures we

defined are at the bottom, along with where they are defined. We can use this

to look up the definition of lue. Let’s use that in the Tasks project:

**$ cd /path/to/code/ch3/b/tasks_proj
$ pytest --fixturestests/func/test_add.py**
========================testsessionstarts========================
**...**
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

Cool. All of our conftest.py fixtures are there. And at the bottom of the builtin

list is the tmpdir and tmpdir_factory that we used also.

### Parametrizing Fixtures

In Parametrized Testing, on page 42, we parametrized tests. We can also

parametrize fixtures. We still use our list of tasks, list of task identifiers, and

an equivalence function, just as before:

**ch3/b/tasks_proj/tests/func/test_add_variety2.py
importpytest
importtasks
fromtasksimport** Task

Lab 3. pytest Fixtures • 66


tasks_to_try= (Task( _'sleep'_ , done=True),
Task( _'wake'_ , _'brian'_ ),
Task( _'breathe'_ , _'BRIAN'_ , True),
Task( _'exercise'_ , _'BrIaN'_ , False))

task_ids= [ _'Task({},{},{})'_ .format(t.summary,t.owner,t.done)
**for** t **in** tasks_to_try]

def equivalent (t1,t2):
"""Checktwo tasksfor equivalence."""
**return ((t1.summary== t2.summary) **and**
(t1.owner== t2.owner) **and**
(t1.done== t2.done))

But now, instead of parametrizing the test, we will parametrize a fixture

called a_task:

**ch3/b/tasks_proj/tests/func/test_add_variety2.py**
@pytest.fixture(params=tasks_to_try)
def a_task (request):
"""Usingno ids."""
    return request.param

def test_add_a (tasks_db,a_task):
"""Usinga_taskfixture(no ids)."""
task_id= tasks.add(a_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,a_task)

The request listed in the fixture parameter is another builtin fixture that repre-

sents the calling state of the fixture. You’ll explore it more in the next lab.

It has a field param that is filled in with one element from the list assigned to

params in @pytest.fixture(params=tasks_to_try).

The a_task fixture is pretty simple—it just returns the request.param as its value

to the test using it. Since our task list has four tasks, the fixture will be called

four times, and then the test will get called four times:

**$ cd /path/to/code/ch3/b/tasks_proj/tests/func
$ pytest -v test_add_variety2.py::test_add_a**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_a[a_task0]PASSED [ 25%]
test_add_variety2.py::test_add_a[a_task1]PASSED [ 50%]
test_add_variety2.py::test_add_a[a_task2]PASSED [ 75%]
test_add_variety2.py::test_add_a[a_task3]PASSED [100%]

================4 passedin 0.05seconds=================

Parametrizing Fixtures • 67


We didn’t provide ids, so pytest just made up some names by appending a

number to the name of the fixture. However, we can use the same string list

we used when we parametrized our tests:

**ch3/b/tasks_proj/tests/func/test_add_variety2.py**
@pytest.fixture(params=tasks_to_try,ids=task_ids)
def b_task (request):
"""Usinga listof ids."""
    return request.param

def test_add_b (tasks_db,b_task):
"""Usingb_taskfixture,withids."""
task_id= tasks.add(b_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,b_task)

This gives us better identifiers:

**$ pytest -v test_add_variety2.py::test_add_b**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_b[Task(sleep,None,True)]PASSED[ 25%]
test_add_variety2.py::test_add_b[Task(wake,brian,False)]PASSED[ 50%]
test_add_variety2.py::test_add_b[Task(breathe,BRIAN,True)]PASSED[ 75%]
test_add_variety2.py::test_add_b[Task(exercise,BrIaN,False)]PASSED[100%]

================4 passedin 0.04seconds=================

We can also set the ids parameter to a function we write that provides the

identifiers. Here’s what it looks like when we use a function to generate the

identifiers:

**ch3/b/tasks_proj/tests/func/test_add_variety2.py
def id_func (fixture_value):
"""Afunctionfor generatingids."""
t = fixture_value
    return _'Task({},{},{})'_ .format(t.summary,t.owner,t.done)

@pytest.fixture(params=tasks_to_try,ids=id_func)
def c_task (request):
"""Usinga function(id_func)to generateids."""
    return request.param

def test_add_c (tasks_db,c_task):
"""Usefixturewithgeneratedids."""
task_id= tasks.add(c_task)
t_from_db= tasks.get(task_id)
**assert** equivalent(t_from_db,c_task)

Lab 3. pytest Fixtures • 68


The function will be called from the value of each item from the parametrization.

Since the parametrization is a list of Task objects, id_func() will be called with a

Task object, which allows us to use the namedtuple accessor methods to access a

single Task object to generate the identifier for one Task object at a time. It’s a bit

cleaner than generating a full list ahead of time, and looks the same:

**$ pytest -v test_add_variety2.py::test_add_c**
===================testsessionstarts===================
collected4 items

test_add_variety2.py::test_add_c[Task(sleep,None,True)]PASSED[ 25%]
test_add_variety2.py::test_add_c[Task(wake,brian,False)]PASSED[ 50%]
test_add_variety2.py::test_add_c[Task(breathe,BRIAN,True)]PASSED[ 75%]
test_add_variety2.py::test_add_c[Task(exercise,BrIaN,False)]PASSED[100%]

================4 passedin 0.05seconds=================

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

**ch3/b/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

@pytest.fixture(scope= _'session'_ )
def tasks_db_session (tmpdir_factory):
"""Connectto db beforetests,disconnectafter."""
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )
**yield**
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
"""Anemptytasksdb."""
tasks.delete_all()

Parametrizing Fixtures • 69


The db_type parameter in the call to start_tasks_db() isn’t magic. It just ends up

switching which subsystem gets to be responsible for the rest of the database

interactions:

**tasks_proj/src/tasks/api.py
def start_tasks_db (db_path,db_type): _# type:(str,str)-> None
"""ConnectAPI functionsto a db."""
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
**raise** ValueError( "db_typemustbe a 'tiny'or 'mongo'" )

To test MongoDB, we need to run all the tests with db_type set to mongo. A small

change does the trick:

**ch3/c/tasks_proj/tests/conftest.py
importpytest
importtasks
fromtasksimport** Task

_#@pytest.fixture(scope='session',params=['tiny',])_
@pytest.fixture(scope= _'session'_ , params=[ _'tiny'_ , _'mongo'_ ])
def tasks_db_session (tmpdir_factory,request):
"""Connectto db beforetests,disconnectafter."""
temp_dir= tmpdir_factory.mktemp( _'temp'_ )
tasks.start_tasks_db(str(temp_dir),request.param)
**yield** _# thisis wherethe testinghappens_
tasks.stop_tasks_db()

@pytest.fixture()
def tasks_db (tasks_db_session):
"""Anemptytasksdb."""
tasks.delete_all()

Here I added params=['tiny','mongo'] to the fixture decorator. I added request to the

parameter list of temp_db, and I set db_type to request.param instead of just picking

'tiny' or 'mongo'.

When you set the --verbose or -v flag with pytest running parametrized tests

or parametrized fixtures, pytest labels the different runs based on the value

of the parametrization. And because the values are already strings, that

works great.

Lab 3. pytest Fixtures • 70


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

**$ cd /path/to/code/ch3/c/tasks_proj
$ pip install pymongo
$ pytest -v --tb=no**
===================testsessionstarts===================
collected96 items

tests/func/test_add.py::test_add_returns_valid_id[tiny]PASSED[ 1%]
tests/func/test_add.py::test_added_task_has_id_set[tiny]PASSED[ 2%]
tests/func/test_add.py::test_add_increases_count[tiny]PASSED[ 3%]
tests/func/test_add_variety.py::test_add_1[tiny]PASSED[ 4%]
tests/func/test_add_variety.py::test_add_2[tiny-task0]PASSED[ 5%]
tests/func/test_add_variety.py::test_add_2[tiny-task1]PASSED[ 6%]
**...**
tests/func/test_add.py::test_add_returns_valid_id[mongo]FAILED[ 43%]
tests/func/test_add.py::test_added_task_has_id_set[mongo]FAILED[ 44%]
tests/func/test_add.py::test_add_increases_count[mongo]PASSED[ 45%]
tests/func/test_add_variety.py::test_add_1[mongo]FAILED[ 46%]
tests/func/test_add_variety.py::test_add_2[mongo-task0]FAILED[ 47%]
tests/func/test_add_variety.py::test_add_2[mongo-task1]FAILED[ 48%]
**...**
==========42 failed,54 passedin 4.85seconds===========

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
7. Re-run pytest --setup-showtest_fixtures.py. What changed?
8. For the fixture from Exercise 6, change return <data> to yield <data>.

Exercises • 71


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

Lab 3. pytest Fixtures • 72


CHAPTER 4

Builtin Fixtures

In the previous lab, you looked at what fixtures are, how to write them, and

how to use them for test data as well as setup and teardown code. You also

used conftest.py for sharing fixtures between tests in multiple test files. By the

end of Lab 3, pytest Fixtures, on page 51, the Tasks project had these fix-

tures: tasks_db_session, tasks_just_a_few, tasks_mult_per_owner, tasks_db, db_with_3_tasks, and

db_with_multi_per_owner defined in conftest.py to be used by any test function in the

Tasks project that needed them.

Reusing common fixtures is such a good idea that the pytest developers

included some commonly needed fixtures with pytest. You’ve already seen tmpdir

and tmpdir_factory in use by the Tasks project in Changing Scope for Tasks Project

Fixtures, on page 61. You’ll take a look at them in more detail in this lab.

The builtin fixtures that come prepackaged with pytest can help you do some

pretty useful things in your tests easily and consistently. For example, in

addition to handling temporary files, pytest includes builtin fixtures to access

command-line options, communicate between tests sessions, validate output

streams, modify environmental variables, and interrogate warnings. The

builtin fixtures are extensions to the core functionality of pytest. Let’s now

take a look at several of the most often used builtin fixtures one by one.

### Using tmpdir and tmpdir_factory

The tmpdir and tmpdir_factory builtin fixtures are used to create a temporary file

system directory before your test runs, and remove the directory when your

test is finished. In the Tasks project, we needed a directory to store the tem-

porary database files used by MongoDB and TinyDB. However, because we

want to test with temporary databases that don’t survive past a test session,

we used tmpdir and tmpdir_factory to do the directory creation and cleanup for us.


If you’re testing something that reads, writes, or modifies files, you can use

tmpdir to create files or directories used by a single test, and you can use

tmpdir_factory when you want to set up a directory for many tests.

The tmpdir fixture has function scope, and the tmpdir_factory fixture has session

scope. Any individual test that needs a temporary directory or file just for the

single test can use tmpdir. This is also true for a fixture that is setting up a

directory or file that should be recreated for each test function.

Here’s a simple example using tmpdir:

**ch4/test_tmpdir.py
def test_tmpdir (tmpdir):
_# tmpdiralreadyhas a pathnameassociatedwithit
# join()extendsthe pathto includea filename
# the fileis createdwhenit'swrittento_
a_file= tmpdir.join( _'something.txt'_ )

```
# you can createdirectories
a_sub_dir= tmpdir.mkdir( 'anything' )
# you can createfilesin directories(createdwhenwritten)
another_file= a_sub_dir.join( 'something_else.txt' )
# thiswritecreates'something.txt'
a_file.write( 'contentsmay settleduringshipping' )
# thiswritecreates'anything/something_else.txt'
another_file.write( 'somethingdifferent' )
# you can readthe filesas well
assert a_file.read()== 'contentsmay settleduringshipping'
assert another_file.read()== 'somethingdifferent'
```
The value returned from tmpdir is an object of type py.path.local.^1 This seems like

everything we need for temporary directories and files. However, there’s one

gotcha. Because the tmpdir fixture is defined as function scope, you can’t use

tmpdir to create folders or files that should stay in place longer than one test

function. For fixtures with scope other than function (class, module, session),

tmpdir_factory is available.

The tmpdir_factory fixture is a lot like tmpdir, but it has a different interface. As

discussed in Specifying Fixture Scope, on page 58, function scope fixtures

run once per test function, module scope fixtures run once per module, class

scope fixtures run once per class, and test scope fixtures run once per session.

Therefore, resources created in session scope fixtures have a lifetime of the

entire session.

1. [http://py.readthedocs.io/en/latest/path.html](http://py.readthedocs.io/en/latest/path.html)

Lab 4. Builtin Fixtures • 74


To see how similar tmpdir and tmpdir_factory are, I’ll modify the tmpdir example

just enough to use tmpdir_factory instead:

**ch4/test_tmpdir.py
def test_tmpdir_factory (tmpdir_factory):
_# you shouldstartwithmakinga directory
# a_diractslikethe objectreturnedfromthe tmpdirfixture_
a_dir= tmpdir_factory.mktemp( _'mydir'_ )
_# base_tempwillbe the parentdir of 'mydir'
# you don'thaveto use getbasetemp()
# usingit herejustto showthatit'savailable_
base_temp= tmpdir_factory.getbasetemp()
**print ( _'base:'_ , base_temp)
_# the restof thistestlooksthe sameas the 'test_tmpdir()'
# exampleexceptI'm usinga_dirinsteadof tmpdir_
a_file= a_dir.join( _'something.txt'_ )
a_sub_dir= a_dir.mkdir( _'anything'_ )
another_file= a_sub_dir.join( _'something_else.txt'_ )
a_file.write( _'contentsmay settleduringshipping'_ )
another_file.write( _'somethingdifferent'_ )
**assert** a_file.read()== _'contentsmay settleduringshipping'_
**assert** another_file.read()== _'somethingdifferent'_

The first line uses mktemp('mydir') to create a directory and saves it in a_dir. For

the rest of the function, you can use a_dir just like the tmpdir returned from

the tmpdir fixture.

In the second line of the tmpdir_factory example, the getbasetemp() function returns

the base directory used for this session. The print statement is in the example

so you can see where the directory is on your system. Let’s see where it is:

**$ cd /path/to/code/ch4
$ pytest-q -s test_tmpdir.py::test_tmpdir_factory**
base:/private/var/folders/53/zv4j_zc506x2xq25l31qxvxm0000gn\
/T/pytest-of-okken/pytest-732
.
1 passedin 0.04seconds

This base directory is system- and user-dependent, and pytest-NUM changes

with an incremented NUM for every session. The base directory is left alone

after a session, but pytest cleans them up and only the most recent few

temporary base directories are left on the system, which is great if you need

to inspect the files after a test run.

You can also specify your own base directory if you need to with pytest --

basetemp=mydir.

Using tmpdir and tmpdir_factory • 75


**Using Temporary Directories for Other Scopes**

We get session scope temporary directories and files from the tmpdir_factory

fixture, and function scope directories and files from the tmpdir fixture. But

what about other scopes? What if we need a module or a class scope temporary

directory? To do this, we create another fixture of the scope we want and have

it use tmpdir_factory.

For example, suppose we have a module full of tests, and many of them need

to be able to read some data from a json file. We could put a module scope

fixture in either the module itself, or in a conftest.py file that sets up the data

file like this:

**ch4/authors/conftest.py**
"""Demonstratetmpdir_factory."""

import json
import pytest

@pytest.fixture(scope= _'module'_ )
def author_file_json (tmpdir_factory):
"""Writesomeauthorsto a datafile."""
python_author_data= {
_'Ned'_ : { _'City'_ : _'Boston'_ },
_'Brian'_ : { _'City'_ : _'Portland'_ },
_'Luciano'_ : { _'City'_ : _'SauPaulo'_ }
}

```
file_= tmpdir_factory.mktemp( 'data' ).join( 'author_file.json' )
print ( 'file:{}' .format(str(file_)))
```
```
with file.open( 'w' ) as f:
json.dump(python_author_data, f)
return file
```
The author_file_json() fixture creates a temporary directory called data and creates

a file called author_file.json within the data directory. It then writes the

python_author_data dictionary as json. Because this is a module scope fixture,

the json file will only be created once per module that has a test using it:

**ch4/authors/test_authors.py**
"""Someteststhatuse tempdatafiles."""
import json**

def test_brian_in_portland (author_file_json):
"""Atestthatusesa datafile."""
**with** author_file_json.open() **as** f:
authors= json.load(f)
**assert** authors[ _'Brian'_ ][ _'City'_ ] == _'Portland'_

Lab 4. Builtin Fixtures • 76


def test_all_have_cities (author_file_json):
"""Samefileis usedfor bothtests."""
**with** author_file_json.open() **as** f:
authors= json.load(f)
**for** a **in** authors:
**assert** len(authors[a][ _'City'_ ]) > 0

Both tests will use the same json file. If one test data file works for multiple

tests, there’s no use recreating it for both.

### Using pytestconfig

With the pytestconfig builtin fixture, you can control how pytest runs through

command-line arguments and options, configuration files, plugins, and the

directory from which you launched pytest. The pytestconfig fixture is a shortcut

to request.config, and is sometimes referred to in the pytest documentation as

“the pytest config object.”

To see how pytestconfig works, you’ll look at how to add a custom command-

line option and read the option value from within a test. You can read the

value of command-line options directly from pytestconfig, but to add the option

and have pytest parse it, you need to add a hook function. _Hook functions_ ,

which I cover in more detail in Lab 5, Plugins, on page 97, are another

way to control how pytest behaves and are used frequently in plugins. How-

ever, adding a custom command-line option and reading it from pytestconfig is

common enough that I want to cover it here.

We’ll use the pytest hook pytest_addoption to add a couple of options to the

options already available in the pytest command line:

**ch4/pytestconfig/conftest.py
def pytest_addoption (parser):
parser.addoption( "--myopt" , action= "store_true" ,
help= "some booleanoption" )
parser.addoption( "--foo" , action= "store" , default= "bar" ,
help= "foo: bar or baz" )

Adding command-line options via pytest_addoption should be done via plugins

or in the conftest.py file at the top of your project directory structure. You

shouldn’t do it in a test subdirectory.

The options --myopt and --foo <value> were added to the previous code, and the

help string was modified, as shown here:

**$ cd /path/to/code/ch4/pytestconfig
$ pytest --help
usage:pytest[options][file_or_dir][file_or_dir][...]
**...**

Using pytestconfig • 77


customoptions:
--myopt somebooleanoption
--foo=FOO foo:bar or baz
**...**

Now we can access those options from a test:

**ch4/pytestconfig/test_config.py
import pytest

def test_option (pytestconfig):
**print ( _'"foo"set to:'_ , pytestconfig.getoption( _'foo'_ ))
**print ( _'"myopt"set to:'_ , pytestconfig.getoption( _'myopt'_ ))

Let’s see how this works:

**$ pytest-s -q test_config.py::test_option**
"foo"set to: bar
"myopt"set to: False
.
1 passedin 0.01seconds
**$ pytest-s -q --myopttest_config.py::test_option**
"foo"set to: bar
"myopt"set to: True
.
1 passedin 0.01seconds
**$ pytest-s -q --myopt--foobaz test_config.py::test_option**
"foo"set to: baz
"myopt"set to: True
.
1 passedin 0.01seconds

Because pytestconfig is a fixture, it can also be accessed from other fixtures.

You can make fixtures for the option names, if you like, like this:

**ch4/pytestconfig/test_config.py**
@pytest.fixture()
def foo (pytestconfig):
    return pytestconfig.option.foo

@pytest.fixture()
def myopt (pytestconfig):
    return pytestconfig.option.myopt

def test_fixtures_for_options (foo, myopt):
**print ( _'"foo"set to:'_ , foo)
**print ( _'"myopt"set to:'_ , myopt)

You can also access builtin options, not just options you add, as well as

information about how pytest was started (the directory, the arguments, and

so on).

Lab 4. Builtin Fixtures • 78


Here’s an example of a few configuration values and options:

**ch4/pytestconfig/test_config.py
def test_pytestconfig (pytestconfig):
**print ( _'args :'_ , pytestconfig.args)
**print ( _'inifile :'_ , pytestconfig.inifile)
**print ( _'invocation_dir :'_ , pytestconfig.invocation_dir)
**print ( _'rootdir :'_ , pytestconfig.rootdir)
**print ( _'-k EXPRESSION :'_ , pytestconfig.getoption( _'keyword'_ ))
**print ( _'-v,--verbose :'_ , pytestconfig.getoption( _'verbose'_ ))
**print ( _'-q,--quiet :'_ , pytestconfig.getoption( _'quiet'_ ))
**print ( _'-l,--showlocals:'_ , pytestconfig.getoption( _'showlocals'_ ))
**print ( _'--tb=style :'_ , pytestconfig.getoption( _'tbstyle'_ ))

You’ll use pytestconfig again when I demonstrate ini files in Lab 6, Config-

uration, on page 115.

### Using cache

Usually we testers like to think about each test as being as independent as

possible from other tests. We want to make sure other dependencies don’t

creep in. We want to be able to run or rerun any test in any order and get the

same result. We also want test sessions to be repeatable and to not change

behavior based on previous test sessions.

However, sometimes passing information from one test session to the next

can be quite useful. When we do want to pass information to future test ses-

sions, we can do it with the cache builtin fixture.

The cache fixture is all about storing information about one test session and

retrieving it in the next. A great example of using the powers of cache for good

is the builtin functionality of --last-failed and --failed-first. Let’s take a look at how

the data for these flags is stored using cache.

Here’s the help text for the --last-failed and --failed-first options, as well as a couple

of cache options:

**$ pytest --help
...**
--lf,--last-failed rerunonlythe teststhatfailedat the lastrun (or
all if nonefailed)
--ff,--failed-first run all testsbut run the lastfailuresfirst.This
may re-ordertestsand thusleadto repeatedfixture
setup/teardown
--nf,--new-first run testsfromnew filesfirst,thenthe restof the
testssortedby filemtime
--cache-show showcache contents,don'tperformcollectionor tests
--cache-clear removeall cachecontentsat startof testrun.
**...**

Using cache • 79


To see these in action, we’ll use these two tests:

**ch4/cache/test_pass_fail.py
def test_this_passes ():
**assert** 1 == 1

def test_this_fails ():
**assert** 1 == 2

Let’s run them using --verbose to see the function names, and --tb=no to hide

the stack trace:

**$ cd /path/to/code/ch4/cache
$ pytest --verbose--tb=notest_pass_fail.py**
===================testsessionstarts===================
collected2 items

test_pass_fail.py::test_this_passesPASSED [ 50%]
test_pass_fail.py::test_this_failsFAILED [100%]

===========1 failed,1 passedin 0.06seconds============

If you run them again with the --ff or --failed-first flag, the tests that failed previ-

ously will be run first, followed by the rest of the session:

**$ pytest --verbose--tb=no--fftest_pass_fail.py**
===================testsessionstarts===================
collected2 items
run-last-failure:rerunprevious1 failurefirst

test_pass_fail.py::test_this_failsFAILED [ 50%]
test_pass_fail.py::test_this_passesPASSED [100%]

===========1 failed,1 passedin 0.06seconds============

Or you can use --lf or --last-failed to just run the tests that failed the last time:

venv)$ pytest --verbose--tb=no--lftest_pass_fail.py
===================testsessionstarts===================
collected2 items/ 1 deselected
run-last-failure:rerunprevious1 failure

test_pass_fail.py::test_this_failsFAILED [100%]

=========1 failed,1 deselectedin 0.06seconds==========

Before we look at how the failure data is being saved and how you can use

the same mechanism, let’s look at another example that makes the value of

--lf and --ff even more obvious.

Here’s a parametrized test with one failure:

Lab 4. Builtin Fixtures • 80


**ch4/cache/test_few_failures.py**
"""Demonstrate-lf and -ff withfailingtests."""

import pytest
frompytestimport** approx

testdata= [
_# x, y, expected_
(1.01,2.01,3.02),
(1e25,1e23,1.1e25),
(1.23,3.21,4.44),
(0.1,0.2,0.3),
(1e25,1e24,1.1e25)
]

@pytest.mark.parametrize( "x,y,expected" , testdata)
def test_a (x, y, expected):
"""Demoapprox()."""
sum_= x + y
**assert** sum_== approx(expected)

And the output:

**$ cd /path/to/code/ch4/cache
$ pytest-q test_few_failures.py**
.F... [100%]
========================FAILURES=========================
_______________test_a[1e+25-1e+23-1.1e+25]_______________

x = 1e+25,y = 1e+23,expected= 1.1e+25

@pytest.mark.parametrize("x,y,expected",testdata)
def test_a(x,y, expected):
"""Demoapprox()."""
sum_= x + y
**> assertsum_== approx(expected)**
E assert1.01e+25== 1.1e+25± 1.1e+19
E + where1.1e+25± 1.1e+19= approx(1.1e+25)

test_few_failures.py:21:AssertionError
1 failed,4 passedin 0.10seconds

Maybe you can spot the problem right off the bat. But let’s pretend the test

is longer and more complicated, and it’s not obvious what’s wrong. Let’s run

the test again to see the failure again. You can specify the test case on the

command line:

**$ pytest-q** "test_few_failures.py::test_a[1e+25-1e+23-1.1e+25]"

If you don’t want to copy/paste or there are multiple failed cases you’d like

to rerun, --lf is much easier. And if you’re really debugging a test failure,

another flag that might make things easier is --showlocals, or -l for short:

Using cache • 81


**$ pytest-q --lf-l test_few_failures.py**
run-last-failure:rerunprevious1 failure
F [100%]
========================FAILURES=========================
_______________test_a[1e+25-1e+23-1.1e+25]_______________

x = 1e+25,y = 1e+23,expected= 1.1e+25

@pytest.mark.parametrize("x,y,expected",testdata)
def test_a(x,y, expected):
"""Demoapprox()."""
sum_= x + y
**> assertsum_== approx(expected)**
E assert1.01e+25== 1.1e+25± 1.1e+19
E + where1.1e+25± 1.1e+19= approx(1.1e+25)

expected = 1.1e+25
sum_ = 1.01e+25
x = 1e+25
y = 1e+23

test_few_failures.py:21:AssertionError
1 failed,4 deselectedin 0.06seconds

The reason for the failure should be more obvious now.

To pull off the trick of remembering what test failed last time, pytest stores

test failure information from the last test session. You can see the stored

information with --cache-show:

**$ pytest --cache-show**
=====================testsessionstarts======================
-------------------------cachevalues-------------------------
cache/lastfailedcontains:
{'test_few_failures.py::test_a[1e+25-1e+23-1.1e+25]':True}

=================no testsran in 0.00seconds=================
**$ pytest --cache-show**
===================testsessionstarts===================
----------------------cachevalues-----------------------
cache/lastfailedcontains:
{'test_few_failures.py::test_a[1e+25-1e+23-1.1e+25]':True}
**...**
==============no testsran in 0.00seconds===============

Or you can look in the cache dir:

**$ cat .pytest_cache/v/cache/lastfailed**
{
"test_few_failures.py::test_a[1e+25-1e+23-1.1e+25]":true
}

You can pass in --cache-clear to clear the cache before the session.

Lab 4. Builtin Fixtures • 82


The cache can be used for more than just --lf and --ff. Let’s make a fixture that

records how long tests take, saves the times, and on the next run, reports an

error on tests that take longer than, say, twice as long as last time.

The interface for the cache fixture is simply

cache.get(key,default)
cache.set(key,value)

By convention, key names start with the name of your application or plugin,

followed by a /, and continuing to separate sections of the key name with /’s.

The value you store can be anything that is convertible to json, since that’s

how it’s represented in the .cache directory.

Here’s our fixture used to time tests:

**ch4/cache/test_slower.py**
@pytest.fixture(autouse=True)
def check_duration (request,cache):
key = _'duration/'_ + request.node.nodeid.replace( _':'_ , _'_'_ )
_# nodeid'scan havecolons
# keysbecomefilenameswithin.cache
# replacecolonswithsomethingfilenamesafe_
start_time= datetime.datetime.now()
**yield**
stop_time= datetime.datetime.now()
this_duration= (stop_time- start_time).total_seconds()
last_duration= cache.get(key,None)
cache.set(key,this_duration)
**if** last_duration **is not** None:
errorstring= "testdurationover2x lastduration"
**assert** this_duration<= last_duration* 2, errorstring

The fixture is autouse, so it doesn’t need to be referenced from the test. The

request object is used to grab the nodeid for use in the key. The nodeid is a unique

identifier that works even with parametrized tests. We prepend the key with

'duration/' to be good cache citizens. The code above yield runs before the test

function; the code after yield happens after the test function.

Now we need some tests that take different amounts of time:

**ch4/cache/test_slower.py**
@pytest.mark.parametrize( _'i'_ , range(5))
def test_slow_stuff (i):
time.sleep(random.random())

Because you probably don’t want to write a bunch of tests for this, I used

random and parametrization to easily generate some tests that sleep for a

Using cache • 83


random amount of time, all shorter than a second. Let’s see it run a couple

of times:

**$ cd /path/to/code/ch4/cache
$ pytest-q --tb=linetest_slower.py**
..... [100%]
5 passedin 1.40seconds
**$ pytest-q --tb=linetest_slower.py**
..E.E.. [100%]
=========================ERRORS==========================
_________ERRORat teardownof test_slow_stuff[1]_________
E AssertionError:testdurationover2x lastduration
assert0.125103<= (0.009833* 2)
_________ERRORat teardownof test_slow_stuff[2]_________
E AssertionError:testdurationover2x lastduration
assert0.28841<= (0.086302* 2)
5 passed,2 errorin 1.77seconds

Well, that was fun. Let’s see what’s in the cache:

**$ pytest-q --cache-show**
----------------------cachevalues-----------------------
cache/lastfailedcontains:
{'test_slower.py::test_slow_stuff[1]':True,
'test_slower.py::test_slow_stuff[2]':True}
**...**
duration/test_slower.py__test_slow_stuff[0]contains:
0.958503
duration/test_slower.py__test_slow_stuff[1]contains:
0.125103
duration/test_slower.py__test_slow_stuff[2]contains:
0.28841
duration/test_slower.py__test_slow_stuff[3]contains:
0.170382
duration/test_slower.py__test_slow_stuff[4]contains:
0.148116

no testsran in 0.01seconds

You can easily see the duration data separate from the cache data due to the

prefixing of cache data names. However, it’s interesting that the lastfailed

functionality is able to operate with one cache entry. Our duration data is taking

up one cache entry per test. Let’s follow the lead of lastfailed and fit our data

into one entry.

We are reading and writing to the cache for every test. We could split up the

fixture into a function scope fixture to measure durations and a session scope

fixture to read and write to the cache. However, if we do this, we can’t use

the cache fixture because it has function scope. Fortunately, a quick peek at

Lab 4. Builtin Fixtures • 84


the implementation on GitHub^2 reveals that the cache fixture is simply

returning request.config.cache. This is available in any scope.

2. https://github.com/pytest-dev/pytest/blob/master/_pytest/cacheprovider.py

Using cache • 85


Here’s one possible refactoring of the same functionality:

**ch4/cache/test_slower_2.py**
Duration= namedtuple( _'Duration'_ , [ _'current'_ , _'last'_ ])

@pytest.fixture(scope= _'session'_ )
def duration_cache (request):
key = _'duration/testdurations'_
d = Duration({},request.config.cache.get(key,{}))
**yield** d
request.config.cache.set(key,d.current)

@pytest.fixture(autouse=True)
def check_duration (request,duration_cache):
d = duration_cache
nodeid= request.node.nodeid
start_time= datetime.datetime.now()
**yield**
duration= (datetime.datetime.now()- start_time).total_seconds()
d.current[nodeid]= duration
**if** d.last.get(nodeid,None) **is not** None:
errorstring= "testdurationover2x lastduration"
**assert** duration<= (d.last[nodeid]* 2), errorstring

The duration_cache fixture is session scope. It reads the previous entry or an empty

dictionary if there is no previous cached data, before any tests are run. In the

previous code, we saved both the retrieved dictionary and an empty one in a

namedtuple called Duration with accessors current and last. We then passed that

namedtuple to the check_duration fixture, which is function scope and runs for every

test function. As the test runs, the same namedtuple is passed to each test, and

the times for the current test runs are stored in the d.current dictionary. At the

end of the test session, the collected current dictionary is saved in the cache.

After running it a couple of times, let’s look at the saved cache:

**$ pytest-q --cache-cleartest_slower_2.py**
..... [100%]
5 passedin 2.27seconds
**$ pytest-q --tb=notest_slower_2.py**
.E.E...E [100%]
5 passed,3 errorin 3.65seconds
**$ pytest-q --cache-show**
----------------------cachevalues-----------------------
cache/lastfailedcontains:
{'test_slower_2.py::test_slow_stuff[0]':True,
'test_slower_2.py::test_slow_stuff[1]':True,
'test_slower_2.py::test_slow_stuff[4]':True}
**...**
duration/testdurationscontains:
{'test_slower_2.py::test_slow_stuff[0]':0.701514,

Lab 4. Builtin Fixtures • 86


```
'test_slower_2.py::test_slow_stuff[1]':0.89724,
'test_slower_2.py::test_slow_stuff[2]':0.997875,
'test_slower_2.py::test_slow_stuff[3]':0.271242,
'test_slower_2.py::test_slow_stuff[4]':0.689478}
```
no testsran in 0.00seconds

That looks better.

### Using capsys

The capsys builtin fixture provides two bits of functionality: it allows you to

retrieve stdout and stderr from some code, and it disables output capture tem-

porarily. Let’s take a look at retrieving stdout and stderr.

Suppose you have a function to print a greeting to stdout:

**ch4/cap/test_capsys.py
def greeting (name):
**print ( _'Hi,{}'_ .format(name))

You can’t test it by checking the return value. You have to test stdout somehow.

You can test the output by using capsys:

**ch4/cap/test_capsys.py
def test_greeting (capsys):
greeting( _'Earthling'_ )
out,err = capsys.readouterr()
**assert** out == _'Hi,Earthling\n'_
**assert** err == _''_
greeting( _'Brian'_ )
greeting( _'Nerd'_ )
out,err = capsys.readouterr()
**assert** out == _'Hi,Brian\nHi, Nerd\n'_
**assert** err == _''_

The captured stdout and stderr are retrieved from capsys.redouterr(). The return

value is whatever has been captured since the beginning of the function, or

from the last time it was called.

The previous example only used stdout. Let’s look at an example using stderr:

**ch4/cap/test_capsys.py
def yikes (problem):
**print ( _'YIKES!{}'_ .format(problem),file=sys.stderr)

def test_yikes (capsys):
yikes( _'Outof coffee!'_ )
out,err = capsys.readouterr()
**assert** out == _''_
**assert** _'Outof coffee!'_ **in** err

Using capsys • 87


pytest usually captures the output from your tests and the code under test.

This includes print statements. The captured output is displayed for failing

tests only after the full test session is complete. The -s option turns off this

feature, and output is sent to stdout while the tests are running. Usually this

works great, as it’s the output from the failed tests you need to see in order

to debug the failures. However, you may want to allow some output to make

it through the default pytest output capture, to print some things without

printing everything. You can do this with capsys. You can use capsys.disabled()

to temporarily let output get past the capture mechanism.

Here’s an example:

**ch4/cap/test_capsys.py
def test_capsys_disabled (capsys):
**with** capsys.disabled():
**print ( _'\nalwaysprintthis'_ )
**print ( _'normalprint,usuallycaptured'_ )

Now, 'alwaysprint this' will always be output:

**$ cd /path/to/code/ch4/cap
$ pytest-q test_capsys.py::test_capsys_disabled**

alwaysprintthis

. [100%]
1 passedin 0.02seconds
**$ pytest-q -s test_capsys.py::test_capsys_disabled**

alwaysprintthis
normalprint,usuallycaptured
.
1 passedin 0.01seconds

As you can see, alwaysprintthis shows up with or without output capturing, since

it’s being printed from within a with capsys.disabled() block. The other print state-

ment is just a normal print statement, so normal print, usuallycaptured is only seen

in the output when we pass in the -s flag, which is a shortcut for --capture=no,

turning off output capture.

### Using monkeypatch

A “monkey patch” is a dynamic modification of a class or module during

runtime. During testing, “monkey patching” is a convenient way to take over

part of the runtime environment of the code under test and replace either

input dependencies or output dependencies with objects or functions that

are more convenient for testing. The monkeypatch builtin fixture allows you to

do this in the context of a single test. And when the test ends, regardless of

pass or fail, the original unpatched is restored, undoing everything changed

Lab 4. Builtin Fixtures • 88


by the patch. It’s all very hand-wavy until we jump into some examples. After

looking at the API, we’ll look at how monkeypatch is used in test code.

The monkeypatch fixture provides the following functions:

- setattr(target, name,value=<notset>,raising=True): Set an attribute.
- delattr(target, name=<notset>,raising=True): Delete an attribute.
- setitem(dic,name,value): Set a dictionary entry.
- delitem(dic,name,raising=True): Delete a dictionary entry.
- setenv(name,value,prepend=None): Set an environmental variable.
- delenv(name,raising=True): Delete an environmental variable.
- syspath_prepend(path): Prepend path to sys.path, which is Python’s list of import
    locations.
- chdir(path): Change the current working directory.

The raising parameter tells pytest whether or not to raise an exception if the

item doesn’t already exist. The prepend parameter to setenv() can be a character.

If it is set, the value of the environmental variable will be changed to value +

prepend + <old value>.

To see monkeypatch in action, let’s look at code that writes a dot configuration

file. The behavior of some programs can be changed with preferences and

values set in a dot file in a user’s home directory. Here’s a bit of code that

reads and writes a cheese preferences file:

**ch4/monkey/cheese.py
importos
importjson**

def read_cheese_preferences ():
full_path= os.path.expanduser( _'~/.cheese.json'_ )
**with** open(full_path, _'r'_ ) **as** f:
prefs= json.load(f)
    return prefs

def write_cheese_preferences (prefs):
full_path= os.path.expanduser( _'~/.cheese.json'_ )
**with** open(full_path, _'w'_ ) **as** f:
json.dump(prefs,f, indent=4)

def write_default_cheese_preferences ():
write_cheese_preferences(_default_prefs)

Using monkeypatch • 89


_default_prefs= {
_'slicing'_ : [ _'manchego'_ , _'sharpcheddar'_ ],
_'spreadable'_ : [ _'SaintAndre'_ , _'camembert'_ ,
_'bucheron'_ , _'goat'_ , _'humboltfog'_ , _'cambozola'_ ],
_'salads'_ : [ _'crumbledfeta'_ ]
}

Let’s take a look at how we could test write_default_cheese_preferences(). It’s a

function that takes no parameters and doesn’t return anything. But it does

have a side effect that we can test. It writes a file to the current user’s home

directory.

One approach is to just let it run normally and check the side effect. Suppose

I already have tests for read_cheese_preferences() and I trust them, so I can use

them in the testing of write_default_cheese_preferences():

**ch4/monkey/test_cheese.py
def test_def_prefs_full ():
cheese.write_default_cheese_preferences()
expected= cheese._default_prefs
actual= cheese.read_cheese_preferences()
**assert** expected== actual

One problem with this is that anyone who runs this test code will overwrite

their own cheese preferences file. That’s not good.

If a user has HOME set, os.path.expanduser() replaces ~ with whatever is in a user’s

HOME environmental variable. Let’s create a temporary directory and redirect

HOME to point to that new temporary directory:

**ch4/monkey/test_cheese.py
def test_def_prefs_change_home (tmpdir,monkeypatch):
monkeypatch.setenv( _'HOME'_ , tmpdir.mkdir( _'home'_ ))
cheese.write_default_cheese_preferences()
expected= cheese._default_prefs
actual= cheese.read_cheese_preferences()
**assert** expected== actual

This is a pretty good test, but relying on HOME seems a little operating-system

dependent. And a peek into the documentation online for expanduser() has some

troubling information, including “On Windows, HOME and USERPROFILE

will be used if set, otherwise a combination of....”^3 Dang. That may not be

good for someone running the test on Windows. Maybe we should take a dif-

ferent approach.

3. https://docs.python.org/3.6/library/os.path.html#os.path.expanduser

Lab 4. Builtin Fixtures • 90


Instead of patching the HOME environmental variable, let’s patch expanduser:

**ch4/monkey/test_cheese.py
def test_def_prefs_change_expanduser (tmpdir,monkeypatch):
fake_home_dir= tmpdir.mkdir( _'home'_ )
monkeypatch.setattr(cheese.os.path, _'expanduser'_ ,
( **lambda** x: x.replace( _'~'_ , str(fake_home_dir))))
cheese.write_default_cheese_preferences()
expected= cheese._default_prefs
actual= cheese.read_cheese_preferences()
**assert** expected== actual

During the test, anything in the cheese module that calls os.path.expanduser() gets

our lambda expression instead. The lambda expression replaces ~ with our

new temporary directory. Now we’ve used setenv() and setattr() to do patching

of environmental variables and attributes. Next up, setitem().

Let’s say we’re worried about what happens if the file already exists. We want

to be sure it gets overwritten with the defaults when write_default_cheese_prefer-

ences() is called:

**ch4/monkey/test_cheese.py
def test_def_prefs_change_defaults (tmpdir,monkeypatch):
_# writethe fileonce_
fake_home_dir= tmpdir.mkdir( _'home'_ )
monkeypatch.setattr(cheese.os.path, _'expanduser'_ ,
( **lambda** x: x.replace( _'~'_ , str(fake_home_dir))))
cheese.write_default_cheese_preferences()
defaults_before= copy.deepcopy(cheese._default_prefs)
_# changethe defaults_
monkeypatch.setitem(cheese._default_prefs, _'slicing'_ , [ _'provolone'_ ])
monkeypatch.setitem(cheese._default_prefs, _'spreadable'_ , [ _'brie'_ ])
monkeypatch.setitem(cheese._default_prefs, _'salads'_ , [ _'pepperjack'_ ])
defaults_modified= cheese._default_prefs
_# writeit againwithmodified defaults_
cheese.write_default_cheese_preferences()
_# read,and check_
actual= cheese.read_cheese_preferences()
**assert** defaults_modified== actual
**assert** defaults_modified!= defaults_before

Because _default_prefs is a dictionary, we can use monkeypatch.setitem() to change

dictionary items just for the duration of the test.

We’ve used setenv(), setattr(), and setitem(). The del forms are pretty similar. They

just delete an environmental variable, attribute, or dictionary item instead of

setting something. The last two monkeypatch methods pertain to paths.

Using monkeypatch • 91


syspath_prepend(path) prepends a path to sys.path, which has the effect of putting

your new path at the head of the line for module import directories. One use

for this would be to replace a system-wide module or package with a stub

version. You can then use monkeypatch.syspath_prepend() to prepend the directory

of your stub version and the code under test will find the stub version first.

chdir(path) changes the current working directory during the test. This would

be useful for testing command-line scripts and other utilities that depend on

what the current working directory is. You could set up a temporary directory

with whatever contents make sense for your script, and then use monkey-

patch.chdir(the_tmpdir).

You can also use the monkeypatch fixture functions in conjunction with

unittest.mock to temporarily replace attributes with mock objects. You’ll look at

that in Lab 7, Using pytest with Other Tools, on page 127.

### Using doctest_namespace

The doctest module is part of the standard Python library and allows you to

put little code examples inside docstrings for a function and test them to

make sure they work. You can have pytest look for and run doctest tests

within your Python code by using the --doctest-modules flag. With the

doctest_namespace builtin fixture, you can build autouse fixtures to add symbols

to the namespace pytest uses while running doctest tests. This allows doc-

strings to be much more readable. doctest_namespace is commonly used to add

module imports into the namespace, especially when Python convention is

to shorten the module or package name. For instance, numpy is often

imported with importnumpyas np.

Let’s play with an example. Let’s say we have a module named unneces-

sary_math.py with multiply() and divide() methods that we really want to make sure

everyone understands clearly. So we throw some usage examples in both the

file docstring and the docstrings of the functions:

**ch4/dt/1/unnecessary_math.py**
"""
Thismoduledefinesmultiply(a,b) and divide(a,b)._

_>>> importunnecessary_mathas um_

_Here'show you use multiply:_

_>>> um.multiply(4,3)
12
>>> um.multiply('a',3)
'aaa'_

Lab 4. Builtin Fixtures • 92


_Here'show you use divide:_

_>>> um.divide(10,5)
2.0
"""

def multiply (a, b):
"""
Returnsa multipliedby b.
>>> um.multiply(4,3)
12
>>> um.multiply('a',3)
'aaa'
"""
    return a * b

def divide (a, b):
"""
Returnsa dividedby b.
>>> um.divide(10,5)
2.0
"""
    return a / b

Since the name unnecessary_math is long, we decide to use um instead by using

importunnecessary_mathas um in the top docstring. The code in the docstrings of

the functions doesn’t include the import statement, but continue with the um

convention. The problem is that pytest treats each docstring with code as a

different test. The import in the top docstring will allow the first part to pass,

but the code in the docstrings of the functions will fail:

**$ cd /path/to/code/ch4/dt/1
$ pytest -v --doctest-modules--tb=shortunnecessary_math.py**
===================testsessionstarts===================
collected3 items

unnecessary_math.py::unnecessary_mathPASSED [ 33%]
unnecessary_math.py::unnecessary_math.divideFAILED[ 66%]
unnecessary_math.py::unnecessary_math.multiplyFAILED[100%]

========================FAILURES=========================
____________[doctest]unnecessary_math.divide ____________
034
035 Returnsa dividedby b.
036
037 >>> um.divide(10,5)
UNEXPECTEDEXCEPTION:NameError("name'um'is not defined")
Traceback(mostrecentcalllast):
**...**
File"<doctestunnecessary_math.divide[0]>", line1, in <module>

Using doctest_namespace • 93


NameError:name'um'is not defined

/path/to/code/ch4/dt/1/unnecessary_math.py:37:UnexpectedException
___________[doctest]unnecessary_math.multiply___________
022
023 Returnsa multipliedby b.
024
025 >>> um.multiply(4,3)
UNEXPECTEDEXCEPTION:NameError("name'um'is not defined")
Traceback(mostrecentcalllast):
**...**
File"<doctestunnecessary_math.multiply[0]>",line1, in <module>

NameError:name'um'is not defined

/path/to/code/ch4/dt/1/unnecessary_math.py:25:UnexpectedException
===========2 failed,1 passedin 0.12seconds============

One way to fix it is to put the import statement in each docstring:

**ch4/dt/2/unnecessary_math.py
def multiply (a, b):
"""
Returnsa multipliedby b.
>>> importunnecessary_mathas um
>>> um.multiply(4,3)
12
>>> um.multiply('a',3)
'aaa'
"""
    return a * b

def divide (a, b):
"""
Returnsa dividedby b.
>>> importunnecessary_mathas um
>>> um.divide(10,5)
2.0
"""
    return a / b

This definitely fixes the problem:

**$ cd /path/to/code/ch4/dt/2
$ pytest -v --doctest-modules--tb=shortunnecessary_math.py**
===================testsessionstarts===================
collected3 items

unnecessary_math.py::unnecessary_mathPASSED [ 33%]
unnecessary_math.py::unnecessary_math.dividePASSED[ 66%]
unnecessary_math.py::unnecessary_math.multiplyPASSED[100%]

================3 passedin 0.03seconds=================

Lab 4. Builtin Fixtures • 94


However, it also clutters the docstrings, and doesn’t add any real value to

readers of the code.

The builtin fixture doctest_namespace, used in an autouse fixture at a top-level

conftest.py file, will fix the problem without changing the source code:

**ch4/dt/3/conftest.py
importpytest
importunnecessary_math**

@pytest.fixture(autouse=True)
def add_um (doctest_namespace):
doctest_namespace[ _'um'_ ] = unnecessary_math

This tells pytest to add the um name to the doctest_namespace and have it

be the value of the imported unnecessary_math module. With this in place in the

conftest.py file, any doctests found within the scope of this conftest.py file will

have the um symbol defined.

I’ll cover running doctest from pytest more in Lab 7, Using pytest with

Other Tools, on page 127.

### Using recwarn

The recwarn builtin fixture is used to examine warnings generated by code

under test. In Python, you can add warnings that work a lot like assertions,

but are used for things that don’t need to stop execution. For example, suppose

we want to stop supporting a function that we wish we had never put into a

package but was released for others to use. We can put a warning in the code

and leave it there for a release or two:

**ch4/test_warnings.py
importwarnings
import pytest

def lame_function ():
warnings.warn( "Pleasestopusingthis" , DeprecationWarning)
_# restof function_

We can make sure the warning is getting issued correctly with a test:

**ch4/test_warnings.py
def test_lame_function (recwarn):
lame_function()
**assert** len(recwarn)== 1
w = recwarn.pop()
**assert** w.category== DeprecationWarning
**assert** str(w.message)== _'Pleasestopusingthis'_

Using recwarn • 95


The recwarn value acts like a list of warnings, and each warning in the list has

a category, message, filename, and lineno defined, as shown in the code.

The warnings are collected at the beginning of the test. If that is inconvenient

because the portion of the test where you care about warnings is near the

end, you can use recwarn.clear() to clear out the list before the chunk of the test

where you do care about collecting warnings.

In addition to recwarn, pytest can check for warnings with pytest.warns():

**ch4/test_warnings.py
def test_lame_function_2 ():
**with** pytest.warns(None) **as** warning_list:
lame_function()
**assert** len(warning_list)== 1
w = warning_list.pop()
**assert** w.category== DeprecationWarning
**assert** str(w.message)== _'Pleasestopusingthis'_

The pytest.warns() context manager provides an elegant way to demark what

portion of the code you’re checking warnings. The recwarn fixture and the

pytest.warns() context manager provide similar functionality, though, so the

decision of which to use is purely a matter of taste.

### Exercises

1. In ch4/cache/test_slower.py, there is an autouse fixture called check_duration(). Copy

it into ch3/tasks_proj/tests/conftest.py.

2. Run the tests in Lab 3.
3. For tests that are really fast, 2x really fast is still really fast. Instead of

2x, change the fixture to check for 0.1 second plus 2x the last duration.

4. Run pytest with the modified fixture. Do the results seem reasonable?

### What’s Next

In this lab, you looked at many of pytest’s builtin fixtures. Next, you’ll

take a closer look at plugins. The nuance of writing large plugins could be a

book in itself; however, small custom plugins are a regular part of the pytest

ecosystem.

Lab 4. Builtin Fixtures • 96


CHAPTER 5

Plugins

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
```
### Installing Plugins

pytest plugins are installed with pip, just like other Python packages. However,

you can use pip in several different ways to install plugins.

**Install from PyPI**

As PyPI is the default location for pip, installing plugins from PyPI is the easiest

method. Let’s install the pytest-cov plugin:

**$ pip install pytest-cov**

This installs the latest stable version from PyPI.

**Install a Particular Version from PyPI**

If you want a particular version of a plugin, you can specify the version

after ‘==‘:

**$ pip install pytest-cov==2.5.1**

**Install from a .tar.gz or .whl File**

Packages on PyPI are distributed as zipped files with the extensions .tar.gz

and/or .whl. These are often referred to as “tar balls” and “wheels.” If you’re

Lab 5. Plugins • 98


having trouble getting pip to work with PyPI directly (which can happen with

firewalls and other network complications), you can download either the .tar.gz

or the .whl and install from that.

You don’t have to unzip or anything; just point pip at it:

**$ pip install pytest-cov-2.5.1.tar.gz**
_# or_
**$ pip install pytest_cov-2.5.1-py2.py3-none-any.whl**

**Install from a Local Directory**

You can keep a local stash of plugins (and other Python packages) in a local

or shared directory in .tar.gz or .whl format and use that instead of PyPI for

installing plugins:

**$ mkdirsome_plugins
$ cp pytest_cov-2.5.1-py2.py3-none-any.whlsome_plugins/
$ pip install --no-index--find-links=./some_plugins/pytest-cov**

The --no-index tells pip to not connect to PyPI. The --find-links=./some_plugins/ tells

pip to look in the directory called some_plugins. This technique is especially

useful if you have both third-party and your own custom plugins stored

locally, and also if you’re creating new virtual environments for continuous

integration or with tox. (We’ll talk about both tox and continuous integration

in Lab 7, Using pytest with Other Tools, on page 127 .)

Note that with the local directory install method, you can install multiple

versions and specify which version you want by adding == and the version

number:

**$ pip install --no-index--find-links=./some_plugins/pytest-cov==2.5.1**

**Install from a Git Repository**

You can install plugins directly from a Git repository—in this case, GitHub:

**$ pip install git+https://github.com/pytest-dev/pytest-cov**

You can also specify a version tag:

**$ pip install git+https://github.com/pytest-dev/pytest-cov@v2.5.1**

Or you can specify a branch:

**$ pip install git+https://github.com/pytest-dev/pytest-cov@master**

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

Lab 5. Plugins • 100


Here are a couple more tests:

**ch5/a/tasks_proj/tests/func/test_api_exceptions.py
importpytest
importtasks
fromtasksimport** Task

@pytest.mark.usefixtures( _'tasks_db'_ )
**class** TestAdd():
"""Testsrelatedto tasks.add()."""
def test_missing_summary (self):
"""Shouldraisean exceptionif summarymissing."""
**with** pytest.raises(ValueError):
tasks.add(Task(owner= _'bob'_ ))
def test_done_not_bool (self):
"""Shouldraisean exceptionif doneis not a bool."""
**with** pytest.raises(ValueError):
tasks.add(Task(summary= _'summary'_ , done= _'True'_ ))

Let’s run them to see if they pass:

**$ cd /path/to/code/ch5/a/tasks_proj
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

Let’s run it again with -v for verbose. Since you’ve already seen the traceback,

you can turn that off with --tb=no.

Writing Your Own Plugins • 101


And now let’s focus on the new tests with -k TestAdd, which works because

there aren’t any other tests with names that contain “TestAdd.”

**$ cd /path/to/code/ch5/a/tasks_proj/tests/func
$ pytest -v --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py::TestAdd::test_done_not_boolFAILED[100%]

====1 failed,1 passed,7 deselectedin 0.10seconds=====

We could go off and try to fix this test (and we should later), but now we are

focused on trying to make failures more pleasant for developers.

Let’s start by adding the “thank you” message to the header, which you can

do with a pytest hook called pytest_report_header().

**ch5/b/tasks_proj/tests/conftest.py
def pytest_report_header ():
"""Thanktesterfor runningtests."""
    return "Thanksfor runningthe tests."

Obviously, printing a thank-you message is rather silly. However, the ability

to add information to the header can be extended to add a username and

specify hardware used and versions under test. Really, anything you can

convert to a string, you can stuff into the test header.

Next, we’ll change the status reporting for tests to change F to O and FAILED to

OPPORTUNITYfor improvement. There’s a hook function that allows for this type of

shenanigans: pytest_report_teststatus():

**ch5/b/tasks_proj/tests/conftest.py
def pytest_report_teststatus (report):
"""Turnfailuresintoopportunities."""
**if** report.when== _'call'_ **and** report.failed:
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )

And now we have just the output we were looking for. A test session with no

--verbose flag shows an O for failures, er, improvement opportunities:

**$ cd /path/to/code/ch5/b/tasks_proj/tests/func
$ pytest --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.O

Lab 5. Plugins • 102


====1 failed,1 passed,7 deselectedin 0.10seconds=====

Writing Your Own Plugins • 103


And the -v or --verbose flag will be nicer also:

**$ pytest -v --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py
::TestAdd::test_done_not_boolOPPORTUNITYfor improvement[100%]

====1 failed,1 passed,7 deselectedin 0.11seconds=====

The last modification we’ll make is to add a command-line option, --nice, to

only have our status modifications occur if --nice is passed in:

**ch5/c/tasks_proj/tests/conftest.py
def pytest_addoption (parser):
"""Turnnicefeatureson with--niceoption."""
group= parser.getgroup( _'nice'_ )
group.addoption( "--nice" , action= "store_true" ,
help= "nice:turnfailuresintoopportunities" )

def pytest_report_header (config):
"""Thanktesterfor runningtests."""
**if** config.getoption( _'nice'_ ):
    return "Thanksfor running the tests."

def pytest_report_teststatus (report,config):
"""Turnfailuresintoopportunities."""
**if** report.when== _'call'_ :
**if** report.failed **and** config.getoption( _'nice'_ ):
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )

This is a good place to note that for this plugin, we are using just a couple of

hook functions. There are many more, which can be found on the main pytest

documentation site.^2

We can manually test our plugin just by running it against our example file.

First, with no --nice option, to make sure just the username shows up:

**$ cd /path/to/code/ch5/c/tasks_proj/tests/func
$ pytest --tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.F [100%]

2. https://docs.pytest.org/en/latest/writing_plugins.html

Lab 5. Plugins • 104


====1 failed,1 passed,7 deselectedin 0.11seconds=====

Now with --nice:

**$ pytest --nice--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py.O [100%]

====1 failed,1 passed,7 deselectedin 0.12seconds=====

And with --nice and --verbose:

**$ pytest -v --nice--tb=notest_api_exceptions.py-k TestAdd**
===================testsessionstarts===================
Thanksfor runningthe tests.
plugins:cov-2.5.1
collected9 items/ 7 deselected

test_api_exceptions.py::TestAdd::test_missing_summaryPASSED[ 50%]
test_api_exceptions.py
::TestAdd::test_done_not_boolOPPORTUNITYfor improvement[100%]

====1 failed,1 passed,7 deselectedin 0.10seconds=====

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

Creating an Installable Plugin • 105


Lab 5. Plugins • 106


pytest-nice
├──LICENCE
├──README.rst
├──pytest_nice.py
├──setup.py
└──tests
├──conftest.py
└──test_nice.py

In pytest_nice.py, we’ll put the exact contents of our conftest.py that were related

to this feature (and take it out of the tasks_proj/tests/conftest.py):

**ch5/pytest-nice/pytest_nice.py**
"""Codefor pytest-niceplugin."""

import pytest

def pytest_addoption (parser):
"""Turnnicefeatureson with--niceoption."""
group= parser.getgroup( _'nice'_ )
group.addoption( "--nice" , action= "store_true" ,
help= "nice:turnFAILEDintoOPPORTUNITYfor improvement" )

def pytest_report_header (config):
"""Thanktesterfor runningtests."""
**if** config.getoption( _'nice'_ ):
    return "Thanksfor running the tests."

def pytest_report_teststatus (report,config):
"""Turnfailuresintoopportunities."""
**if** report.when== _'call'_ :
**if** report.failed **and** config.getoption( _'nice'_ ):
**return (report.outcome, _'O'_ , _'OPPORTUNITYfor improvement'_ )

In setup.py, we need a very minimal call to setup():

**ch5/pytest-nice/setup.py**
"""Setupfor pytest-niceplugin."""

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

Creating an Installable Plugin • 107


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

entry_points={ _'pytest11'_ : [ _'name_of_plugin= myproject.pluginmodule'_ ,], },

I haven’t talked about the README.rst file yet. Some form of README is a

requirement by setuptools. If you leave it out, you’ll get this:

**...**
warning:sdist:standardfilenot found:shouldhaveone of README,
README.rst,README.txt,README.md
**...**

Keeping a README around as a standard way to include some information

about a project is a good idea anyway. Here’s what I’ve put in the file for

pytest-nice:

**ch5/pytest-nice/README.rst**
pytest-nice: A pytestplugin
=============================

Makespytestoutputjusta bit nicerduringfailures.

Features
--------

- Adds``--nice``optionthat:
    - turns``F``to ``O``

Lab 5. Plugins • 108


- with``-v``,turns``FAILURE``to ``OPPORTUNITYfor improvement``

Installation
------------

Giventhatour pytestpluginsare beingsavedin .tar.gzformin the
shareddirectoryPATH,theninstalllikethis:

::

```
$ pip install PATH/pytest-nice-0.1.0.tar.gz
$ pip install --no-index--find-linksPATHpytest-nice
```
Usage
-----

::

```
$ pytest --nice
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

**ch5/pytest-nice/tests/conftest.py**
"""pytesteris neededfor testingplugins."""
pytest_plugins= _'pytester'_

This turns on the pytester plugin. We will be using a fixture called testdir that

becomes available when pytester is enabled.

Often, tests for plugins take on the form we’ve described in manual steps:

1. Make an example test file.
2. Run pytest with or without some options in the directory that contains

our example file.

3. Examine the output.
4. Possibly check the result code—0 for all passing, 1 for some failing.

Let’s look at one example:

Testing Plugins • 109


**ch5/pytest-nice/tests/test_nice.py
def test_pass_fail (testdir):

```
# createa temporarypytest testmodule
testdir.makepyfile( """
def test_pass():
assert1 == 1
def test_fail():
assert1 == 2
""" )
```
```
# run pytest
result= testdir.runpytest()
```
```
# fnmatch_linesdoesan assertioninternally
result.stdout.fnmatch_lines([
'*.F*' , #. for Pass,F for Fail
])
```
```
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

**ch5/pytest-nice/tests/test_nice.py**
@pytest.fixture()
def sample_test (testdir):
testdir.makepyfile( """
def test_pass():
assert1 == 1
def test_fail():
assert1 == 2
""" )
    return testdir

5. https://docs.pytest.org/en/latest/writing_plugins.html#_pytest.pytester.RunResult

Lab 5. Plugins • 110


Now, for the rest of the tests, we can use sample_test as a directory that already

contains our sample test file. Here are the tests for the other option variants:

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

Just a couple more tests to write. Let’s make sure our thank-you message is

in the header:

**ch5/pytest-nice/tests/test_nice.py
def test_header (sample_test):
result= sample_test.runpytest( _'--nice'_ )
result.stdout.fnmatch_lines([ _'Thanksfor runningthe tests.'_ ])

def test_header_not_nice (sample_test):
result= sample_test.runpytest()
thanks_message= _'Thanksfor running the tests.'_
**assert** thanks_message **not in** result.stdout.str()

This could have been part of the other tests also, but I like to have it in a

separate test so that one test checks one thing.

Finally, let’s check the help text:

**ch5/pytest-nice/tests/test_nice.py
def test_help_message (testdir):
result= testdir.runpytest( _'--help'_ )
_# fnmatch_linesdoesan assertioninternally_
result.stdout.fnmatch_lines([
_'nice:'_ ,
_'*--nice*nice:turnFAILEDintoOPPORTUNITYfor improvement'_ ,
])

I think that’s a pretty good check to make sure our plugin works.

Testing Plugins • 111


To run the tests, let’s start in our pytest-nice directory and make sure our plugin

is installed. We do this either by installing the .zip.gz file or installing the cur-

rent directory in editable mode:

**$ cd /path/to/code/ch5/pytest-nice/
$ pip install .**
Processing/path/to/code/ch5/pytest-nice
**...**
Runningsetup.pybdist_wheelfor pytest-nice... done
**...**
Successfullybuiltpytest-nice
Installingcollectedpackages:pytest-nice
Successfullyinstalledpytest-nice-0.1.0

Now that it’s installed, let’s run the tests:

**$ pytest -v**
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

Yay! All the tests pass. We can uninstall it just like any other Python package

or pytest plugin:

**$ pip uninstallpytest-nice**
Uninstallingpytest-nice-0.1.0:
**...**
Proceed(y/n)?y
Successfullyuninstalledpytest-nice-0.1.0

A great way to learn more about plugin testing is to look at the tests contained

in other pytest plugins available through PyPI.

### Creating a Distribution

Believe it or not, we are almost done with our plugin. From the command

line, we can use this setup.py file to create a distribution:

**$ cd /path/to/code/ch5/pytest-nice
$ pythonsetup.pysdist**
runningsdist
runningegg_info

Lab 5. Plugins • 112


creatingpytest_nice.egg-info
**...**
runningcheck
creatingpytest-nice-0.1.0
**...**
creatingdist
Creatingtar archive
**...
$ ls dist**
pytest-nice-0.1.0.tar.gz

(Note that sdist stands for “source distribution.”)

Within pytest-nice, a dist directory contains a new file called pytest-nice-0.1.0.tar.gz.

This file can now be used anywhere to install our plugin, even in place:

**$ pip install dist/pytest-nice-0.1.0.tar.gz**
Processing./dist/pytest-nice-0.1.0.tar.gz
**...**
Installingcollectedpackages:pytest-nice
Successfullyinstalledpytest-nice-0.1.0

However, you can put your .tar.gz files anywhere you’ll be able to get at them

to use and share.

**Distributing Plugins Through a Shared Directory**

pip already supports installing packages from shared directories, so all we

have to do to distribute our plugin through a shared directory is pick a location

we can remember and put the .tar.gz files for our plugins there. Let’s say we

put pytest-nice-0.1.0.tar.gz into a directory called myplugins.

To install pytest-nice from myplugins:

**$ pip install --no-index--find-linksmypluginspytest-nice**

The --no-index tells pip to not go out to PyPI to look for what you want to install.

The --find-linksmyplugins tells PyPI to look in myplugins for packages to install. And

of course, pytest-nice is what we want to install.

If you’ve done some bug fixes and there are newer versions in myplugins, you

can upgrade by adding --upgrade:

**$ pip install --upgrade--no-index--find-linksmypluginspytest-nice**

This is just like any other use of pip, but with the --no-index --find-linksmyplugins

added.

Creating a Distribution • 113


**Distributing Plugins Through PyPI**

If you want to share your plugin with the world, there are a few more steps

we need to do. Actually, there are quite a few more steps. However, because

this book isn’t focused on contributing to open source, I recommend checking

out the thorough instruction found in the Python Packaging User Guide.^6

When you are contributing a pytest plugin, another great place to start is by

using the cookiecutter-pytest-plugin^7 :

**$ pip install cookiecutter
$ cookiecutterhttps://github.com/pytest-dev/cookiecutter-pytest-plugin**

This project first asks you some questions about your plugin. Then it creates

a good directory for you to explore and fill in with your code. Walking through

this is beyond the scope of this book; however, please keep this project in

mind. It is supported by core pytest folks, and they will make sure this project

stays up to date.

### Exercises

In ch4/cache/test_slower.py, there is an autouse fixture called check_duration(). You

used it in the Lab 4 exercises as well. Now, let’s make a plugin out of it.

1. Create a directory named pytest-slower that will hold the code for the new

```
plugin, similar to the directory described in Creating an Installable Plugin,
on page 105.
```
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

Lab 5. Plugins • 114


CHAPTER 6

Configuration

So far in this book, I’ve talked about the various non-test files that affect

pytest mostly in passing, with the exception of conftest.py, which I covered quite

thoroughly in Lab 5, Plugins, on page 97. In this lab, we’ll take a

look at the configuration files that affect pytest, discuss how pytest changes

its behavior based on them, and make some changes to the configuration

files of the Tasks project.

### Understanding pytest Configuration Files

Before I discuss how you can alter pytest’s default behavior, let’s run down

all of the non-test files in pytest and specifically who should care about them.

Everyone should know about these:

- _pytest.ini_ : This is the primary pytest configuration file that allows you to
    change default behavior. Since there are quite a few configuration changes
    you can make, a big chunk of this lab is about the settings you can
    make in pytest.ini.
- _conftest.py_ : This is a local plugin to allow hook functions and fixtures for
    the directory where the conftest.py file exists and all subdirectories. conftest.py
    files are covered Lab 5, Plugins, on page 97.
- ___init__.py_ : When put into every test subdirectory, this file allows you to
    have identical test filenames in multiple test directories. We’ll look at an
    example of what can go wrong without __init__.py files in test directories in
    Avoiding Filename Collisions, on page 123.

If you use tox, you’ll be interested in:

- _tox.ini_ : This file is similar to pytest.ini, but for tox. However, you can put your
    pytest configuration here instead of having both a tox.ini and a pytest.ini file,


```
saving you one configuration file. Tox is covered in Lab 7, Using pytest
with Other Tools, on page 127.
```
If you want to distribute a Python package (like Tasks), this file will be of

interest:

- _setup.cfg_ : This is a file that’s also in ini file format and affects the behavior
    of setup.py. It’s possible to add a couple of lines to setup.py to allow you to
    run pythonsetup.pytest and have it run all of your pytest tests. If you are
    distributing a package, you may already have a setup.cfg file, and you can
    use that file to store pytest configuration. You’ll see how in Appendix 4,
    Packaging and Distributing Python Projects, on page 175.

Regardless of which file you put your pytest configuration in, the format will

mostly be the same.

For pytest.ini:

**ch6/format/pytest.ini
[pytest]**
addopts= _-rsxX-l --tb=short--strict_
xfail_strict= _true
;...moreoptions..._

For tox.ini:

**ch6/format/tox.ini**
_;...tox specificstuff..._

**[pytest]**
addopts= _-rsxX-l --tb=short--strict_
xfail_strict= _true
;...moreoptions..._

For setup.cfg:

**ch6/format/setup.cfg**
;...packagingspecificstuff...

[tool:pytest]
addopts= -rsxX-l --tb=short--strict
xfail_strict= true
;...moreoptions...

The only difference is that the section header for setup.cfg is [tool:pytest] instead

of [pytest].

**List the Valid ini-file Options with pytest –help**

You can get a list of all the valid settings for pytest.ini from pytest --help. This is

a sampling:

Lab 6. Configuration • 116


**$ pytest --help
...**
[pytest]ini-optionsin the firstpytest.ini|tox.ini|setup.cfgfilefound:

markers(linelist) markersfor testfunctions
norecursedirs(args) directorypatternsto avoidfor recursion
testpaths(args) directoriesto searchfor testswhenno filesor
directoriesare givenin the commandline.
usefixtures(args) listof defaultfixturesto be usedwiththisproject
python_files(args) glob-stylefilepatternsfor Pythontestmodulediscovery
python_classes(args) prefixesor globnamesfor Pythontestclassdiscovery
python_functions(args) prefixesor globnamesfor Pythontestfunctionand
methoddiscovery
xfail_strict(bool) defaultfor the strictparameterof xfailmarkers
whennot givenexplicitly(default:False)
doctest_optionflags(args)optionflagsfor doctests
addopts(args) extracommandlineoptions
minversion(string) minimallyrequiredpytestversion
**...**

You’ll look at all of these settings in this lab, except doctest_optionflags, which

is covered in Lab 7, Using pytest with Other Tools, on page 127.

**Plugins Can Add ini-file Options**

The previous settings list is not a constant. It is possible for plugins (and

conftest.py files) to add ini file options. The added options will be added to the

pytest --help output as well.

Now, let’s explore some of the configuration changes we can make with the

builtin ini file settings available from core pytest.

### Changing the Default Command-Line Options

You’ve used a lot of command-line options for pytest so far, like -v/--verbose

for verbose output and -l/--showlocals to see local variables with the stack trace

for failed tests. You may find yourself always using some of those options—or

preferring to use them—for a project. If you set addopts in pytest.ini to the options

you want, you don’t have to type them in anymore. Here’s a set I like:

**[pytest]**
addopts= _-rsxX-l --tb=short--strict_

The -rsxX tells pytest to report the reasons for all tests that skipped, xfailed, or

xpassed. The -l tells pytest to report the local variables for every failure with

the stacktrace. The --tb=short removes a lot of the stack trace. It leaves the file

and line number, though. The --strict option disallows markers to be used if they

aren’t registered in a config file. You’ll see how to do that in the next section.

Changing the Default Command-Line Options • 117


### Registering Markers to Avoid Marker Typos

Custom markers, as discussed in Marking Test Functions, on page 31, are

great for allowing you to mark a subset of tests to run with a specific marker.

However, it’s too easy to misspell a marker and end up having some tests

marked with @pytest.mark.smoke and some marked with @pytest.mark.somke. By

default, this isn’t an error. pytest just thinks you created two markers. This

can be fixed, however, by registering markers in pytest.ini, like this:

**[pytest]**
markers=
**smoke:Run the smoketestfunctionsfor tasksproject
get:Run the testfunctionsthattesttasks.get()**

With these markers registered, you can now also see them with pytest --markers

with their descriptions:

**$ cd /path/to/code/ch6/b/tasks_proj/tests
$ pytest --markers**
@pytest.mark.smoke:Run the smoketesttestfunctions

@pytest.mark.get:Run the testfunctionsthattesttasks.get()

**...**

@pytest.mark.skip(reason=None):skipthe ...

**...**

If markers aren’t registered, they won’t show up in the --markers list. With them

registered, they show up in the list, and if you use --strict, any misspelled or

unregistered markers show up as an error. The only difference between

ch6/a/tasks_proj and ch6/b/tasks_proj is the contents of the pytest.ini file. It’s empty

in ch6/a. Let’s try running the tests without registering any markers:

**$ cd /path/to/code/ch6/a/tasks_proj/tests
$ pytest --strict --tb=line**
===================testsessionstarts===================
plugins:cov-2.5.1
collected45 items/ 2 errors

=========================ERRORS==========================
____________ERRORcollectingfunc/test_add.py____________
func/test_add.py:20:in <module>
@pytest.mark.smoke
**...**
E AttributeError:'smoke'not a registeredmarker
______ERRORcollectingfunc/test_api_exceptions.py_______
func/test_api_exceptions.py:30:in <module>
@pytest.mark.smoke
**...**

Lab 6. Configuration • 118


E AttributeError:'smoke'not a registeredmarker
!!!!!!!!!Interrupted:2 errorsduringcollection!!!!!!!!!
=================2 errorin 0.29seconds=================

If you use markers in pytest.ini to register your markers, you may as well add

--strict to your addopts while you’re at it. You’ll thank me later. Let’s go ahead

and add a pytest.ini file to the tasks project:

**ch6/b/tasks_proj/tests/pytest.ini
[pytest]**
addopts= _-rsxX-l --tb=short--strict_
markers=
**smoke:Run the smoketesttestfunctions
get:Run the testfunctionsthattesttasks.get()**

This has a combination of flags I prefer over the defaults: -rsxX to report which

tests skipped, xfailed, or xpassed, --tb=short for a shorter traceback for failures,

and --strict to only allow declared markers. And then a list of markers to allow

for the project.

This should allow us to run tests, including the smoke tests:

**$ cd /path/to/code/ch6/b/tasks_proj/tests
$ pytest --strict-m smoke**
===================testsessionstarts===================
plugins:cov-2.5.1
collected57 items/ 54 deselected

func/test_add.py. [ 33%]
func/test_api_exceptions.py.. [100%]

=========3 passed,54 deselectedin 0.13seconds=========

### Requiring a Minimum pytest Version

The minversion setting enables you to specify a minimum pytest version you

expect for your tests. For instance, I like to use approx() when testing floating

point numbers for “close enough” equality in tests. But this feature didn’t get

introduced into pytest until version 3.0. To avoid confusion, I add the following

to projects that use approx():

**[pytest]**
minversion= _3.0_

This way, if someone tries to run the tests using an older version of pytest,

an error message appears.

Requiring a Minimum pytest Version • 119


### Stopping pytest from Looking in the Wrong Places

Did you know that one of the definitions of “recurse” is to swear at your code

twice? Well, no. But, it does mean to traverse subdirectories. In the case of

pytest, test discovery traverses many directories recursively. But there are

some directories you just know you don’t want pytest looking in.

The default setting for norecurse is '.* build dist CVS _darcs {arch} and *.egg. Having '.*'

is a good reason to name your virtual environment '.venv', because all directo-

ries starting with a dot will not be traversed. However, I have a habit of

naming it venv, so I could add that to norecursedirs.

In the case of the Tasks project, you could list src in there also, because having

pytest look for test files there would just be a waste of time.

**[pytest]**
norecursedirs= _.* venvsrc *.eggdistbuild_

When overriding a setting that already has a useful value, like this setting,

it’s a good idea to know what the defaults are and put the ones back you care

about, as I did in the previous code with *.egg dist build.

The norecursedirs is kind of a corollary to testpaths, so let’s look at that next.

### Specifying Test Directory Locations

Whereas norecursedirs tells pytest where not to look, testpaths tells pytest where

to look. testspaths is a list of directories relative to the root directory to look in

for tests. It’s only used if a directory, file, or nodeid is not given as an argument.

Suppose for the Tasks project we put pytest.ini in the tasks_proj directory instead

of under tests:

tasks_proj/
├──pytest.ini
├──src
│ └──tasks
│ ├──api.py
│ └──...
└──tests
├──conftest.py
├──func
│ ├──__init__.py
│ ├──test_add.py
│ ├──...
└──unit
├──__init__.py
├──test_task.py

Lab 6. Configuration • 120


#### └──...

It could then make sense to put tests in testpaths:

**[pytest]**
testpaths= _tests_

Now, as long as you start pytest from the tasks_proj directory, pytest will only

look in tasks_proj/tests. My problem with this is that I often bounce around a

test directory during test development and debugging, so I can easily test a

subdirectory or file without typing out the whole path. Therefore, for me, this

setting doesn’t help much with interactive testing.

However, it’s great for tests launched from a continuous integration server

or from tox. In those cases, you know that the root directory is going to be

fixed, and you can list directories relative to that fixed root. These are also

the cases where you really want to squeeze your test times, so shaving a bit

off of test discovery is awesome.

At first glance, it might seem silly to use both testpaths and norecursedirs at the

same time. However, as you’ve seen, testspaths doesn’t help much with interac-

tive testing from different parts of the file system. In those cases, norecursedirs

can help. Also, if you have directories with tests that don’t contain tests, you

could use norecursedirs to avoid those. But really, what would be the point of

putting extra directories in tests that don’t have tests?

### Changing Test Discovery Rules

pytest finds tests to run based on certain test discovery rules. The standard

test discovery rules are:

- Start at one or more directory. You can specify filenames or directory
    names on the command line. If you don’t specify anything, the current
    directory is used.
- Look in the directory and all subdirectories recursively for test modules.
- A test module is a file with a name that looks like test_*.py or *_test.py.
- Look in test modules for functions that start with test_.
- Look for classes that start with Test. Look for methods in those classes
    that start with test_ but don’t have an __init__ method.

These are the standard discovery rules; however, you can change them.

Changing Test Discovery Rules • 121


**python_classes**

The usual test discovery rule for pytest and classes is to consider a class a

potential test class if it starts with Test*. The class also can’t have an __init__()

function. But what if we want to name our test classes <something>Test or

<something>Suite? That’s where python_classes comes in:

**[pytest]**
python_classes= _*TestTest**Suite_

Lab 6. Configuration • 122


This enables us to name classes like this:

**class** DeleteSuite():
def test_delete_1 ():
...
def test_delete_2 ():
...
....

**python_files**

Like pytest_classes, python_files modifies the default test discovery rule, which is

to look for files that start with test_* or end in *_test.

Let’s say you have a custom test framework in which you named all of your

test files check_<something>.py. Seems reasonable. Instead of renaming all of

your files, just add a line to pytest.ini like this:

**[pytest]**
python_files= _test_**_testcheck_*_

Easy peasy. Now you can migrate your naming convention gradually if you

want to, or just leave it as check_*.

**python_functions**

python_functions acts like the previous two settings, but for test function and

method names. The default is test_*. To add check_*—you guessed it—do this:

**[pytest]**
python_functions= _test_*check_*_

Now the pytest naming conventions don’t seem that restrictive, do they? If you

don’t like the default naming convention, just change it. However, I encourage

you to have a better reason. Migrating hundreds of test files is definitely a

good reason.

### Disallowing XPASS

Setting xfail_strict= true causes tests marked with @pytest.mark.xfail that don’t fail to

be reported as an error. I think this should always be set. For more information

on the xfail marker, go to Marking Tests as Expecting to Fail, on page 37.

### Avoiding Filename Collisions

The utility of having __init__.py files in every test subdirectory of a project con-

fused me for a long time. However, the difference between having these and

Disallowing XPASS • 123


not having these is simple. If you have __init__.py files in all of your test subdi-

rectories, you can have the same test filename show up in multiple directories.

If you don’t, you can’t. That’s it. That’s the effect on you.

Here’s an example. Directory a and b both have the file, test_foo.py. It doesn’t

matter what these files have in them, but for this example, they look like this:

**ch6/dups/a/test_foo.py
def test_a ():
**pass**

**ch6/dups/b/test_foo.py
def test_b ():
**pass**

With a directory structure like this:

dups
├──a
│ └──test_foo.py
└──b
└──test_foo.py

These files don’t even have the same content, but it’s still mucked up. Run-

ning them individually will be fine, but running pytest from the dups directory

won’t work:

**$ cd /path/to/code/ch6/dups
$ pytesta**
===================testsessionstarts===================
plugins:cov-2.5.1
collected1 item

a/test_foo.py. [100%]

================1 passedin 0.01seconds=================
**$ pytestb**
===================testsessionstarts===================
plugins:cov-2.5.1
collected1 item

b/test_foo.py. [100%]

================1 passedin 0.01seconds=================
**$ pytest
===================testsessionstarts===================
plugins:cov-2.5.1
collected1 item/ 1 errors

=========================ERRORS==========================
_____________ERRORcollectingb/test_foo.py______________
importfilemismatch:
importedmodule'test_foo'has this__file__attribute:

Lab 6. Configuration • 124


/path/to/code/ch6/dups/a/test_foo.py
whichis not the sameas the testfilewe wantto collect:
/path/to/code/ch6/dups/b/test_foo.py
HINT:remove__pycache__/ .pycfiles
and/oruse a uniquebasename for yourtestfilemodules
!!!!!!!!!Interrupted:1 errorsduringcollection!!!!!!!!!
=================1 errorin 0.15seconds=================

That error message highlights that we have two files named the same, but

doesn’t tell us how to fix it.

To fix this test, just add empty __init__.py files in the subdirectories. Here, the

example directory dups_fixed is the same as dups, but with __init__.py files added:

dups_fixed/
├──a
│ ├──__init__.py
│ └──test_foo.py
└──b
├──__init__.py
└──test_foo.py

Now, let’s try this again from the top level in dups_fixed:

**$ cd /path/to/code/ch6/dups_fixed
$ pytest
===================testsessionstarts===================
plugins:cov-2.5.1
collected2 items

a/test_foo.py. [ 50%]
b/test_foo.py. [100%]

================2 passedin 0.03seconds=================

There, all better. You might say to yourself that you’ll never have duplicate

filenames, so it doesn’t matter. That’s fine. But projects grow and test direc-

tories grow, and do you really want to wait until it happens to you before you

fix it? I say just put those files in there as a habit and don’t worry about it

again.

### Exercises

In Lab 5, Plugins, on page 97, you created a plugin called pytest-nice that

included a --nice command-line option. Let’s extend that to include a pytest.ini

option called nice.

1. Add the following line to the pytest_addoption hook function in pytest_nice.py:

```
parser.addini('nice',type='bool',help='Turn failures into opportunities.')
```
Exercises • 125


2. The places in the plugin that use getoption() will have to also call getini('nice').

Make those changes.

3. Manually test this by adding nice to a pytest.ini file.
4. Don’t forget the plugin tests. Add a test to verify that the setting ‘nice’

from pytest.ini works correctly.

5. Add the tests to the plugin tests directory. You’ll need to look up some

extra pytester functionality.^1

### What’s Next

While pytest is extremely powerful on its own—especially so with plugins—it

also integrates well with other software development and software testing

tools. In the next lab, you’ll look at using pytest in conjunction with

other powerful testing tools.

1. https://docs.pytest.org/en/latest/_modules/_pytest/pytester.html#Testdir

Lab 6. Configuration • 126


CHAPTER 7

Using pytest with Other Tools

You don’t usually use pytest on its own, but rather in a testing environment

with other tools. This lab looks at other tools that are often used in

combination with pytest for effective and efficient testing. While this is by no

means an exhaustive list, the tools discussed here give you a taste of the

power of mixing pytest with other tools.

### pdb: Debugging Test Failures

The pdb module is the Python debugger in the standard library. You use --pdb

to have pytest start a debugging session at the point of failure. Let’s look at

pdb in action in the context of the Tasks project.

In Parametrizing Fixtures, on page 66, we left the Tasks project with a few

failures:

**$ cd /path/to/code/ch3/c/tasks_proj
$ pytest --tb=no-q**
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
As mentioned in Lab 3, pytest Fixtures, on page 51, running
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

**$ pytest --tb=no--verbose--lf--maxfail=3**
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

**$ pytest -v --lf-l -x**
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
Lab 7. Using pytest with Other Tools • 128


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

**$ pytest -v --lf-x --pdb**
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

Lab 7. Using pytest with Other Tools • 130


some of the Tasks functionality is executed with every test, but not all of it.

Code coverage tools are great for telling you which parts of the system are

being completely missed by tests.

Coverage.py is the preferred Python coverage tool that measures code coverage.

You’ll use it to check the Tasks project code under test with pytest.

Before you use coverage.py, you need to install it. I’m also going to have you

install a plugin called pytest-cov that will allow you to call coverage.py from pytest

with some extra pytest options. Since coverage is one of the dependencies of

pytest-cov, it is sufficient to install pytest-cov, as it will pull in coverage.py:

**$ pip install pytest-cov
...**
Installingcollectedpackages:coverage,pytest-cov
Successfullyinstalledcoverage-4.5.1 pytest-cov-2.5.1

Let’s run the coverage report on version 2 of Tasks. If you still have the first

version of the Tasks project installed, uninstall it and install version 2:

**$ pip uninstalltasks**
Uninstallingtasks-0.1.0:
**...**
Proceed(y/n)?y
Successfullyuninstalledtasks-0.1.0
**$ cd /path/to/code/ch7/tasks_proj_v2
$ pip install -e.**
Obtainingfile:///path/to/code/ch7/tasks_proj_v2
**...**
Installingcollectedpackages:tasks
Runningsetup.pydevelopfor tasks
Successfullyinstalledtasks
**$ pip list
...**
tasks 0.1.1 /path/to/code/tasks_proj_v2/src
**...**

Coverage.py: Determining How Much Code Is Tested • 131


Now that the next version of Tasks is installed, we can run our baseline cov-

erage report:

**$ cd /path/to/code/ch7/tasks_proj_v2
$ pytest --cov=src**
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

**$ pytest --cov=src--cov-report=html**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1

Lab 7. Using pytest with Other Tools • 132


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


Lab 7. Using pytest with Other Tools • 134


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

Lab 7. Using pytest with Other Tools • 136


Let’s pause and install version 2 of Tasks:

**$ cd /path/to/code/
$ pip install -e ch7/tasks_proj_v2
...**
Successfullyinstalledtasks

In the rest of this section, you’ll develop some tests for the “list” functionality.

Let’s see it in action to understand what we’re going to test:

**$ taskslist**
ID owner donesummary
-- ----- -----------
**$ tasksadd** "do somethinggreat"
**$ tasksadd** "repeat" **-o Brian
$ tasksadd** "againand again" **--ownerOkken
$ taskslist**
ID owner donesummary
-- ----- -----------
1 Falsedo somethinggreat
2 BrianFalserepeat
3 OkkenFalseagainand again
**$ taskslist-o Brian**
ID owner donesummary
-- ----- -----------
2 BrianFalserepeat
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
def tasks_cli ():
"""Runthe tasksapplication."""
**pass**

mock: Swapping Out Part of the System • 137


Obvious? No. But hold on, it gets better (or worse, depending on your perspec-

tive). Here’s one of the commands—the list command:

**ch7/tasks_proj_v2/src/tasks/cli.py**
@tasks_cli.command(name= "list" , help= "listtasks" )
@click.option( _'-o'_ , _'--owner'_ , default=None,
help= _'listtaskswiththisowner'_ )
def list_tasks (owner):
"""
Listtasksin db.
If ownergiven,onlylisttaskswiththatowner.
"""
formatstr= "{: >4} {: >10}{: >5} {}"
**print (formatstr.format( _'ID'_ , _'owner'_ , _'done'_ , _'summary'_ ))
**print (formatstr.format( _'--'_ , _'-----'_ , _'----'_ , _'-------'_ ))
**with** _tasks_db():
**for** t **in** tasks.list_tasks(owner):
done= _'True'_ **if** t.done **else** _'False'_
owner= _''_ **if** t.owner **is** None **else** t.owner
**print (formatstr.format(
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
def _tasks_db ():
config= tasks.config.get_config()
tasks.start_tasks_db(config.db_path,config.db_type)
**yield**
tasks.stop_tasks_db()

The _tasks_db() function is a context manager that retrieves the configuration

from tasks.config.get_config(), another external dependency, and uses the config-

uration to start a connection with the database. The yield releases control to

Lab 7. Using pytest with Other Tools • 138


the with block of list_tasks(), and after everything is done, the database connection

is stopped.

For the purpose of just testing the CLI behavior up to the point of calling API

functions, we don’t need a connection to an actual database. Therefore, we

can replace the context manager with a simple stub:

**ch7/tasks_proj_v2/tests/unit/test_cli.py**
@contextmanager
def stub_tasks_db ():
**yield**

Because this is the first time we’ve looked at our test code for test_cli,py, let’s

look at this with all of the import statements:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
fromclick.testingimport** CliRunner
**fromcontextlibimport** contextmanager
import pytest
fromtasks.apiimport** Task
import tasks.cli
importtasks.config**

@contextmanager
def stub_tasks_db ():
**yield**

Those imports are for the tests. The only import needed for the stub is from

contextlib importcontextmanager.

We’ll use mock to replace the real context manager with our stub. Actually,

we’ll use mocker, which is a fixture provided by the pytest-mock plugin. Let’s look

at an actual test. Here’s a test that calls tasks list:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
def test_list_no_args (mocker):
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
def no_db (mocker):
mocker.patch.object(tasks.cli, _'_tasks_db'_ , new=stub_tasks_db)

def test_list_print_empty (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ , return_value=[])
runner= CliRunner()
result= runner.invoke(tasks.cli.tasks_cli,[ _'list'_ ])
expected_output= ( " ID owner donesummary\n"
" -- ----- -----------\n" )
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

Lab 7. Using pytest with Other Tools • 140


The rest of the tests for the tasks list functionality don’t add any new concepts,

but perhaps looking at several of these makes the code easier to understand:

**ch7/tasks_proj_v2/tests/unit/test_cli.py
def test_list_print_many_items (no_db,mocker):
many_tasks= (
Task( _'writelab'_ , _'Brian'_ , True,1),
Task( _'editlab'_ , _'Katie'_ , False,2),
Task( _'modifylab'_ , _'Brian'_ , False,3),
Task( _'finalizelab'_ , _'Katie'_ , False,4),
)
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ ,
return_value=many_tasks)
runner= CliRunner()
result= runner.invoke(tasks.cli.tasks_cli,[ _'list'_ ])
expected_output= ( " ID owner donesummary\n"
" -- ----- -----------\n"
" 1 Brian Truewritelab\n"
" 2 KatieFalseeditlab\n"
" 3 BrianFalsemodifylab\n"
" 4 KatieFalsefinalizelab\n" )
**assert** result.output== expected_output

def test_list_dash_o (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ )
runner= CliRunner()
runner.invoke(tasks.cli.tasks_cli,[ _'list'_ , _'-o'_ , _'brian'_ ])
tasks.cli.tasks.list_tasks.assert_called_once_with( _'brian'_ )

def test_list_dash_dash_owner (no_db,mocker):
mocker.patch.object(tasks.cli.tasks, _'list_tasks'_ )
runner= CliRunner()
runner.invoke(tasks.cli.tasks_cli,[ _'list'_ , _'--owner'_ , _'okken'_ ])
tasks.cli.tasks.list_tasks.assert_called_once_with( _'okken'_ )

Let’s make sure they all work:

**$ cd /path/to/code/ch7/tasks_proj_v2
$ pytest -v tests/unit/test_cli.py**
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
2. tox pip install s some dependencies.
3. tox pip install s your package from the sdist in step 1.
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

Lab 7. Using pytest with Other Tools • 142


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

to configure pytest, as discussed in Lab 6, Configuration, on page 115. In

this case, addopts is used to turn on extra summary information for skips,

xfails, and xpasses (-rsxX) and turn on showing local variables in stack traces

tox: Testing Multiple Configurations • 143


(-l). It also defaults to shortened stack traces (--tb=short) and makes sure all

markers used in tests are declared first (--strict). The markers section is where

the markers are declared.

Before running tox, you have to make sure you install it:

**$ pip install tox**

This can be done within a virtual environment.

Then to run tox, just run, well, tox:

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

Lab 7. Using pytest with Other Tools • 144


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
${venv}/bin/pytest --junitxml=${WORKSPACE}/results.xml

The bottom line has ${venv}/bin/pytest --junit-xml=${WORKSPACE}/results.xml. The --junit-

xml flag is the only thing needed to produce the junit.xml format results file

Jenkins needs.

Next, we’ll add a post-build action: Add post-buildaction->PublishJunit test result report.

Fill in the Test report XMLs with results.xml, as shown in the next screen.

Lab 7. Using pytest with Other Tools • 146


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

Lab 7. Using pytest with Other Tools • 148


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

def setUpModule ():
"""Maketempdir,initializeDB."""
**global** temp_dir
temp_dir= tempfile.mkdtemp()
tasks.start_tasks_db(str(temp_dir), _'tiny'_ )

def tearDownModule ():
"""Cleanup DB, removetempdir."""
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

**$ cd /path/to/code/ch7/unittest
$ python-m unittest-v test_delete_unittest.py**
test_delete_decreases_count(test_delete_unittest.TestNonEmpty)... ok

----------------------------------------------------------------------
Ran 1 testin 0.029s

OK

It also runs fine in pytest:

**$ pytest -v test_delete_unittest.py**
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

def test_delete_decreases_count (db_with_3_tasks):
ids = [t.id **for** t **in** tasks.list_tasks()]
_# GIVEN3 items_
**assert** tasks.count()== 3
_# WHENwe deleteone_
tasks.delete(ids[0])
_# THENcountdecreasesby 1_
**assert** tasks.count()== 2

Lab 7. Using pytest with Other Tools • 150


The fixtures we’ve been using for the Tasks project, including db_with_3_tasks

introduced in Using Multiple Fixtures, on page 57, help set up the database

before the test. It’s a much smaller file, even though the test function itself

is almost identical.

Both tests pass individually:

**$ pytest-q test_delete_pytest.py**

. [100%]
1 passedin 0.02seconds
**$ pytest-q test_delete_unittest.py**
. [100%]
1 passedin 0.02seconds

You can even run them together if—and only if—you make sure the unittest

version runs first:

**$ pytest -v test_delete_unittest.pytest_delete_pytest.py**
===================testsessionstarts===================
plugins:mock-1.10.0,cov-2.5.1
collected2 items

test_delete_unittest.py::TestNonEmpty::test_delete_decreases_countPASSED[ 50%]
test_delete_pytest.py::test_delete_decreases_countPASSED[100%]

================2 passedin 0.03seconds=================

If you run the pytest version first, something goes haywire:

**$ pytest -v test_delete_pytest.pytest_delete_unittest.py**
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
Lab 7. Using pytest with Other Tools • 152


def test_delete_decreases_count (self):
_# GIVEN3 items_
self.assertEqual(tasks.count(),3)
_# WHENwe deleteone_
tasks.delete(self.ids[0])
_# THENcountdecreasesby 1_
self.assertEqual(tasks.count(),2)

Now both unittest and pytest can cooperate and not run into each other:

**$ pytest -v test_delete_pytest.pytest_delete_unittest_fix.py**
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
def tasks_db_non_empty (tasks_db_session,request):
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

permutations in the labs.

Lab 7. Using pytest with Other Tools • 154


### What’s Next

You are definitely ready to go out and try pytest with your own projects. And

check out the appendixes that follow. If you’ve made it this far, I’ll assume

you no longer need help with pip or virtual environments. However, you may

not have looked at Appendix 3, Plugin Sampler Pack, on page 163. If you

enjoyed this lab, it deserves your time to at least skim through it. Then,

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


APPENDIX 1

Virtual Environments

Python virtual environments enable you to set up a Python sandbox with its

own set of packages separate from the system site-packages in which to work.

There are many reasons to use virtual environments, such as if you have

multiple services running with the same Python installation, but with different

packages and package version requirements. In addition, you might find it

handy to keep the dependent package requirements separate for every Python

project you work on. Virtual environments let you do that.

As of Python 3.3, the venv virtual environment module is included as part of

the standard library. However, some problems with venv have been reported

on some versions of Linux. If you have any trouble with venv, use virtualenv

instead. Just remember to pip install virtualenv first.

Here’s how to set up a virtual environment in macOS and Linux:

**$ python3-m venvenv_name
$ sourceenv_name/bin/activate**
(env_name)$
**... do yourwork...**
(env_name)$ deactivate


In Windows, there’s a change to the activate line:

**C:/>python3-m venvenv_name
C:/>env_name/Scripts/activate.bat**
(env_name)C:/>
**... do yourwork...**
(env_name)C:/>deactivate

I usually put the virtual environment directory, env_name, directly in my

project’s top directory. I’ve also seen two additional installation methods that

are interesting and could work for you:

1. Put the virtual environment in the project directory (as was done in the

```
previous code), but name the env directory something consistent, such
as venv or .venv. The benefit of this is that you can put venv or .venv in your
global .gitignore file. The downside is that the environment name hint in
the command prompt just tells you that you are using a virtual environ-
ment, but not which one.
```
2. Put all virtual environments into a common directory, such as ~/venvs/.

```
Now the environment names will be different, letting the command prompt
be more useful. You also don’t need to worry about .gitignore, since it’s not
in your project tree. Finally, this directory is one place to look if you forget
all of the projects you’re working on.
```
Remember, a virtual environment is a directory with links back to the python.exe

file and the pip.exe file of the site-wide Python version it’s using. But anything

you install is installed in the virtual environment directory, and not in the

global site-packages directory. When you’re done with a virtual environment,

you can just delete the directory and it completely disappears.

I’ve covered the basics and common use case of venv. However, venv is a flexible

tool with many options. Be sure to check out python-m venv --help. It may pre-

emptively answer questions you may have about your specific situation. Also,

the Python docs on venv^1 are worth reading if you still have questions.

1. https://docs.python.org/3/library/venv.html

Appendix 1. Virtual Environments • 158


APPENDIX 2

pip

pip is the tool used to install Python packages, and it is installed as part of

your Python installation. pip supposedly is a recursive acronym that stands

for _pip install s Python_ or _pip install s Packages._ (Programmers can be pretty

nerdy with their humor.) If you have more than one version of Python installed

on your system, each version has its own pip package manager.

By default, when you run pip install something, pip will:

1. Connect to the PyPI repository at https://pypi.python.org/pypi.
2. Look for a package called something.
3. Download the appropriate version of something for your version of Python

and your system.

4. Install something into the site-packages directory of your Python installation

that was used to call pip.

This is a gross understatement of what pip does—it also does cool stuff like

setting up scripts defined by the package, wheel caching, and more.

As mentioned, each installation of Python has its own version of pip tied to it.

If you’re using virtual environments, pip and python are automatically linked

to whichever Python version you specified when creating the virtual environ-

ment. If you aren’t using virtual environments, and you have multiple Python

versions installed, such as python3.5 and python3.6, you will probably want to

use python3.5-m pip or python3.6-m pip instead of pip directly. It works just the

same. (For the examples in this appendix, I assume you are using virtual

environments so that pip works just fine as-is.)


To check the version of pip and which version of Python it’s tied to, use pip --

version:

(my_env)$ pip --version
pip 18.0from/path/to/code/venv/lib/python3.7/site-packages/pip(python3.7)

To list the packages you have currently installed with pip, use pip list. If there’s

something there you don’t want anymore, you can uninstall it with pip uninstall

something.

Package Version
-----------------
pip 18.0
setuptools39.0.1
(my_env)$ pip install pytest
**...**
Installingcollectedpackages:six,more-itertools,atomicwrites,
pluggy,attrs,py, pytest
Successfullyinstalledatomicwrites-1.2.1attrs-18.2.0
more-itertools-4.3.0pluggy-0.7.1py-1.6.0pytest-3.8.0six-1.11.0
(my_env)$ pip list
Package Version
---------------------
Package Version
---------------------
atomicwrites 1.2.1
attrs 18.2.0
more-itertools4.3.0
pip 18.0
pluggy 0.7.1
py 1.6.0
pytest 3.8.1
setuptools 39.0.1
six 1.11.0

As shown in this example, pip install s the package you want and also any

dependencies that aren’t already installed.

pip is pretty flexible. It can install things from other places, such as GitHub,

your own servers, a shared directory, or a local package you’re developing

yourself, and it always sticks the packages in site-packages unless you’re using

Python virtual environments.

You can use pip to install packages with version numbers from pypi.python.org

if it’s a release version PyPI knows about:

**$ pip install pytest==3.2.1**

You can use pip to install a local package that has a setup.py file in it:

Appendix 2. pip • 160


**$ pip install /path/to/package**

Use ./package_name if you are in the same directory as the package to install it

locally:

**$ cd /path/just/above/package
$ pip install my_package** _# pip is lookingin PyPIfor "my_package"
**$ pip install ./my_package** _# now pip lookslocally_

You can use pip to install packages that have been downloaded as zip files or

wheels without unpacking them.

You can also use pip to download a lot of files at once using a requirements.txt file:

(my_env)$ cat requirements.txt
pytest==3.8.1
pytest-xdist==1.23.2
(my_env)$ pip install -r requirements.txt
**...**
Successfullyinstalledapipkg-1.5execnet-1.5.0pytest-3.8.1pytest-xdist-1.23.2

You can use pip to download a bunch of various versions into a local cache

of packages, and then point pip there instead of PyPI to install them into vir-

tual environments later, even when offline.

The following downloads pytest and all dependencies:

(my_env)$ mkdir~/.pipcache
(my_env)$ pip download-d ~/pipcachepytest
Collectingpytest
**...**
Successfullydownloadedpytestatomicwritespluggy
more-itertoolssetuptoolspy six attrs

Later, even if you’re offline, you can install from the cache:

(my_env)$ pip install --no-index--find-links=~/pipcachepytest
Lookingin links:/Users/okken/pipcache
Collectingpytest
**...**
Installingcollectedpackages:attrs,six,more-itertools,atomicwrites,
py, pluggy,pytest
Successfullyinstalledatomicwrites-1.x.yattrs-18.x.y
more-itertools-4.x.ypluggy-0.x.ypy-1.x.ypytest-3.x.ysix-1.x.y

This is great for situations like running tox or continuous integration test

suites without needing to grab packages from PyPI. I also use this method to

grab a bunch of packages before taking a trip so that I can code on the plane.

Appendix 2. pip • 161


The Python Packaging Authority documentation^1 is a great resource for more

information on pip.

1. https://pip.pypa.io

Appendix 2. pip • 162


APPENDIX 3

Plugin Sampler Pack

Plugins are the booster rockets that enable you to get even more power out

of pytest. So many useful plugins are available, it’s difficult to pick just a

handful to showcase. You’ve already seen the pytest-cov plugin in Coverage.py:

Determining How Much Code Is Tested, on page 130 , and the pytest-mock plugin

in mock: Swapping Out Part of the System, on page 136. The following plugins

give you just a taste of what else is out there.

All of the plugins featured here are available on PyPI and are installed with

pip install <plugin-name>.

### Plugins That Change the Normal Test Run Flow

The following plugins in some way change how pytest runs your tests.

**pytest-repeat: Run Tests More Than Once**

To run tests more than once per session, use the pytest-repeat plugin.^1 This

plugin is useful if you have an intermittent failure in a test.

Following is a normal test run of tests that start with test_list from ch7/tasks

_proj_v2:

**$ cd /path/to/code/ch7/tasks_proj_v2
$ pip install .
$ pip install pytest-repeat
$ cd tests
$ pytest -v -k test_list
$ pytest -v -k test_list**
=====================testsessionstarts=====================
plugins:repeat-0.7.0,mock-1.10.0
collected62 items/ 56 deselected

1. https://pypi.python.org/pypi/pytest-repeat


func/test_api_exceptions.py::test_list_raisesPASSED [ 16%]
unit/test_cli.py::test_list_no_argsPASSED [ 33%]
unit/test_cli.py::test_list_print_emptyPASSED [ 50%]
unit/test_cli.py::test_list_print_many_itemsPASSED [ 66%]
unit/test_cli.py::test_list_dash_oPASSED [ 83%]
unit/test_cli.py::test_list_dash_dash_ownerPASSED [100%]

===========6 passed,56 deselectedin 0.13seconds===========

With the pytest-repeat plugin, you can use --count to run everything twice:

**$ pytest --count=2-v -k test_list**
=====================testsessionstarts=====================
plugins:repeat-0.7.0,mock-1.10.0
collected124 items/ 112 deselected

func/test_api_exceptions.py::test_list_raises[1/2]PASSED[ 8%]
func/test_api_exceptions.py::test_list_raises[2/2]PASSED[ 16%]
unit/test_cli.py::test_list_no_args[1/2]PASSED [ 25%]
unit/test_cli.py::test_list_no_args[2/2]PASSED [ 33%]
unit/test_cli.py::test_list_print_empty[1/2]PASSED [ 41%]
unit/test_cli.py::test_list_print_empty[2/2]PASSED [ 50%]
unit/test_cli.py::test_list_print_many_items[1/2]PASSED[ 58%]
unit/test_cli.py::test_list_print_many_items[2/2]PASSED[ 66%]
unit/test_cli.py::test_list_dash_o[1/2]PASSED [ 75%]
unit/test_cli.py::test_list_dash_o[2/2]PASSED [ 83%]
unit/test_cli.py::test_list_dash_dash_owner[1/2]PASSED[ 91%]
unit/test_cli.py::test_list_dash_dash_owner[2/2]PASSED[100%]

==========12 passed,112 deselectedin 0.16seconds==========

You can repeat a subset of the tests or just one, and even choose to run it

1,000 times overnight if you want to see if you can catch the failure. You can

also set it to stop on the first failure.

**pytest-xdist: Run Tests in Parallel**

Usually all tests run sequentially. And that’s just what you want if your tests

hit a resource that can only be accessed by one client at a time. However, if

your tests do not need access to a shared resource, you could speed up test

sessions by running multiple tests in parallel. The pytest-xdist plugin allows

you to do that. You can specify multiple processors and run many tests in

parallel. You can even push off tests onto other machines and use more than

one computer.

Here’s a test that takes at least a second to run, with parametrization such

that it runs ten times:

**appendices/xdist/test_parallel.py
importpytest
importtime**

Appendix 3. Plugin Sampler Pack • 164


@pytest.mark.parametrize( _'x'_ , list(range(10)))
def test_something (x):
time.sleep(1)

Notice that it takes over ten seconds to run normally:

**$ pip install pytest-xdist
$ cd /path/to/code/appendices/xdist
$ pytest test_parallel.py**
===================testsessionstarts===================
plugins:xdist-1.23.0,forked-0.2
collected10 items

test_parallel.py.......... [100%]

===============10 passedin 10.06 seconds================

With the pytest-xdist plugin, you can use -n numprocesses to run each test in a

subprocess, and use -n auto to automatically detect the number of CPUs on

the system. Here’s the same test run on multiple processors:

**$ pytest-n autotest_parallel.py**
===================testsessionstarts===================
plugins:xdist-1.23.0,forked-0.2
gw0 [10]/ gw1 [10]/ gw2 [10]/ gw3 [10]
schedulingtestsvia LoadScheduling
.......... [100%]
================10 passedin 4.00seconds================

It’s not a silver bullet to speed up your test times by a factor of the number

of processors you have—there is overhead time. However, many testing sce-

narios enable you to run tests in parallel. And when the tests are long, you

may as well let them run in parallel to speed up your test time.

The pytest-xdist plugin does a lot more than we’ve covered here, including the

ability to offload tests to different computers altogether, so be sure to read

more about the pytest-xdist plugin^2 on PyPI.

**pytest-timeout: Put Time Limits on Your Tests**

There are no normal timeout periods for tests in pytest. However, if you’re

working with resources that may occasionally disappear, such as web services,

it’s a good idea to put some time restrictions on your tests.

The pytest-timeout plugin^3 does just that. It allows you pass a timeout period

on the command line or mark individual tests with timeout periods in seconds.

2. https://pypi.python.org/pypi/pytest-xdist
3. https://pypi.python.org/pypi/pytest-timeout

Plugins That Change the Normal Test Run Flow • 165


The mark overrides the command-line timeout so that the test can be either

longer or shorter than the timeout limit.

Let’s run the tests from the previous example (with one-second sleeps) with

a half-second timeout:

**$ cd /path/to/code/appendices/xdist
$ pip install pytest-timeout
$ pytest --timeout=0.5-x test_parallel.py**
===================testsessionstarts===================
plugins:xdist-1.23.0,timeout-1.3.2, forked-0.2
timeout:0.5s
timeoutmethod:signal
timeoutfunc_only:False
collected10 items

test_parallel.pyF

========================FAILURES=========================
____________________test_something[0]____________________

x = 0

@pytest.mark.parametrize('x',list(range(10)))
def test_something(x):
**> time.sleep(1)**
E Failed:Timeout>0.5s

test_parallel.py:7:Failed
================1 failedin 0.59seconds=================

The -x stops testing after the first failure.

### Plugins That Alter or Enhance Output

These plugins don’t change how test are run, but they do change the output

you see.

**pytest-instafail: See Details of Failures and Errors as They Happen**

Usually pytest displays the status of each test, and then after all the tests

are finished, pytest displays the tracebacks of the failed or errored tests. If

your test suite is relatively fast, that might be just fine. But if your test suite

takes quite a bit of time, you may want to see the tracebacks as they happen,

rather than wait until the end. This is the functionality of the pytest-instafail

plugin.^4 When tests are run with the --instafail flag, the failures and errors

appear right away.

4. https://pypi.python.org/pypi/pytest-instafail

Appendix 3. Plugin Sampler Pack • 166


Here’s a test with normal failures at the end:

**$ cd /path/to/code/appendices/xdist
$ pytest --timeout=0.5 --tb=line--maxfail=2test_parallel.py**
===================testsessionstarts===================
plugins:xdist-1.23.0,timeout-1.3.2, forked-0.2
timeout:0.5s
timeoutmethod:signal
timeoutfunc_only:False
collected10 items

test_parallel.pyFF

========================FAILURES=========================
/path/to/code/appendices/xdist/test_parallel.py:7:Failed:Timeout>0.5s
/path/to/code/appendices/xdist/test_parallel.py:7:Failed:Timeout>0.5s
================2 failedin 1.09seconds=================

Here’s the same test with --instafail:

**$ pip install pytest-instafail
$ pytest --instafail --timeout=0.5 --tb=line--maxfail=2test_parallel.py**
===================testsessionstarts===================
plugins:xdist-1.23.0,timeout-1.3.2, instafail-0.4.0,forked-0.2
timeout:0.5s
timeoutmethod:signal
timeoutfunc_only:False
collected10 items

test_parallel.pyF

/path/to/code/appendices/xdist/test_parallel.py:7:Failed:Timeout>0.5s

test_parallel.pyF

/path/to/code/appendices/xdist/test_parallel.py:7:Failed:Timeout>0.5s

================2 failedin 1.10seconds=================

The --instafail functionality is especially useful for long-running test suites when

someone is monitoring the test output. You can read the test failures,

including the stack trace, without stopping the test suite.

**pytest-sugar: Instafail + Colors + Progress Bar**

The pytest-sugar plugin^5 lets you see status not just as characters, but also in

color. It also shows failure and error tracebacks during execution, and has

a cool progress bar to the right of the shell.

A test without sugar is shownon page 168.

5. https://pypi.python.org/pypi/pytest-sugar

Plugins That Alter or Enhance Output • 167


And here’s the test with sugar:

The checkmarks (or x’s for failures) show up as the tests finish. The progress

bars grow in real time, too. It’s quite satisfying to watch.

**pytest-emoji: Add Some Fun to Your Tests**

The pytest-emoji plugin^6 allows you to replace all of the test status characters

with emojis. You can also change the emojis if you don’t like the ones picked

by the plugin author. Although this project is perhaps an example of silliness,

it’s included in this list because it’s a small plugin and is a good example on

which to base your own plugins.

To demonstrate the emoji plugin in action, following is sample code that

produces pass, fail, skip, xfail, xpass, and error. Here it is with normal output

and tracebacks turned off:

6. https://pypi.python.org/pypi/pytest-emoji

Appendix 3. Plugin Sampler Pack • 168


Here it is with verbose, -v:

Now, here is the sample code with --emoji:

And then with both -v and --emoji:

It’s a pretty fun plugin, but don’t dismiss it as silly out of hand; it allows you

to change the emoji using hook functions. It’s one of the few pytest plugins

that demonstrates how to add hook functions to plugin code.

**pytest-html: Generate HTML Reports for Test Sessions**

The pytest-html plugin^7 is quite useful in conjunction with continuous integra-

tion, or in systems with large, long-running test suites. It creates a webpage

to view the test results for a pytest session. The HTML report created includes

the ability to filter for type of test result: passed, skipped, failed, errors,

expected failures, and unexpected passes. You can also sort by test name,

duration, or status. And you can include extra metadata in the report,

including screenshots or data sets. If you have reporting needs greater than

pass vs. fail, be sure to try out pytest-html.

7. https://pypi.python.org/pypi/pytest-html

Plugins That Alter or Enhance Output • 169


The pytest-html plugin is really easy to start. Just add --html=report_name.html:

**$ cd /path/to/code/appendices/outcomes
$ pytest --html=report.html**
======================testsessionstarts======================
metadata:...
collected6 items

test_outcomes.py.FxXsE

generatedhtmlfile:/path/to/code/appendices/outcomes/report.html
============================ERRORS=============================
_________________ERRORat setupof test_error__________________

@pytest.fixture()
def flaky_fixture():
**> assert1 == 2**
E assert1 == 2

test_outcomes.py:24:AssertionError
===========================FAILURES============================
___________________________test_fail___________________________

def test_fail():
**> assert1 == 2**
E assert1 == 2

test_outcomes.py:8:AssertionError
1 failed,1 passed,1 skipped,1 xfailed,1 xpassed,1 errorin 0.08seconds
**$ openreport.html**

This produces a report that includes the information about the test session

and a results and summary page.

The following screen shows the session environment information and summary:

Appendix 3. Plugin Sampler Pack • 170


The next screen shows the summary and results:

The report includes JavaScript that allows you to filter and sort, and you can

add extra information to the report, including images. If you need to produce

reports for test results, this plugin is worth checking out.

### Plugins for Static Analysis

Static analysis tools run checks against your code without running it. The Python

community has developed some of these tools. The following plugins allow you

to run a static analysis tool against both your code under test and the tests

themselves in the same session. Static analysis failures show up as test failures.

Plugins for Static Analysis • 171


**pytest-pycodestyle, pytest-pep8: Comply with Python’s Style Guide**

PEP 8 is a style guide for Python code.^8 It is enforced for standard library

code, and is used by many—if not most—Python developers, open source or

otherwise. The pycodestyle^9 command-line tool can be used to check Python

source code to see if it complies with PEP 8. Use the pytest-pycodestyle plugin^10

to run pycodestyle on code in your project, including test code, with the --pep8

flag. The pycodestyle tool used to be called pep8,^11 and pytest-pep8^12 is available if

you want to run the legacy tool.

**pytest-flake8: Check for Style Plus Linting**

While pep8 checks for style, flake8 is a full linter that also checks for PEP 8

style. The flake8 package^13 is a collection of different style and static analysis

tools all rolled into one. It includes lots of options, but has reasonable default

behavior. With the pytest-flake8 plugin,^14 you can run all of your source code

and test code through flake8 and get a failure if something isn’t right. It checks

for PEP 8, as well as for logic errors. Use the --flake8 option to run flake8 during

a pytest session. You can extend flake8 with plugins that offer even more

checks, such as flake8-docstrings,^15 which adds pydocstyle checks for PEP 257,

Python’s docstring conventions.^16

### Plugins for Web Development

Web-based projects have their own testing hoops to jump through. Even

pytest doesn’t make testing web applications trivial. However, quite a few

pytest plugins help make it easier.

**pytest-selenium: Test with a Web Browser**

Selenium is a project that is used to automate control of a web browser. The

pytest-selenium plugin^17 is the Python binding for it. With it, you can launch a

web browser and use it to open URLs, exercise web applications, and fill out

8. https://www.python.org/dev/peps/pep-0008
9. https://pypi.python.org/pypi/pycodestyle
10.https://pypi.python.org/pypi/pytest-pycodestyle
11.https://pypi.python.org/pypi/pep8
12.https://pypi.python.org/pypi/pytest-pep8
13.https://pypi.python.org/pypi/flake8
14.https://pypi.python.org/pypi/pytest-flake8
15.https://pypi.python.org/pypi/flake8-docstrings
16.https://www.python.org/dev/peps/pep-0257
17.https://pypi.python.org/pypi/pytest-selenium

Appendix 3. Plugin Sampler Pack • 172


forms. You can also programmatically control the browser to test a web site

or web application.

**pytest-django: Test Django Applications**

Django is a popular Python-based web development framework. It comes with

testing hooks that allow you to test different parts of a Django application

without having to use browser-based testing. By default, the builtin testing

support in Django is based on unittest. The pytest-django plugin^18 allows you

to use pytest instead of unittest to gain all the benefits of pytest. The plugin

also includes helper functions and fixtures to speed up test implementation.

**pytest-flask: Test Flask Applications**

Flask is another popular framework that is sometimes referred to as a

microframework. The pytest-flask plugin^19 provides a handful of fixtures to assist

in testing Flask applications.

18.https://pypi.python.org/pypi/pytest-django
19.https://pypi.python.org/pypi/pytest-flask

Plugins for Web Development • 173


APPENDIX 4

Packaging and Distributing Python Projects

The idea of packaging and distribution seems so serious. Most of Python has

a rather informal feeling about it, and now suddenly, we’re talking “packaging

and distribution.” However, sharing code is part of working with Python.

Therefore, it’s important to learn to share code properly with the builtin Python

tools. And while the topic is bigger than what I cover here, it needn’t be

intimidating. All I’m talking about is how to share code in a way that is more

traceable and consistent than emailing zipped directories of modules.

This appendix is intended to give you a comfortable understanding of how to

set up a project so that it is installable with pip, how to create a source distri-

bution, and how to create a wheel. This is enough for you to be able to share

your code locally with a small team. To share it further through PyPI, I’ll refer

you to some other resources. Let’s see how it’s done.

### Creating an Installable Module

We’ll start by learning how to make a small project installable with pip. For a

simple one-module project, the minimal configuration is small. I don’t recom-

mend you make it quite this small, but I want to show a minimal structure

in order to build up to something more maintainable, and also to show how

simple setup.py can be. Here’s a simple directory structure:

some_module_proj/
├──setup.py
└──some_module.py

The code we want to share is in some_module.py:

**appendices/packaging/some_module_proj/some_module.py
def some_func ():
    return 42


To make it installable with pip, we need a setup.py file. This is about as bare

bones as you can get:

**appendices/packaging/some_module_proj/setup.py
fromsetuptoolsimport** setup

setup(
name= _'some_module'_ ,
py_modules=[ _'some_module'_ ]
)

One directory with one module and a setup.py file is enough to make it instal-

lable via pip:

**$ cd /path/to/code/appendices/packaging
$ pip install ./some_module_proj**
Processing./some_module_proj
Installingcollectedpackages:some-module
Runningsetup.pyinstallfor some-module... done
Successfullyinstalledsome-module-0.0.0

And we can now use some_module from Python (or from a test):

**$ python**
Python3.7.0(v3.7.0:1bf9cc5093,Jun 26 2018,23:26:24)
[Clang6.0 (clang-600.0.57)]on darwin
Type"help","copyright","credits"or "license"for moreinformation.
**>>> fromsome_moduleimportsome_func
>>> some_func()**
42
**>>> exit()**

That’s a minimal setup, but it’s not realistic. If you’re sharing code, odds are

you are sharing a package. The next section builds on this to write a setup.py

file for a package.

### Creating an Installable Package

Let’s make this code a package by adding an __init__.py and putting the __init__.py

file and module in a directory with a package name:

**$ treesome_package_proj/**
some_package_proj/
├──setup.py
└──src
└──some_package
├──__init__.py
└──some_module.py

Appendix 4. Packaging and Distributing Python Projects • 176


The content of some_module.py doesn’t change. The __init__.py needs to be written

to expose the module functionality to the outside world through the package

namespace. There are lots of choices for this. I recommend skimming the two

sections of the Python documentation^1 that cover this topic.

If we do something like this in __init__.py:

import some_package.some_module**

the client code will have to specify some_module:

import some_package**
some_package.some_module.some_func()

However, I’m thinking that some_module.py is really our API for the package,

and we want everything in it to be exposed to the package level. Therefore,

we’ll use this form:

**appendices/packaging/some_package_proj/src/some_package/__init__.py
fromsome_package.some_moduleimport** *

Now the client code can do this instead:

import some_package**
some_package.some_func()

We also have to change the setup.py file, but not much:

**appendices/packaging/some_package_proj/setup.py
fromsetuptoolsimport** setup,find_packages

setup(
name= _'some_package'_ ,
packages=find_packages(where= _'src'_ ),
package_dir={ _''_ : _'src'_ },
)

Instead of using py_modules, we specify packages.

This is now installable:

**$ cd /path/to/code/appendices/packaging
$ pip install ./some_package_proj/**
Processing./some_package_proj
Installingcollectedpackages:some-package
Runningsetup.pyinstallfor some-package... done
Successfullyinstalledsome-package-0.0.0

1. https://docs.python.org/3/tutorial/modules.html#packages

Creating an Installable Package • 177


and usable:

**$ python**
Python3.7.0(v3.7.0:1bf9cc5093,Jun 26 2018,23:26:24)
[Clang6.0 (clang-600.0.57)]on darwin
Type"help","copyright","credits"or "license"for moreinformation.
**>>> fromsome_packageimportsome_func
>>> some_func()**
42

Our project is now installable and in a structure that’s easy to build on. You

can add a tests directory at the same level of src to add our tests if you want.

However, the setup.py file is still missing some metadata needed to create a

proper source distribution or wheel. It’s just a little bit more work to make

that possible.

### Creating a Source Distribution and Wheel

For personal use, the configuration shown in the previous section is enough

to create a source distribution and a wheel. Let’s try it:

**$ cd /path/to/code/appendices/packaging/some_package_proj/
$ pip install wheel
$ pythonsetup.pysdistbdist_wheel**
runningsdist
**...**
warning:sdist:standardfilenot found:
shouldhaveone of README,README.rst, README.txt,README.md

runningcheck
warning:check:missingrequiredmeta-data:url

warning:check:missingmeta-data:
either(authorand author_email)
or (maintainerand maintainer_email)mustbe supplied

runningbdist_wheel
**...
$ ls dist**
some_package-0.0.0-py3-none-any.whlsome_package-0.0.0.tar.gz

Well, with some warnings, a .whl and a .tar.gz file are created. Let’s get rid of

those warnings.

To do that, we need to:

- Add one of these files: README, README.rst, or README.txt.
- Add metadata for url.
- Add metadata for either (author and author_email) or (maintainer and maintain-
    er_email).

Appendix 4. Packaging and Distributing Python Projects • 178


Let’s also add:

- A version number
- A license
- A change log

It makes sense that you’d want these things. Including some kind of README

allows people to know how to use the package. The url, author, and author_email

(or maintainer) information makes sense to let users know who to contact if they

have issues or questions about the package. A license is important to let

people know how they can distribute, contribute, and reuse the package. And

if it’s not open source, say so in the license data. To choose a license for open

source projects, I recommend looking at https://choosealicense.com.

Those extra bits don’t add too much work. Here’s what I’ve come up with for

a minimal default.

The setup.py:

**appendices/packaging/some_package_proj_v2/setup.py
fromsetuptoolsimport** setup,find_packages

setup(
name= _'some_package'_ ,
description= _'Demonstratepackagingand distribution'_ ,

```
version= '1.0' ,
author= 'BrianOkken' ,
author_email= 'brian@pythontesting.net' ,
url= 'https://pragprog.com/book/bopytest/python-testing-with-pytest' ,
```
packages=find_packages(where= _'src'_ ),
package_dir={ _''_ : _'src'_ },
)

You should put the terms of the licensing in a LICENSE file. All of the code in

this book follows the following license:

**appendices/packaging/some_package_proj_v2/LICENSE**
Copyright(c) 2017The PragmaticProgrammers,LLC

All rightsreserved.

Copyrightsapplyto thissourcecode.

You may use the sourcecodein yourown projects,howeverthe sourcecode
may not be usedto createcommercialtrainingmaterial,courses,books,
articles,and the like.We makeno guaranteesthatthissourcecodeis fit
for any purpose.

Creating a Source Distribution and Wheel • 179


Here’s the README.rst:

**appendices/packaging/some_package_proj_v2/README.rst**
====================================================
some_package:Demonstratepackagingand distribution
====================================================

``some_package``is the Pythonpackageto demostratehow easyit is
to createinstallable,maintainable,shareablepackagesand distributions.

It doescontainone function,called``some_func()``.

.. code-block

```
>>> importsome_package
>>> some_package.some_func()
42
```
That'sit, really.

The README.rst is formatted in reStructuredText.^2 I’ve done what many have

done before me: I copied a README.rst from an open source project, removed

everything I didn’t like, and changed everything else to reflect this project.

You can also use a markdown formated README.md, or an ASCII-formatted

README.txt or README, but I’m okay with copy/paste/edit in this instance.

I recommend also adding a change log. Here’s the start of one:

**appendices/packaging/some_package_proj_v2/CHANGELOG.rst**
Changelog
=========

------------------------------------------------------

1.0
---

Changes:
~~~~~~~~

- Initialversion.

See [http://keepachangelog.com](http://keepachangelog.com) for some great advice on what to put in your change

log. All of the changes to tasks_proj over the course of this book have been logged

into a CHANGELOG.rst file.

Let’s see if this was enough to remove the warnings:

**$ cd /path/to/code/appendices/packaging/some_package_proj_v2
$ pythonsetup.pysdistbdist_wheel**
runningsdist
runningbuild

2. [http://docutils.sourceforge.net/rst.html](http://docutils.sourceforge.net/rst.html)

Appendix 4. Packaging and Distributing Python Projects • 180


runningbuild_py
creatingbuild
creatingbuild/lib
creatingbuild/lib/some_package
**...**

**$ ls dist**
some_package-1.0-py3-none-any.whlsome_package-1.0.tar.gz

Yep. No warnings.

Now, we can put the .whl and/or .tar.gz files in a local shared directory and pip

install to our heart’s content:

**$ cd /path/to/code/appendices/packaging/some_package_proj_v2
$ mkdir~/packages/
$ cp dist/some_package-1.0-py3-none-any.whl~/packages
$ cp dist/some_package-1.0.tar.gz~/packages
$ pip install --no-index--find-links=~/packagessome_package**
Collectingsome_package
Installingcollectedpackages:some-package
Successfullyinstalledsome-package-1.0
**$ pip install --no-index--find-links=./distsome_package==1.0**
Requirementalreadysatisfied:some_package==1.0in
/path/to/venv/lib/python3.6/site-packages
**$**

Now you can create your own stash of local project packages from your team,

including multiple versions of each, and install them almost as easily as

packages from PyPI.

### Creating a PyPI-Installable Package

You need to add more metadata to your setup.py to get a package ready to

distribute on PyPI. You also need to use a tool such as Twine^3 to push pack-

ages to PyPI. (Twine is a collection of utilities to help make interacting with

PyPI easy and secure. It handles authentication over HTTPS to keep your PyPI

credentials secure, and handles the uploading of packages to PyPI.)

This is now beyond the scope of this book. However, for information about

how to start contributing through PyPI, take a look at the Python Packaging

User Guide^4 and the the PyPI^5 section of the Python documentation.

3. https://pypi.python.org/pypi/twine
4. https://python-packaging-user-guide.readthedocs.io
5. https://docs.python.org/3/distutils/packageindex.html

Creating a PyPI-Installable Package • 181


APPENDIX 5

xUnit Fixtures

In addition to the fixture model described in Lab 3, pytest Fixtures, on

page 51, pytest also supports xUnit style fixtures, which are similar to jUnit

for Java, cppUnit for C++, and so on.

Generally, xUnit frameworks use a flow of control that looks something

like this:

setup()
test_function()
teardown()

This is repeated for every test that will run. pytest fixtures can do anything you

need this type of configuration for and more, but if you really want to have setup()

and teardown() functions, pytest allows that, too, with some limitations.

### Syntax of xUnit Fixtures

xUnit fixtures include setup()/teardown() functions for module, function, class,

and method scope:

_setup_module()/teardown_module()_

```
These run at the beginning and end of a module of tests. They run once
each. The module parameter is optional.
```
_setup_function()/teardown_function()_

```
These run before and after top-level test functions that are not methods
of a test class. They run multiple times, once for every test function. The
function parameter is optional.
```
_setup_class()/teardown_class()_

```
These run before and after a class of tests. They run only once. The class
parameter is optional.
```

_setup_method()/teardown_method()_

```
These run before and after test methods that are part of a test class. They
run multiple times, once for every test method. The method parameter is
optional.
```
Here is an example of all the xUnit fixtures along with a few test functions:

**appendices/xunit/test_xUnit_fixtures.py
def setup_module (module):
**print (f _'\nsetup_module()for {module.__name__}'_ )

def teardown_module (module):
**print (f _'teardown_module()for {module.__name__}'_ )

def setup_function (function):
**print (f _'setup_function()for {function.__name__}'_ )

def teardown_function (function):
**print (f _'teardown_function()for {function.__name__}'_ )

def test_1 ():
**print ( _'test_1()'_ )

def test_2 ():
**print ( _'test_2()'_ )

**class** TestClass:
@classmethod
def setup_class (cls):
**print (f _'setup_class()for class{cls.__name__}'_ )

```
@classmethod
def teardown_class (cls):
print (f 'teardown_class()for {cls.__name__}' )
```
```
def setup_method (self,method):
print (f 'setup_method()for {method.__name__}' )
def teardown_method (self,method):
print (f 'teardown_method()for {method.__name__}' )
def test_3 (self):
print ( 'test_3()' )
def test_4 (self):
print ( 'test_4()' )
```
I used the parameters to the fixture functions to get the name of the module/func-

tion/class/method to pass to the print statement. You don’t have to use the

parameter names module, function, cls, and method, but that’s the convention.

Appendix 5. xUnit Fixtures • 184


Here’s the test session to help visualize the control flow:

**$ cd /path/to/code/appendices/xunit
$ pytest-s test_xUnit_fixtures.py**
============testsessionstarts=============
plugins:mock-1.6.0,cov-2.5.1
collected4 items

test_xUnit_fixtures.py
setup_module()for test_xUnit_fixtures
setup_function()for test_1
test_1()
.teardown_function()for test_1
setup_function()for test_2
test_2()
.teardown_function()for test_2
setup_class()for classTestClass
setup_method()for test_3
test_3()
.teardown_method()for test_3
setup_method()for test_4
test_4()
.teardown_method()for test_4
teardown_class()for TestClass
teardown_module()for test_xUnit_fixtures

==========4 passedin 0.01seconds==========

### Mixing pytest Fixtures and xUnit Fixtures

You can mix pytest fixtures and xUnit fixtures:

**appendices/xunit/test_mixed_fixtures.py
import pytest

def setup_module ():
**print ( _'\nsetup_module()- xUnit'_ )

def teardown_module ():
**print ( _'teardown_module()- xUnit'_ )

def setup_function ():
**print ( _'setup_function()- xUnit'_ )

def teardown_function ():
**print ( _'teardown_function()- xUnit\n'_ )

Mixing pytest Fixtures and xUnit Fixtures • 185


@pytest.fixture(scope= _'module'_ )
def module_fixture ():
**print ( _'module_fixture()setup- pytest'_ )
**yield
print ( _'module_fixture()teardown- pytest'_ )

@pytest.fixture(scope= _'function'_ )
def function_fixture ():
**print ( _'function_fixture()setup- pytest'_ )
**yield
print ( _'function_fixture()teardown- pytest'_ )

def test_1 (module_fixture,function_fixture):
**print ( _'test_1()'_ )

def test_2 (module_fixture,function_fixture):
**print ( _'test_2()'_ )

You _can_ do it. But please don’t. It gets confusing. Take a look at this:

**$ cd /path/to/code/appendices/xunit
$ pytest-s test_mixed_fixtures.py**
============testsessionstarts=============
plugins:mock-1.6.0,cov-2.5.1
collected2 items

test_mixed_fixtures.py
setup_module()- xUnit
setup_function()- xUnit
module_fixture()setup- pytest
function_fixture()setup- pytest
test_1()
.function_fixture()teardown- pytest
teardown_function()- xUnit

setup_function()- xUnit
function_fixture()setup- pytest
test_2()
.function_fixture()teardown- pytest
teardown_function()- xUnit

module_fixture()teardown- pytest
teardown_module()- xUnit

==========2 passedin 0.01seconds==========

In this example, I’ve also shown that the module, function, and method parameters

to the xUnit fixture functions are optional. I left them out of the function

definition, and it still runs fine.

Appendix 5. xUnit Fixtures • 186


### Limitations of xUnit Fixtures

Following are a few of the limitations of xUnit fixtures:

- xUnit fixtures don’t show up in -setup-show and -setup-plan. This might seem
    like a small thing, but when you start writing a bunch of fixtures and
    debugging them, you’ll love these flags.
- There are no session scope xUnit fixtures. The largest scope is module.
- Picking and choosing which fixtures a test needs is more difficult with
    xUnit fixtures. If a test is in a class that has fixtures defined, the test will
    use them, even if it doesn’t need to.
- Nesting is at most three levels: module, class, and method.
- The only way to optimize fixture usage is to create modules and classes
    with common fixture requirements for all the tests in them.
- No parametrization is supported at the fixture level. You can still use
    parametrized tests, but xUnit fixtures cannot be parametrized.

There are enough limitations of xUnit fixtures that I strongly encourage you

to forget you even saw this appendix and stick with normal pytest fixtures.

Limitations of xUnit Fixtures • 187


Index

SYMBOLS
--instafail, 166

. (dot syntax), 1, 8
:: syntax, 40

A
a (pdb module), 130
add(), example of parametrized
testing, 42 – 49
addopts, 116– 117 , 143
alwaysprint this, 88
and
combining markers, 32
running tests by name,
41
API (application programming
interface)
CLI interactions, xii
mocks, 136 – 142
types and functions, 30
approx(), 119
argparse, 136
args (pdb module), 130
_asdict(), 5
assert rewriting, 28 – 30
assert statements
assert rewriting, 28 – 30
exercise, 21
simplicity of, xii
using, 27 – 30
assert_called_once_with(), 140
attributes, setting and delet-
ing, 89, 91
author field, packaging and
distribution, 108 , 178

```
author_email field, packaging
and distribution, 108 , 178
autouse
initializing database for
Task Project, 34
parametrized testing of
Task Project, 43
using for fixtures, 63
```
```
B
base directory, 75
build-name-setter, 145
builtin fixtures, see fixtures,
builtin
```
```
C
C (class scope), 60
cache, 79– 87
--cache-clear option, 79, 82
--cache-show option, 79, 82
caches
caching test sessions, 79 –
87
installing from, 161
using multiple versions,
161
capsys, 87
--capture=fd option, 15
--capture=method option, 10, 14
--capture=no option, 88
--capture=sys option, 15
category (warning), 96
change logs, 24, 178, 180
CHANGELOG.rst file, 24, 180
chdir(path), 89, 92
check_duration(), 86, 96, 114
```
```
cheese preference example,
88 – 92
class scope, 58, 74, 76, 183
classes
defined, 40
fixtures scope, 58, 74,
76, 183
naming, 7, 116, 122– 123
parametrized testing, 46
specifying fixtures, 63
test discovery rules, 121
testing single, 40
xUnit fixtures, 183
CLI (command-line interface)
adding options with
pytest_addoption, 77
configuring, 117 – 123
interactions through API,
xii
mocks, 136 – 142
Click, 136 – 142
CliRunner, 140
cls object, 153
Cobertura, 148
code, for this book, xvi
code coverage, 130 – 135 , 154
--collect-only option, 10 – 11
color displays, 1, 167
command-line interface,
see CLI (command-line in-
terface)
comments, 54, 58
configuration, 115 – 126
command-line options,
117 – 123
exercises, 125
files, 115
```

listing options, 20, 116
marking xfail tests as
failed, 38
minimum required ver-
sion, 119
options, 20, 79, 116
with pytest.ini, 115– 123
with pytestconfig, 77– 79
registering markers, 118
test discovery, 116 , 120–
123
in test output, 8
testing multiple with tox,
142 – 145
conftest.py file
about, 115
additional configuration
options, 117
in file structure, 25
as plugin, 52
sharing fixtures with, 52
context manager, mocks, 139
continuous integration
HTML report plugin, 169 –
171
installing plugins from
local directory, 99
with Jenkins, 145 – 149
Python advantages, xi
specifying test directories,
121
cookiecutter-pytest-plugin, 114
--count option, 164
--cov-report option, 132
--cov=src option, 132
coverage.py, 130– 135 , 154
current directory, 86

D
d (pdb module), 130
data
passing fixture data to
unittest functions, 153
test data with fixtures,
55 – 57
databases
initializing, 33
mocks, 138 – 142
parametrized fixtures,
69 – 71
running legacy unittest
tests, 151
setup, 53, 69
db_type parameter, 70
debugging
display options, 127

```
with pdb module, 127 –
130 , 154
stopping tests for, 13, 15
delattr(), 89
deleting
attributes, 89, 91
environment variables,
89 – 91
delitem(), 89
dictionaries
returning, 5
setting and deleting en-
tries, 89, 91
directories
avoiding filename colli-
sions, 115 , 123
base, 75
cache, 82
changing current work-
ing, 89, 92
code coverage, 132
current, 86, 89, 92
dist, 113
distributing plugins from
shared, 113
exercises, 49
help options, 21
installing plugins locally,
99
Jenkins testing, 146 – 149
packages file structure,
24
packaging plugins, 105
prepending paths, 92
rootdir, 8
specifying, 4, 6, 39, 49,
75, 121
temporary, 34, 53, 57–
63, 66, 73–77, 110
test discovery configura-
tion options, 116 , 120–
123
test discovery rules, 121
in test output, 8
virtual environments, 158
dist directory, 113
distribution, 175 – 181
metadata, 108 , 178
plugins, 112
resources, 105 , 114, 181
setup.cfg file, 116
source, 113 , 178– 181
Django, 173
docstrings, 172
```
```
doctest
configuration options,
116
doctest_namespace fixture,
92 – 95
doctest_namespace, 92– 95
doctest_optionflags, 116
dot syntax, 1, 8
down (pdb module), 130
duration
exercises, 96, 114
measuring with cache, 83–
87
ordering test results by,
10, 19
duration_cache, 83– 87
--durations=N option, 10, 19
```
```
E
E (errors)
defined, 8
displays, 8, 29
emojis, 168 – 169
entry_points field, packaging
plugins, 108
environment variables
listing, 20
setting and deleting, 89 –
91
environments, see virtual en-
vironments
equivalent(), 43
errors, see also failing tests
defined, 8
displays, 8, 29, 166– 171
emojis, 168 – 169
raising with monkeypatch
fixture, 89
testing for expected, 30
traceback as they hap-
pen, 166
ExceptionInfo, 31
as excinfo, 31
exercises
configuration, 125
files and directories, 49
fixtures, 71, 96
installation, 21, 49
Jenkins, 154
markers, 49
mocks, 154
pdb module, 154
plugins, 114 , 125
tox, 154
virtual environments, 21
```
Index • 190


--exit first option
exercises, 154
timeouts, 166
using, 10, 13, 127
EXPRESSION option, 10 – 11
expressions, running tests
with, 10 – 11, 41

F
F (failures)
changing indicator plugin
example, 100 – 114
defined, 8
displays, 8
F (function scope), 55, 60
failing tests, _see also_ xfail
tests
assert rewriting, 28 – 30
changing indicator plugin
example, 100 – 114
debugging with pdb mod-
ule, 127 – 130
defined, 8
disallowing xpass, 116 ,
123
displays, 1, 8, 29, 143,
166 – 171
emojis, 168 – 169
marking expected fail-
ures, 8, 37, 49
reporting failing line, 16
running first failed, 10,
15, 79– 82
running last failed, 10,
15, 79–82, 84, 127,
154
stopping tests at, 10, 13,
127 , 164
stopping with --maxfail,
10, 14
testing for expected excep-
tions, 30
timeouts, 166
tox, 143
traceback as they hap-
pen, 166
unittest tests and first fail-
ures, 154
fakes, _see_ mocks
--ff option
with cache, 79– 82
using, 10, 15
file descriptors, capture op-
tions, 15
filename (warning), 96
filename collisions, avoiding,
96, 115, 123

```
files
avoiding filename colli-
sions, 96, 115, 123
downloading multiple
with pip, 161
exercises, 49
file descriptors capture
options, 15
fixtures in individual, 52
help options, 21
naming conventions, 6 –
7, 83, 116, 123
packages file structure,
24
specifying, 4, 6, 40, 49
specifying one, 7, 40, 49
specifying only one test,
9
test discovery rules, 116 ,
123
--find-linksmyplugins, 113
find-links=./some_plugins/, 99
--first-failed option
with cache, 79– 82
using, 10, 15
fixture(), 51
fixtures, 51 – 72, see also fix-
tures, builtin
autouse, 34, 43, 63
changing scope, 61 – 63
configuration options,
116
defined, 25, 51
directories, 25
exercises, 71, 96
help options, 20
listing, 65
marking with, 63
mixing xUnit with pytest,
185
multiple, 57
names, 51, 65– 66
parametrized, 66 – 71,
154 , 187
passing data to unittest
functions, 153
as plugins, 97
renaming, 65 – 66
scope, 55, 58–63, 71, 74,
152
session scope, 152
setup and teardown with,
53 – 55, 60
sharing with conftest.py
file, 52
specifying with usefixtures,
63
as term, 52
```
```
for test data, 55 – 57
testdir, 109– 112
testing plugins, 109 – 112
tracing execution with
--setup-show, 54, 60, 71,
187
xUnit, 183 – 187
--fixtures option, 65
fixtures, builtin, 73 – 96
advantages, 73
cache, 79– 87
capsys, 87
doctest_namespace, 92– 95
exercises, 96
monkeypatch, 88– 92
options, 78
pytestconfig, 77– 79
recwarn, 95
request, 67, 70, 83, 153
scope, 74
tmpdir, 34, 53, 57–63, 66,
73 – 77
tmpdir_factory, 53, 57–63,
66, 73– 77
flake8, 172
Flask, 173
floating point numbers, 119
fnmatch_lines, 110
--foo <value> option, 77
function scope
cache, 84
display, 55
fixtures, 55, 58, 74
tmpdir, 74
xUnit fixtures, 183
functional tests
defined, xiii
directories, 24
functions, 23 – 49, see also fix-
tures; hook functions
API calls and types, 30
assert statements, 27 – 30
importing, 25
marking, 31 – 38
names, 7, 116, 123
parametrized testing, 42 –
49
resources, 100
test discovery rules, 116 ,
123
testing for expected excep-
tions, 30
testing single, 40
xUnit fixtures, 183
```
Index • 191


G
getoption(), 125
GitHub, 84, 98– 99
.gitignore file, 158

H
-h, using, 20
headers, test, 102
--help
ini file options, 116
listing options with, 10
using, 20
virtual environments, 158
hook functions
adding to plugins, 169
defined, 25
directories, 25
as plugins, 97
pytest_addoption, 77, 125
pytest_report_header(), 102
pytest_report_teststatus(), 102
resources, 100 , 104
HTML
code coverage reports,
132
report plugin, 169 – 171
HTTPS, 181

I
id field
checking, 43
exercises, 49
generating identifiers, 68
optional, 45
parametrized fixtures, 68
parametrized testing, 43,
45, 48
importing
docstring failures, 93 – 95
functions, 25
packages, 25 – 27
ini files and configuration, 8,
25, 115– 126
__init__.py files
about, 25, 115
avoiding filename colli-
sions, 123
creating installable pack-
ages, 176
--instafail option, 166
installing
from cache, 161
coverage.py, 131
exercises, 21, 49
Jenkins plugins, 145
MongoDB, 71, 128

```
packages locally, 25 – 27,
99
plugins and packages,
25 – 27, 98–99, 112–
113 , 159– 162
pytest, 4, 21
from .tar.gz and .whl files,
98, 161
tox, 144
virtual environments, 157
integration tests, defined, xiii
```
```
J
Jenkins CI, 145 – 149 , 154
--junit-xml option, 146
```
```
K
-k option, 10 – 11, 41
key names, 83
```
```
L
l (pdb module), 130
-l option (print statements),
10, 14, 17, 81, 117, 127
lambda expression, 91
--last-failed option
with cache, 79– 82
measuring duration with
cache, 84
using, 10, 15, 127, 154
--lf option
with cache, 79– 82
measuring duration with
cache, 84
using, 10, 15, 127, 154
license field, packaging and
distribution, 108 , 178
LICENSE file, 24, 108, 178
licenses, 24, 108, 178
lineno (warning), 96
list (pdb module), 130
list begin,end (pdb module), 130
local variables
printing output, 10, 14,
17, 81, 117, 127
tox, 143
```
```
M
M (module scope), 60
-m option, using, 10, 12, 32
MagicMock, 140
maintainer field, packaging and
distribution, 108 , 178
maintainer_email field, packaging
and distribution, 108 , 178
```
```
makepyfile(), 110
MANIFEST.in file, 24
markers
configuration options,
116 – 117
exercises, 49
expected failures, 8, 37,
49
with fixtures, 63
functions, 31 – 38
help options, 20
multiple, 12, 32
names, 12
registering, 118
skipping tests with, 8,
34 – 37, 49
--strict option, 117 , 143
timeouts, 165
tox, 143
unittest tests, 153
using, 10, 12
--markers list, 118
markers option, 116 , 143
MARKEXPR option, using, 10, 12
math example of doctest_names-
pace, 92– 95
--maxfail option, 10, 14
members, accessing by name,
5
message (warning), 96
metadata
HTML report plugin, 169
packaging and distribu-
tion, 108 , 178
method scope, 183
methods, see also fixtures
method scope, 183
names, 7, 116, 123
parametrized testing, 46
test discovery rules, 116 ,
123
testing single, 41
xUnit fixtures, 183
minversion, 116, 119
mocker, 139
mocks, 136 – 142 , 154
module scope, 58, 74, 76, 183
modules
creating installable, 175 –
176
doctest_namespace fixture,
92 – 95
fixtures scope, 58, 74,
76, 183
```
Index • 192


packaging plugins, 108 ,
178
prepending paths, 92
specifying one, 40
test discovery rules, 121
xUnit fixtures, 183
MongoDB, 69 – 71, 128
monkeypatch, 88– 92
--myopt option, 77

N
-n auto, 165
-n numprocesses, 165
name parameter, 65
namedtuple(), 3
names
accessing members by, 5
avoiding filename colli-
sions, 115 , 123
doctest_namespace fixture,
92 – 95
files, 6 – 7, 83, 116, 123
fixtures, 51, 65– 66
functions, 7, 116, 123
key, 83
markers, 12
methods, 7, 116, 123
naming conventions, 6 –
7, 83, 121, 123
renaming fixtures, 65 – 66
running tests by, 41
test discovery rules, 121
__new__.__defaults__, 5
nice status indicators plugin
example, 100 – 114
--no-index option (pip), 99, 113
nodeid, 83
nodes, 44
norecursedirs, 116, 120
normal print, usuallycaptured, 88
not
combining markers, 32
running tests by name,
41
NUM, 75

O
O (opportunity), changing indi-
cator plugin example, 100 –
114
ObjectID, 129
or
combining markers, 32
running tests by name,
41

```
output
with capsys, 87
options, 1, 8, 10, 14, 88,
117 , 119, 127
plugins for enhancing,
166 – 171
understanding, 1, 7– 9
```
```
P
packages
doctest_namespace fixture,
92 – 95
file structure, 24
importing functions, 25
installing, 25 – 27, 99,
142 , 159– 162
installing locally, 25 – 27,
99, 160
installing with tox, 142
listing, 160
namespace, 177
packaging tips, 105 – 109 ,
175 – 181
resources, 114 , 181
setup.cfg file, 116
specifying versions, 160
from .tar.gz and .whl files,
98, 161
uninstalling, 160
parallel tests, 164
param(), 48
parameters
monkeypatch fixture, 89
testing for expected excep-
tions, 31
parametrize(), 43– 49
parametrized fixtures, 66 – 71,
154 , 187
parametrized testing, 42 – 49,
187
passing tests, see also xpass
tests
defined, 8
displays, 1, 8, 168– 169
dot syntax, 1, 8
emojis, 168 – 169
paths
configuration options,
116
prepending, 89, 92
--pdb command, 127 , 129
pdb module, 127 – 130 , 154
PEP 257, 172
PEP 8, 172
pep8 command-line tool, 172
--pep8 option, 172
```
```
pip
about, xv, 159– 162
distributing plugins from,
113
downloading multiple
files, 161
ignoring PyPI option, 99,
113
installing MongoDB, 128
installing Task Project,
26
installing plugins, 98 – 99
installing pytest, 4
packaging and distribu-
tion, 113 , 175– 181
resources, 162
tox and, 142
uninstalling with, 160
versions, 159
platform darwin, 7
pluggy, 7
plugins, 97 – 114
changing test flow, 163 –
166
configuration options,
117
conftest.py as, 52
creating, 100 – 114
directories, 105
exercises, 114 , 125
help options, 20
hook functions, adding
to, 169
installing, 98 – 99, 112–
113 , 159– 162
installing from Git reposi-
tory, 99
installing from local direc-
tory, 99
installing multiple ver-
sions, 99
installing specific ver-
sions, 98
Jenkins, 145
listing, 160
output enhancing, 166 –
171
packaging and distribu-
tion, 105 – 109 , 112,
175 – 181
resources, 97 – 98
static analysis, 171
suggested, 163 – 173
testing, 104 , 109– 112 ,
125
testing manually, 104 ,
110 , 125
uninstalling, 112 , 160
```
Index • 193


versions, 98 – 99, 108
web development plugins,
172
pp expr (pdb module), 130
prepend parameter, 89
pretty printing, pdb module,
130
print expr (pdb module), 130
print statements
with capsys, 87
disabling, 88
fixtures exercise, 71
options, 10, 14, 17, 81,
117 , 127
pdb options, 130
progress bar, 167
py, 7
pycodestyle command-line tool,
172
pydocstyle, 172
pymongo, 128
PyPI, _see_ Python Package In-
dex (PyPI)
pytest, _see also_ configuration;
fixtures; functions; plugins
advantages, xi, 2– 3
basics, 1 – 22
installing, 4, 21
minimum required ver-
sion, 116 , 119
options, 10 – 21
resources, 4, 100
running, 4 – 10
versions, xii, 7, 10, 20,
116 , 119
pytest config object, 77 – 79
pytest-cov, 98, 131– 135
pytest-django, 173
pytest-emojis, 168– 169
pytest-flake8, 172
pytest-flask, 173
pytest-html, 169– 171
pytest-instafail, 166
pytest-mock, 136– 142
pytest-nice, 100– 114
pytest-pep8, 172
pytest-pycodestyle, 172
pytest-repeat, 163
pytest-selenium, 172
pytest-sugar, 167
pytest-timeout, 165
pytest-xdist, 164
pytest.ini file, 25, 115– 126

```
pytest11, 108
pytest_addoption, 77, 125
pytest_report_header(), 102
pytest_report_teststatus(), 102
pytestconfig, 77– 79
pytester, 109– 112 , 125
Python
about, xi
PEP 8, 172
resources, 105 , 114, 130
versions, xii, 4, 7, 136,
159
Python Package Index (PyPI)
credentials, 181
distribution on, 114 , 181
exercises, 114
ignoring, 99, 113
installing pytest, 4
plugins from, 98, 165
resources, 181
Python Packaging Authority,
158 , 162
Python Packaging User
Guide, 114 , 181
python_classes, 116, 122– 123
python_files, 116, 123
python_functions, 116, 123
```
```
Q
q (pdb module), 130
-q option, 10, 16
--quiet option, 10, 16
quit (pdb module), 130
quotes, parametrized testing,
44, 46
```
```
R
raising parameter, 89
random, 83
readability, xii, 45
README files, 24, 108, 178
recursion, configuration op-
tions, 116 , 120– 123
recwarn, 95
registering markers, 118
repeating tests plugin, 163
_replace(), 5
reports
code coverage, 132
HTML report plugin, 169 –
171
Jenkins Test Results An-
alyzer, 145
```
```
request, 67, 70, 83, 153
requirements.txt file, 161
resources
for this book, xvi, 155
code coverage, 135
docstrings, 172
functions, 100 , 104
hook functions, 100 , 104
Jenkins, 148
mocks, 140 , 142
packaging and distribu-
tion, 105 , 114, 177,
181
packaging namespace,
177
pdb module, 130
pip, 162
plugins, 97 – 98
pytest, 4, 100
pytest-xdist, 165
pytester, 125
Python, 105 , 114, 130
tox, 145
virtual environments, 158
ret, 110
rootdir, 8
-rs option, 37
-rsxX option, 117 , 119, 143
```
```
S
S (session scope), 55, 60
s (skipped tests)
defined, 8
displays, 8, 36
-s option (print statements),
10, 14, 88
scope
cache, 84
changing, 61 – 63
fixtures, 55, 58–63, 71,
74, 152, 183
tmpdir, 61, 74
tmpdir_factory, 61, 74, 76
xUnit fixtures, 183 , 187
scope parameter, 58
screenshots, HTML report
plugin, 169
sdist, 113
Selenium, 172
session, see also session
scope
cache fixture, 79 – 87
duration, 8, 83–87, 114
HTML report plugin, 169 –
171
```
Index • 194


repeating tests in, 163
in test output, 7
session scope
cache, 86
changing, 61 – 63
display, 55, 60
fixtures, 55, 58, 74
tmpdir_factory, 74
unittest tests, 152
xUnit fixtures, 187
setUp(), 153
setattr(), 89, 91
setenv(), 89, 91
setitem(), 89, 91
setting
attributes, 89, 91
environment variables,
89 – 91
setup, _see also_ fixtures
and --durations=N option, 20
with fixtures, 53 – 55, 60
packaging and distribu-
tion, 24, 107, 112,
116 , 176– 181
passing ids with fixtures,
153
--setup-show option, 54, 60,
71, 187
with xUnit fixtures, 183 –
187
setup() (xUnit fixture), 183 – 187
setup() file (packaging plugins),
107
--setup-plan option, 187
--setup-show option, 54, 60, 71,
187
setup.cfg, 116
setup.py file
distribution and setup.cfg
file, 116
installing local package,
160
in package file structure,
24
packaging and distribu-
tion, 107 , 112, 116,
176 – 181
tox, 142
setup_class(), 183
setup_function(), 183
setup_method(), 184
setup_module(), 183
--showlocals option, 10, 14, 17,
81, 127
skip(), 8, 34– 36

```
skipif(), 8, 34, 36
skipping tests
defined, 8
displays, 8, 143
emojis, 168 – 169
exercises, 49
with markers, 8, 34–37,
49
tox, 143
smoke tests, 31 – 34
source distribution, 113 , 178–
181
speed
exercise, 96
measuring duration with
cache, 83– 87
ordering tests by dura-
tion, 10, 19
parallel tests, 164
spies, see mocks
src/ directory, 25
stack trace, see traceback
static analysis tools, 171
stderr
with capsys, 87
options, 10, 14, 88
stdout
with capsys, 87
options, 10, 14, 88
testing plugins, 110
stopping
tests with --maxfail, 10, 14
tests with -x and --exit first,
10, 13, 127
tests with pytest-repeat, 164
--strict option, 117 , 143
strings
parametrized testing, 44
testing plugins, 110
stubs, see mocks
subcutaneous tests, defined,
xiii
sugar plugin, 167
syspath_prepend(path), 89, 92
system tests, defined, xiii
```
```
T
t_after local variable, 18
t_before local variable, 18
t_expected local variable, 18
```
```
tar balls
distribution from, 113 ,
178 – 181
installing plugins and
packages from, 98
.tar.gz
distribution from, 113 ,
178 – 181
installing plugins and
packages from, 98
tasks
installing, 25 – 27
sample session, xii
Tasks project
about, xii
assert statements, 27 – 30
builtin fixtures for, 73 – 96
changing scope, 61 – 63
changing status indica-
tors plugin example,
100 – 114
code coverage, 130 – 135 ,
154
configuration, 115 – 126
creating objects, 5
debugging with pdb mod-
ule, 127 – 130
functions, 31 – 38
installing locally, 25 – 27
with Jenkins CI, 145 – 149
legacy testing, 149 – 154
marking expected fail-
ures, 37
mocks, 136 – 142
parametrized fixtures,
66 – 71
parametrized testing, 42 –
49
plugins, creating, 100 –
114
plugins, using, 97 – 99,
163
renaming fixtures, 65 – 66
running subsets of tests,
38 – 42
setup and teardown with
fixtures, 53 – 55, 60
setup options, 10 – 21
skipping tests, 34 – 37
smoke tests, 31 – 34
source code, xvi
structure, 3, 24
test data with fixtures,
55 – 57
test discovery configura-
tion options, 120
testing for expected excep-
tions, 30
```
Index • 195


testing multiple configura-
tions with tox, 142 – 145
timer for, 63
--tb=auto option, 19
--tb=line option, 16, 18
--tb=long option, 19
--tb=native option, 19
--tb=no option, 13, 18
--tb=short option, 18, 117, 119,
143
--tb=style option, 10, 18, 127,
143
teardown, _see also_ fixtures
and --durations=N option, 20
with fixtures, 53 – 55, 60
with xUnit fixtures, 183 –
187
teardown() (xUnit fixture), 183 –
187
teardown_class(), 183
teardown_function(), 183
teardown_method(), 184
teardown_module(), 183
test classes, _see_ classes
test discovery
configuration options,
116 , 120– 123
file structure, 25
naming conventions, 6 – 7
rules, 121 , 123
test doubles, _see_ mocks
test files, _see_ files
test functions, _see_ functions
test headers, 102
test methods, _see_ methods
Test Results Analyzer, 145
test scope, 74
test_defaults(), 5
test_failing(), order and --first-failed
option, 16
test_member_access(), 5
testdir, 109– 112
testdir.runpytest(), 110
testing
code coverage, 130 – 135 ,
154
legacy, 149 – 154
parallel tests, 164
parametrized, 42 – 49, 187
repeating tests per ses-
sion, 163
running subsets of tests,
38 – 42

```
smoke tests, 31 – 34
specifying files and direc-
tories, 4, 6–7, 9, 11,
39 – 40
specifying only one test,
9
terms, xiii
testpaths, 116, 120
tests/ directory, 25
timeouts, 165
TinyDB, 53, 69
tmpdir
defined, 66
initializing database for
Task Project, 34
scope, 58 – 63, 74
using, 53, 57, 73– 77
tmpdir_factory, 53, 57–63, 66,
73 – 77
tox
configuration, 115
exercises, 154
installing, 144
installing plugins from
local directory, 99
Jenkins with, 148
resources, 145
specifying test directories,
121
testing multiple configura-
tions with, 142 – 145
tox.ini, 115, 142
traceback
color and progress bar,
167
displaying as failures
happen, 166
emojis plugin, 168 – 169
fixtures, 55
navigating in pdb module,
130
options, 10, 13, 16, 18–
19, 117, 119, 127
tox, 143
turning off, 13, 18
Twine, 181
types
API calls and functions,
30
database, 70
parametrized testing, 44
```
```
U
u (pdb module), 130
Ubuntu and issues with venv,
157
```
```
uninstallsomething, 160
uninstalling
packages, 160
plugins, 112 , 160
unit tests
defined, xiii
directories, 24
unittest
Django support, 173
mocks, 136 – 142
running legacy tests,
149 – 154
up (pdb module), 130
url field, packaging and distri-
bution, 108 , 178
usefixtures, 63, 116
```
```
V
-v option
changing indicator plugin
example, 104
checking for right tests,
12
marking functions, 32
parametrized tests and
fixtures, 70
running specific directo-
ries, classes, and tests,
39, 41
skipping tests, 36
using, 1, 8, 10, 16, 127
xfail tests, 38
venv, 21, 157
--verbose option
changing indicator plugin
example, 104
checking for right tests,
12
marking functions, 32
parametrized tests and
fixtures, 70
running specific directo-
ries, classes, and tests,
39, 41
skipping tests, 36
using, 1, 8, 10, 16, 127
xfail tests, 38
--version option, 10, 20
version field, packaging and
distribution, 108 , 178
versions
checking, 160
downloading multiple
with pip, 161
minimum pytest, 116 ,
119
mock package, 136
```
Index • 196


pip, 159
plugins, 98 – 99, 108
pytest, xii, 7, 10, 20,
116 , 119
Python, xii, 4, 7, 136,
159
specifying package, 160
in test output, 7
using multiple, 142 – 145 ,
159 , 161
--version option, 10, 20
virtualenv, 157
virtual environments
advantages, 157
exercises, 21
installing, 157
installing plugins from
local directory, 99
installing pytest, 4
installing tox, 144
Jenkins, 146 – 149
multiple, 142 – 145 , 148,
161
pip and, 159
resources, 158
using, 157 – 158 , 161
using multiple versions,
161
--version option, 10, 20
virtualenv
exercises, 21
installing pytest, 4
using, 157 – 158
versions, 157

```
W
warnings
with recwarn, 95
with warns(), 96
warns(), 96
web browser plugins, 172
web development plugins,
172
wheels
distributing from, 178 –
181
installing plugins and
packages from, 98, 161
.whl
distributing from, 178 –
181
installing plugins and
packages from, 98, 161
Windows
installing pytest, 4
platform in test output, 7
virtual environment set-
up, 158
```
```
X
x (xfail)
defined, 8
displays, 8
X (xpass)
defined, 8
displays, 8
```
```
-x (–exit first) option
exercises, 154
timeouts, 166
using, 10, 13, 127
xUnit fixtures, 183 – 187
xdist plugin, 164
xfail tests
defined, 8
displays, 8, 143, 168– 169
emojis, 168 – 169
exercises, 49
marking, 37
strict, 116 , 123
tox, 143
xfail(), 8, 37
xfail_strict, 116, 123
XML, Jenkins, 145 – 146
xpass tests
defined, 8
disallowing, 116 , 123
displays, 8, 143, 168– 169
emojis, 168 – 169
tox, 143
```
```
Y
yield, 34, 53
```
```
Z
zip files, installing plugins
and packages from, 98,
112 , 161
```
Index • 197


Thank you!

How did you enjoy this book? Please let us know. Take a moment and email

us at support@pragprog.com with your feedback. Tell us your story and you

could win free ebooks. Please use the subject line “Book Feedback.”

Ready for your next great Pragmatic Bookshelf book? Come on over to

https://pragprog.com and use the coupon code BUYANOTHER2020 to save 30%

on your next ebook.

Void where prohibited, restricted, or otherwise unwelcome. Do not use

ebooks near water. If rash persists, see a doctor. Doesn’t apply to _The_

_Pragmatic Programmer_ ebook because it’s older than the Pragmatic Bookshelf

itself. Side effects may include increased knowledge and skill, increased

marketability, and deep satisfaction. Increase dosage regularly.

And thank you for your continued support,

Andy Hunt, Publisher

SAVE 30%!

Use coupon code

**BUYANOTHER2020**


# The Pragmatic Bookshelf

The Pragmatic Bookshelf features books written by professional developers for professional
developers. The titles continue the well-known Pragmatic Programmer style and continue
to garner awards and rave reviews. As development gets more and more difficult, the Prag-
matic Programmers will be there with more titles and products to help you stay on top of
your game.

# Visit Us Online

**This Book’s Home Page**
_https://pragprog.com/book/bopytest_
Source code from this book, errata, and other resources. Come give us feedback, too!

**Keep Up to Date**
_https://pragprog.com_
Join our announcement mailing list (low volume) or follow us on twitter @pragprog for new
titles, sales, coupons, hot tips, and more.

**New and Noteworthy**
_https://pragprog.com/news_
Check out the latest pragmatic developments, new titles and other offerings.

# Buy the Book

If you liked this ebook, perhaps you’d like to have a paper copy of the book. Paperbacks are
available from your local independent bookstore and wherever fine books are sold.

# Contact Us

Online Orders: _https://pragprog.com/catalog_

Customer Service: _support@pragprog.com_

International Rights: _translations@pragprog.com_

Academic Use: _academic@pragprog.com_

Write for Us: _[http://write-for-us.pragprog.com](http://write-for-us.pragprog.com)_

Or Call: +1 800-699-7764


