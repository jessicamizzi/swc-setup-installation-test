For learners
============

This directory contains scripts for testing your machine to make sure
you have the software you'll need for your workshop installed.  Your
instructors should provide you with a URL listing the workshop's
dependencies.  See the comments at the head of each script for more
details, but you'll basically want to see something like:

    $ python swc-installation-test-1.py
    Passed
    $ python swc-installation-test-2.py --url https://swcarpentry.github.io/2015-11-09-abc/lessons.json
    check virtual-shell...  pass
    …
    Successes:

    virtual-shell Bourne Again Shell (bash) 4.2.37
    …

If you see something like:

    $ python swc-installation-test-2.py --url https://swcarpentry.github.io/2015-11-09-abc/lessons.json
    check virtual-shell...  fail
    …
    check for command line shell (virtual-shell) failed:
      command line shell (virtual-shell) requires at least one of the following dependencies
      For instructions on installing an up-to-date version, see
      http://software-carpentry.org/setup/
      causes:
      check for Bourne Again Shell (bash) failed:
        could not find 'bash' executable for Bourne Again Shell (bash)
        For instructions on installing an up-to-date version, see
        http://software-carpentry.org/setup/
    …

follow the suggestions to try and install any missing software.  For
additional troubleshooting information, you can use the `--verbose`
option:

    $ python swc-installation-test-2.py --url https://swcarpentry.github.io/2015-11-09-abc/lessons.json --verbose
    check virtual-shell...  fail
    …
    ==================
    System information
    ==================
    os.name            : posix
    …

For instructors
===============

`swc-installation-test-1.py` is pretty simple, and just checks that
the students have a recent enough version of Python installed that
they'll be able to parse `swc-installation-test-2.py`.  The latter
checks for your workshop's requirements and prints error messages if a
package is not installed, or if the installed version is not current
enough.  When you setup your workshop, you should write a [JSON][]
file listing the lessons you will cover.  For example:

```json
{
  "shell": {
    "requirements": "https://raw.githubusercontent.com/swcarpentry/shell-novice/v5.4/requirements.json"
  },
  "git": {
    "requirements": "https://raw.githubusercontent.com/swcarpentry/git-novice/v5.3/requirements.json"
  },
  "python": {
    "requirements": "https://raw.githubusercontent.com/swcarpentry/python-novice-inflammation/v5.4/requirements.json"
  },
  "sql": {
    "requirements": "https://raw.githubusercontent.com/swcarpentry/sql-novice-survey/v5.7/requirements.json"
  }
}
```

The lesson names (“shell”, “git”, …) are only used for logging, and
you may add other attributes or entries that don't define
`requirements` if you wish.  `swc-installation-test-2.py` will collect
requirements from the lesson requirement files and then check to make
sure they are all satisfied.  If you want to use a lesson that does
not provide a `requirements.json`, you can write your own requirement
file for that lesson and use your URL in your workshop's JSON:

```json
{
  "shell": {
    "requirements": "https://swcarpentry.github.io/2015-11-09-abc/shell-requirements.json"
  }
}
```

For lesson maintainers
======================

By providing a `requirements.json` file, you make it easy for others
to discover its requirements.  For example, [instructors can write
workshop requirements](#for-instructors) referencing your listing.
The [JSON][] file should be an object whose keys are check names and
whose values are objects that may contain a `minimum-version` key.  If
`minimum-version` is set, it must be an array specifying the minimum
version of the checked software compatible with your lesson.  For
example:

```json
{
  "git": {
    "minimum-version": [1, 7, 0]
  },
  "virtual-editor": {},
  "virtual-browser": {},
  "virtual-shell": {}
}
```

You may add other attributes to the per-check objects if you wish.

Virtual dependencies can be satisfied by any of several packages.  If
you don't want to support a particular package (e.g. if you have no
Emacs experience and don't want to be responsible for students who
show up with Emacs as their only editor), you can blacklist the
package:

```json
{
  "virtual-editor": {
    "blacklist": ["emacs"]
  }
}
```

[JSON]: http://json.org/
