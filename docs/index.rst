.. toctree::
   :hidden:

   actions
   remote
   changelog


============================
    SST - Web Test Framework
============================

:Web Home: http://testutils.org/sst
:Project Home: https://launchpad.net/selenium-simple-test
:PyPI: http://pypi.python.org/pypi/sst
:License: Apache License, Version 2.0
:Author: Copyright (c) 2011-2012 Canonical Ltd.


---------------------------------
    Automated Testing with Python
---------------------------------

SST (selenium-simple-test) is a web test framework that uses Python
to generate functional browser-based tests.

Tests are made up of scripts, created by composing actions that drive
a browser and assert conditions. You have the flexibilty of the full
Python language, along with a convenient set of functions to simplify
web testing.

SST consists of:

 * user actions and assertions (API) in Python
 * test case loader (generates/compiles scripts to unittest cases)
 * console test runner
 * data parameterization/injection
 * selectable output reports
 * selectable browsers
 * headless (xvfb) mode
 * screenshots on errors

Test output can be displayed to the console, saved as an HTML report, or
JUnit-compatible XML for compatibility with CI systems.


-----------
    Install
-----------

SST can be installed from `PyPI <http://pypi.python.org/pypi/sst>`_ using
`pip <http://www.pip-installer.org>`_::

    pip install -U sst

For example, on an Ubuntu/Debian system, you could Install SST (system-wide)
like this::

    $ sudo apt-get install python-pip xvfb
    $ sudo pip install -U sst

* note: `xvfb` is only needed if you want to run SST in headless mode


---------------------------
    Example SST test script
---------------------------

a sample test case in SST::

    from sst.actions import *

    go_to('http://www.ubuntu.com/')
    assert_title_contains('Ubuntu')


------------------------------------
    Running a test with SST
------------------------------------

Create a Python script (.py) file, and add your test code.

Then call your test script from the command line, using `sst-run`::

    $ sst-run mytest

* note: you don't add the .py extension to your test invocation


-----------------------------------
    Actions reference (sst.actions)
-----------------------------------

Test scripts perform actions in the browser as if they were a user.
SST provides a set of "actions" (functions) for use in your tests.
These actions are defined in the following API:

 * `Actions Reference <http://testutils.org/sst/actions.html>`_


------------------------------------
    Command line options for sst-run
------------------------------------

Usage: `sst-run <options> [testname]`

* Calling sst-run with testname(s) as arguments will just run
  those tests. The testnames should not include '.py' at
  the end of the filename.

* You may optionally create a data file for data-driven
  testing.  Create a '^' delimited txt data file with the same
  name as the test, plus the '.csv' extension.  This will
  run a test using each row in the data file (1st row of data
  file is variable name mapping)

Options::

  -h, --help                show this help message and exit
  -d DIR_NAME               directory of test case files
  -r REPORT_FORMAT          results report format (html, xml, console)
  -b BROWSER_TYPE           select webdriver (Firefox, Chrome, Ie, etc)
  -j                        disable javascript in browser
  -m SHARED_MODULES         directory for shared modules
  -q                        output less debugging info during test run
  -V                        print version info and exit
  -s                        save screenshots on failures
  -x                        run browser in headless xserver
  --failfast                stop test execution after first failure
  --debug                   drop into debugger on test fail or error
  --with-flags=FLAGS        comma separated list of flags to run tests with
  --disable-flag-skips      run all tests, disable skipping tests due to flags
  --extended-tracebacks     Add extra information (page source) to failure reports
  --browsermob=BROWSERMOB   enable browsermob proxy (launcher location)
  --test                    run selftests


-----------------
  Interactive use
-----------------

After installing sst, you can experiment with it from the python interactive
interpreter by calling `start()` to launch the browser (Firefox by default).
After that, you can call any of the actions as you would use them in a test::

    >>> from sst.actions import *
    >>> start()

        Starting Firefox
    >>> go_to('http://google.com')
        Going to... http://google.com
        Waiting for get_element
    >>>


--------------------
    Organizing tests
--------------------

For logical organization of tests, you can use directories in your filesystem.
SST will recursively walk your directory tree and gather all tests for
execution.

For example, a simple test setup might look like::

    /selenium-simple-test
        /mytests
            foo.py

and you would call this from the command line::

    $ sst-run -d mytests

A more complex setup might look like::

    /selenium-simple-test
        /mytests
            /project_foo
                /feature_foo
                    foo.py
            /project_bar
                feature_bar.py
                feature_baz.py
            /shared
                module.py
                utils.py

and you would still call this from the command like::

    $ sst-run -d mytests

SST will find all of the tests in subdirectories (including symlinks) and
execute them. SST won't look in directories starting with an underscore. This
allows you to put Python packages/modules directly in your test directories
if you want. A better option is to use the shared directory.


--------------------
    Shared directory
--------------------

SST allows you to have a directory called `shared` in the top level directory
of your tests, which is added to `sys.path`. Here you can keep helper modules
used by all your tests. `sst-run` will not run Python files in the `shared`
directory as tests.

By default SST looks in the test directory you specify to find `shared`,
alternatively you can specify a different directory using the `-m` command
line argument to `sst-run`.

If there is no `shared` directory in the test directory, then `sst-run` will
walk up from the test directory to the current directory looking for one. This
allows you to run tests just from a subdirectory without having to explicitly
specify where the shared directory is.


