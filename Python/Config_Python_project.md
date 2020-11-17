# How to setup a Python project

This tutorial provides an overview of the recommended practices in configuring your Python projects from scratches.
The layout suggested here is a superset of the standard Python package layout, the details of which can be found on this [page](https://packaging.python.org/tutorials/packaging-projects/).

```
MyAwesomePackage
├── LICENSE
├── MANIFEST.in
├── README.md
├── my_awesome_package
│   └── __init__.py
|   └── module_1.py
|          ⋮
|   └── module_n.py
|   └── subpackage_a
|       └── __init__.py
|       └── module_1.py
|              ⋮
|       └── module_n.py
|      ⋮
|   └── subpackage_n
|       └── __init__.py
|       └── module_1.py
|              ⋮
|       └── module_n.py
├── tests
│   └── test_module_1.py
|          ⋮
│   └── test_module_n.py
|   └── subpackage_a
|       └── test_module_1.py
|              ⋮
|       └── test_module_n.py
|      ⋮
|   └── subpackage_n
|       └── test_module_1.py
|              ⋮
|       └── test_module_n.py
├── data
├── scripts
├── requirements_dev.txt
├── requirements.txt
├── setup.py
└── versioneer.py
```
> Some convention in this documents
> - Directory
> - `SingleFile`
> - package_name, sub_package_name
> - `source_file.py`

## MyAwesomePackage
-------------------
This is your awesome package, and you should be able to name it whatever you want (in theory).
However, given that you will most likely want others to be able to benefit from your work, here are some general guidelines:
* The folder name should be representative of the actual package name.
* Try to avoid using special characters (reserved symbols in shell, non-ASCII characters, etc.) in the package name.
* Abbriveation is common, but make sure you explain it either in
  * `README.md`
  * `__init__.py`  (package level)
  * Package docstring


## `LICENSE`
----------
Most of the projects in this group uses the GPL3 (GNU GENERAL PUBLIC LICENSE Version 3).
You can find more information of this license on
* [official website](https://www.gnu.org/licenses/gpl-3.0.en.html)
* [Wikipedia](https://en.wikipedia.org/wiki/GNU_General_Public_License)
* [Github](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/licensing-a-repository) 
> NOTE 1: If your project needs a different license, contact your programe manager about which license is best suited for you.  
> NOTE 2: Both `Github` and `Gitlab` has template readily available when you setup your repository.


## `MANIFEST.in`
--------------
Python will automatically include a small set of files when building a source distribution of your pacakge.
However, we recommended explicitly stating what needs to be included in your package using `MANIFECT.in` for long-term maintainability.
An example of `MANIFECT.in` can be something like the following
```
include versioneer.py
include my_awesome_package/_version.py
include README.md
include my_awesome_package/*
recursive-include icons *.png *.svg *.ico
```
> NOTE: For more details on `MANIFECT.in`, please visit the [official documentation](https://packaging.python.org/guides/using-manifest-in/).


## `README.md`
--------------
This is the document where you can describe the purpose, application and various other aspects of your package.
The most common way to compose this document is using `MarkDown` as both `Github` and `Gitlab` can automatically render it into html for better visual experience.
More details on the syntax and usage of `MarkDown` can be found in the following locations
* [Basics](https://docs.github.com/en/free-pro-team@latest/github/writing-on-github/basic-writing-and-formatting-syntax)
* [Tutorial](https://guides.github.com/features/mastering-markdown/)
* [CheatSheet](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)


## my_awesome_pacakge
---------------------
This is where the main action happens.

### `__init__.py`
This file is needed so that the directory my_awesome_package can be treated as a Python package.
More information about this file can be found [here](https://docs.python.org/3/tutorial/modules.html#:~:text=The%20__init__.py,on%20the%20module%20search%20path.).
Most of the time, you can simply leave this file empty.
However, you can also import some common functionality to the package level 

### `module_x.py`
This is where you can implement your algorithms, and a few things to think about when you are working on these files

* Starting from version 3.5, Python supports type hinting, which allows you to specify variable types.
You can take advantage of this feature to make your code easier to read, but keep-in-mind that the standard Python interpretor still does not use these information during run time.
More details about this feature can be found on the [official documentation](https://docs.python.org/3/library/typing.html).
  * Some 3rd-party interpretor such as [mypy](https://mypy.readthedocs.io/en/stable/) does use type information, but whether or not to use them is your own decision.
* As a general practice, we recommend following the [PEP8](https://www.python.org/dev/peps/pep-0008/) convention.
However, a much simpler approach of dealing with coding style is by using automated tools, such as
  * [flake8](https://flake8.pycqa.org/en/latest/)
  * [pycodestyle](https://pycodestyle.pycqa.org/en/latest/)
  * [black](https://black.readthedocs.io/en/stable/)  

### subpackage_n
Sometimes your projects can consist multiple sub-projects, each one of which deserves its own package.
This is where the subpackage structure comes in handy.
More details about how to use subpackage can be found on the offical documentation on [namespace package](https://packaging.python.org/guides/packaging-namespace-packages/).


## tests
--------
It is important to write tests for your package, especially if your code is performing calculation that down-stream packages relies on.
There are many blogs and books dedicated on the topics of writing tests for your Python proejcts, for examples
* [Official Python](https://docs.python.org/3/library/unittest.html)
* [Python Testing](https://pythontesting.net/start-here/)
* [The Hitchhiker's Guid to Python](https://docs.python-guide.org/writing/tests/)

Here we have a few recommendations when it comes to setup your unit testing
* The folder structure of tests should mimic your acutal package.
  * This will make it easier for other people contributing to your project.
  * This will make it easier for debugging as the location of the failed tests can be a good indicator of the error source.
* Use [pytest](https://docs.pytest.org/en/stable/) as your first choice of testing framework.
  * Try to use the assertion functions provided from [numpy.testing](https://numpy.org/doc/stable/reference/routines.testing.html) in your tests instead of the standard ones provided by Python.
  * Make sure you have the following code in each of your testing module so that each module can be executed without invoking the framework.
  ```python
  if __name__ == '__main__':
    pytest.main([__file__])
  ```
  * If your testing or your function relies on random number generator, make sure set the seed of your random generator so that the results can be predictable.
  For example, you can set the random seed of `numpy.random` by 
  ```python
  numpy.random.seed(YOUR_SEED_OF_CHOICE)
  ```
  * If you testing requires additional testing data
    * For testing data that can be generated on-the-fly, put the data generating function in your testing module.
    * For testing data that cannot be generated on-the-fly, but __small__ enough to be included in a repository, put them in the data directory.
    * For large testing data, store them on a SFTP/FTP server and download them during the test.
    > NOTE: For storing large flies, contact your programe manager about access to the SFTP/FTP server.
* To the best of your capability, write __at least__ one test for each _calculation_ and _io_ type functionality in your package.


## data
-------
As mentioned in the previous section, this is the location you can store small data files for various purposes, including
* Testing data files
* Reference data files

Generally speaking, it is not necessary to structure the files inside the data folder as the number and size of data files need to be kept as minimum as possible.
However, you might want to consider provide a structured data folder if you have many data files as it can be helpful in the long term.


## scripts
----------
Sometimes you would like to provide some scripts that uses your package as backend.
This is very common in packages related to data processing and workflow.
You can put all the scripts in this folder, and they will be added to the system path once installed via `pip` or `conda`.


## `requirements_dev.txt` & `requirements.txt`
-------------------------
These two files are [requirement files](https://pip.pypa.io/en/latest/user_guide/#requirements-files) which specify the dependencies of your package.
More specifically, 
* `requirements.txt` is used to specify the dependencies that are needed to use your package as a general user.
* `requirements_dev.txt` provides additional dependencies that are needed to develop additional features for your package, most of the time these are _testing frameworks_ and _linting_ packages.

For example, Mithrandir likes your my_awesome_package and would like to try it out.
To do so, he would need to 
```bash
$ pip install -r requirement.txt
```
to install all the necessary dependencies before using the package.

After some testing, he would like to introduce a new feature to your package.
Now in addition to the installation he did a moment ago, he would also need to
```bash
$ pip install -r requirements_dev.txt
```
so that the tesging and linting related packages can be installed as well.

> NOTE:  
> You can also use `conda install --file requirements.txt` and `conda install --file requirements_dev.txt` to install corresponding dependencies from Anaconda.
> However, it is worth keeping in mind that not all packages are published on `conda` and `conda-forge`.
> Therefore, if you preferred distribution channel is `anaconda`, please verify locally that all packages can be installed using `conda install --file requirements.txt` before publication.

## `setup.py` & `versioneer.py`
------------
The file `setup.py` is the build script for `setuptools`, which helps you packaing your project for realse.
You can find detailed instructions on how to write a `setup.py` on the [official documentation page](https://docs.python.org/3/distutils/setupscript.html).
One particular thing we would recommend is to also use `versioneer.py`, which automatically generate the version of your package based on your verions control software log info.
You can find more information about `versioneer.py` on its [offical Github page](https://github.com/python-versioneer/python-versioneer).
> NOTE: some additiona files such as `setup.cfg` is needed by `versioneer.py`, please visit its [offical Github page](https://github.com/python-versioneer/python-versioneer) for more details.

Here is an example `setup.py` to help you get started

```python
# file exmaple adapted from PyRS project
# https://github.com/neutrons/PyRS
import codecs
import os
import re
import versioneer  # https://github.com/warner/python-versioneer
from setuptools import setup, find_packages

NAME = "my_awesome_package"
META_PATH = os.path.join("my_awesome_package", "__init__.py")
KEYWORDS = ["class", "attribute", "boilerplate"]
CLASSIFIERS = [
    "Development Status :: 5 - Production/Stable",
    "Intended Audience :: Developers",
    "Natural Language :: English",
    "License :: OSI Approved :: GPL License",
    "Operating System :: OS Independent",
    "Programming Language :: Python",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.3",
    "Programming Language :: Python :: 3.4",
    "Programming Language :: Python :: 3.5",
    "Programming Language :: Python :: 3.6",
    "Programming Language :: Python :: Implementation :: CPython",
    "Programming Language :: Python :: Implementation :: PyPy",
    "Topic :: Software Development :: Libraries :: Python Modules",
]

THIS_DIR = os.path.abspath(os.path.dirname(__file__))


def read_requirements_from_file(filepath):
    '''Read a list of requirements from the given file and split into a
    list of strings. It is assumed that the file is a flat
    list with one requirement per line.
    :param filepath: Path to the file to read
    :return: A list of strings containing the requirements
    '''
    with open(filepath, 'rU') as req_file:
        return req_file.readlines()


def read(*parts):
    """
    Build an absolute path from *parts* and and return the contents of the
    resulting file.  Assume UTF-8 encoding.
    """
    with codecs.open(os.path.join(THIS_DIR, *parts), "rb", "utf-8") as f:
        return f.read()


META_FILE = read(META_PATH)


def find_meta(meta):
    """
    Extract __*meta*__ from META_FILE.
    """
    # print (r"^__{meta}__ = ['\"]([^'\"]*)['\"]".format(meta=meta))
    # print (META_FILE)
    # print (re.M)
    meta_match = re.search(
        r"^__{meta}__ = ['\"]([^'\"]*)['\"]".format(meta=meta),
        META_FILE, re.M
    )
    if meta_match:
        return meta_match.group(1)
    raise RuntimeError("Unable to find __{meta}__ string.".format(meta=meta))


install_requires = read_requirements_from_file(os.path.join(THIS_DIR, 'requirements.txt'))
test_requires = read_requirements_from_file(os.path.join(THIS_DIR, 'requirements_dev.txt'))


if __name__ == "__main__":
    scripts = ['scripts/my_data_procssing.py']
    setup(
        name=NAME,
        description=find_meta("description"),
        license=find_meta("license"),
        url=find_meta("url"),
        version=versioneer.get_version(),
        cmdclass=versioneer.get_cmdclass(),
        author=find_meta("author"),
        author_email=find_meta("email"),
        maintainer=find_meta("author"),
        maintainer_email=find_meta("email"),
        keywords=KEYWORDS,
        long_description=read("README.md"),
        packages=find_packages(exclude=['tests', 'tests.*']),
        zip_safe=False,
        classifiers=CLASSIFIERS,
        install_requires=install_requires,
        tests_require=test_requires,
        package_dir={},
        package_data={},
        scripts=scripts,
        setup_requires=['pytest-runner'],
    )
```