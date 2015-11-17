Title: Essential python tools - Quality
Slug: essential-python-tools-quality
Date: 2015-11-11 14:35:00
Tags: tools, python
Summary: A list of very useful python quality tools
Status: Published
Schema: Article
Keywords: code, zen, pep8, flake8, pyflakes, quality, python, tools, maintainable, radon, mastool, pylint, linter

As a developer you should benefit from all the tools and utilities available to
you, whether they're within your IDE or right in your terminal, you should use
or better yet abuse them to the maximum. It's very simple to overlook the
benefits if you do not consider the impact of clean, well written and maintainable
code.

### The Zen

This poses a big problem in the python world too, especially since we as a,
dare I say _CULT_, tend to be very strict about the code we write. How many
languages do you know that actually has its ideology available as part of the
system itself?

    :::text
    % python -m this

    The Zen of Python, by Tim Peters

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    ...

You'll frequently find authors or speakers quoting lines off the above and while
they can be misconstrued as a law, they really tends to be more of guidelines to
how you should be writing your code.

### The PEP8

Coincidentally there also exists another set of guidelines pertaining to the
style you should be writing your python code in. It comes in its own Python
Enhancement Proposal (or just PEP) called [PEP8](https://www.python.org/dev/peps/pep-0008/).

Now while PEP8 is a _GUIDE_, following it to the letter makes the majority of
the code you or your peer write, quite readable to both. Always remember that
code is more read than written, so you should always take that extra step into
ensuring your code is very, very, readable.

I'd assume at first glance over PEP8, one can be overwhelmed with its content
but I assure you that you wouldn't need to _read_ that page more than once for
the most part (unless you're a freak like me that constantly links to segments
of PEP8 :-)). Fear not, there are several tools at your disposal that should
assist you with your PEP8 needs. The first notable one being `pep8`, a python
package that you can install (if not already installed) using:

    :::text
    % pip install pep8

Sample usage:

    :::text
    % pep8 code.py
    code.py:4:14: E231 missing whitespace after ','
    code.py:6:18: E231 missing whitespace after ','
    code.py:7:80: E501 line too long (81 > 79 characters)

`pep8` is able to report most PEP8 violations which you can solve manually.

