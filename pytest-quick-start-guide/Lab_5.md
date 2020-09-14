
[]{#ch05}Chapter 5. Plugins {#chapter-5.-plugins .title}
---------------------------

</div>

</div>
:::

In the previous chapter, we explored one of the most important features
of pytest: fixtures. We learned how we can use fixtures to manage
resources and make our lives easier when writing tests.

pytest is constructed with customization and flexibility in mind, and
allows developers to write powerful extensions
called [**plugins**]{.strong}. Plugins in pytest can do all sorts of
things, from simply providing a new fixture, all the way to adding
command line options, changing how tests are executed, and even running
tests written in other languages.

In this chapter, we will do the following:

::: {.itemizedlist}
-   Learn how to find and install plugins
-   Have a taste of what plugins the ecosystem has to offer



[]{#ch05lvl1sec33}Finding and installing plugins {#finding-and-installing-plugins .title style="clear: both"}
------------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

As mentioned at the beginning[]{#id324673416 .indexterm} of the chapter,
pytest is written[]{#id325533900 .indexterm} from the ground up with
customization and flexibility in mind. The plugin mechanism is at the
core of the pytest architecture, so much so that many of pytest\'s
built-in features are implemented in terms of internal plugins, such as
marks, parametrization, fixtures---nearly everything, even command-line
options.

This flexibility has led to an enormous and rich plugin ecosystem. At
the time of writing, the number of plugins available is over 500, and
that number keeps increasing at an astonishing rate. 

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec49}Finding plugins {#finding-plugins .title}

</div>

</div>
:::

Given the large number of plugins, it would be nice if there was a site
that showed all pytest plugins along with their descriptions. It would
also be nice if this place also showed[]{#id325533917 .indexterm}
information about compatibility with different Python and pytest
versions.

Well, the good news is that such a site exists, and it is maintained by
the core development team: pytest plugin compatibility
(<http://plugincompat.herokuapp.com/>). On it, you will find a list of
all the pytest plugins available in PyPI, along with Python- and
pytest-version compatibility information. The site is fed daily with new
plugins and updates directly from PyPI, making it a great place to
browse for new plugins.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec50}Installing plugins {#installing-plugins .title}

</div>

</div>
:::

Plugins are usually installed with `pip`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pip install <PLUGIN_NAME>
```
:::

For example, to install `pytest-mock`{.literal}, we
execute[]{#id325534028 .indexterm} the following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pip install pytest-mock
```
:::

No registration of any kind is necessary; pytest automatically detects
the installed plugins in your virtual environment or Python
installation. 

This simplicity makes it dead easy to try out new plugins. 



[]{#ch05lvl1sec34}An overview of assorted plugins {#an-overview-of-assorted-plugins .title style="clear: both"}
-------------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Now, we will take a look at some useful[]{#id324673427 .indexterm}
and/or interesting plugins. Of course, it is not possible to cover all
plugins here, so we will try to cover the ones that cover popular
frameworks and general capabilities, with a few obscure plugins thrown
in. Of course, this barely scratches the surface, but let\'s get to it.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec51}pytest-xdist {#pytest-xdist .title}

</div>

</div>
:::

This is a very popular plugin[]{#id325162608 .indexterm} and is
maintained by the[]{#id325162616 .indexterm} core developers; it allows
you to run tests under multiple CPUs, to speed up the test run.

After installing it, simply use the `-n`{.literal} command-line flag to
use the given number of CPUs to run the tests:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -n 4
```
:::

And that\'s it! Now, your tests will run across four cores and hopefully
speed up the test suite quite a bit, if it is CPU intensive, thought
I/O-bound tests won\'t see much improvement, though. You can also use
`-n auto`{.literal} to let `pytest-xdist`{.literal} automatically figure
out the number of CPUs you have available.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip11}Note {#note .title}

Keep in mind that when your tests are running concurrently, and in
random order, they must be careful to avoid stepping on each other\'s
toes, for example, reading/writing to the same directory. While they
should be idempotent anyway, running the tests in a random order often
brings attention to problems that were lying dormant until then. 
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec52}pytest-cov {#pytest-cov .title}

</div>

</div>
:::

The `pytest-cov`{.literal} plugin provides[]{#id325533878 .indexterm}
integration with the popular coverage module, which
provides[]{#id325533886 .indexterm} detailed coverage reports for your
code when running tests. This lets you detect sections of code that are
not covered by any test code, which is an opportunity to write more
tests to cover those cases.

After installation, you can use the `--cov`{.literal} option to provide
a coverage report at the end of the test run:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --cov=src
...
----------- coverage: platform win32, python 3.6.3-final-0 -----------
Name                  Stmts   Miss  Cover
----------------------------------------
src/series.py           108      5   96%
src/tests/test_series    22      0  100%
----------------------------------------
TOTAL                   130      5   97%
```
:::

The `--cov`{.literal} option accepts a path to source files that should
have reports generated, so you should pass your `src`{.literal} or
package directory depending on your project\'s layout. 

You can also use the `--cov-report`{.literal} option to generate reports
in various formats: XML, annotate, and HTML. The latter is especially
useful to use locally because it generates HTML files showing your code,
with missed lines highlighted in red, making it very easy to find those
uncovered spots.

This plugin also works with `pytest-xdist`{.literal} out of the box.

Finally, the `.coverage`{.literal} file generated by this plugin is
compatible with many online services that provide coverage tracking and
reporting, such as `coveralls.io`{.literal} (<https://coveralls.io/>[)
and
`codecov.io`{.literal} (](https://coveralls.io/){.ulink}<https://codecov.io/>[).](https://coveralls.io/){.ulink}
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec53}pytest-faulthandler {#pytest-faulthandler .title}

</div>

</div>
:::

This plugin automatically[]{#id325091794 .indexterm} enables
the[]{#id325091802 .indexterm} built-in
`faulthandler`{.literal} (<https://docs.python.org/3/library/faulthandler.html>)
module when running your tests, which outputs Python tracebacks in
catastrophic cases such as a segmentation fault. After installed, no
other setup or flag is required; the `faulthandler`{.literal} module
will be enabled automatically.

This plugin is strongly recommended if you regularly use extension
modules written in C/C++, as those are more susceptible to crashes.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec54}pytest-mock {#pytest-mock .title}

</div>

</div>
:::

The `pytest-mock`{.literal} plugin provides[]{#id325091837 .indexterm} a
fixture that allows a smoother[]{#id325091847 .indexterm} integration
between pytest and the
`unittest.mock`{.literal} (<https://docs.python.org/3/library/unittest.mock.html>)
module of the standard library. It provides[]{#id325091861 .indexterm}
functionality similar to the built-in `monkeypatch`{.literal} fixture,
but the mock objects produced by `unittest.mock`{.literal} also record
information on how they are accessed. This makes many common testing
tasks easier, such as verifying that a mocked function has been called,
and with which arguments.

The plugin provides a `mocker`{.literal} fixture that can be used for
patching classes and methods. Using the `getpass`{.literal} example from
the last chapter, here is how you could write it using this plugin:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import getpass

def test_login_success(mocker):
    mocked = mocker.patch.object(getpass, "getpass", 
                                 return_value="valid-pass")
    assert user_login("test-user")
mocked.assert_called_with("enter password: ")
```
:::

Note that besides replacing `getpass.getpass()`{.literal} and always
returning the same value, we can also ensure that the
`getpass`{.literal} function has been called with the correct arguments.

The same advice on how and where to patch the `monkeypatch`{.literal}
fixture from the previous chapter also applies when using this plugin.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec55}pytest-django {#pytest-django .title}

</div>

</div>
:::

As the name suggests, this plugin[]{#id325091957 .indexterm} allows you
to[]{#id325091965 .indexterm} test your `Django`{.literal}
(<https://www.djangoproject.com/>) applications using pytest.
`Django`{.literal} is one[]{#id325091982 .indexterm} of the most famous
web frameworks in use today.

The plugin provides a ton of features:

::: {.itemizedlist}
-   A very nice Quick Start tutorial
-   Command-line and `pytest.ini`{.literal} options to configure Django
-   Compatibility with `pytest-xdist`{.literal}
-   Database access using the `django_db`{.literal} mark, with automatic
    transaction rollback between tests, as well as a bunch of fixtures
    that let you control how the database is managed
-   Fixtures to make requests to your application: `client`{.literal},
    `admin_client`{.literal}, and `admin_user`{.literal}
-   A `live_server`{.literal} fixture that runs a `Django`{.literal}
    server in a background thread
:::

All in all, this is one of the most complete plugins available in the
ecosystem, with too many features to cover here. It is a must-have for
`Django`{.literal} applications, so make sure to check out its extensive
documentation.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec56}pytest-flakes {#pytest-flakes .title}

</div>

</div>
:::

This plugin allows you[]{#id325092045 .indexterm} to check
your[]{#id325092054 .indexterm} code
using `pyflakes`{.literal} (<https://pypi.org/project/pyflakes/>), which
is a static checker of source files for common errors, such as missing
imports and unknown variables.

After installed, use the `--flakes`{.literal} option to activate it:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest pytest-flakes.py --flake
...
============================= FAILURES ==============================
__________________________ pyflakes-check ___________________________
CH5\pytest-flakes.py:1: UnusedImport
'os' imported but unused
CH5\pytest-flakes.py:6: UndefinedName
undefined name 'unknown'
```
:::

This will run the flake checks alongside your normal tests, making it an
easy and cheap way to keep your code tidy and prevent some errors. The
plugin also keeps a local cache of files that have not changed since the
last check, so it is fast and convenient to use locally.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec57}pytest-asyncio {#pytest-asyncio .title}

</div>

</div>
:::

The `asyncio`{.literal}
(<https://docs.python.org/3/library/asyncio.html>) module is
one[]{#id325092110 .indexterm} of the hot new additions to Python 3,
providing a new[]{#id325092119 .indexterm} framework for asynchronous
applications. The `pytest-asyncio`{.literal} plugin lets[]{#id325092131
.indexterm} you write asynchronous test functions, making it a snap to
test your asynchronous code.

All you need to do is make your test function `async def`{.literal} and
mark it with the `asyncio`{.literal} mark:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.asyncio
async def test_fetch_requests():
    requests = await fetch_requests("example.com/api")
    assert len(requests) == 2
```
:::

The plugin also manages the event loop behind the scenes, providing a
few options on how to change it if you need to use a custom event loop.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip12}Note {#note-1 .title}

You are, of course, free to have normal synchronous test functions along
with the asynchronous ones.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec58}pytest-trio {#pytest-trio .title}

</div>

</div>
:::

Trio\'s motto is Pythonic[]{#id325092203 .indexterm} async I/O
for[]{#id325092212 .indexterm} humans
(<https://trio.readthedocs.io/en/latest/>). It uses the same
`async def`{.literal}/`await`{.literal} keywords of the
`asyncio`{.literal} standard module, but it is considered simpler and
more friendly to use, containing some novel ideas about how to deal with
timeouts and groups of parallel tasks in a way to avoid common errors in
parallel programming. It is definitely worth checking out if you are
into asynchronous development.

 

`pytest-trio`{.literal} works similarly to `pytest-asyncio`{.literal}:
you write asynchronous test functions and mark them using the
`trio`{.literal} mark. It also provides other functionality that makes
testing easier and more reliable, such as controllable clocks for
testing timeouts, special functions to deal with tasks, mocking network
sockets and streams, and a lot more.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec59}pytest-tornado {#pytest-tornado .title}

</div>

</div>
:::

Tornado (<http://www.tornadoweb.org/en/stable/>) is a web[]{#id325536844
.indexterm} framework and asynchronous[]{#id325536853 .indexterm}
network library. It is very mature, works in Python 2 and 3, and the
standard `asyncio`{.literal} module borrowed many ideas and concepts
from it.

`pytest-asyncio`{.literal} was heavily[]{#id325536867 .indexterm}
inspired by `pytest-tornado`{.literal}, so it works with the same idea
of using a `gen_test`{.literal} to mark your test as a coroutine. It
uses the `yield`{.literal} keyword instead of `await`{.literal}, as it
supports Python 2, but otherwise it looks very similar:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.gen_test
def test_tornado(http_client):
    url = "https://docs.pytest.org/en/latest"
    response = yield http_client.fetch(url)
    assert response.code == 200
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec60}pytest-postgresql {#pytest-postgresql .title}

</div>

</div>
:::

This plugin[]{#id325536899 .indexterm} allows you to test code that
needs a running PostgreSQL database.

Here\'s a quick example[]{#id325536915 .indexterm} of it in action:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_fetch_series(postgresql):
    cur = postgresql.cursor()
    cur.execute('SELECT * FROM comedy_series;')
    assert len(cur.fetchall()) == 5
    cur.close()
```
:::

It provides two fixtures:

::: {.itemizedlist}
-   `postgresql`{.literal}: a client fixture that starts and closes
    connections to the running test database. At the end of the test, it
    drops the test database to ensure tests don\'t interfere with one
    another.
-   `postgresql_proc`{.literal}: a session-scoped fixture that starts
    the PostgreSQL process once per session and ensures that it stops at
    the end.
:::

 

It also provides several configuration options on how to connect and
configure the testing database.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec61}docker-services {#docker-services .title}

</div>

</div>
:::

This plugin starts and manages[]{#id325536957 .indexterm} Docker
services you need in order[]{#id325536966 .indexterm} to test your code.
This makes it simple to run the tests because you don\'t need to
manually start the services yourself; the plugin will start and stop
them during the test session, as needed.

You configure the services using a `.services.yaml`{.literal} file; here
is a simple example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
database:
    image: postgresenvironment:
        POSTGRES_USERNAME: pytest-userPOSTGRES_PASSWORD: pytest-passPOSTGRES_DB: test
    image: regis:10
```
:::

This will start two services: `postgres`{.literal} and
`redis`{.literal}.

With that, all that\'s left to do is to run your suite with the
following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
pytest --docker-services
```
:::

The plugin takes care of the rest.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec62}pytest-selenium {#pytest-selenium .title}

</div>

</div>
:::

Selenium is a framework[]{#id325537033 .indexterm} targeted to
automating[]{#id325537043 .indexterm} browsers, to test web applications
(<https://www.seleniumhq.org/>). It lets you do things such as opening a
web page, clicking on a button, and then[]{#id325537053 .indexterm}
ensuring that a certain page loads, all programmatically. It supports
all the major browsers out there and has a thriving community.

`pytest-selenium`{.literal} provides you with a fixture that lets you
write tests that do all of those things, taking care of setting up
`Selenium`{.literal} for you.

Here\'s a basic example of how to visit a page, click on a link, and
check the title of the loaded page:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_visit_pytest(selenium):
    selenium.get("https://docs.pytest.org/en/latest/")
    assert "helps you write better programs" in selenium.title
    elem = selenium.find_element_by_link_text("Contents")
    elem.click()
    assert "Full pytest documentation" in selenium.title
```
:::

`Selenium`{.literal} and `pytest-selenium`{.literal} are sophisticated
enough to test a wide range of applications, from static pages to full
single-page frontend applications.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec63}pytest-html {#pytest-html .title}

</div>

</div>
:::

`pytest-html`{.literal} generates beautiful[]{#id325537093 .indexterm}
HTML reports of your test[]{#id325537101 .indexterm} results. After
installing the plugin, simply run this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --html=report.html
```
:::

This will generate a `report.html`{.literal} file at the end of the test
session.

Because pictures speak louder than words, here is an example:

::: {.mediaobject}
![](3_files/e79f3c6e-41c4-4d27-bc8c-ec1c455b913d.png)
:::

 

The reports can be served in a web server for easier viewing, plus they
contain nice functionality such as checkboxes to show/hide different
types[]{#id325537137 .indexterm} of test results, and other
plugins[]{#id325537145 .indexterm} such as `pytest-selenium`{.literal}
are even able to attach screenshots to failed tests, as in the previous
image.

It\'s definitely worth checking out.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec64}pytest-cpp {#pytest-cpp .title}

</div>

</div>
:::

To prove the point[]{#id325537163 .indexterm} that pytest\'s
framework[]{#id325537172 .indexterm} is very flexible, the
`pytest-cpp`{.literal} plugin allows you to run tests written in Google
Test (<https://github.com/google/googletest>) or Boost.Test
([https://www.boost.org](https://www.boost.org/){.ulink}[)](https://www.boost.org/){.ulink},
which are frameworks for writing and running[]{#id325537194 .indexterm}
tests in the C++ language. 

After they are installed, you just need to run pytest as normal:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest bin/tests
```
:::

Pytest will find executable files containing test cases,
detecting  automatically whether they are written in
`Google Test`{.literal} or `Boost.Python`{.literal}. It will run the
tests and report results normally, with neat formatting that is familiar
to pytest users.

Running those tests with pytest means that they now can make use of
several features, such as parallel running with
`pytest-xdist`{.literal}, test selection with `-k`{.literal}, JUnitXML
reports, and so on. This plugin is particularly useful for code bases
that use Python and C++ because it allows you to run all tests with a
single command, and you can obtain a unique report.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec65}pytest-timeout {#pytest-timeout .title}

</div>

</div>
:::

The `pytest-timeout`{.literal} plugin terminates[]{#id325537238
.indexterm} tests automatically after[]{#id325537282 .indexterm} they
reach a certain timeout. 

You use it by setting a global timeout in the command-line:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --timeout=60
```
:::

Or you can mark individual tests with the
`@pytest.mark.timeout`{.literal} mark:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
@pytest.mark.timeout(600)
def test_long_simulation():
   ...
```
:::

It works by using one of the two following methods to implement its
timeout mechanism:

::: {.itemizedlist}
-   `thread`{.literal}: during test setup, the plugin starts a thread
    that sleeps for the desired timeout period. If the thread wakes up,
    it will dump the tracebacks of all the threads to `stderr`{.literal}
    and kill the current process. If the test finishes before the thread
    wakes up, then the thread is cancelled and the test run continues.
    This is the method that works on all platforms.
-   `signal`{.literal}: a `SIGALRM`{.literal} is scheduled during test
    setup and canceled when the test finishes. If the alarm is
    triggered, it will dump the tracebacks of all threads to
    `stderr`{.literal} and fail the test, but it will allow the test run
    to continue. The advantage over the thread method is that it won\'t
    cancel the entire run when a timeout occurs, but it is not supported
    on all platforms.
:::

The method is chosen automatically based on platform, but it can be
changed in the command line or per-test by passing the
`method=`{.literal} parameter to `@pytest.mark.timeout`{.literal}.

This plugin is indispensable in large test suites to avoid having tests
hanging the CI.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec66}pytest-annotate {#pytest-annotate .title}

</div>

</div>
:::

Pyannotate (<https://github.com/dropbox/pyannotate>) is a
project[]{#id325540082 .indexterm} that observes runtime
type[]{#id325540090 .indexterm} information and can use that
information[]{#id325540097 .indexterm} to insert type annotations into
the source code, and `pytest-annotate`{.literal} makes it easy to use
with pytest.

Let\'s get back to this simple test case:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def highest_rated(series):
    return sorted(series, key=itemgetter(2))[-1][0]

def test_highest_rated():
    series = [
        ("The Office", 2005, 8.8),
        ("Scrubs", 2001, 8.4),
        ("IT Crowd", 2006, 8.5),
        ("Parks and Recreation", 2009, 8.6),
        ("Seinfeld", 1989, 8.9),
    ]
    assert highest_rated(series) == "Seinfeld"
```
:::

After installing `pytest-annotate`{.literal}, we can generate an
annotations file passing the `--annotations-output`{.literal} flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --annotate-output=annotations.json
```
:::

This will run the test suite as usual, but it will collect type
information for later use.

Afterward, you can call `PyAnnotate`{.literal} to apply the type
information directly to the source code:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pyannotate --type-info annotations.json -w
Refactored test_series.py
--- test_series.py (original)
+++ test_series.py (refactored)
@@ -1,11 +1,15 @@
 from operator import itemgetter
+from typing import List
+from typing import Tuple


 def highest_rated(series):
+    # type: (List[Tuple[str, int, float]]) -> str
     return sorted(series, key=itemgetter(2))[-1][0]


 def test_highest_rated():
+    # type: () -> None
     series = [
         ("The Office", 2005, 8.8),
         ("Scrubs", 2001, 8.4),
Files that were modified:
pytest-annotate.py
```
:::

It is very neat to quickly and efficiently annotate a large code base,
especially if that code base is well covered by tests.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec67}pytest-qt {#pytest-qt .title}

</div>

</div>
:::

The `pytest-qt`{.literal} plugin allows[]{#id325540231 .indexterm} you
to write tests[]{#id325540240 .indexterm} for GUI applications written
in the `Qt`{.literal} framework (<https://www.qt.io/>), supporting the
more popular sets of Python bindings for
`Qt`{.literal}: `PyQt4`{.literal}/`PyQt5`{.literal},
and `PySide`{.literal}/`PySide2`{.literal}. 

It provides a `qtbot`{.literal} fixture that has methods to interact
with a GUI application, such as clicking on buttons, entering text in
fields, waiting for windows to pop up, and others. Here\'s a quick
example showing it in action:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_main_window(qtbot):
    widget = MainWindow()
    qtbot.addWidget(widget)

    qtbot.mouseClick(widget.about_button, QtCore.Qt.LeftButton)
    qtbot.waitUntil(widget.about_box.isVisible)
    assert widget.about_box.text() == 'This is a GUI App'
```
:::

Here, we create a window, click on the **`about`** button, wait for the
**`about`** box to show up, and then ensure it shows the text we
expect. 

It also contains other goodies:

::: {.itemizedlist}
-   Utilities to wait for specific `Qt`{.literal} signals
-   Automatic capturing of errors in virtual methods
-   Automatic capturing of `Qt`{.literal} logging messages
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec68}pytest-randomly {#pytest-randomly .title}

</div>

</div>
:::

Tests ideally should be independent[]{#id325540347 .indexterm} from each
other, making sure[]{#id325540356 .indexterm} to clean up after
themselves so they can be run in any order and don\'t affect one another
in any way. 

`pytest-randomly`{.literal} helps you keep your test suite true to that
point, by randomly ordering tests, changing their order every time you
run your test suite. This helps detect whether the tests have hidden
inter-dependencies that you would not find otherwise.

It shuffles the order of the test items at module level, then at class
level, and finally at the order of functions. It also resets
`random.seed()`{.literal} before each test to a fixed number, which is
shown at the beginning of the test section. The random seed can be used
at a later time to reproduce the same order with the
`--randomly-seed`{.literal} command line to reproduce a failure.

As an extra bonus, it also has special support
for `factory boy`{.literal}
(<https://factoryboy.readthedocs.io/en/latest/reference.html>),
`faker`{.literal} (<https://pypi.python.org/pypi/faker>), and
`numpy`{.literal} (<http://www.numpy.org/>) libraries, resetting their
random state before each test.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec69}pytest-datadir {#pytest-datadir .title}

</div>

</div>
:::

Often, tests need a supporting[]{#id325540407 .indexterm} file, for
example a CSV file containing data[]{#id325540416 .indexterm} about
comedy series, as we saw in the last chapter. `pytest-datadir`{.literal}
allows you to save files alongside your tests and easily access them
from the tests in a safe manner.

Suppose you have a file structure such as this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
tests/
    test_series.py
```
:::

In addition to this, you have a `series.csv`{.literal} file that you
need to access from tests defined in `test_series.py`{.literal}.

With `pytest-datadir`{.literal} installed, all you need to do is to
create a directory with the name of the test file in the same directory
and put the file there:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
tests/
    test_series/
        series.csv
    test_series.py
```
:::

The `test_series`{.literal} directory and `series.csv`{.literal} should
be saved to your version-control system.

Now, tests in `test_series.py`{.literal} can use the `datadir`{.literal}
fixture to access the file:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_ratings(datadir):
    with open(datadir / "series.csv", "r", newline="") as f:
        data = list(csv.reader(f))
    ...
```
:::

`datadir`{.literal} is a Path instance pointing to the[]{#id325540528
.indexterm} data directory
(<https://docs.python.org/3/library/pathlib.html>).

One important thing to note is that when we use the `datadir`{.literal}
fixture in a test, we are not accessing the path to the original file,
but a temporary[]{#id325540546 .indexterm} copy. This ensures that
tests[]{#id325540554 .indexterm} can modify the files inside the data
directory without affecting other tests because each test has its own
copy.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch05lvl2sec70}pytest-regressions {#pytest-regressions .title}

</div>

</div>
:::

It is normally the case that your[]{#id325540569 .indexterm} application
or library contains[]{#id325540578 .indexterm} functionality that
produces a data set as the result.

Testing these results is often tedious and error-prone, producing tests
such as this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_obtain_series_asserts():
    data = obtain_series()
    assert data[0]["name"] == "The Office"
    assert data[0]["year"] == 2005
    assert data[0]["rating"] == 8.8
    assert data[1]["name"] == "Scrubs"
    assert data[1]["year"] == 2001
    ...
```
:::

This gets old very quickly. Also, if any of the assertion fails, then
the test stops at that point, and you won\'t know whether any other
asserts after that point would also have failed. In other words, you
don\'t get a clear picture of the overall failures. Most of all, this is
also heavily unmaintainable because if the data returned by
`obtain_series()`{.literal} ever changes, you are in for a tedious and
error-prone task of updating all the code.

`pytest-regressions`{.literal} provides fixtures to solve this kind of
problem. General data such as the previous example is a job for the
`data_regression`{.literal} fixture:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_obtain_series(data_regression):
    data = obtain_series()
data_regression.check(data)
```
:::

The first time you execute this test, it will fail with a message such
as this:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
...
E Failed: File not found in data directory, created:
E - CH5\test_series\test_obtain_series.yml
```
:::

It will dump the data passed to `data_regression.check()`{.literal} in a
nicely formatted YAML file into the data directory of the
`test_series.py`{.literal} file (courtesy of the
`pytest-datadir`{.literal} fixture we saw earlier):

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
- name: The Office
  rating: 8.8
  year: 2005
- name: Scrubs
  rating: 8.4
  year: 2001
- name: IT Crowd
  rating: 8.5
  year: 2006
- name: Parks and Recreation
  rating: 8.6
  year: 2009
- name: Seinfeld
  rating: 8.9
  year: 1989
```
:::

The next time you run this test, `data_regression`{.literal} now
compares the data passed to `data_regressions.check()`{.literal} with
the data found in `test_obtain_series.yml`{.literal} inside the data
directory. If they match, the test passes.

If the data is changed, however, the test fails with a nicely
formatted text differential  between the new data and the recorded one:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
E AssertionError: FILES DIFFER:
E ---
E
E +++
E
E @@ -13,3 +13,6 @@
E
E  - name: Seinfeld
E    rating: 8.9
E    year: 1989
E +- name: Rock and Morty
E +  rating: 9.3
E +  year: 2013
```
:::

In some cases, this might be a regression, in which case you can hunt
down the bug in the code.

But in this case, the new data is [*correct;*]{.emphasis} you just need
to run pytest with the `--force-regen`{.literal} flag and
`pytest-regressions`{.literal} will update the data file with the new
content for you:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
E Failed: Files differ and --force-regen set, regenerating file at:
E - CH5\test_series\test_obtain_series.yml
```
:::

Now, the test passes if we run it again, as the file contains the new
data.

This is an immense time[]{#id325540734 .indexterm} saver when you have
dozens[]{#id325540743 .indexterm} of tests that suddenly produce
different but correct results. You can bring them all up to date with a
single pytest execution.

I use this plugin myself, and I can\'t count the hours it has saved me.



[]{#ch05lvl1sec35}Honorable mentions {#honorable-mentions .title style="clear: both"}
------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

There are just too many good plugins to fit into this chapter. The
previous sample is really just a small taste, where I tried to strike a
balance between useful, interesting, and showing the flexibility of the
plugin architecture.

Here are a few other plugins that are worth mentioning:

::: {.itemizedlist}
-   `pytest-bdd`{.literal}: a behavior-driven development for pytest
-   `pytest-benchmark`{.literal}: a fixture to benchmark code. It
    outputs benchmark results with color output
-   `pytest-csv`{.literal}: outputs test status as CSV files
-   `pytest-docker-compose`{.literal}: this manages Docker containers,
    using Docker compose during test runs
-   `pytest-excel`{.literal}: outputs test status reports in Excel
-   `pytest-git`{.literal}: provides a git fixture for tests that need
    to deal with git repositories
-   `pytest-json`{.literal}: outputs test statuses as json files
-   `pytest-leaks`{.literal}: detects memory leaks, by running tests
    repeatedly and comparing reference counts
-   `pytest-menu`{.literal}: lets the user select tests to run from a
    menu in the console
-   `pytest-mongo`{.literal}: process and client fixtures for MongoDB
-   `pytest-mpl`{.literal}: plugin that tests figures output from
    Matplotlib
-   `pytest-mysql`{.literal}: process and client fixtures for MySQL
-   `pytest-poo`{.literal}: replaces the `F`{.literal} character for
    failing tests with the \"pile of poo\" emoji
-   `pytest-rabbitmq`{.literal}: process and client fixtures for
    RabbitMQ
-   `pytest-redis`{.literal}: process and client fixtures for Redis
-   `pytest-repeat`{.literal}: repeats all tests or specific tests a
    number of times to find intermittent failures
-   `pytest-replay`{.literal}: saves test runs and allows the user to
    execute them later, so as to reproduce crashes and flaky tests
-   `pytest-rerunfailures`{.literal}: this marks tests that can be run
    more than once to eliminate flaky tests
-   `pytest-sugar`{.literal}: changes the look and feel of the pytest
    console, by adding progress bars, emojis, instant failures, and so
    on
-   `pytest-tap`{.literal}: toutputs test reports in TAP format
-   `pytest-travis-fold`{.literal}: folds captured output and coverage
    reports in the Travis CI build log
-   `pytest-vagrant`{.literal}: pytest fixture that works with vagrant
    boxes
-   `pytest-vcr`{.literal}: automatically[]{#id325536764 .indexterm}
    manages `VCR.py`{.literal} cassettes
    (<https://vcrpy.readthedocs.io/en/latest/>), using a simple mark
-   `pytest-virtualenv`{.literal}: this provides a virtualenv fixture to
    manage virtual environments in tests
:::

::: {.itemizedlist}
-   `pytest-watch`{.literal}: this continuously watches for changes in
    the source code and reruns pytest
-   `pytest-xvfb`{.literal}: this runs `Xvfb`{.literal} (a virtual frame
    buffer) for your UI tests
-   `tavern`{.literal}: is tan automated test for APIs using a
    YAML-based syntax
-   `xdoctest`{.literal}: rewrite of the built-in doctests module, to
    make doctests easier to write and simpler to configure
:::

Remember, at the time of writing, the number of pytest plugins available
is over 500, so make sure to browse the list of plugins so that you can
find something to your liking.



[]{#ch05lvl1sec36}Summary {#summary .title style="clear: both"}
-------------------------

</div>

</div>

------------------------------------------------------------------------
:::

In this chapter, we looked at how easy it is to find and install
plugins. We also have been shown some plugins that I use daily and find
interesting. I hope this has given you a taste of what\'s possible in
pytest, but please explore the vast number of plugins to see whether you
can find any that are useful.

Creating your own plugins is not a topic that is covered in this book,
but if you are interested, here are some resources to get you started:

::: {.itemizedlist}
-   The pytest documentation: writing plugins
    (<https://docs.pytest.org/en/latest/writing_plugins.html>).
-   Brian Okken\'s wonderful book about pytest Python testing with
    pytest, which delves deeper than this book does, has an excellent
    chapter on how to write your own plugins. 
:::

In the next chapter, we will learn how to use pytest with existing
`unittest`{.literal}-based test suites, including tips and suggestions
on how to migrate them and incrementally use more of pytest\'s features.
