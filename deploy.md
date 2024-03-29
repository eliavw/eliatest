# eliatest

Deployment information.

1 Development workflows
=======================

1.1 Start project
-----------------

Using the power of [cookiecutter](https://cookiecutter.readthedocs.io/en/latest/), this single command provides a pretty solid starting point for any new project.

```bash
cookiecutter gh:eliavw/cookiecutter-datascience
```

1.2 git
-------

Version control goes without saying. For the local repository, do;

```bash
git init
git add .
git commit -m "First commit"
```

For the remote repository, do;

```bash
git remote add origin git@github.com:eliavw/eliatest.git
git remote -v
git push origin master
```

And that's it for git.

### One-liners

We can summarize the above procedure in two one-liners, should you really care about doing this fast.

```bash
git init; git add .; git commit -m "First commit";
```

and

```bash
git remote add origin git@github.com:eliavw/eliatest.git; git remote -v; git push origin master
```

1.3 Conda Environments
----------------------

### Introduction

This cookiecutter is set up for optimal use with [conda](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf), for **local dependency managment**. The takeaway is this; _for local dependency managment, we rely on conda and nothing else._

Note that this has nothing to do with **remote dependency managment**. This is what you need to take care of when preparing a _release_ of your code which goes via [PyPi](https://pypi.org/) or alternatives. We treat that as an independent problem. Mixing remote and local dependency managment tends to add complexity instead of removing it.

### Workflow

To create our default environment, do:

```bash
conda env create -f dependencies-deploy.yaml -n eliatest
```

To additionally add the packages which are relevant for the development phase, do:

```bash
conda activate eliatest
conda env update -n eliatest -f dependencies-develop.yaml
```

### Jupyterlab

To add your isolated python installation (i.e., the one in your new conda environment) to the list of "kernels" found by Jupyter, execute the following.

```bash
conda activate eliatest
python -m ipykernel install --user --name eliatest --display-name "eliatest"
```

1.4 Local Installation
----------------------

One fundamental assumption is the following; 

> All code in this repository is a) part of a python package or b) a standalone script.

- Code in [src](./src) is considered as a Python package.
- Code in [scripts](./scripts) or [note](./note), the code acts as standalone scripts.

This means that even our own code has to be installed before we are able to use it. This seems a bit tedious but has some important advantages too:

1. Our scripts will consider our own algorithms and external competitors just as packages that have to be imported. This forces us to explicitly declare everything we want to use.
2. Since our own algorithm will have to behave just like the competition, this enhances modularity in our code.
3. If we build it like a package from the start on our local machine, the transition to an actual package will be a lot smoother afterwards.

### Installation instructions

To install, activate the conda environment and execute this line of code.

```bash
python setup.py develop # or `install`
```

What is the difference between `develop` or `install`? When you install the package with the `develop` flag, symlinks are created from yoru code to the python installation. That means that every time you change something in your codebase, the installed package in your python environment will also change. Typically, this is what you'd want: to see your changes reflected immediately.

The install option just copies your code as it is at time of installation and install the package in the python environment. This mimics what a third party would do.




1.5 CI (Travis)
---------------

Do not allow yourself to proceed without at least accumulating some tests. Therefore, we've set out to intigrate [CI](https://en.wikipedia.org/wiki/Continuous_integration) (i.e. [Travis](https://travis-ci.com)) right from the start.

Follow these steps:

1. Go to the [Travis](https://travis-ci.com/eliavw/eliatest) page of this repo.
2. See if it ran.

**Note:** The tests depend on our **local dependecy managment**. Why? Because we have full control of the Travis servers running our tests. Therefore, we can simply treat it as a computer we'd control. We only need to fall back on remote dependency managment if other people need to get our code up and running, without our intervention.



2 Distribution workflows
========================

This part is about publishing your project on PyPi.

2.1 Pypi
--------

Make your project publicly available on the Python Package Index, [PyPi](https://pypi.org/). To achieve this, we need **remote dependency managment**, since you want your software to run without forcing the users to recreate your conda environments. All dependencies have to be managed, automatically, during installation. To make this work, we need to do some extra work.

We follow the steps as outlined in the most basic (and official) [PyPi tutorial](https://packaging.python.org/tutorials/packaging-projects/).

### Generate distribution archives

Generate distribution packages for the package. These are archives that are uploaded to the Package Index and can be installed by pip.

```bash
python setup.py sdist bdist_wheel
```

After this, your package can be uploaded to the python package index. This is as easy as:

```bash
python -m twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```

and this will prompt some questions, but your package will end up in the index.


2.2 Docs
--------
Every good open source project at least consists of a bit of documentation. A part of this documentation is generated from decent docstrings you wrote together with your code.

```bash
python setup.py docs
```

2.3 Docker
----------


2.4 Singularity
---------------


2.5 Website
-----------
A more serious project can benefit from an additional website, apart from the documentation site.
