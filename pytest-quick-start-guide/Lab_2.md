

[]{#ch02}Chapter 2. Writing and Running Tests {#chapter-2.-writing-and-running-tests .title}
---------------------------------------------

</div>

</div>
:::

In the previous chapter, we discussed why testing is so important and
looked at a brief overview of the `unittest`{.literal} module. We also
took a cursory look at pytest\'s features, but barely got a taste of
them.

In this chapter, we will start our journey with pytest. We will be
pragmatic, so this means that we will not take an exhaustive look at all
of the things it\'s possible to do with pytest, but instead provide you
with a quick overview of the basics to make you productive quickly. We
will take a look at how to write tests, how to organize them into files
and directories, and how to use pytest\'s command line effectively. 

Here\'s what is covered in this chapter:

::: {.itemizedlist}
-   Installing pytest
-   Writing and running tests
-   Organizing files and packages
-   Useful command-line options
-   Configuration: `pytest.ini`{.literal} file
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note1}Note {#note .title}

In the chapter, there are a lot of examples typed into the command line.
They are marked by the λ character. To avoid clutter and to focus on the
important parts, the pytest header (which normally displays the pytest
version, the Python version, installed plugins, and so on) will be
suppressed. 
:::

Let\'s jump right into how to install pytest.



[]{#ch02lvl1sec14}Installing pytest {#installing-pytest .title style="clear: both"}
-----------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Installing pytest is really[]{#id325091737 .indexterm} simple, but
first, let\'s take a moment to review good practices for Python
development.

 

 

 

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note2}Note {#note .title}

All of the examples are for Python 3. They should be easy to adapt to
Python 2 if necessary.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec3}pip and virtualenv {#pip-and-virtualenv .title}

</div>

</div>
:::

The recommended practice[]{#id325534025 .indexterm} for installing
dependencies is to[]{#id325534034 .indexterm} create a
`virtualenv`{.literal}. A `virtualenv`{.literal}
(<https://packaging.python.org/guides/installing-using-pip-and-virtualenv/>)
acts like a complete[]{#id325538628 .indexterm} separate Python
installation from the one that comes with your operating system, making
it safe to install the packages required by your application without
risk of breaking your system Python or tools.

Now we will learn how to create a virtual environment and install pytest
using pip. If you are already familiar with `virtualenv`{.literal} and
pip, you can skip this section:

::: {.orderedlist}
1.  Type this in your Command Prompt to create a `virtualenv`{.literal}:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ python -m venv .env
```
:::

::: {.orderedlist}
2.  This command will create a `.env`{.literal} folder in the current
    directory, containing a full-blown Python installation. Before
    proceeding, you should `activate`{.literal} the
    `virtualenv`{.literal}:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ source .env/bin/activate
```
:::

Or on Windows:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ .env\Scripts\activate
```
:::

This will put the `virtualenv`{.literal} Python in front of the
`$PATH`{.literal} environment variable, so Python, pip, and other tools
will be executed from the `virtualenv`{.literal}, not from your system.

::: {.orderedlist}
3.  Finally, to install pytest, type:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pip install pytest
```
:::

You can verify that everything went well by typing:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --version
This is pytest version 3.5.1, imported from x:\fibo\.env36\lib\site-packages\pytest.py
```
:::

 

 

 

 

Now, we are all set up and can begin!



[]{#ch02lvl1sec15}Writing and running tests {#writing-and-running-tests .title style="clear: both"}
-------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Using pytest, all you need to do to start[]{#id324673415 .indexterm}
writing tests is to create a new file named `test_*.py`{.literal} and
write test functions that start[]{#id324673427 .indexterm} with
`test`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    # contents of test_player_mechanics.py
    def test_player_hit():
        player = create_player()
        assert player.health == 100
        undead = create_undead()
        undead.hit(player)
        assert player.health == 80
```
:::

To execute this test, simply execute `pytest`{.literal}, passing the
name of the file:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest test_player_mechanics.py
```
:::

If you don\'t pass anything, pytest will look for all of the test files
from the current directory recursively and execute them automatically.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note3}Note {#note .title}

You might encounter examples on the internet that use
`py.test`{.literal} in the command line instead of `pytest`{.literal}.
The reason for that is historical: pytest used to be part of the
`py`{.literal} package, which provided several general purpose
utilities, including tools that followed the convention of starting with
`py.<TAB>`{.literal} for tab completion, but since then, it has been
moved into its own project. The old `py.test`{.literal} command is still
available and is an alias to `pytest`{.literal}, but the latter is the
recommended modern usage.
:::

Note that there\'s no need to create classes; just simple functions and
plain `assert`{.literal} statements are enough, but if you want to use
classes to group tests you can do so:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    class TestMechanics:

        def test_player_hit(self):
            ...

        def test_player_health_flask(self):
            ...
```
:::

 

 

 

 

 

 

Grouping tests can be useful when you want to put a number of tests
under the same scope: you can execute tests based on the class they are
in, apply markers to all of the tests in a class ([Chapter
3](https://subscription.packtpub.com/book/web_development/9781789347562/3){.link},
[*Markers and Parametrization*]{.emphasis}), and create fixtures bound
to a class ([Chapter
4](https://subscription.packtpub.com/book/web_development/9781789347562/4){.link},
[*Fixtures*]{.emphasis}).

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec4}Running tests {#running-tests .title}

</div>

</div>
:::

Pytest can run your tests[]{#id325533873 .indexterm} in a number of
ways. Let\'s quickly get into the basics now and, later on in the
chapter, we will move on to more advanced options.

You can start by just simply executing the `pytest`{.literal} command:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest
```
:::

This will find all of the `test_*.py`{.literal} and
`*_test.py`{.literal} modules in the current directory and below
recursively, and will run all of the tests found in those files:

::: {.itemizedlist}
-   You can reduce the search to specific directories:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      λ pytest tests/core tests/contrib
```
:::

::: {.itemizedlist}
-   You can also mix any number of files and directories:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      λ pytest tests/core tests/contrib/test_text_plugin.py
```
:::

::: {.itemizedlist}
-   You can execute specific tests by using the
    syntax `<test-file>::<test-function-name>`{.literal}:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      λ pytest tests/core/test_core.py::test_regex_matching
```
:::

::: {.itemizedlist}
-   You can execute all of the `test`{.literal} methods of a
    `test`{.literal} class:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      λ pytest tests/contrib/test_text_plugin.py::TestPluginHooks
```
:::

::: {.itemizedlist}
-   You can execute a specific `test`{.literal} method of a
    `test`{.literal} class using the
    syntax `<test-file>::<test-class>::<test-method-name>`{.literal}:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      λ pytest tests/contrib/
      test_text_plugin.py::TestPluginHooks::test_registration
```
:::

The syntax used above is created internally by pytest, is unique to each
test collected, and is called
a `node id`{.literal} or `item id`{.literal}[*. *]{.emphasis}It
basically consists of the filename of the testing module, class, and
functions joined together by the `::`{.literal} characters.

 

Pytest will show a more verbose output, which includes node IDs, with
the `-v`{.literal} flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
 λ pytest tests/core -v
======================== test session starts ========================
...
collected 6 items

tests\core\test_core.py::test_regex_matching PASSED            [ 16%]
tests\core\test_core.py::test_check_options FAILED             [ 33%]
tests\core\test_core.py::test_type_checking FAILED             [ 50%]
tests\core\test_parser.py::test_parse_expr PASSED              [ 66%]
tests\core\test_parser.py::test_parse_num PASSED               [ 83%]
tests\core\test_parser.py::test_parse_add PASSED               [100%]
```
:::

To see which tests there are without running them, use
the `--collect-only`{.literal}  flag:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests/core --collect-only
======================== test session starts ========================
...
collected 6 items
<Module 'tests/core/test_core.py'>
  <Function 'test_regex_matching'>
  <Function 'test_check_options'>
  <Function 'test_type_checking'>
<Module 'tests/core/test_parser.py'>
  <Function 'test_parse_expr'>
  <Function 'test_parse_num'>
  <Function 'test_parse_add'>

=================== no tests ran in 0.01 seconds ====================
```
:::

`--collect-only`{.literal} is especially useful if you want to execute a
specific test but can\'t remember its exact name.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec5}Powerful asserts {#powerful-asserts .title}

</div>

</div>
:::

As you\'ve probably already noticed, pytest makes use of the built-in
`assert`{.literal} statement to check assumptions during testing.
Contrary to other frameworks, you don\'t need[]{#id325091907 .indexterm}
to remember various `self.assert*`{.literal} or `self.expect*`{.literal}
functions. While this may not seem like a big deal at first, after
spending some time using plain asserts, you will realize how much that
makes writing tests more enjoyable and natural.

 

Again, here\'s an example of a failure:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
________________________ test_default_health ________________________

    def test_default_health():
        health = get_default_health('warrior')
>       assert health == 95
E       assert 80 == 95

tests\test_assert_demo.py:25: AssertionError
```
:::

Pytest shows the line of the failure, as well as the variables and
expressions involved in the failure. By itself, this would be pretty
cool already, but pytest goes a step further and provides specialized
explanations of failures involving other data types.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec0}Text differences {#text-differences .title}

</div>

</div>
:::

When showing the explanation for[]{#id325091979 .indexterm} short
strings, pytest uses a simple difference method:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
_____________________ test_default_player_class _____________________

    def test_default_player_class():
        x = get_default_player_class()
>       assert x == 'sorcerer'
E       AssertionError: assert 'warrior' == 'sorcerer'
E         - warrior
E         + sorcerer
```
:::

Longer strings show a smarter delta, using `difflib.ndiff`{.literal} to
quickly spot the differences:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
__________________ test_warrior_short_description ___________________

    def test_warrior_short_description():
        desc = get_short_class_description('warrior')
>       assert desc == 'A battle-hardened veteran, can equip heavy armor and weapons.'
E       AssertionError: assert 'A battle-har... and weapons.' == 'A battle-hard... and weapons.'
E         - A battle-hardened veteran, favors heavy armor and weapons.
E         ?                            ^ ^^^^
E         + A battle-hardened veteran, can equip heavy armor and weapons.
E         ?                            ^ ^^^^^^^
```
:::

Multiline strings are also treated specially:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    def test_warrior_long_description():
        desc = get_long_class_description('warrior')
>       assert desc == textwrap.dedent('''\
            A seasoned veteran of many battles. Strength and Dexterity
            allow to yield heavy armor and weapons, as well as carry
            more equipment. Weak in magic.
            ''')
E       AssertionError: assert 'A seasoned v... \n' == 'A seasoned ve... \n'
E         - A seasoned veteran of many battles. High Strength and Dexterity
E         ?                                     -----
E         + A seasoned veteran of many battles. Strength and Dexterity
E           allow to yield heavy armor and weapons, as well as carry
E         - more equipment while keeping a light roll. Weak in magic.
E         ?               ---------------------------
E         + more equipment. Weak in magic. 
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec1}Lists {#lists .title}

</div>

</div>
:::

Assertion failures for lists also show[]{#id325092090 .indexterm} only
differing items by default:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
____________________ test_get_starting_equiment _____________________

    def test_get_starting_equiment():
        expected = ['long sword', 'shield']
>       assert get_starting_equipment('warrior') == expected
E       AssertionError: assert ['long sword'...et', 'shield'] == ['long sword', 'shield']
E         At index 1 diff: 'warrior set' != 'shield'
E         Left contains more items, first extra item: 'shield'
E         Use -v to get the full diff

tests\test_assert_demo.py:71: AssertionError
```
:::

Note that pytest shows which index differs, and also that the
`-v`{.literal} flag can be used to show the complete difference between
the lists:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
____________________ test_get_starting_equiment _____________________

    def test_get_starting_equiment():
        expected = ['long sword', 'shield']
>       assert get_starting_equipment('warrior') == expected
E       AssertionError: assert ['long sword'...et', 'shield'] == ['long sword', 'shield']
E         At index 1 diff: 'warrior set' != 'shield'
E         Left contains more items, first extra item: 'shield'
E         Full diff:
E         - ['long sword', 'warrior set', 'shield']
E         ?               ---------------
E         + ['long sword', 'shield']

tests\test_assert_demo.py:71: AssertionError
```
:::

If the difference is too big, pytest is smart enough to show only a
portion to avoid showing too much output, displaying a message like the
following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
E         ...Full output truncated (100 lines hidden), use '-vv' to show
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec2}Dictionaries and sets {#dictionaries-and-sets .title}

</div>

</div>
:::

Dictionaries are probably one of the most[]{#id325092152 .indexterm}
used data structures in Python, so, unsurprisingly, pytest has
specialized representation for them:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
_______________________ test_starting_health ________________________

    def test_starting_health():
        expected = {'warrior': 85, 'sorcerer': 50}
>       assert get_classes_starting_health() == expected
E       AssertionError: assert {'knight': 95...'warrior': 85} == {'sorcerer': 50, 'warrior': 85}
E         Omitting 1 identical items, use -vv to show
E         Differing items:
E         {'sorcerer': 55} != {'sorcerer': 50}
E         Left contains more items:
E         {'knight': 95}
E         Use -v to get the full diff
```
:::

Sets also have similar output:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
________________________ test_player_classes ________________________

    def test_player_classes():
>       assert get_player_classes() == {'warrior', 'sorcerer'}
E       AssertionError: assert {'knight', 's...r', 'warrior'} == {'sorcerer', 'warrior'}
E         Extra items in the left set:
E         'knight'
E         Use -v to get the full diff
```
:::

As with lists, there are also `-v`{.literal} and `-vv`{.literal} options
for displaying more detailed output.

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec3}How does pytest do it? {#how-does-pytest-do-it .title}

</div>

</div>
:::

By default, Python\'s assert statement does[]{#id325092257 .indexterm}
not provide any details when it fails, but as we just saw, pytest shows
a lot of information about the variables and expressions involved in a
failed assertion. So how does pytest do it?

Pytest is able to provide useful exceptions because it implements a
mechanism called [*assertion rewriting*]{.emphasis}.

Assertion rewriting works by installing a custom import hook that
intercepts the standard Python import mechanism. When pytest detects
that a test file (or plugin) is about to be imported, instead of loading
the module, it first compiles the source code into an [**abstract syntax
tree**]{.strong} ([**AST**]{.strong}) using the built-in `ast`{.literal}
module. Then, it searches for any `assert`{.literal} statements and
[*rewrites*]{.emphasis} them so that the variables used[]{#id325536776
.indexterm} in the expression are kept so that they can be used to show
more helpful messages if the assertion fails. Finally, it saves the
rewritten `pyc`{.literal} file to disk for caching.

This all might seem very magical, but the process is actually simple,
deterministic, and, best of all, completely transparent.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note4}Note {#note-1 .title}

If you want more details, refer
to <http://pybites.blogspot.com.br/2011/07/behind-scenes-of-pytests-new-assertion.html>,
written by the original developer of this feature, Benjamin
Peterson. The `pytest-ast-back-to-python`{.literal} plugin shows exactly
what the AST of your test files looks like after the rewriting process.
Refer to: <https://github.com/tomviner/pytest-ast-back-to-python>.
:::
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec6}Checking exceptions: pytest.raises {#checking-exceptions-pytest.raises .title}

</div>

</div>
:::

A good API documentation will[]{#id325536812 .indexterm} clearly explain
what the purpose of each function is, its parameters, and return values.
Great API documentation also clearly explains which exceptions are
raised and when.

For that reason, testing that exceptions are raised in the appropriate
circumstances is just as important as testing the main functionality of
APIs. It is also important to make sure that exceptions contain an
appropriate and clear message to help users understand the issue.

Suppose we are writing an API for a game. This API allows programmers to
write `mods`{.literal}, which are a plugin of sorts that can change
several aspects of a game, from new textures to complete new story lines
and types of characters.

 

 

This API has a function that allows mod writers to create a new
character, and it can raise exceptions in some situations:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def create_character(name: str, class_name: str) -> Character:
    """
    Creates a new character and inserts it into the database.

    :raise InvalidCharacterNameError:
        if the character name is empty.

    :raise InvalidClassNameError:
        if the class name is invalid.

    :return: the newly created Character.
    """
    ...
```
:::

Pytest makes it easy to check that your code is raising the proper
exceptions with the `raises`{.literal} statement:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_empty_name():
    with pytest.raises(InvalidCharacterNameError):
        create_character(name='', class_name='warrior')


def test_invalid_class_name():
    with pytest.raises(InvalidClassNameError):
        create_character(name='Solaire', class_name='mage')
```
:::

`pytest.raises`{.literal} is a with-statement that ensures the exception
class passed to it will be [**raised**]{.strong} inside its
execution [**block**]{.strong}. For more details
(<https://docs.python.org/3/reference/compound_stmts.html#the-with-statement>).
Let\'s see how `create_character`{.literal} implements those checks:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def create_character(name: str, class_name: str) -> Character:
    """
    Creates a new character and inserts it into the database.
    ...
    """
    if not name:
        raise InvalidCharacterNameError('character name empty')

    if class_name not in VALID_CLASSES:
        msg = f'invalid class name: "{class_name}"'
        raise InvalidCharacterNameError(msg)
    ...
```
:::

 

 

 

 

If you are paying close attention, you probably noticed that the
copy-paste error in the preceding code should[]{#id325536910 .indexterm}
actually raise an  `InvalidClassNameError`{.literal} for the class name
check.

Executing this file:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
======================== test session starts ========================
...
collected 2 items

tests\test_checks.py .F                                        [100%]

============================= FAILURES ==============================
______________________ test_invalid_class_name ______________________

    def test_invalid_class_name():
        with pytest.raises(InvalidCharacterNameError):
>           create_character(name='Solaire', class_name='mage')

tests\test_checks.py:51:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

name = 'Solaire', class_name = 'mage'

    def create_character(name: str, class_name: str) -> Character:
        """
        Creates a new character and inserts it into the database.

        :param name: the character name.

        :param class_name: the character class name.

        :raise InvalidCharacterNameError:
            if the character name is empty.

        :raise InvalidClassNameError:
            if the class name is invalid.

        :return: the newly created Character.
        """
        if not name:
            raise InvalidCharacterNameError('character name empty')

        if class_name not in VALID_CLASSES:
            msg = f'invalid class name: "{class_name}"'
>           raise InvalidClassNameError(msg)
E           test_checks.InvalidClassNameError: invalid class name: "mage"

tests\test_checks.py:40: InvalidClassNameError
================ 1 failed, 1 passed in 0.05 seconds =================
```
:::

`test_empty_name`{.literal} passed as expected.
`test_invalid_class_name`{.literal} raised
`InvalidClassNameError`{.literal}, so the exception was not captured
by `pytest.raises`{.literal}, which failed the test (as any other
exception would).

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec4}Checking exception messages {#checking-exception-messages .title}

</div>

</div>
:::

As stated at the start of this section, APIs should[]{#id325537115
.indexterm} provide clear messages in the exceptions they raise. In the
previous examples, we only verified that the code was raising the
appropriate exception type, but not the actual message.

`pytest.raises`{.literal} can receive an optional `match`{.literal}
argument, which is a regular expression string that will be matched
against the exception message, as well as checking the exception type.
For more details, go to: <https://docs.python.org/3/howto/regex.html>.
We can use that to improve our tests even further:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_empty_name():
    with pytest.raises(InvalidCharacterNameError,
match='character name empty'):
        create_character(name='', class_name='warrior')


def test_invalid_class_name():
    with pytest.raises(InvalidClassNameError,
match='invalid class name: "mage"'):
        create_character(name='Solaire', class_name='mage')
```
:::

Simple!
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec7}Checking warnings: pytest.warns {#checking-warnings-pytest.warns .title}

</div>

</div>
:::

APIs also evolve. New and better[]{#id325537192 .indexterm} alternatives
to old functions are provided, arguments are removed, old ways of using
a certain functionality evolve into better ways, and so on.

 

API writers have to strike a balance between keeping old code working to
avoid breaking clients and providing better ways of doing things, while
all the while keeping their own API code maintainable. For this reason,
a solution often adopted is to start to issue `warnings`{.literal} when
API clients use the old behavior, in the hope that they update their
code to the new constructs. Warning messages are shown in situations
where the current usage is not wrong to warrant an exception, it just
happens that there are new and better ways of doing it. Often, warning
messages are shown during a grace period for this update to take place,
and afterward the old way is no longer supported.

Python provides the standard warnings module exactly for this purpose,
making it easy to warn developers about forthcoming changes in APIs. For
more details, go to: <https://docs.python.org/3/library/warnings.html>.
It lets you choose from a number of warning classes, for example:

::: {.itemizedlist}
-   `UserWarning`{.literal}: user warnings (`user`{.literal} here means
    developers, not software users)
-   `DeprecationWarning`{.literal}: features that will be removed in the
    future
-   `ResourcesWarning`{.literal}: related to resource usage
:::

(This list is not exhaustive. Consult the warnings documentation for the
full listing. For more details, go
to: <https://docs.python.org/3/library/warnings.html>[).](https://docs.python.org/3/library/warnings.html){.ulink}

Warning classes help users control which warnings should be shown and
which ones should be suppressed.

For example, suppose an API for a computer game provides this handy
function to obtain the starting hit points of player characters given
their class name:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def get_initial_hit_points(player_class: str) -> int:
    ...
```
:::

Time moves forward and the developers decide to use an `enum`{.literal}
instead of class names in the next release. For more details, go to:
<https://docs.python.org/3/library/enum.html>, which is more adequate to
represent a limited set of values:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
class PlayerClass(Enum):
    WARRIOR = 1
    KNIGHT = 2
    SORCERER = 3
    CLERIC = 4
```
:::

 

 

 

 

 

 

 

 

 

 

 

 

 

 

But changing this suddenly would break all clients, so they wisely
decide to support both forms for the next release: `str`{.literal} and
the `PlayerClass`{.literal}`enum`{.literal}. They don\'t want to keep
supporting this forever, so they start showing a warning whenever a
class is passed as a `str`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def get_initial_hit_points(player_class: Union[PlayerClass, str]) -> int:
    if isinstance(player_class, str):
        msg = 'Using player_class as str has been deprecated' \
              'and will be removed in the future'
warnings.warn(DeprecationWarning(msg))
        player_class = get_player_enum_from_string(player_class)
    ...
```
:::

In the same vein as `pytest.raises`{.literal} from the previous section,
the `pytest.warns`{.literal} function lets you test whether your API
code is producing the warnings you expect:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_get_initial_hit_points_warning():
    with pytest.warns(DeprecationWarning):
        get_initial_hit_points('warrior')
```
:::

As with `pytest.raises`{.literal}, `pytest.warns`{.literal} can receive
an optional `match`{.literal} argument, which is a regular expression
string. For more details, go
to:<https://docs.python.org/3/howto/regex.html>, which will be matched
against the exception message:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_get_initial_hit_points_warning():
    with pytest.warns(DeprecationWarning,
match='.*str has been deprecated.*'):
        get_initial_hit_points('warrior')
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec8}Comparing floating point numbers: pytest.approx {#comparing-floating-point-numbers-pytest.approx .title}

</div>

</div>
:::

Comparing floating point numbers can be tricky. For more details, go to:
<https://docs.python.org/3/tutorial/floatingpoint.html>. Numbers that we
consider equal in the real world[]{#id325540093 .indexterm} are not so
when represented by computer hardware:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
>>> 0.1 + 0.2 == 0.3
False
```
:::

When writing tests, it is very common to compare the results produced by
our code against what we expect as floating point values. As shown
above, a simple `==`{.literal} comparison often won\'t be sufficient. A
common approach is to use a known tolerance instead and use
`abs`{.literal} to correctly deal with negative numbers:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_simple_math():
    assert abs(0.1 + 0.2) - 0.3 < 0.0001
```
:::

But besides being ugly and hard to understand, it is sometimes difficult
to come up with a tolerance that works in most situations. The chosen
tolerance of `0.0001`{.literal} might work for the numbers above, but
not for very large numbers or very small ones. Depending on the
computation performed, you would need to find a suitable tolerance for
every set of input numbers, which is tedious and error-prone.

`pytest.approx`{.literal} solves this problem by automatically choosing
a tolerance appropriate for the values involved in the expression,
providing a very nice syntax to boot:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_approx_simple():
    assert 0.1 + 0.2 == approx(0.3)
```
:::

You can read the above as
`assert that 0.1 + 0.2 equals approximately to 0.3`{.literal}.

But the  `approx`{.literal} function does not stop there; it can be used
to compare:

::: {.itemizedlist}
-   Sequences of numbers:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      def test_approx_list():
          assert [0.1 + 1.2, 0.2 + 0.8] == approx([1.3, 1.0])
```
:::

::: {.itemizedlist}
-   Dictionary `values`{.literal} (not keys):
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      def test_approx_dict():
          values = {'v1': 0.1 + 1.2, 'v2': 0.2 + 0.8}
          assert values == approx(dict(v1=1.3, v2=1.0))
```
:::

::: {.itemizedlist}
-   `numpy`{.literal} arrays:
:::

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
      def test_approx_numpy():
          import numpy as np
          values = np.array([0.1, 0.2]) + np.array([1.2, 0.8])
          assert values == approx(np.array([1.3, 1.0]))
```
:::

When a test fails, `approx`{.literal} provides a nice error message
displaying the values that failed and the tolerance used:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
    def test_approx_simple_fail():
>       assert 0.1 + 0.2 == approx(0.35)
E       assert (0.1 + 0.2) == 0.35 ± 3.5e-07
E        + where 0.35 ± 3.5e-07 = approx(0.35)
```




[]{#ch02lvl1sec16}Organizing files and packages {#organizing-files-and-packages .title style="clear: both"}
-----------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Pytest needs to import your code[]{#id325091737 .indexterm} and test
modules, and it is up to you how to organize them. Pytest supports two
common test[]{#id325533896 .indexterm} layouts, which we will discuss
next.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec9}Tests that accompany your code {#tests-that-accompany-your-code .title}

</div>

</div>
:::

You can place your test[]{#id325533911 .indexterm} modules together with
the code they are testing by creating a `tests`{.literal} folder next to
the modules themselves:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
setup.py
mylib/
    tests/
         __init__.py
         test_core.py
         test_utils.py    
    __init__.py
    core.py
    utils.py
```
:::

By putting the tests near the code they test, you gain the following
advantages:

::: {.itemizedlist}
-   It is easier to add new tests and test modules in this hierarchy and
    keep them in sync
-   Your tests are now part of your package, so they can be deployed and
    run in other environments
:::

The main disadvantage with this approach is that some folks don\'t like
the added package size of the extra modules, which are now packaged
together with the rest of the code, but this is usually minimal and of
little concern.

As an additional benefit, you can use the `--pyargs`{.literal} option to
specify tests using their module import path. For example:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --pyargs mylib.tests
```
:::

This will execute all test modules found under `mylib.tests`{.literal}.

 

 

 

 

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip5}Note {#note .title}

You might consider using `_tests`{.literal} for the test module names
instead of `_test`{.literal}. This makes the directory easier to find
because the leading underscore usually makes them appear at the top of
the folder hierarchy. Of course, feel free to use `tests`{.literal} or
any other name that you prefer; pytest doesn\'t care as long as the test
modules themselves are named `test_*.py`{.literal} or
`*_test.py`{.literal}.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec10}Tests separate from your code {#tests-separate-from-your-code .title}

</div>

</div>
:::

An alternative to the method[]{#id325538647 .indexterm} above is to
organize your tests in a separate directory from the main package:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
setup.py
mylib/  
    __init__.py
    core.py
    utils.py
tests/
    __init__.py
    test_core.py
    test_utils.py 
```
:::

Some people prefer this layout because:

::: {.itemizedlist}
-   It keeps library code and testing code separate
-   The testing code is not included in the source package
:::

One disadvantage of the above method is that, once you have a more
complex hierarchy, you will probably want to keep the same hierarchy
inside your tests directory, and that\'s a little harder to maintain and
keep in sync:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
mylib/  
    __init__.py
    core/
        __init__.py
        foundation.py
    contrib/
        __init__.py
        text_plugin.py
tests/
    __init__.py
    core/
        __init__.py
        test_foundation.py
    contrib/
        __init__.py
        test_text_plugin.py
```
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note6}Note {#note-1 .title}

So, which layout is the best? Both layouts have advantages and
disadvantages. Pytest itself works perfectly well with either of them,
so feel free to choose a layout that you are more comfortable with.




[]{#ch02lvl1sec17}Useful command-line options {#useful-command-line-options .title style="clear: both"}
---------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Now we will take a look[]{#id324673415 .indexterm} at command-line
options that will make you more productive in your daily work. As stated
at the beginning of the chapter, this is not a complete list of all of
the command-line features; just the ones that you will use (and love)
the most.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec11}Keyword expressions: -k {#keyword-expressions--k .title}

</div>

</div>
:::

Often, you don\'t exactly remember the[]{#id325091707 .indexterm} full
path or name of a test that you want to execute. At other times, many
tests in your suite follow a similar pattern and you want to execute all
of them because you just refactored a sensitive area of the code.

By using the `-k <EXPRESSION>`{.literal} flag (from [*keyword
expression*]{.emphasis}), you can run tests whose
`item id`{.literal} loosely matches the given expression:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -k "test_parse"
```
:::

This will execute all tests that contain the string `parse`{.literal} in
their item IDs. You can also write simple Python expressions using
Boolean operators:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -k "parse and not num"
```
:::

This will execute all tests that contain `parse`{.literal} but not
`num`{.literal} in their item IDs.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec12}Stop soon: -x, \--maxfail {#stop-soon--x---maxfail .title}

</div>

</div>
:::

When doing large-scale refactorings, you might not know beforehand how
or which tests are going to be affected. In those situations, you might
try to guess which modules will be affected[]{#id325091770 .indexterm}
and start running tests for those. But, often, you end up breaking more
tests than you initially estimated and quickly try to stop the test
session by hitting `CTRL+C`{.literal} when everything starts to fail
unexpectedly. 

 

In those situations, you might try using the `--maxfail=N`{.literal}
command-line flag, which stops the test session automatically after
`N`{.literal} failures or errors, or the shortcut `-x`{.literal}, which
equals `--maxfail=1`{.literal}.

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests/core -x
```
:::

This allows you to quickly see the first failing test and deal with the
failure. After fixing the reason for the failure, you can continue
running with `-x`{.literal} to deal with the next problem.

If you find this brilliant, you don\'t want to skip the next section!
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec13}Last failed, failed first: \--lf, \--ff {#last-failed-failed-first---lf---ff .title}

</div>

</div>
:::

Pytest always remembers tests that[]{#id325533868 .indexterm} failed in
previous sessions, and can reuse that information to skip right to the
tests that have failed previously. This is excellent news if you are
incrementally fixing a test suite after a large refactoring, as
mentioned in the previous section.

You can run the tests that failed before by passing the `--lf`{.literal}
flag (meaning last failed[*):*]{.emphasis}

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --lf tests/core
...
collected 6 items / 4 deselected
run-last-failure: rerun previous 2 failures
```
:::

When used together with `-x`{.literal} (`--maxfail=1`{.literal}) these
two flags are refactoring heaven:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -x --lf
```
:::

This lets you start executing the full suite and then pytest stops at
the first test that fails. You fix the code, and execute the same
command line again. Pytest starts right at the failed test, and goes on
if it passes (or stops again if you haven\'t yet managed to fix the code
yet). It will then stop at the next failure. Rinse and repeat until all
tests pass again.

Keep in mind that it doesn\'t matter if you execute another subset of
tests in the middle of your refactoring; pytest always remembers which
tests failed, regardless of the command-line executed.

If you have ever done a large refactoring and had to keep track of which
tests were failing so that you didn\'t waste your time running the test
suite over and over again, you will definitely appreciate this boost in
your productivity.

 

Finally, the `--ff`{.literal} flag is similar to `--lf`{.literal}, but
it will reorder your tests so the previous failures are run
[**first**]{.strong}, followed by the tests that passed or that were not
run yet:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -x --lf
======================== test session starts ========================
...
collected 6 items
run-last-failure: rerun previous 2 failures first
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec14}Output capturing: -s and \--capture {#output-capturing--s-and---capture .title}

</div>

</div>
:::

Sometimes, developers leave `print`{.literal} statements laying around
by mistake, or even on purpose, to be used later for debugging. Some
applications also may write to `stdout`{.literal}  or `stderr`{.literal}
as part of their[]{#id325091801 .indexterm} normal operation or logging.

All that output would make understanding the test suite display much
harder. For this reason, by default, pytest captures all output written
to `stdout`{.literal} and `stderr`{.literal} automatically. 

Consider this function to compute a hash of some text given to it that
has some debugging code left on it:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
import hashlib

def commit_hash(contents):
    size = len(contents)
    print('content size', size)
    hash_contents = str(size) + '\0' + contents
    result = hashlib.sha1(hash_contents.encode('UTF-8')).hexdigest()
    print(result)
    return result[:8]
```
:::

We have a very simple test for it:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
def test_commit_hash():
    contents = 'some text contents for commit'
    assert commit_hash(contents) == '0cf85793'
```
:::

When executing this test, by default, you won\'t see the output of the
`print`{.literal} calls:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests\test_digest.py
======================== test session starts ========================
...

tests\test_digest.py .                                         [100%]

===================== 1 passed in 0.03 seconds ======================
```
:::

 

 

 

 

That\'s nice and clean.

But those print statements are there to help you understand and debug
the code, which is why pytest will show the captured output if the
test [**fails**]{.strong}.

Let\'s change the contents of the hashed text but not the hash itself.
Now, pytest will show the captured output in a separate section after
the error traceback:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests\test_digest.py
======================== test session starts ========================
...

tests\test_digest.py F                                         [100%]

============================= FAILURES ==============================
_________________________ test_commit_hash __________________________

    def test_commit_hash():
        contents = 'a new text emerges!'
>       assert commit_hash(contents) == '0cf85793'
E       AssertionError: assert '383aa486' == '0cf85793'
E         - 383aa486
E         + 0cf85793

tests\test_digest.py:15: AssertionError
----------------------- Captured stdout call ------------------------
content size 19
383aa48666ab84296a573d1f798fff3b0b176ae8
===================== 1 failed in 0.05 seconds ======================
```
:::

Showing the captured output on failing tests is very handy when running
tests locally, and even more so when running tests on CI.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec5}Disabling capturing with -s {#disabling-capturing-with--s .title}

</div>

</div>
:::

While running your tests locally, you might[]{#id325092004 .indexterm}
want to disable output capturing to see what messages are being printed
in real-time, or whether the capturing is interfering with other
capturing your code might be doing.

In those cases, just pass `-s`{.literal} to pytest to completely disable
capturing:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests\test_digest.py -s
======================== test session starts ========================
...

tests\test_digest.py content size 29
0cf857938e0b4a1b3fdd41d424ae97d0caeab166
.

===================== 1 passed in 0.02 seconds ======================
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec6}Capture methods with \--capture {#capture-methods-with---capture .title}

</div>

</div>
:::

Pytest has two methods to capture[]{#id325092091 .indexterm} output.
Which method is used can be chosen with the `--capture`{.literal}
command-line flag:

::: {.itemizedlist}
-   `--capture=fd`{.literal}: captures output[]{#id325092110 .indexterm}
    at the [**file-descriptor level**]{.strong}, which means that all
    output written to the file descriptors, 1 (stdout) and 2 (stderr),
    is captured. This will capture output even from C extensions and is
    the default.
-   `--capture=sys`{.literal}: captures output written directly to
    `sys.stdout`{.literal} and `sys.stderr`{.literal} at the Python
    level, without trying to capture system-level file descriptors.
:::

Usually, you don\'t need to change this, but in a few corner cases,
depending on what your code is doing, changing the capture method might
be useful.

For completeness, there\'s also `--capture=no`{.literal}, which is the
same as `-s`{.literal}.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec15}Traceback modes and locals: \--tb, \--showlocals {#traceback-modes-and-locals---tb---showlocals .title}

</div>

</div>
:::

Pytest will show a complete[]{#id325092150 .indexterm} traceback of a
failing test, as expected from a testing framework. However, by default,
it doesn\'t show the standard traceback that most Python programmers are
used to; it shows a different traceback:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
============================= FAILURES ==============================
_______________________ test_read_properties ________________________

 def test_read_properties():
 lines = DATA.strip().splitlines()
> grids = list(iter_grids_from_csv(lines))

tests\test_read_properties.py:32:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
tests\test_read_properties.py:27: in iter_grids_from_csv
 yield parse_grid_data(fields)
tests\test_read_properties.py:21: in parse_grid_data
 active_cells=convert_size(fields[2]),
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

s = 'NULL'

 def convert_size(s):
> return int(s)
E ValueError: invalid literal for int() with base 10: 'NULL'

tests\test_read_properties.py:14: ValueError
===================== 1 failed in 0.05 seconds ======================
```
:::

This traceback shows only a single line of code and file location for
all frames in the traceback stack, except for the first and last one,
where a portion of the code is shown as well (in bold).

While some might find it strange at first, once you get used to it you
realize that it makes spotting the cause of the error much simpler. By
looking at the surrounding code of the start and end of the traceback,
you can usually understand the error better. I suggest that you try to
get used to the default traceback provided by pytest for a few weeks;
I\'m sure you will love it and never look back.

If you don\'t like pytest\'s default traceback, however, there are other
traceback modes, which are controlled by the `--tb`{.literal} flag. The
default is `--tb=auto`{.literal} and was shown previously. Let\'s have a
look at an overview of the other modes in the next sections.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec7}\--tb=long {#tblong .title}

</div>

</div>
:::

This mode will show a [**portion of the code
for**]{.strong}[**all**]{.strong}[**frames**]{.strong} of failure
tracebacks, making it quite[]{#id325092305 .indexterm} verbose: 

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
============================= FAILURES ==============================
_______________________ t________

    def test_read_properties():
        lines = DATA.strip().splitlines()
>       grids = list(iter_grids_from_csv(lines))

tests\test_read_properties.py:32:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

lines = ['Main Grid,48,44', '2nd Grid,24,21', '3rd Grid,24,null']

    def iter_grids_from_csv(lines):
        for fields in csv.reader(lines):
>       yield parse_grid_data(fields)

tests\test_read_properties.py:27:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

fields = ['3rd Grid', '24', 'null']

    def parse_grid_data(fields):
        return GridData(
            name=str(fields[0]),
            total_cells=convert_size(fields[1]),
>       active_cells=convert_size(fields[2]),
        )

tests\test_read_properties.py:21:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

s = 'null'

    def convert_size(s):
>       return int(s)
E       ValueError: invalid literal for int() with base 10: 'null'

tests\test_read_properties.py:14: ValueError
===================== 1 failed in 0.05 seconds ======================
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec8}\--tb=short {#tbshort .title}

</div>

</div>
:::

This mode will show a single line[]{#id325536928 .indexterm} of code
from all the frames of the failure traceback, providing short and
concise output:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
============================= FAILURES ==============================
_______________________ test_read_properties ________________________
tests\test_read_properties.py:32: in test_read_properties
    grids = list(iter_grids_from_csv(lines))
tests\test_read_properties.py:27: in iter_grids_from_csv
    yield parse_grid_data(fields)
tests\test_read_properties.py:21: in parse_grid_data
    active_cells=convert_size(fields[2]),
tests\test_read_properties.py:14: in convert_size
    return int(s)
E   ValueError: invalid literal for int() with base 10: 'null'
===================== 1 failed in 0.04 seconds ======================
```
:::

 

 

 

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec9}\--tb=native {#tbnative .title}

</div>

</div>
:::

This mode will output the exact[]{#id325537044 .indexterm} same
traceback normally used by Python to report exceptions and is loved by
purists:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
_______________________ test_read_properties ________________________
Traceback (most recent call last):
  File "X:\CH2\tests\test_read_properties.py", line 32, in test_read_properties
    grids = list(iter_grids_from_csv(lines))
  File "X:\CH2\tests\test_read_properties.py", line 27, in iter_grids_from_csv
    yield parse_grid_data(fields)
  File "X:\CH2\tests\test_read_properties.py", line 21, in parse_grid_data
    active_cells=convert_size(fields[2]),
  File "X:\CH2\tests\test_read_properties.py", line 14, in convert_size
    return int(s)
ValueError: invalid literal for int() with base 10: 'null'
===================== 1 failed in 0.03 seconds ======================
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec10}\--tb=line {#tbline .title}

</div>

</div>
:::

This mode will output a single[]{#id325537181 .indexterm} line per
failing test, showing only the exception message and the file location
of the error:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
============================= FAILURES ==============================
X:\CH2\tests\test_read_properties.py:14: ValueError: invalid literal for int() with base 10: 'null'
```
:::

This mode might be useful if you are[]{#id325537203 .indexterm} doing a
massive refactoring and except a ton of failures anyway, planning to
enter [**refactoring-heaven mode**]{.strong} with
the `--lf -x`{.literal} flags afterwards.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec11}\--tb=no {#tbno .title}

</div>

</div>
:::

This does not show any[]{#id325537258 .indexterm} traceback or failure
message at all, making it also useful to run the suite first to get a
glimpse of how many failures there are, so that you can start using
`--lf -x`{.literal} flags to fix tests step-wise:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
tests\test_read_properties.py F                                [100%]

===================== 1 failed in 0.04 seconds ======================
```
:::

 

 

 

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

#### []{#ch02lvl3sec12}\--showlocals (-l) {#showlocals--l .title}

</div>

</div>
:::

Finally, while this is not a traceback[]{#id325537300 .indexterm} mode
flag specifically, `--showlocals`{.literal} (or `-l`{.literal} as
shortcut) augments the traceback modes by showing a list of the [**local
variables and their values**]{.strong} when using `--tb=auto`{.literal},
`--tb=long`{.literal}, and `--tb=short`{.literal} modes.

For example, here\'s the output of `--tb=auto`{.literal} and
`--showlocals`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
_______________________ test_read_properties ________________________

    def test_read_properties():
        lines = DATA.strip().splitlines()
>       grids = list(iter_grids_from_csv(lines))

lines      = ['Main Grid,48,44', '2nd Grid,24,21', '3rd Grid,24,null']

tests\test_read_properties.py:32:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
tests\test_read_properties.py:27: in iter_grids_from_csv
    yield parse_grid_data(fields)
tests\test_read_properties.py:21: in parse_grid_data
    active_cells=convert_size(fields[2]),
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

s = 'null'

    def convert_size(s):
>       return int(s)
E       ValueError: invalid literal for int() with base 10: 'null'

s          = 'null'

tests\test_read_properties.py:14: ValueError
===================== 1 failed in 0.05 seconds ======================
```
:::

Notice how this makes it much easier to see where the bad data is coming
from: the `'3rd Grid,24,null'`{.literal} string that is being read from
a file at the start of the test.

`--showlocals`{.literal} is extremely useful both when running your
tests locally and in CI, being a firm favorite. Be careful, though, as
this might be a security risk: local variables might expose passwords
and other sensitive information, so make sure to transfer tracebacks
using secure connections and be careful to make them public.

 

 
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec16}Slow tests with \--durations {#slow-tests-with---durations .title}

</div>

</div>
:::

At the start of a project,  your test suite[]{#id325540141 .indexterm}
is usually blazingly fast, running in a few seconds, and life is good.
But as projects grow in size, so do their test suites, both in the
number of tests and the time it takes for them to run.

Having a slow test suite affects productivity, especially if you follow
TDD and run tests all the time. For this reason, it is healthy to
periodically take a look at your longest running tests and perhaps
analyze whether they can be made faster: perhaps you are using a large
dataset in a place where a much smaller (and faster) dataset would do,
or you might be executing redundant steps that are not important for the
actual test being done. 

When that happens, you will love the `--durations=N`{.literal} flag.
This flag provides a summary of the `N`{.literal} longest running tests,
or uses zero to see a summary of all tests:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --durations=5
...
===================== slowest 5 test durations ======================
3.40s call CH2/tests/test_slow.py::test_corner_case
2.00s call CH2/tests/test_slow.py::test_parse_large_file
0.00s call CH2/tests/core/test_core.py::test_type_checking
0.00s teardown CH2/tests/core/test_parser.py::test_parse_expr
0.00s call CH2/tests/test_digest.py::test_commit_hash
================ 3 failed, 7 passed in 5.51 seconds =================
```
:::

This output provides invaluable information when you start hunting for
tests to speed up.

Although this flag is not something that you will use daily, because it
seems that many people don\'t know about it, it is worth mentioning.
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec17}Extra test summary: -ra {#extra-test-summary--ra .title}

</div>

</div>
:::

Pytest shows rich traceback[]{#id325540213 .indexterm} information on
failing tests. The extra information is great, but the actual footer is
not very helpful in identifying which tests have actually failed:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
...
________________________ test_type_checking _________________________

    def test_type_checking():
>       assert 0
E       assert 0

tests\core\test_core.py:12: AssertionError
=============== 14 failed, 17 passed in 5.68 seconds ================
```
:::

 

 

 

 

The `-ra`{.literal} flag can be passed to produce a nice summary with
the full name of all failing tests at the end of the session:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
...
________________________ test_type_checking _________________________

    def test_type_checking():
>       assert 0
E       assert 0

tests\core\test_core.py:12: AssertionError
====================== short test summary info ======================
FAIL tests\test_assert_demo.py::test_approx_simple_fail
FAIL tests\test_assert_demo.py::test_approx_list_fail
FAIL tests\test_assert_demo.py::test_default_health
FAIL tests\test_assert_demo.py::test_default_player_class
FAIL tests\test_assert_demo.py::test_warrior_short_description
FAIL tests\test_assert_demo.py::test_warrior_long_description
FAIL tests\test_assert_demo.py::test_get_starting_equiment
FAIL tests\test_assert_demo.py::test_long_list
FAIL tests\test_assert_demo.py::test_starting_health
FAIL tests\test_assert_demo.py::test_player_classes
FAIL tests\test_checks.py::test_invalid_class_name
FAIL tests\test_read_properties.py::test_read_properties
FAIL tests\core\test_core.py::test_check_options
FAIL tests\core\test_core.py::test_type_checking
=============== 14 failed, 17 passed in 5.68 seconds ================
```
:::

This flag is particularly useful when running the suite from the command
line directly, because scrolling the terminal to find out which tests
failed can be annoying.

The flag is actually `-r`{.literal}, which accepts a number of
single-character arguments:

::: {.itemizedlist}
-   `f`{.literal} (failed): `assert`{.literal} failed
-   `e`{.literal} (error): raised an unexpected exception
-   `s`{.literal} (skipped): skipped (we will get to this in the next
    chapter)
-   `x`{.literal} (xfailed): expected to fail, did fail (we will get to
    this in the next chapter)
-   `X`{.literal} (xpassed): expected to fail, but passed (!) (we will
    get to this in the next chapter)
-   `p`{.literal} (passed): test passed
-   `P`{.literal} (passed with output): displays captured output even
    for passing tests (careful -- this usually produces a lot of output)
-   `a`{.literal}: shows all the above, except for `P`{.literal}; this
    is the [**default**]{.strong} and is usually the most useful.
:::

 

The flag can receive any combination of the above. So, for example, if
you are interested in failures and errors only, you can pass
`-rfe`{.literal} to pytest.

In general, I recommend sticking with `-ra`{.literal}, without thinking
too much about it and you will obtain the most benefits.

::: {.cc-window .cc-banner .cc-type-info .cc-theme-classic .cc-bottom .cc-color-override-637850434 .cc-invisible role="dialog" aria-live="polite" aria-label="cookieconsent" aria-describedby="cookieconsent:desc" style="display: none;"}
[This website uses cookies and other tracking technology to analyse
traffic, personalise ads and learn how we can improve the experience for
our visitors and customers. We may also share information with trusted
third-party providers. For an optimal-browsing experience please click
\'Accept\'. [Learn
more](https://www.packtpub.com/about/cookie-policy){.cc-link}]{#cookieconsent:desc
.cc-message}

::: {.cc-compliance}
[Accept]{.cc-btn .cc-dismiss}
:::
:::

![](https://www.facebook.com/tr?id=445429252334850&ev=PageView&noscript=1){width="1"
height="1"}

::: {.container}
::: {.navbar-header}
[Toggle navigation]{.sr-only} MENU

[Toggle account]{.sr-only}

[Toggle search]{.sr-only}

Search

::: {.navbar-brand .ng-scope ng-if="!$ctrl.canGoBack()"}
![](6_files/e402412b40b3af8e306f122a9f53ea5d.png)
:::
:::

::: {#main-nav .main-nav .dropdown-closed ng-class="{
                'dropdown-closed': !$ctrl.isOpen.hamburger,
                'dropdown-open': $ctrl.isOpen.hamburger,
            }" ng-show="!$root.inCheckout"}
::: {.form-group .relative-parent}
Search
:::

[ Browse []{.caret} ]{.dropdown-toggle
ng-click="$ctrl.toggleMenu($event, 'browse', true)" role="button"
aria-haspopup="true" aria-expanded="false"}

Web Development

::: {.navbar__browse-submenu .open ng-class="{
                            'open': technologyName !== 'View All Technologies'
                                && $ctrl.submenus[technologyName].open
                            }"}
-   Books
-   [JavaScript](https://subscription.packtpub.com/search?query=JavaScript&products=Book&category=Web%20Development){.ng-binding}
-   [Angular](https://subscription.packtpub.com/search?query=Angular&products=Book&category=Web%20Development){.ng-binding}
-   [React](https://subscription.packtpub.com/search?query=React&products=Book&category=Web%20Development){.ng-binding}
-   [Node.js](https://subscription.packtpub.com/search?query=Node.js&products=Book&category=Web%20Development){.ng-binding}
-   [Django](https://subscription.packtpub.com/search?query=Django&products=Book){.ng-binding}
-   [View all
    Books \>](https://subscription.packtpub.com/search?products=Book&category=Web%20Development){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Videos
-   [React](https://subscription.packtpub.com/search?query=React&products=Video&category=Web%20Development){.ng-binding}
-   [Angular](https://subscription.packtpub.com/search?query=Angular&products=Video&category=Web%20Development){.ng-binding}
-   [Vue](https://subscription.packtpub.com/search?query=Vue&products=Video&category=Web%20Development){.ng-binding}
-   [Flask](https://subscription.packtpub.com/search?query=Flask&products=Video&category=Web%20Development){.ng-binding}
-   [Node.js](https://subscription.packtpub.com/search?query=Node.js&products=Video&category=Web%20Development){.ng-binding}
-   [View all
    Videos \>](https://subscription.packtpub.com/search?products=Video&category=Web%20Development){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Paths
-   [Getting Started with
    JavaScript](https://subscription.packtpub.com/learning-paths/getting-started-with-javascript){.ng-binding}
-   [Getting Started with
    Angular](https://subscription.packtpub.com/learning-paths/getting-started-with-angular){.ng-binding}
-   [Getting Started with
    React](https://subscription.packtpub.com/learning-paths/getting-started-with-react){.ng-binding}
-   [View all
    Paths \>](https://subscription.packtpub.com/search?products=Learning%20Path&category=Web%20Development){.link
    .link-primary .ng-binding .ng-scope}
:::

Data

::: {.navbar__browse-submenu ng-class="{
                            'open': technologyName !== 'View All Technologies'
                                && $ctrl.submenus[technologyName].open
                            }"}
-   Books
-   [Python](https://subscription.packtpub.com/search?query=Python&products=Book&category=Data){.ng-binding}
-   [Data
    Science](https://subscription.packtpub.com/search?query=Data%20Science&products=Book&category=Data){.ng-binding}
-   [Machine
    Learning](https://subscription.packtpub.com/search?query=Machine%20Learning&products=Book&category=Data){.ng-binding}
-   [Big
    Data](https://subscription.packtpub.com/search?query=Big%20Data&products=Book&category=Data){.ng-binding}
-   [R](https://subscription.packtpub.com/search?query=R&products=Book&category=Data){.ng-binding}
-   [View all
    Books \>](https://subscription.packtpub.com/search?products=Book&category=Data){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Videos
-   [Python](https://subscription.packtpub.com/search?query=Python&products=Video&category=Data){.ng-binding}
-   [TensorFlow](https://subscription.packtpub.com/search?query=TensorFlow&products=Video&category=Data){.ng-binding}
-   [Machine
    Learning](https://subscription.packtpub.com/search?query=Machine%20Learning&products=Video&category=Data){.ng-binding}
-   [Deep
    Learning](https://subscription.packtpub.com/search?query=Deep%20Learning&products=Video&category=Data){.ng-binding}
-   [Data
    Science](https://subscription.packtpub.com/search?query=Data%20Science&products=Video&category=Data){.ng-binding}
-   [View all
    Videos \>](https://subscription.packtpub.com/search?products=Video&category=Data){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Paths
-   [Getting Started with Python Data
    Science](https://subscription.packtpub.com/learning-paths/getting-started-with-python-data-science){.ng-binding}
-   [Getting Started with Python Machine
    Learning](https://subscription.packtpub.com/learning-paths/getting-started-with-python-machine-learning){.ng-binding}
-   [Getting Started with
    TensorFlow](https://subscription.packtpub.com/learning-paths/getting-started-with-tensorflow){.ng-binding}
-   [View all
    Paths \>](https://subscription.packtpub.com/search?products=Learning%20Path&category=Data){.link
    .link-primary .ng-binding .ng-scope}
:::

Programming

::: {.navbar__browse-submenu ng-class="{
                            'open': technologyName !== 'View All Technologies'
                                && $ctrl.submenus[technologyName].open
                            }"}
-   Books
-   [Python](https://subscription.packtpub.com/search?query=Python&products=Book&category=Programming){.ng-binding}
-   [Go](https://subscription.packtpub.com/search?query=Go&products=Book&category=Programming){.ng-binding}
-   [Java](https://subscription.packtpub.com/search?query=Java&products=Book&category=Programming){.ng-binding}
-   [Android](https://subscription.packtpub.com/search?query=Android&products=Book){.ng-binding}
-   [C\#](https://subscription.packtpub.com/search?query=C%23&products=Book&category=Programming){.ng-binding}
-   [View all
    Books \>](https://subscription.packtpub.com/search?products=Book&category=Programming){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Videos
-   [Python](https://subscription.packtpub.com/search?query=Python&products=Video&category=Programming){.ng-binding}
-   [Java](https://subscription.packtpub.com/search?query=Java&products=Video&category=Programming){.ng-binding}
-   [Flutter](https://subscription.packtpub.com/search?query=Flutter&products=Video){.ng-binding}
-   [Spring](https://subscription.packtpub.com/search?query=Spring&products=Video&category=Programming){.ng-binding}
-   [Git](https://subscription.packtpub.com/search?query=Git&products=Video&category=Programming){.ng-binding}
-   [View all
    Videos \>](https://subscription.packtpub.com/search?products=Video&category=Programming){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Paths
-   [Getting Started with
    Python](https://subscription.packtpub.com/learning-paths/getting-started-with-python){.ng-binding}
-   [Improving your Python
    Skills](https://subscription.packtpub.com/learning-paths/improving-your-python-skills){.ng-binding}
-   [Getting Started with
    Go](https://subscription.packtpub.com/learning-paths/getting-started-with-go){.ng-binding}
-   [View all
    Paths \>](https://subscription.packtpub.com/search?products=Learning%20Path&category=Programming){.link
    .link-primary .ng-binding .ng-scope}
:::

Cloud & Networking

::: {.navbar__browse-submenu ng-class="{
                            'open': technologyName !== 'View All Technologies'
                                && $ctrl.submenus[technologyName].open
                            }"}
-   Books
-   [Docker](https://subscription.packtpub.com/search?query=Docker&products=Book&category=Cloud%20%26%20Networking){.ng-binding}
-   [Kubernetes](https://subscription.packtpub.com/search?query=Kubernetes&products=Book&category=Cloud%20%26%20Networking){.ng-binding}
-   [Ansible](https://subscription.packtpub.com/search?query=Ansible&products=Book&category=Cloud%20%26%20Networking){.ng-binding}
-   [AWS](https://subscription.packtpub.com/search?query=AWS&products=Book&category=Cloud%20%26%20Networking){.ng-binding}
-   [Linux](https://subscription.packtpub.com/search?query=Linux&products=Book&category=Cloud%20%26%20Networking){.ng-binding}
-   [View all
    Books \>](https://subscription.packtpub.com/search?products=Book&category=Cloud%20%26%20Networking){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Videos
-   [Docker](https://subscription.packtpub.com/search?query=Docker&products=Video&category=Cloud%20%26%20Networking){.ng-binding}
-   [AWS](https://subscription.packtpub.com/search?query=AWS&products=Video&category=Cloud%20%26%20Networking){.ng-binding}
-   [Kubernetes](https://subscription.packtpub.com/search?query=Kubernetes&products=Video&category=Cloud%20%26%20Networking){.ng-binding}
-   [Linux](https://subscription.packtpub.com/search?query=Linux&products=Video&category=Cloud%20%26%20Networking){.ng-binding}
-   [Azure](https://subscription.packtpub.com/search?query=Azure&products=Video&category=Cloud%20%26%20Networking){.ng-binding}
-   [View all
    Videos \>](https://subscription.packtpub.com/search?products=Video&category=Cloud%20%26%20Networking){.link
    .link-primary .ng-binding .ng-scope}

```{=html}
<!-- -->
```
-   Paths
-   [Getting Started with
    AWS](https://subscription.packtpub.com/learning-paths/getting-started-with-aws){.ng-binding}
-   [Getting Started with
    Azure](https://subscription.packtpub.com/learning-paths/getting-started-with-azure){.ng-binding}
-   [Getting Started with
    Linux](https://subscription.packtpub.com/learning-paths/getting-started-with-linux){.ng-binding}
-   [View al

[]{#ch02lvl1sec18}Configuration: pytest.ini {#configuration-pytest.ini .title style="clear: both"}
-------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Users can customize some pytest behavior using[]{#id325536777
.indexterm} a configuration file called `pytest.ini`{.literal}. This
file is usually placed at the root of the repository and contains a
number of configuration values that are applied to all test runs for
that project. It is meant to be kept under version control and committed
with the rest of the code.

The format follows a simple ini-style format with all pytest-related
options under a `[pytest]`{.literal} section. For more details, go
to:<https://docs.python.org/3/library/configparser.html>.

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
```
:::

The location of this file also defines what pytest calls the [**root
directory**]{.strong} (`rootdir`{.literal}): if present, the directory
that contains the configuration file is considered the root directory.

The root directory is used for the following:

::: {.itemizedlist}
-   To create the tests node IDs
-   As a stable location to store information about the project (by
    pytest plugins and features)
:::

Without the configuration file, the root directory will depend on which
directory you execute pytest from and which arguments are passed (the
description of the algorithm can be found
here: <https://docs.pytest.org/en/latest/customize.html#finding-the-rootdir>).
For this reason, it is always recommended to have a
`pytest.ini`{.literal} file in all but the simplest projects, even if
empty.

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip7}Note {#note .title}

Always define a `pytest.ini`{.literal} file, even if empty.
:::

If you are using `tox`{.literal}, you can put a `[pytest]`{.literal}
section in the traditional `tox.ini`{.literal} file and it will work
just as well. For more details, go
to:<https://tox.readthedocs.io/en/latest/>:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[tox]
envlist = py27,py36
...

[pytest]
# pytest options
```
:::

This is useful to avoid cluttering your repository root with too many
files, but it is really a matter of preference.

Now, we will take a look at more common configuration options. More
options will be introduced in the coming chapters as we cover new
features.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec18}Additional command-line: addopts {#additional-command-line-addopts .title}

</div>

</div>
:::

We learned some very useful[]{#id325533920 .indexterm} command-line
options. Some of them might become personal favorites, but having to
type them all the time would be annoying.

The `addopts`{.literal} configuration option can be used instead to
always add a set of options to the command line:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
addopts=--tb=native --maxfail=10 -v
```
:::

With that configuration, typing the following:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest tests/test_core.py
```
:::

Is the same as typing:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --tb=native --max-fail=10 -v tests/test_core.py
```
:::

Note that, despite its name, `addopts`{.literal} actually inserts the
options [**before**]{.strong} other options typed in the command line.
This makes it possible to override most options in `addopts`{.literal}
when passing them in explicitly. 

For example, the following code will now
display [**auto **]{.strong}tracebacks, instead of native ones, as
configured in `pytest.ini`{.literal}:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest --tb=auto tests/test_core.py
```
:::

 

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec19}Customizing a collection {#customizing-a-collection .title}

</div>

</div>
:::

By default, pytest collects[]{#id325538646 .indexterm} tests using this
heuristic:

::: {.itemizedlist}
-   Files that match `test_*.py`{.literal} and `*_test.py`{.literal}
-   Inside test modules, functions that match `test*`{.literal} and
    classes that match `Test*`{.literal}
-   Inside test classes, methods that match `test*`{.literal}
:::

This convention is simple to understand and works for most projects, but
they can be overwritten by these configuration options:

::: {.itemizedlist}
-   `python_files`{.literal}: a list of patterns to use to collect test
    modules
-   `python_functions`{.literal}: a list of patterns to use to collect
    test functions and test methods
-   `python_classes`{.literal}: a list of patterns to use to collect
    test classes
:::

Here\'s an example of a configuration file changing the defaults:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
python_files = unittests_*.py
python_functions = check_*
python_classes = *TestSuite
```
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#tip8}Note {#note-1 .title}

The recommendation is to only use these configuration options for legacy
projects that follow a different convention, and stick with the defaults
for new projects. Using the defaults is less work and avoids confusing
other collaborators.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec20}Cache directory: cache\_dir {#cache-directory-cache_dir .title}

</div>

</div>
:::

The `--lf`{.literal} and `--ff`{.literal} options shown[]{#id325541444
.indexterm} previously are provided by an internal plugin named
`cacheprovider`{.literal}, which saves data on a directory on disk so it
can be accessed in future sessions. This directory by default is located
in the [**root directory**]{.strong} under the name
`.pytest_cache`{.literal}. This directory should never be committed to
version control.

If you would like to change the location of that directory, you can use
the `cache_dir`{.literal} option. This option also expands environment
variables automatically:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
cache_dir=$TMP/pytest-cache
```
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec21}Avoid recursing into directories: norecursedirs {#avoid-recursing-into-directories-norecursedirs .title}

</div>

</div>
:::

pytest by default will recurse over[]{#id325541522 .indexterm} all
subdirectories of the arguments given on the command line. This might
make test collection take more time than desired when recursing into
directories that never contain any tests, for example:

::: {.itemizedlist}
-   virtualenvs
-   Build artifacts
-   Documentation
-   Version control directories
:::

pytest by default tries to be smart and will not recurse inside folders
with the patterns `.*`{.literal}, `build`{.literal}, `dist`{.literal},
`CVS`{.literal}, `_darcs`{.literal}, `{arch}`{.literal},
`*.egg`{.literal}, `venv`{.literal}. It also tries to detect virtualenvs
automatically by looking at known locations for activation scripts.

The `norecursedirs`{.literal} option can be used to override the default
list of pattern names that pytest should never recurse into:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
norecursedirs = artifacts _build docs
```
:::

You can also use the `--collect-in-virtualenv`{.literal} flag to skip
the `virtualenv`{.literal} detection.

In general, users have little need to override the defaults, but if you
find yourself adding the same directory over and over again in your
projects, consider opening an issue. For more details
(<https://github.com/pytest-dev/pytest/issues/new>).
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec22}Pick the right place by default: testpaths {#pick-the-right-place-by-default-testpaths .title}

</div>

</div>
:::

As discussed previously, a common directory structure is [*out-of-source
layout*]{.emphasis}, with tests separated from[]{#id325023584
.indexterm} the application/library code in a `tests`{.literal} or
similarly named directory. In that layout it is useful to use the
`testpaths`{.literal} configuration option:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
[pytest]
testpaths = tests
```
:::

This will tell pytest where to look for tests when no files,
directories, or node ids are given in the command line, which might
speed up test collection. Note that you can configure more than one
directory, separated by spaces.

 

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch02lvl2sec23}Override options with -o/\--override {#override-options-with--o--override .title}

</div>

</div>
:::

Finally, a little known feature is that[]{#id325536609 .indexterm} you
can override any configuration option directly in the command-line using
the `-o`{.literal} /`--override`{.literal} flags. This flag can be
passed multiple times to override more than one option:

::: {.informalexample}
::: {.toolbar .clearfix}
Copy
:::

``` {.programlisting .language-markup}
λ pytest -o python_classes=Suite -o cache_dir=$TMP/pytest-cache
```



[]{#ch02lvl1sec19}Summary {#summary .title style="clear: both"}
-------------------------

</div>

</div>

------------------------------------------------------------------------
:::

In this chapter, we covered how to use `virtualenv`{.literal} and
`pip`{.literal} to install pytest. After that, we jumped into how to
write tests, and the different ways to run them so that we can execute
just the tests we are interested in. We had an overview of how pytest
can provide rich output information for failing tests for different
built-in data types. We learned how to use `pytest.raises`{.literal} and
`pytest.warns`{.literal} to check exceptions and warnings, and
`pytest.approx`{.literal} to avoid common pitfalls when comparing
floating point numbers. Then, we briefly discussed how to organize test
files and modules in your projects. We also took a look at some of the
more useful command-line options so that we can get productive right
away. Finally, we covered how `pytest.ini`{.literal} files are used for
persistent command-line options and other configuration.

In the next chapter, we will learn how to use marks to help us skip
tests on certain platforms, how to let our test suite know when a bug is
fixed in our code or in external libraries, and how to group sets of
tests so that we can execute them selectively in the command line. After
that, we will learn how to apply the same checks to different sets of
data to avoid copying and pasting testing code.