---------------------
    sst.config module
---------------------

Inside tests you can import the `sst.config` module to know various things
about the current test environment. The `sst.config` module has the following
information::

    from sst import config

    # is javascript disabled?
    config.javascript_disabled

    # which browser is being used?
    config.browser_type

    # full path to the shared directory
    config.shared_directory

    # full path to the results directory
    config.results_directory

    # is browsermob proxy enabled?
    config.browsermob_enabled

    # flags for the current test run
    config.flags

    # A per test cache. A dictionary that is cleared at the start of each test.
    config.cache

------------------------
    Disabling Javascript
------------------------

If you need to disable Javascript for an individual test you can do it by
putting the following at the start of the test::

    JAVASCRIPT_DISABLED = True


--------------------------------
    Development on Ubuntu/Debian
--------------------------------

* SST is primarily being developed on Linux, specifically Ubuntu. It should
  work fine on other platforms, but any issues (or even better - patches)
  should be reported on the Launchpad project.

* Get a copy of SST Trunk, install requirements, and run self-tests/examples
  from the branch::

    $ sudo apt-get install bzr python-pip xvfb
    $ bzr branch lp:selenium-simple-test
    $ cd selenium-simple-test
    $ sudo pip install -U -r requirements.txt
    $ ./sst-run --test -x
    $ ./sst-run -d examples

* `Launchpad Project <https://launchpad.net/selenium-simple-test>`_

* `Browse the Source (Trunk)
  <http://bazaar.launchpad.net/~canonical-isd-qa/selenium-simple-test/trunk/files>`_

* To manually setup dependencies, SST uses the following non-stdlib packages:

    * selenium
    * unittest2
    * junitxml
    * django (optional - needed for internal self-tests only)


------------------------
    Running the examples
------------------------

SST source code repository and package download contain some trivial example
scripts.

You can run them from your local sst directory like this::

    $ ./sst-run -d examples


--------------------------
    Running the self-tests
--------------------------

SST source code repository and package download contain a set of self-tests
based on an included test Django project.

You can run the suite of self-tests (and the test Django server) from your
local branch like this::

    $ ./sst-run --test

---------------------------------
Using sst in unittest test suites
---------------------------------

sst uses unittest test cases internally to wrap the execution of the script
and taking care of starting and stopping the browser. If you prefer to
integrate some sst tests into an existing unittest test suite you can use
SSTTestCase from runtests.py::

  from sst.actions import *
  from sst import runtests

  class TestUbuntu(runtests.SSTTestCase):

      def test_ubuntu_home_page(self):
          go_to('http://www.ubuntu.com/')
          assert_title_contains('Ubuntu')

So, with the above in a file name test_ubuntu.py you can run the test with
(for example)::

  python -m unittest test_ubuntu.py

`sst-run` provides an headless xserver via the `-x` option. `SSTTestCase`
provides the same feature (sharing the same implementation) via two class
attributes.

`xserver_headless` when set to `True` will start an headless server for each
test (and stop it after the test). If you want to share the same server
across several tests, set `xvfb`. You're then responsible for starting and
stopping this server (see `src/sst/xvfbdisplay.py` for details or
`src/sst/tests/test_xvfb.py` for examples.


---------------------------------------------------
    Performance tracing with Browsermob Proxy (HAR)
---------------------------------------------------

SST can generate `HAR (HTTP Archive format)
<http://www.softwareishard.com/blog/har-12-spec/>`_ output for performance
profiling and tracing.

HAR format is based on JSON, and is used by tools that consume/produce data
collected by monitoring HTTP communication. These files contain a log of HTTP
client/server conversation and can be used for additional analysis of page
load performance.

This is achieved by routing browser requests through `BrowserMob Proxy
<https://github.com/webmetrics/browsermob-proxy>`_, which records web page
loads while your tests run.  SST will launch the proxy and save output to
.har files if you enable the ``--browsermob`` command line option.  HAR files
are saved in the `results` directory for each page load.

* Setup Browsermob Proxy

 * install a Java runtime::

    $ sudo apt-get install default-jre

 * download latest `browsermob-proxy-*.zip`:

  * `GitHub browsermob-proxy/downloads <https://github.com/webmetrics/browsermob-proxy/downloads>`_

 * unzip archive, and give execute permissions to the launcher script::

    $ chmod +x bin/browsermob-proxy

* Invoke SST using the ``--browsermob`` option, providing location of the launcher.

 * Example::

    $ sst-run mytest --browsermob=/home/foo/browsermob-proxy/bin/browsermob-proxy

* HAR analysis tools:

 * `harviewer <http://code.google.com/p/harviewer/>`_
 * `HTTP Archive Viewer <http://www.softwareishard.com/har/viewer/>`_
 * `PCAP Web Performance Analyzer <http://pcapperf.appspot.com/>`_

* Browsermob integration notes:

 * not yet working for SSL sites
 * does not record local requests.  test against remote servers only.


-----------------
    Related links
-----------------

* `Selenium Project Home <http://selenium.googlecode.com>`_
* `Selenium WebDriver (from 'Architecture of Open Source Applications')
  <http://www.aosabook.org/en/selenium.html>`_
* `Python Unittest <http://docs.python.org/library/unittest.html>`_
* `unittest2 <http://pypi.python.org/pypi/unittest2/>`_

