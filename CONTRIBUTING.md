# Contributing

Thank you for your interest in contributing to plotly.py! We are actively looking for
diverse contributors, with diverse background and skills.

This guide starts with a general description of the different ways to contribute
to plotly.py, then we explain some technical aspects of preparing your
contribution.

## Code of Conduct

Please check out the [Code of Conduct](CODE_OF_CONDUCT.md). Don't skip it,
but the general idea is to be nice.

## What are the different ways to contribute?

There are many ways to contribute to plotly.py. To contribute effectively, it is important to first gain an understanding of the structure of the code and of the repository.

- [the `plotly.graph_objects` module](https://plotly.com/python/graph-objects/) (usually imported as `go`)
  is [generated from the Plotly.js schema](https://plotly.com/python/figure-structure/),
  so changes to be made in this package need to be
  [contributed to Plotly.js](https://github.com/plotly/plotly.js) or to the `codegen` system
  in `codegen/`. Most of the codegen code concerns the generation of docstrings from
  the schema JSON in Plotly.js. Traces and
  Layout classes have a direct correspondence with their Javascript
  counterpart. Higher-level methods that work on figures regardless of the current schema (e.g., `BaseFigure.for_each_trace`) are defined in `plotly/basedatatypes.py`. Additional helper methods are defined there for the `Figure` object, such as
  `update_layout`, `add_trace`, etc.

- [the `plotly.express` module](https://plotly.com/python/plotly-express/) (usually imported as `px`) is a high-level
  functional API that uses `graph_objects` under the hood. Its code is in `plotly/express/`.
  Plotly Express functions
  are designed to be highly consistent with each other, and to do *as little computation
  in Python as possible*, generally concerning themselves with formatting data and creating
  figures out of `plotly.graph_objects` instances. Most
  functions of `plotly.express` call the same internal `_make_figure` function
  in `_core.py`. More generally, the internals of `px` consist of general
  functions taking care of building the figure (defining subplots, traces
  or frames, for example), with special cases for different traces handled
  within these functions. There is also subsequent code reuse for `px`
  docstrings, in particular for documenting parameters.

- [the `plotly.figure_factory` module](https://plotly.com/python/figure-factories/) (usually imported as `ff`)
  provides Python "recipes" for building
  advanced visualizations with involved computation done in Python, such as
  Hexbin maps, ternary contour plots, etc.
  Figure factories are one of the easiest entry points into contributing to plotly.py, since
  they consist of Python-only code, with standalone, well-separated functions.
  However, please note that some of the figure factories become less relevant
  as we are introducing more features into `plotly.express`. Some issues in the
  tracker are labeled "figure_factory" and can be good issues to work on. More
  instructions on figure factories are found
  [here](plotly/figure_factory/README.md).

- other pure-Python submodules are: `plotly.io` (low-level interface for
  displaying, reading and writing figures), `plotly.subplots` (helper function
  for layout of multi-plot figures)

- tests are found in `plotly/tests`. Different
  directories correspond to different test jobs (with different dependency sets)
  run in continuous integration. More is explained about tests
  in the following "Technical aspects" section.

- the **documentation** is part of this repository. Its structure and some
  explanations are described [here](doc/README.md). The documentation, in
  particular example-based tutorials, is a great place to start contributing.
  The contribution process is also more lightweight, since you can modify
  tutorial notebooks without setting up an environment, etc.
  We maintain a wishlist of examples to add on
  https://github.com/plotly/plotly.py/issues/1965. If you have writing skills,
  the wording of existing examples can also be improved in places.

Contributing code or documentation is not the only way to contribute! You can
also contribute to the project by

- reporting bugs (see below).

- submitting feature requests (maybe we'll convince you to contribute it as a
  pull request!).

- helping other users on the [community forum](https://community.plot.ly/).
  Join the list of [nice people](https://community.plot.ly/u) helping other
  plotly users :-).

We also recommend reading the great
[how to contribute to open source](https://opensource.guide/how-to-contribute/)
guide.

## Have a Bug Report?

Open an issue! Go to https://github.com/plotly/plotly.py/issues. It's possible that your issue was already addressed. If it wasn't, open it. We also accept pull requests; take a look at the steps below for instructions on how to do this.

## Have Questions about Plotly?

Check out our Community Forum: https://community.plot.ly/.

## Want to improve the plotly documentation?

Thank you! Instructions on how to contribute to the documentation are given [here](doc/README.md). Please also read the next section if you need to setup a development environment.

## How to contribute - Technical Aspects

Below we explain the technical aspects of contributing. It is not strictly necessary to follow all points (for example, you will not write tests when writing documentation, most of the time), but we want to make sure that you know how to deal with most cases.

Note that if you are modifying a single documentation page, you can do it
directly on Github by clicking on the "Edit this page on GitHub" link, without
cloning the repository.

## Setup a Development Environment

### Fork, Clone, Setup Your Version of the Plotly Python API

First, you'll need to *get* our project. This is the appropriate *clone* command (if you're unfamiliar with this process, https://help.github.com/articles/fork-a-repo):

**DO THIS (in the directory where you want the repo to live)**

```bash
git clone https://github.com/your_github_username/plotly.py.git
cd plotly.py
```

Note: if you're just getting started with git, there exist great resources to
learn and become confident about git, like http://try.github.io/.

### Create a virtual environment for plotly development

You can use either [conda][conda-env] or [virtualenv][virtualenv] to create a virtual environment for plotly development, e.g.:

```bash
conda create -n plotly-dev python=3.11
conda activate plotly-dev
```

As of May 2024 our dependencies have been tested against Python versions 3.8 to 3.11.
We will support Python 3.12 and higher versions soon.

[conda-env]: https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands
[virtualenv]: http://docs.python-guide.org/en/latest/dev/virtualenvs/

### Install development requirements (Non-Windows)
```bash
(plotly_dev) $ pip install -r requires-optional.txt
 ```
### Install development requirements (Windows + Conda)
Because Windows requires Visual Studio libraries to compile some of the optional dependencies, follow these steps to
complete installation and avoid gdal-config errors.

```bash
(plotly_dev) $ conda install fiona
(plotly_dev) $ pip install -r requires-optional.txt
```

### Editable install of plotly packages
```bash
(plotly_dev) $ pip install -e .
```

**Note**: To test `go.FigureWidget` locally, you'll need to generate the javascript bundle as follows:
```
cd js
npm install && npm run build
```

Running `pip install -e` will ensure that the installed packages links to your local development
directory, meaning that all changes you make reflect directly in your
environment (don't forget to restart the Jupyter kernel though!). For more
information see the
[`setuptools`](https://setuptools.readthedocs.io/en/latest/setuptools.html#development-mode)
and
[`pip`](https://pip.pypa.io/en/stable/reference/pip_install/#install-editable)
documentation on _development mode_.

### Updating the `js/` directory
**This is only necessary if you're making changes to files in the `js/` directory.**
If you make changes to any files in the `js/` directory, you must run `npm install && npm run build` from the `js/` directory to rebuild the FigureWidget and JupyterLab extension.
You must then commit the build artifacts produced in `plotly/labextension`. A CI job will verify that this step has been done correctly.

**Notes on the contents of the `js/` directory:**
The `js/` directory contains Javascript code which helps with using Plotly in Jupyter notebooks.

There are two kinds of Jupyter support included in the `js/` directory:
1. **Mime Renderer JupyterLab extension**: This is the default renderer for Plotly `Figure()` objects in Jupyter notebooks. The Plotly mime renderer JupyterLab extension is used automatically by JupyterLab / Jupyter Notebook
when it sees the mimetype `application/vnd.plotly.v1+json` in the notebook output. The mime renderer loads `plotly.js` a single time and references it each time a Plotly figure is used in the notebook. This allows us to avoid embedding `plotly.js` in the notebook output. The JupyterLab extension source code is located at `js/src/mimeExtension.ts` and the compiled extension code is located at `plotly/labextension` in the built Python package. The command `jupyter labextension build` (which is one of the steps called by `npm run build`) compiles the extension and places the build artifacts in `plotly/labextension`. 
2. **FigureWidget**: This is a more-interactive method for rendering Plotly charts in notebooks. FigureWidget used by creating a `FigureWidget()` object inside the notebook code (in place of a `Figure()`). It allows for communication between the Javascript frontend and Python backend, and requires the installation of an additional Python package (`anywidget`). The FigureWidget source code is located at `js/src/widget.ts`, and is included in the built Python package at `plotly/package_data/widgetbundle.js`. 

### Configure black code formatting

This repo uses the [Black](https://black.readthedocs.io/en/stable/) code formatter,
and the [pre-commit](https://pre-commit.com/) library to manage a git commit hook to
run Black prior to each commit.  Both pre-commit and black are included in the
`requires-optional.txt` file, so you should have them
installed already if you've been following along.

To enable the Black formatting git hook, run the following from within your virtual
environment.

```bash
(plotly_dev) $ pre-commit install
```

Now, whenever you perform a commit, the Black formatter will run.  If the formatter
makes no changes, then the commit will proceed.  But if the formatter does make changes,
then the commit will abort.  To proceed, stage the files that the formatter
modified and commit again.

If you don't want to use `pre-commit`, then you can run black manually prior to making
a PR as follows.

```bash
(plotly_dev) $ black .
```

### Making a Development Branch

Third, *don't* work in the `main` branch. As soon as you get your main branch ready, run:

**DO THIS (but change the branch name)**
```bash
git checkout -b my-dev-branch
```

... where you should give your branch a more descriptive name than `my-dev-branch`

### Pull Request When Ready

Once you've made your changes (and hopefully written some tests, see below for more about testing...),
make that pull request!


## Update to a new version of Plotly.js
First update the version of the `plotly.js` dependency in `js/package.json`.

Then run the `updateplotlyjs` command with:

```bash
$ python commands.py updateplotlyjs
```

This will download new versions of `plot-schema.json` and `plotly.min.js` from
the `plotly/plotly.js` GitHub repository (and place them in
`plotly/package_data`). It will then regenerate all of the `graph_objs`
classes based on the new schema.

For dev branches, it is also possible to use `updateplotlyjsdev` in two configurations:

### CircleCI Release

If your devbranch is part of the official plotly.js repository, you can use
```bash
python commands.py updateplotlyjsdev --devrepo reponame --devbranch branchname
```
to update to development versions of `plotly.js`. This will fetch the `plotly.js` in the CircleCI artifact of the branch `branchname` of the repo `reponame`. If `--devrepo` or `--devbranch` are omitted, `updateplotlyjsdev` defaults using `plotly/plotly.js` and `master` respectively.

### Local Repository

If you have a local repository of `plotly.js` you'd like to try, you can run:

```bash
# In your plotly.js/ directory, prepare the package:

$ npm run build
$ npm pack
$ mv plotly.js-*.tgz plotly.js.tgz

# In your plotly.py/ directory:
$ python commands.py updateplotlyjsdev --local /path/to/your/plotly.js/
```

## Testing

To run tests, we use [`pytest`](https://docs.pytest.org/en/latest/), a powerful framework for unit testing.

### Running Tests with `pytest`

Since our tests cover *all* the functionality, to prevent tons of errors from showing up and having to parse through a messy output, you'll need to install `requires-optional.txt` as explained above.

After you've done that, go ahead and run the test suite!

```bash
pytest tests/
```

Or for more *verbose* output:

```bash
pytest -v tests/
```

Either of those will run *every* test we've written for the Python API. You can get more granular by running something like:

```bash
pytest tests/test_core/
```

... or even more granular by running something like:

```bash
pytest tests/test_plotly/test_plot.py
```

or for a specific test function

```bash
pytest tests/test_plotly/test_plot.py::test_function
```

### Writing Tests

You're *strongly* encouraged to write tests that check your added functionality.

When you write a new test anywhere under the `tests` directory, if your PR gets accepted, that test will run in a virtual machine to ensure that future changes don't break your contributions!
