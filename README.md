# riscos-tooltester

[![Documentation](https://img.shields.io/badge/Generated-Documentation-yellow)](https://gerph.github.io/riscos-tooltester/)

`riscos-tooltester` is a test runner for command-line tools. A suite is a plain
text description of the commands to run, the output and return code each should
give, and the files it should create. The runner executes each test, compares the
result with what the suite expects, and reports what passed, failed or crashed.

It is intended for integration testing of how a tool behaves as a user sees it,
rather than unit testing. It is a single Perl script with no dependencies, and
it can write JUnit XML so that results can be shown by CI systems. It runs on
Linux and macOS, and under RISC OS itself, where the same suites can be used.

## Installing

Each [release](https://github.com/gerph/riscos-tooltester/releases) provides:

* `riscos-tooltester` - the script itself. Download it, make it executable, and put it on your `PATH`.
* `riscos-tooltester-<version>.zip` - the script and a `!Run` file for starting it on RISC OS, with this README and the licence.
* `riscos-tooltester-docs-<version>.zip` - the documentation as HTML.

For example, to fetch a specific release in a CI job:

    curl -fsSL -o riscos-tooltester \
        https://github.com/gerph/riscos-tooltester/releases/download/v0.1.22/riscos-tooltester
    chmod +x riscos-tooltester

The script needs Perl 5. It can also be run directly from a checkout as `perl tooltester.pl`.

On RISC OS, unpack the zip, and run the script with the RISC OS Perl, where its
name is `tooltester/pl`. `!Run` does this:

    perl <Obey$Dir>.tooltester/pl <options> <tool-under-test> <test-directory>

On RISC OS the script file is named as `tests/txt` and expectation files may use
RISC OS names, so the same suites can be run on either system if they are
written with this in mind. The script must remain compatible with the Perl 5.001
interpreter, so avoid newer Perl features when changing it.

## Using it

    riscos-tooltester [<options>] <tool-under-test> <test-directory>

The test directory holds a `tests.txt` describing the tests, together with the
files of expected output. A small suite looks like this:

    Suite: mytool

    Group: Basic behaviour
    Command: $TOOL $ARGS
    Expect: expect/mytool/$BASE

    Test: Help text
    Args: --help
    Expect: expect/mytool/help

    Test: Rejects a bad option
    Args: --nonsense
    RC: 1
    Expect: expect/mytool/bad-option

Run it with the path of the tool, which must be absolute (or on `PATH`) because
the tests run inside the test directory:

    riscos-tooltester -show-output /usr/local/bin/mytool tests

When the output differs, the actual output is written beside the expected file
with `-actual` added to its name, so that it can be compared and, if correct,
adopted as the new expectation. Use `-junitxml results.xml` to write a JUnit XML report.

Everything else - the full list of options, every statement that can appear in a
test script, the checkers for created files, and the replacement scripts used to
hide values that change from run to run - is described in the
[documentation](https://gerph.github.io/riscos-tooltester/).

## Versions

The version number is `<major>.<minor>.<count>`. The major and minor numbers are
set in `project.config`, and the count is the number of commits on `master`,
calculated by `ci-vars`. A release is made by pushing a tag of the form
`v<version>` (for example `v0.1.22`) for a commit on `master`; the release
workflow checks that the tag matches the calculated version.

## Documentation

The documentation is written as PRM-in-XML in `prminxml/tooltester.xml`, and is
published to GitHub Pages on each change to `master`, and included in each release.

## Licence

MIT. See `LICENSE`.
