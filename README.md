# pds-template-repo-python

This is the template repository for PDS's Python projects.

This repository aims at being a base for new python repositories used in PDS. It guides developers to ease the initialization of a project and recommends preferred options to standardize developments and ease maintenance. Simply click the <kbd>Use this template</kbd> button ↑ (or use [this hyperlink](https://github.com/NASA-PDS/pds-template-repo-python/generate)).

## 🏃 Getting Started With This Template

See our wiki page for more info on setting up your new repo. You can remove this section once you have completed the necessary start-up steps.

https://github.com/NASA-PDS/nasa-pds.github.io/wiki/Git-and-Github-Guide#creating-a-new-repo


## Prerequisites

Include any system-wide requirements (`brew install`, `apt-get install`, `yum install`, …) **Python 3** should be used regardless as [Python 2 reached end-of-life on January 1st, 2020](https://pythonclock.org/).


## User Quickstart

Install with:

    pip install my_pds_module

If possible, make it so that your program works out of the box without any additional configuration—but see the [Configuration](###configuration) section for details.

To execute, run:

    (put your run commands here)


## Development

To develop this project, use your favorite text editor, or an integrated development environment with Python support, such as [PyCharm](https://www.jetbrains.com/pycharm/).

### Installation

Install in editable mode and with extra developer dependencies into your virtual environment of choice:

    pip install --editable '.[dev]'

Configure the `pre-commit` hooks:

    pre-commit install && pre-commit install -t pre-push

### Packaging

To isolate and be able to re-produce the environment for this package, you should use a [Python Virtual Environment](https://docs.python.org/3/tutorial/venv.html). To do so, run:

    python -m venv venv

Then exclusively use `venv/bin/python`, `venv/bin/pip`, etc. (It is no longer recommended to use `venv/bin/activate`.)

If you have `tox` installed and would like it to create your environment and install dependencies for you run:

    tox --devenv <name you'd like for env> -e dev

Dependencies for development are specified as the `dev` `extras_require` in `setup.cfg`; they are installed into the virtual environment as follows:

    pip install --editable '.[dev]'

All the source code is in a sub-directory under `src`.

You should update the `setup.cfg` file with:

- name of your module
- license, default apache, update if needed
- description
- download url, when you release your package on github add the url here
- keywords
- classifiers
- install_requires, add the dependencies of you package
- extras_require, add the development Dependencies of your package
- entry_points, when your package can be called in command line, this helps to deploy command lines entry points pointing to scripts in your package

For the packaging details, see https://packaging.python.org/tutorials/packaging-projects/ as a reference.


### Configuration

It is convenient to use ConfigParser package to manage configuration. It allows a default configuration which can be overwritten by the user in a specific file in their environment. See https://pymotw.com/2/ConfigParser/

For example:

    candidates = ['my_pds_module.ini', 'my_pds_module.ini.default']
    found = parser.read(candidates)


### Logs

You should not use `print()`vin the purpose of logging information on the execution of your code. Depending on where the code runs these information could be redirected to specific log files.

To make that work, start each Python file with:

```python
import logging

logger = logging.getLogger(__name__)
```

To log a message:

    logger.info("my message")

In your `main` routine, include:

    logging.basicConfig(level=logging.INFO)

to get a basic logging system configured.

### Tooling

The `dev` `extras_require` included in the template repo installs `black`, `flake8` (plus some plugins), and `mypy` along with default configuration for all of them. You can run all of these (and more!) with:

    tox -e lint

### Code Style

So that your code is readable, you should comply with the [PEP8 style guide](https://www.python.org/dev/peps/pep-0008/). Our code style is automatically enforced in via [black](https://pypi.org/project/black/). See the [Tooling section](#-tooling)


### Recommended Libraries

Python offers a large variety of libraries. In PDS scope, for the most current usage we should use:

| Library      | Usage                                           |
|--------------|------------------------------------------------ |
| configparser | manage and parse configuration files            |
| argparse     | command line argument documentation and parsing |
| requests     | interact with web APIs                          |
| lxml         | read/write XML files                            |
| json         | read/write JSON files                           |
| pyyaml       | read/write YAML files                           |
| pystache     | generate files from templates                   |

Some of these are built into Python 3; others are open source add-ons you can include in your `requirements.txt`.


### Tests

This section describes testing for your package.

A complete "build" including test execution, linting (`mypy`, `black`, `flake8`, etc.), and documentation build is executed via:

    tox

#### Unit tests

Your project should have built-in unit tests, functional, validation, acceptance, etc., tests.

For unit testing, check out the [unittest](https://docs.python.org/3/library/unittest.html) module, built into Python 3.

Tests objects should be in packages `test` modules or preferably in project 'tests' directory which mirrors the project package structure.

Our unit tests are launched with command:

    pytest

If you want your tests to run automatically as you make changes start up `pytest` in watch mode with:

    ptw

#### Integration/Behavioral Tests

One should use the `behave package` and push the test results to "testrail".

See an example in https://github.com/NASA-PDS/pds-doi-service#behavioral-testing-for-integration--testing


### Documentation

Your project should use [Sphinx](https://www.sphinx-doc.org/en/master/) to build its documentation. PDS' documentation template is already configured as part of the default build. You can build your projects docs with:

    python setup.py build_sphinx

You can access the build files in the following directory relative to the project root:

    build/sphinx/html/


## Build

    pip install wheel
    python setup.py sdist bdist_wheel

## Publication

NASA PDS packages can publish automatically using the [Roundup Action](https://github.com/NASA-PDS/roundup-action), which leverages GitHub Actions to perform automated continuous integration and continuous delivery. A default workflow that includes the Roundup is provided in the `.github/workflows/unstable-cicd.yaml` file. (Unstable here means an interim release.)

### Manual Publication

Create the package:

    python setup.py sdist

Publish it as a Github release.

Publish on pypi (you need a pypi account and configure `$HOME/.pypirc`):

    pip install twine
    twine upload dist/*

Or publish on testpypi (you need a testpypi account and configure `$HOME/.pypirc`):

    pip install twine
    twine upload --repository testpypi dist/*