You can find the official package [here](https://pypi.python.org/pypi/pep8).

### The Pyflakes

Now that we have the style guidelines out of the way, now we need to detect
as many errors statically as we possibly can. The more errors you can catch
before the code runs, the less headache you get when it does run. A neat tool
that allows us to catch errors in python source files is `Pyflakes` which you
can install using:

    :::text
    % pip install pyflakes

Sample usage:

    :::text
    % pyflakes code.py
    code.py:14: undefined name 'operate'

`Pyflakes` can detect several issues like shadowing variables in loops, undefined
names, duplicate arguments and more.

You can find the official package [here](https://pypi.python.org/pypi/pyflakes)

### The Flake8

`Flake8` wraps both tools mentioned earlier as well as a [third](https://pypi.python.org/pypi/mccabe) and provides you with a uniform way of checking your source code for issues
before running the code. You can install this package using:

    :::text
    % pip install flake8

Sample usage:

    :::text
    % flake8 code.py
    code.py:4:14: E231 missing whitespace after ','
    code.py:4:14: undefined name 'operate'
    code.py:6:18: E231 missing whitespace after ','
    code.py:7:80: E501 line too long (81 > 79 characters)

`Flake8` should be the primary check you make as soon as you write python code.
It doesn't matter if you do it manually from the command line, or configure your
text editor or IDE to do it for you, just make sure it does run!

You can find the official package [here](https://pypi.python.org/pypi/flake8)

### The Hacking

`hacking` improves upon `Flake8` by providing plug-ins that check against the
[OpenStack Style Guidelines](http://docs.openstack.org/developer/hacking/). Given
OpenStack's primary language is Python and contains over a million lines of code,
the need for an extensive guideline is a must. You can install this package using:

    :::text
    % pip install hacking

You can find the official package [here](https://pypi.python.org/pypi/hacking)

### The Pylint

`Pylint` is your personal code quality dictating machine! it checks against a
huge list of [violations](http://pylint-messages.wikidot.com/all-messages) and
provides you with a summary all the while keeping track of your sequence runs,
showing you if the code improved and by what factor. Obviously each violation
has a different weight and if (and most probably will) you wish to suppress a
specific warning or error code, you can configure `Pylint` to do so. You can install
`Pylint` using:

    :::text
    % pip install pylint

Sample usage:

    :::text
    % pylint code.py
    ************* Module code
    C:  4, 0: Exactly one space required after comma
            for j,row in enumerate(array):
                 ^ (bad-whitespace)
    C:  6, 0: Exactly one space required after comma
                for i,column in enumerate(row):
                     ^ (bad-whitespace)
    C: 19, 0: No space allowed before bracket
                sum(array[lmi(j-1)   ][lmi(i-1):lma(i+2, w)]),
                                     ^ (bad-whitespace)
    C: 20, 0: No space allowed before bracket
                sum(array[j          ][lmi(i-1):lma(i+2, w)]),
                                     ^ (bad-whitespace)
    C: 23, 0: Exactly one space required after comma
            return 1 if (alive and living_cells in (2,3)) or \
                                                     ^ (bad-whitespace)
    C: 27, 0: Trailing whitespace (trailing-whitespace)
    C:  1, 0: Missing module docstring (missing-docstring)
    C:  1, 0: Missing class docstring (missing-docstring)
    C:  2, 4: Missing method docstring (missing-docstring)
    W:  6,18: Unused variable 'column' (unused-variable)
    C: 13, 4: Invalid argument name "h" (invalid-name)
    C: 13, 4: Invalid argument name "w" (invalid-name)
    C: 13, 4: Missing method docstring (missing-docstring)
    R: 13, 4: Too many arguments (6/5) (too-many-arguments)
    W: 14, 8: Statement seems to have no effect (pointless-statement)
    E: 14, 8: Undefined variable 'a' (undefined-variable)
    R: 13, 4: Method could be a function (no-self-use)


You can find the official package [here](https://pypi.python.org/pypi/pylint)

### The Radon

`Radon` is a tool that measures various metrics by inspecting your source code.
It's capable of providing raw metrics, maintainability metrics, cyclomatic
complexity and more. You can review the details of these measurements on
[radon's official documentation site](https://radon.readthedocs.org/en/latest/intro.html).
You can install this tool using:

    :::text
    % pip install radon

Sample usage:

    :::text
    % radon cc code.py
    code.py
        M 13:4 Operation.get_successor - B
        C 1:0 Operation - A
        M 2:4 Operation.next_gen - A
        M 8:10 Process.next - C
        C 20:0 Process- C

I prefer maintaining a grade of `B+` for the majority of the code I write though
to be fair, if you're already abusing the tools mentioned previously, you're well
on your way in achieving `A` grade code.

You can find the official package [here](https://pypi.python.org/pypi/radon)

### The Mastool

This would be a personal plug. `Mastool` is a static analysis checker that I've
written after witnessing a bundle of easily avoidable practices in python. It's
still in its early stage of development, but it's capable of reporting a decent
amount of violations (albeit in some context these violations can be permitted).
The tool can check against built-in assignments, silent generic exceptions and
other. You can install this tool using:

    :::text
    % pip install mastool

Sample usage:

    :::text
    % mastool code.py
    code.py:14: AssignToBuiltin
    code.py:30: SilentGenericException

You can find the official package [here](https://pypi.python.org/pypi/mastool)

-

While some of the tools mentioned above may overlap in some violation reporting,
you really need to make sure that you use these tools constantly. Get in the habit
of running them as part of your development work flow and you will soon reap
the benefits when others applaud you for writing stable, clean, readable
and most importantly maintainable code.

Always keep in mind that while these tools will not magically make you a better
programmer, they will simply ensure that your code is _consistent_ with others
and while a team of developers can decide on their own guidelines, it would be
far more fitting to ensure that the guidelines can carry over from a team to the
next.

Finally, I leave you all with a talk by Raymond Hettinger titled
[Beyond PEP 8 -- Best practices for beautiful intelligible code](https://www.youtube.com/watch?v=wf-BqAjZb8M)

Enjoy

#### Update

1. added examples.
2. adjusted `this` module import.
3. added final comments.
4. added Raymond's talk.
