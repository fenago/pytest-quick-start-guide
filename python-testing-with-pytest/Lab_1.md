

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

This book is the missing chapter absent from every comprehensive Python book.

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

of this book, the first four chapters were sent to a handful of reviewers.

Luciano was one of them, and his review was the hardest to read. I don’t think

I followed all of his advice, but because of his feedback, I re-examined and

rewrote much of the first three chapters and changed the way I thought about

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
    in isolation of the rest of the system. I consider the tests in Chapter 1,
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

In Chapter 1, Getting Started with pytest, on page 1, you’ll install pytest

and get it ready to use. You’ll then take one piece of the Tasks project—the

data structure representing a single task (a namedtuple called Task)—and use it

to test examples. You’ll learn how to run pytest with a handful of test files.

You’ll look at many of the popular and hugely useful command-line options

for pytest, such as being able to re-run test failures, stop execution after the

first failure, control the stack trace and test run verbosity, and much more.

In Chapter 2, Writing Test Functions, on page 23, you’ll install Tasks locally

using pip and look at how to structure tests within a Python project. You’ll do

this so that you can get to writing tests against a real application. All the

examples in this chapter run tests against the installed application, including

writing to the database. The actual test functions are the focus of this chapter,

and you’ll learn how to use assert effectively in your tests. You’ll also learn

about markers, a feature that allows you to mark many tests to be run at one

time, mark tests to be skipped, or tell pytest that we already know some tests

will fail. And I’ll cover how to run just some of the tests, not just with markers,

but by structuring our test code into directories, modules, and classes, and

how to run these subsets of tests.

Not all of your test code goes into test functions. In Chapter 3, pytest Fixtures,

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


In Chapter 4, Builtin Fixtures, on page 73, you will look at some builtin fix-

tures provided out-of-the-box by pytest. You will learn how pytest builtin

fixtures can keep track of temporary directories and files for you, help you

test output from your code under test, use monkey patches, check for

warnings, and more.

In Chapter 5, Plugins, on page 97, you’ll learn how to add command-line

options to pytest, alter the pytest output, and share pytest customizations,

including fixtures, with others through writing, packaging, and distributing

your own plugins. The plugin we develop in this chapter is used to make the

test failures we see while testing Tasks just a little bit nicer. You’ll also look

at how to properly test your test plugins. How’s that for meta? And just in

case you’re not inspired enough by this chapter to write some plugins of your

own, I’ve hand-picked a bunch of great plugins to show off what’s possible

in Appendix 3, Plugin Sampler Pack, on page 163.

Speaking of customization, in Chapter 6, Configuration, on page 115 , you’ll

learn how you can customize how pytest runs by default for your project with

configuration files. With a pytest.ini file, you can do things like store command-

line options so you don’t have to type them all the time, tell pytest to not look

into certain directories for test files, specify a minimum pytest version your

tests are written for, and more. These configuration elements can be put in

tox.ini or setup.cfg as well.

In the final chapter, Chapter 7, Using pytest with Other Tools, on page 127 ,

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
def test_passing** ():
**assert** (1, 2, 3) == (1, 2, 3)

This is what it looks like when it’s run:

**$ cd /path/to/code/ch
$ pytesttest_one.py**
=====================testsessionstarts======================
collected1 item

test_one.py.

===================1 passedin 0.01seconds===================

The dot after test_one.py means that one test was run and it passed. The [100%]

is a percentage inticator showing how much of the test suite is done so far.

Since there is just one test in the test session, one test is 100% of the tests.

If you have two tests, each would be 50%. If you need more information, you

can use -v or --verbose:

**$ pytest-v test_one.py**
=====================testsessionstarts======================
collected1 item

test_one.py::test_passingPASSED [100%]

===================1 passedin 0.01seconds===================

If you have a color terminal, the PASSED and bottom line are green. It’s nice.

This is a failing test:

**ch1/test_two.py
def test_failing** ():
**assert** (1, 2, 3) == (3, 2, 1)


The way pytest shows you test failures is one of the many reasons developers

love pytest. Let’s watch this fail:

**$ pytesttest_two.py**
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

**$ pytest-v test_two.py**
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

Chapter 1. Getting Started with pytest • 2


around and let me show you why I think pytest is the absolute best test

framework available.

In the rest of this chapter, you’ll install pytest, look at different ways to run

