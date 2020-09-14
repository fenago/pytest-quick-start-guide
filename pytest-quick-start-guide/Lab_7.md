

[]{#ch07}Chapter 7. Wrapping Up {#chapter-7.-wrapping-up .title}
-------------------------------

</div>

</div>
:::

In the previous chapter, we learned a number of techniques that can be
used to convert `unittest`{.literal}-based suites to pytest, ranging
from simply starting using it as a runner, all the way to porting
complex existing functionality to a more pytest-friendly style.

This is the final chapter in this quick-start guide, and we will discuss
the following topics:

::: {.itemizedlist}
-   An overview of what we have learned
-   The pytest community
-   Next steps 
-   Final summary



[]{#ch07lvl1sec44}Overview of what we have learned {#overview-of-what-we-have-learned .title style="clear: both"}
--------------------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

The following sections will summarize what we have learned in this book.

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec73}Introduction {#introduction .title}

</div>

</div>
:::

::: {.itemizedlist}
-   You should consider writing tests as your safety net. It will make
    you more confident in your work, allow you to refactor with
    confidence, and be certain that you are not breaking other parts of
    the system.
-   A test suite is a must-have if you are [**porting a Python 2 code
    base to Python 3**]{.strong}, as any guide will tell you,
    (<https://docs.python.org/3/howto/pyporting.html#have-good-test-coverage>).
-   It is a good idea to write tests for the [**external
    APIs**]{.strong} you depend on, if they don\'t have automated tests.
-   One of the reasons pytest is a great choice for beginners is because
    it is easy to get started; write your tests using simple functions
    and `assert`{.literal} statements. 
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec74}Writing and running tests {#writing-and-running-tests .title}

</div>

</div>
:::

::: {.itemizedlist}
-   Always[]{#id325091917 .indexterm} use a [**virtual
    environment**]{.strong} to manage your packages[]{#id325091930
    .indexterm} and dependencies. This advice goes for any Python
    project.
-   pytest [**introspection features**]{.strong} make it easy to express
    your checks concisely; it is easy to compare dictionaries, text, and
    lists directly.
-   Check exceptions with `pytest.raises`{.literal} and warnings with
    `pytest.warns`{.literal}.
-   Compare floating-point numbers and arrays with
    `pytest.approx`{.literal}.
-   Test organization; you can [**inline your tests**]{.strong} with
    your application code or keep them in a separate directory.
-   Select tests with the `-k`{.literal} flag:
    `-k test_something`{.literal}.
-   Stop at the [**first failure**]{.strong} with `-x`{.literal}.
-   Remember the awesome [**refactoring duo**]{.strong}:
    `--lf -x`{.literal}.
-   Disable [**output capturing**]{.strong} with `-s`{.literal}.
-   Show the [**complete summary**]{.strong} of test failures, xfails,
    and skips with `-ra`{.literal}.
-   Use `pytest.ini`{.literal} for [**per-repository
    configuration**]{.strong}.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec75}Marks and parametrization {#marks-and-parametrization .title}

</div>

</div>
:::

::: {.itemizedlist}
-   [**Create marks**]{.strong} in test functions[]{#id325092040
    .indexterm} and classes with[]{#id325092084 .indexterm} the
    `@pytest.mark`{.literal} decorator. To apply to
    [**modules**]{.strong}, use the `pytestmark`{.literal} special
    variable.
-   Use `@pytest.mark.skipif`{.literal},
    `@pytest.mark.skip`{.literal} and
    `pytest.importorskip("module")`{.literal} to skip tests that are not
    applicable to the [**current environment**]{.strong}.
-   Use `@pytest.mark.xfail(strict=True)`{.literal} or
    `pytest.xfail("reason")`{.literal} to mark tests that are
    [**expected to fail**]{.strong}. 
-   Use `@pytest.mark.xfail(strict=False)`{.literal} to mark [**flaky
    tests**]{.strong}.
-   Use `@pytest.mark.parametrize`{.literal} to quickly test code for
    [**multiple inputs**]{.strong} and to test [**different
    implementations**]{.strong} of the same interface.
:::

 

 
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec76}Fixtures {#fixtures .title}

</div>

</div>
:::

::: {.itemizedlist}
-   [**Fixtures**]{.strong} are one of the main[]{#id325092179
    .indexterm} pytest features, used to [**share resources**]{.strong}
    and provide easy-to-use [**test helpers**]{.strong}.
-   Use `conftest.py`{.literal} files to [**share fixtures**]{.strong}
    across test modules. Remember to prefer local imports to speed up
    test collection.
-   Use [**autouse**]{.strong} fixtures to ensure every test in a
    hierarchy uses a certain fixture to perform a required setup or
    teardown action.
-   Fixtures can assume [**multiple
    scopes**]{.strong}:`function`{.literal},`class`{.literal},`module`{.literal},
    and`session`{.literal}. Use them wisely to reduce the total time of
    the test suite, keeping in mind that high-level fixture instances
    are shared between tests.
-   Fixtures can be [**parametrized**]{.strong} using the
    `params`{.literal} parameter of the `@pytest.fixture`{.literal}
    decorator. All tests that use a parametrized fixture will be
    parametrized automatically, making this a very powerful tool.
-   Use `tmpdir`{.literal} and `tmpdir_factory`{.literal} to create
    empty directories.
-   Use `monkeypatch`{.literal} to temporarily change attributes of
    objects, dictionaries, and environment variables. 
-   Use `capsys`{.literal} and `capfd`{.literal} to capture and verify
    output sent to standard out and standard error.
-   One important feature of fixtures is that they [**abstract way
    dependencies**]{.strong}, and there\'s a balance between
    using [**simple functions versus fixtures**]{.strong}. 
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec77}Plugins {#plugins .title}

</div>

</div>
:::

::: {.itemizedlist}
-   Use`plugincompat`{.literal} (<http://plugincompat.herokuapp.com/>)
    and PyPI (<https://pypi.org/>[) to
    search](https://pypi.org/){.ulink}[]{#id325092346 .indexterm} for
    new plugins.
-   Plugins are [**simple to install**]{.strong}: install with
    `pip`{.literal} and they are activated automatically.
-   There are a huge number of plugins available, for all needs.
:::
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec78}Converting unittest suites to pytest {#converting-unittest-suites-to-pytest .title}

</div>

</div>
:::

::: {.itemizedlist}
-   You can start by switching[]{#id325092378 .indexterm} to [**pytest
    as a runner**]{.strong}. Usually, this can be done[]{#id325022589
    .indexterm} with [**zero changes**]{.strong} in existing code.
-   Use `unittest2pytest`{.literal} to convert `self.assert*`{.literal}
    methods to plain `assert`{.literal}.
-   Existing [**set-up**]{.strong} and [**teardown**]{.strong} code can
    be reused with a small refactoring using [**autouse**]{.strong}
    fixtures.
-   Complex test utility [**hierarchies**]{.strong} can be refactored
    into more [**modular fixtures**]{.strong} while keeping the existing
    tests working.
-   There are a number of ways to approach migration: convert
    [**everything**]{.strong} at once, convert tests as you
    [**change**]{.strong} existing tests, or only use pytest for
    [**new**]{.strong} tests. It depends on your test-suite size and
    time budget.



[]{#ch07lvl1sec45}The pytest community {#the-pytest-community .title style="clear: both"}
--------------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

Our community lives[]{#id324673415 .indexterm} in the
`pytest-dev`{.literal} organizations on GitHub
(<https://github.com/pytest-dev>) and BitBucket
(<https://bitbucket.org/pytest-dev>). The pytest repository
(<https://github.com/pytest-dev/pytest>) itself is hosted on GitHub,
while both GitHub and Bitbucket host a number of plugins. Members strive
to make the community as welcome and friendly to new contributors as
possible, for people from all backgrounds. We also have a mailing list
on `pytest-dev@python.org`{.literal}, which everyone is welcome to join
(<https://mail.python.org/mailman/listinfo/pytest-dev>).

Most pytest-dev members reside in Western Europe, but we have members
from all around the globe, including UAE, Russia, India, and Brazil
(which is where I live).

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec79}Getting involved {#getting-involved .title}

</div>

</div>
:::

Because all pytest maintenance is[]{#id325091763 .indexterm} completely
voluntary, we are always looking for people who would like to join the
community and help out, working in good faith with others[]{#id325091841
.indexterm} towards improving pytest and its plugins. There are a number
of ways to get involved:

::: {.itemizedlist}
-   Submit feature requests; we love to hear from users about new
    features they would like to see in pytest or plugins. Make sure to
    report them as issues to start a discussion
    (<https://github.com/pytest-dev/pytest/issues>). 
-   Report bugs: if you encounter a bug, please report it. We do our
    best to fix bugs in a timely manner.
-   Update documentation; we have many open issues related to
    documentation
    (<https://github.com/pytest-dev/pytest/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22status%3A+easy%22+label%3A%22type%3A+docs%22+>).
    If you like to help others and write good documents, this is an
    excellent opportunity to help out.
-   Implement new features; although the code base might appear daunting
    for newcomers, there are a number of features or improvements marked
    with an easy label
    (<https://github.com/pytest-dev/pytest/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22status%3A+easy%22>),
    which is friendly to new contributors. Also, if you are unsure, feel
    free to ask!
-   Fix bugs; although pytest has more than 2,000 tests against itself,
    it has known bugs as any software. We are always glad to review pull
    requests for known bugs
    (<https://github.com/pytest-dev/pytest/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3A%22type%3A+bug%22>).
-   Spread your love on twitter by using the `#pytest`{.literal} hash
    tag or mentioning `@pytestdotorg`{.literal}. We also love to read
    blog posts about your experiences with pytest.
-   At many conferences, there are members of the community organizing
    workshops, sprints, or giving talks. Be sure to say hi!
:::

It is easy to become a contributor; you need only to contribute a pull
request about a relevant code change, documentation, or bug-fix, and you
can become a member of the `pytest-dev`{.literal} organization if you
wish. As a member, you can help answer, label, and close issues, and
review and merge pull requests. 

Another way to contribute is to submit new plugins to
`pytest-dev`{.literal}, either on GitHub or BitBucket. We love when new
plugins are added to the organization, as this provides more visibility
and helps share maintenance with other members.

You can read our full contribution guide on the pytest website
(<https://docs.pytest.org/en/latest/contributing.html>).
:::

::: {.section lang="en" lang="en"}
::: {.titlepage}
<div>

<div>

### []{#ch07lvl2sec80}2016 Sprint {#sprint .title}

</div>

</div>
:::

In June 2016, the core group[]{#id325092038 .indexterm} held a big
sprint in Freiburg, Germany. Over 20 participants attended, over six
days; the event was themed around implementing new features and fixing
issues. We had a ton of group discussions and lightning talks, taking a
one-day break to go hiking in the beautiful Black Forest.

The team managed to raise a successful Indiegogo campaign
(<https://www.indiegogo.com/projects/python-testing-sprint-mid-2016#/>),
aiming for US \$11,000 to reimburse travel costs, sprint venue, and
catering for the participants. In the end, we managed to raise over US
\$12,000, which shows the appreciation of users and companies that use
pytest.

It was great fun! We are sure to repeat it in the future, hopefully with
even more attendees.

 


[]{#ch07lvl1sec46}Next steps {#next-steps .title style="clear: both"}
----------------------------

</div>

</div>

------------------------------------------------------------------------
:::

After all we learned, you might be anxious to get started with pytest or
be eager to use it more frequently.

Here are a few ideas of the next steps you can take:

::: {.itemizedlist}
-   Use it at work; if you already use Python in your day job and have
    plenty of tests, that\'s the best way to start. You can start slowly
    by using pytest as a test runner, and use more pytest features at a
    pace you feel comfortable with.
-   Use it in your own open source projects: if you are a member or an
    owner of an open source project, this is a great way to get some
    pytest experience. It is better if you already have a test suite,
    but if you don\'t, certainly starting with pytest will be an
    excellent choice.
-   Contribute to open source projects; you might choose an open source
    project that has `unittest`{.literal} style tests and decide to
    offer to change it to use pytest. In April 2015, the pytest
    community organized what was called Adopt pytest month
    (<https://docs.pytest.org/en/latest/adopt.html>), where open
    source[]{#id325091717 .indexterm} projects paired up with community
    members to convert their test suites to pytest. The event was
    successful and most of those involved had a blast. This is a great
    way to get involved in another open source project and learn pytest
    at the same time.
-   Contribute to pytest itself; as mentioned in the previous section,
    the pytest community is very welcoming to new contributors. We would
    love to have you!
:::

::: {.note style="margin-left: 0.5in; margin-right: 0.5in;"}
### []{#note16}Note {#note .title}

Some topics were deliberately left out of this book, as they are
considered a little advanced for a quick start, or because we couldn\'t
fit them into the book due to space constraints. 
:::

::: {.itemizedlist}
-   tox (<https://tox.readthedocs.io/en/latest/>) is a generic virtual
    environment manager[]{#id325091869 .indexterm} and command-line tool
    that can be used to test projects with multiple Python versions and
    dependencies. It is a godsend if you maintain projects that support
    multiple Python versions and environments. pytest and
    `tox`{.literal} are brother projects and work extremely well
    together, albeit independent and useful for their own purposes.
-   Plugins: this book does not cover how to extend pytest with plugins,
    so if you are interested, be sure to check the plugins section
    (<https://docs.pytest.org/en/latest/fixture.html>) of the pytest
    documentation and look around for other plugins that can serve as an
    example. Also, be sure to checkout the examples section
    (<https://docs.pytest.org/en/latest/example/simple.html>) for
    snippets of advanced pytest customization.
-   Logging and warnings are two Python features that pytest has
    built-in support for and were not covered in detail in this book,
    but they certainly deserve a good look if you use those features
    extensively.



[]{#ch07lvl1sec47}Final summary {#final-summary .title style="clear: both"}
-------------------------------

</div>

</div>

------------------------------------------------------------------------
:::

So, we have come to the end of our quick start guide. In this book, we
had a complete overview, from using pytest on the command-line all the
way to tips and tricks to convert existing test suites to make use of
the powerful pytest features. You should now be comfortable using pytest
daily and be able to help others as needed.

You have made it this far, so congratulations! I hope you have learned
something and had fun along the way!