it, and run through some of the most often used command-line options. In

future chapters, you’ll learn how to write test functions that maximize the

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

For the rest of this chapter, I’ll use Task to demonstrate running pytest and

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

Chapter 1. Getting Started with pytest • 3


### Getting pytest

The headquarters for pytest is https://docs.pytest.org. That’s the official documen-

tation. But it’s distributed through PyPI (the Python Package Index) at

https://pypi.python.org/pypi/pytest.

Like other Python packages distributed through PyPI, use pip to install pytest

into the virtual environment you’re using for testing:

$ python3-m venvvenv
$ sourcevenv/bin/activate
$ pip installpytest

If you are not familiar with virtualenv or pip, I have got you covered. Check

out Appendix 1, Virtual Environments, on page 157 and Appendix 2, pip,

on page 159.

**What About Windows, Python 2, and virtualenv?**

```
The example for venv and pip should work on many POSIX systems, such as Linux
and macOS, and many versions of Python, including Python 3.4 and later. With
Python 2.7 and with some distributions of Linux, you will need to use virtualenv:
$ python-m pip installvirtualenv
$ python-m virtualenvvenv
$ sourcevenv/bin/activate
$ pip installpytest
```
```
The sourcevenv/bin/activate line won’t work for Windows, use venv\Scripts\activate.bat instead.
Do this:
```
```
C:\>python3-m venvvenv
C:\>venv\Scripts\activate.bat
C:\>pip installpytest
```
### Running pytest

**$ pytest--help**
usage:pytest[options][file_or_dir][file_or_dir][...]
**...**

Given no arguments, pytest looks at your current directory and all subdirec-

tories for test files and runs the test code it finds. If you give pytest a filename,

a directory name, or a list of those, it looks there instead of the current

directory. Each directory listed on the command line is recursively traversed

to look for test code.

For example, let’s create a subdirectory called tasks, and start with this test file:

Chapter 1. Getting Started with pytest • 4


**ch1/tasks/test_three.py**
_"""Testthe Taskdatatype."""_

**fromcollectionsimport** namedtuple

Task= namedtuple( _'Task'_ , [ _'summary'_ , _'owner'_ , _'done'_ , _'id'_ ])
Task.__new__.__defaults__= (None, None,False,None)

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

You can use __new__.__defaults__ to create Task objects without having to specify

all the fields. The test_defaults() test is there to demonstrate and validate how

the defaults work.

The test_member_access() test is to demonstrate how to access members by name

and not by index, which is one of the main reasons to use namedtuples.

Let’s put a couple more tests into a second file to demonstrate the _asdict() and

_replace() functionality:

**ch1/tasks/test_four.py**
_"""Testthe Taskdatatype."""_

**fromcollectionsimport** namedtuple

Task= namedtuple( _'Task'_ , [ _'summary'_ , _'owner'_ , _'done'_ , _'id'_ ])
Task.__new__.__defaults__= (None, None,False,None)

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
$ pytest**
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

**$ pytesttasks/test_three.pytasks/test_four.py**
=====================testsessionstarts======================
collected4 items

tasks/test_three.py.. [ 50%]
tasks/test_four.py.. [100%]

===================4 passedin 0.02seconds===================
**$ pytesttasks**
=====================testsessionstarts======================
collected4 items

tasks/test_four.py.. [ 50%]
tasks/test_three.py.. [100%]

===================4 passedin 0.02seconds===================
**$ cd tasks
$ pytest**
=====================testsessionstarts======================

Chapter 1. Getting Started with pytest • 6


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

I’ll cover that in Chapter 6, Configuration, on page 115.

Let’s take a closer look at the output of running just one file:

**$ cd /path/to/code/ch1/tasks
$ pytesttest_three.py**
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
configuration files in more detail in Chapter 6, Configuration, on page 115.
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
    a fixture, discussed in Chapter 3, pytest Fixtures, on page 51, or in a
    hook function, discussed in Chapter 5, Plugins, on page 97.

Chapter 1. Getting Started with pytest • 8


### Running Only One Test

One of the first things you’ll want to do once you’ve started writing tests is to

run just one. Specify the file directly, and add a ::test_name, like this:

Running Only One Test • 9


**$ cd /path/to/code/ch1
$ pytest-v tasks/test_four.py::test_asdict**
=====================testsessionstarts======================
collected1 item

tasks/test_four.py::test_asdictPASSED [100%]

===================1 passedin 0.01seconds===================

Now, let’s take a look at some of the options.

### Using Options

We’ve used the verbose option, -v or --verbose, a couple of times already, but

there are many more options worth knowing about. We’re not going to use

all of the options in this book, but quite a few. You can see all of them with

pytest--help.

The following are a handful of options that are quite useful when starting out

with pytest. This is by no means a complete list, but these options in partic-

ular address some common early desires for controlling how pytest runs when

you’re first getting started.

**$ pytest--help
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

Chapter 1. Getting Started with pytest • 10


**--collect-only**

The --collect-only option shows you which tests will be run with the given options

and configuration. It’s convenient to show this option first so that the output

can be used as a reference for the rest of the examples. If you start in the ch1

directory, you should see all of the test functions you’ve looked at so far in

this chapter:

**$ cd /path/to/code/ch1
$ pytest--collect-only**
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
$ pytest-k** _"asdictor defaults"_ **--collect-only**
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


**$ pytest-k** _"asdictor defaults"_
===================testsessionstarts===================
collected6 items/ 4 deselected

tasks/test_four.py. [ 50%]
tasks/test_three.py. [100%]

=========2 passed,4 deselectedin 0.03seconds==========

Hmm. Just dots. So they passed. But were they the right tests? One way to

find out is to use -v or --verbose:

**$ pytest-v -k** _"asdictor defaults"_
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

**importpytest**

...
@pytest.mark.run_these_please
**def test_member_access** ():
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

Chapter 1. Getting Started with pytest • 12


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
$ pytest--tb=no**
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
$ pytest--maxfail=2--tb=no**
===================testsessionstarts===================
collected6 items

test_one.py. [ 16%]
test_two.pyF [ 33%]
tasks/test_four.py.. [ 66%]
tasks/test_three.py.. [100%]

===========1 failed,5 passedin 0.07seconds============
**$ pytest--maxfail=1--tb=no**
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

Chapter 1. Getting Started with pytest • 14


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
$ pytest--lf**
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
$ pytest--ff--tb=no
$ pytest--ff--tb=no**
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
$ pytest-v --ff--tb=no**
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

Chapter 1. Getting Started with pytest • 16


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
$ pytest--tb=notasks**
===================testsessionstarts===================
collected4 items

tasks/test_four.py.F [ 50%]
tasks/test_three.py.. [100%]

===========1 failed,3 passedin 0.06seconds============

--tb=line in many cases is enough to tell what’s wrong. If you have a ton of

failing tests, this option can help to show a pattern in the failures:

Chapter 1. Getting Started with pytest • 18


**$ pytest--tb=linetasks**
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

**$ pytest--tb=shorttasks**
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

pytest--tb=long will show you the most exhaustive, informative traceback possi-

ble. pytest--tb=auto will show you the long version for the first and last trace-

backs, if you have multiple failures. This is the default behavior. pytest--tb=native

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
$ pytest--durations=3tasks**
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

necessary. I cover fixtures in depth in Chapter 3, pytest Fixtures, on page 51.

**--version**

The --version option shows the version of pytest and the directory where it’s

installed:

**$ pytest--version**
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
    more in Chapter 6, Configuration, on page 115

Chapter 1. Getting Started with pytest • 20


- A list of environmental variables that can affect pytest behavior (also
    discussed in Chapter 6, Configuration, on page 115 )
- A reminder that pytest--markers can be used to see available markers,
    discussed in Chapter 2, Writing Test Functions, on page 23
- A reminder that pytest--fixtures can be used to see available fixtures, dis-
    cussed in Chapter 3, pytest Fixtures, on page 51

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

a project. You’ll learn about conftest.py and ini files such as pytest.ini in Chapter

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
4. Create a few test files. You can use the ones we used in this chapter or

make up your own. Practice running pytest against these files.

5. Change the assert statements. Don’t just use assertsomething==something_else;

try things like:

- assert1 in [2, 3, 4]
- asserta < b
- assert'fizz' not in 'fizzbuzz'

### What’s Next

In this chapter, we looked at where to get pytest and the various ways to run

it. However, we didn’t discuss what goes into test functions. In the next

chapter, we’ll look at writing test functions, parametrizing them so they get

called with different data, and grouping tests into classes, modules, and

packages.

Chapter 1. Getting Started with pytest • 22
