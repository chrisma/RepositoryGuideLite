# Interactive data visualization using browser-based Jupyter notebooks

You can find the current deployment of this prototype at [nikkelm.github.io/RepositoryGuide-Python](https://nikkelm.github.io/RepositoryGuide-Python), along with all required files and information needed to run through the prototyped workflow.

TODO: Quickstart/How to use

## The idea/why we are doing this (The "RepositoryGuide")

The Enterprise System and Integration Concepts (EPIC) chair at the Hasso-Plattner-Institute (HPI) owns and develops the [RepositoryGuide](https://github.com/hpi-epic/repositoryguide) tool, which "aims at helping development teams gain insights into their teamwork based on the produced GitHub project data."
The tool accesses data that is publicly available through the Github API to provide a variety of data analytics and visualizations to inform development teams about their work processes.
Users can customize the tool by providing time intervals (sprints) and teams (Github users) which will be used during the analysis.

The major downside of the current implementation of the tool (written in Javascript) is its static nature.
Aside from the input provided through the configuration files, users cannot quickly make changes to the way the data is analyzed or visualized.
However, given that the way in which teams work differs greatly not only between organizations and teams, but even between projects, it is important to enable users to customize the tool to fit their needs.

Such customization should not stop at the ability to change color schemes or time intervals that are analyzed, but should also allow users to add or remove visualizations, change input parameters, or even add completely new types of data analytics.
Such major changes are not possible from within the tool with the current implementation, but would require users to download the tool's source code and host it on their own server.
All the same, it is not feasible to try and add such functionality to the current implementation, as its tech stack does not work well with the required level of interactivity and creative freedom. 

## What the tool should offer/goal of the project/Requirements

Instead, it was decided to try and prototype a new approach to the RepositoryGuide using Jupyter notebooks, which are known for their interactive nature.
Moving from Javascript to Python would also relax the barrier to entry for non-technical users, as Python and Jupyter notebooks in particular are already widely used in the data science community, which is certainly a group of users that would benefit from the RepositoryGuide.

In this initial prototype, we wanted to test the feasibility of using Jupyter notebooks as a replacement for the current implementation of the RepositoryGuide.
To do so, we wanted to meet the following requirements:

1. The tool *must* be implemented using Jupyter notebooks, to test their feasability as a more interactive replacement for the current implementation.
2. The tool *must not* require users to install or download any software.
Most notably, users *must not* be required to have a Python installation on their machine.
This is due to the fact that the tool is intended to be used by non-technical users, who are not to be expected to have any programming experience.
3. The tool *must* be able to run in a browser.
4. The tool *must* offer functionality to allow users to change the tool's functionality *through code* from within the tool itself, without the need to download any files.
5. The tool *must* be able to access data required for the analysis using the Github API, and not require users to provide more than the name of the repository in question to get started.
As a minimum requirement, the commit history for a given repository should be accessed.
6. The tool *must* provide at least one data visualization.
(Note: This was chosen to be a heatmap of weekly commit activity)
7. The tool *must* offer at least one interactive feature to influence the way the data is visualized using the previously mentioned visualization.
(Note: Among others, this was chosen to be the ability to switch between an hourly and time-of-day based visualization).
8. The abovementioned interactive feature *should* not require the user to refresh the page or re-run a code cell, but instead update the visualization in real-time.
9. The tool *should* offer the possibility to set a number of sprints and teams to be used to subdivide data during the analysis. 
10. These sprints and teams *should* be configurable interactively, without the need to provide extensive configuration files.
11. The tool *should* be presented in a way that is easy to understand and use for non-technical users.
Most importantly, the tool *should* only expose code cells when actively requested by the user, and otherwise only expose the interactive elements and visualizations.

## Tools we wanted to use (JupyterLite with Mercury: The perfect match?)

Two external tools prompted the idea for this prototype, as they alone would allow us to meet a majority of the requirements:

- [JupyterLite](https://github.com/jupyterlite/jupyterlite)
- [Mercury](https://github.com/mljar/mercury)

### JupyterLite

JupyterLite is a JupyterLab distribution that runs entirely in the browser, using a [Pyodide](https://pyodide.org/en/stable/) backed Python kernel running in a Web Worker.
Through the use of JupyterLite, we would be able to meet the requirements 1 through 4, as we can host the tool's Jupyter notebooks using JupyterLite, which allows users to access, run and modify them from within their browser, without the need to install any software.

### Mercury

Mercury is a tool that enables users to turn Jupyter notebooks into interactive web applications.
Using Mercury, we would be able to take the notebooks created or modified in JupyterLite, and turn them into an interactive app that would hide all code cells, and only expose the interactive elements and visualizations, as required by requirement 11.
You can think of Mercury as a way to "compile" Jupyter notebooks into a static web app, as JupyterLite is in itself still more of an IDE than a frontend which would be used by an end user.

The ideal workflow would be to first create the initial notebooks with the tool's basic functionality locally on a developer's machine, which would then be hosted on a server using Mercury.
This would be the frontend the user initially sees, and would allow them to use all of the tool's functionalities.
If needed or desired, the user would then be able to access and edit the underlying notebooks and thereby add, change or remove functionality using JupyterLite, which would allow them to modify the tool's code to their liking without the need to download any files or external tools, before again converting it to an interactive app using Mercury.

## Getting to work

So, we went ahead and set out to deploy a JupyterLite instance (to Github pages), which, thanks to a great [template repository](https://github.com/jupyterlite/demo) was no problem at all.
The basic JupyterLite deployment already offers a great host of features.
However, in order to be able to display interactive widgets (using [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/)) or create visualizations (using [plotly](https://plotly.com/python/)), we would need to install additional packages.
As we are working with Python, we can easily do so using a `requirements.txt` file. Such a file is already included in the template repository and initially contains packages that are required to run the JupyterLite instance itself.
The deployment workflow for JupyterLite installs all of those packages to the *backend* of the JupyterLite instance, which is the Python kernel running in the Web Worker.
However, in order to enable users to use those packages in their notebooks, they need to also be installed in the *frontend* of the JupyterLite instance, which is the user's local browser, as this is separated from the backend.
Luckily for us, it is quite easy to install additional pip packages form within a notebook using some pip magic:

```python
%pip install ipywidgets plotly pandas
```

Running this command in the first code cell of the notebook installs the required packages to the frontend of the JupyterLite instance, and allows us to use them in the notebook.

Now, as you may remember we had planned on using Mercury to turn the notebooks we create in JupyterLite into an interactive web app.
To do so, we would need to install the Mercury package and build our widgets and visualizations using it.
As we know by know, we can easily do so using pip magic:

```python
%pip install mercury
```

This is however where we run into the first limitations of JupyterLite (and Pyodide), which is after all still in the early stages of development and still unofficial, even though it is being developed by core Jupyter developers:
As Pyodide runs completely in the browser, there are a number of limitations to what kind of packages can be installed to the frontend (meaning, which kind of packages can be run in the browser-based notebooks), most prominently, only packages that provide a pure Python wheel (`.whl`) on PyPI can be installed.
Unfortunately, Mercury does not provide a pure Python wheel, which means that we would need to package and host the wheel ourselves, which would create a lot of overhead, especially as we would need to do so for every new version of Mercury.

For the moment however, as we saw Mercury at the core of our tool, we decided to go for it, and package and host the wheel ourselves.

### The sun sets on Mercury

After a number of hacks and workarounds (including hosting our own fork of Mercury on PyPi), Pyodide was finally ready to install Mercury in our JupyterLite instance.
However, this was not the end of our troubles, as we soon ran into another issues:
Mercury, as complex as it is, of course has a number of dependencies.
As you may have guessed, not all of those dependencies provide a pure Python wheel, which would mean that we would need to package and host those as well.
Rather fortunately for us, we were able to find the final nail in the coffin of our hopes to use Mercury before we could even think about doing so:
Some of Mercury's dependencies not only do not provide a pure Python wheel, but are not pure Python packages at all, containing a number of C extensions, which are unsupported by Pyodide.

Actually, this is not *entirely* true, as you are able to [create a Pyodide package](https://pyodide.org/en/stable/development/new-packages.html#new-packages) for such packages.
But by doing so - to stay true to our coffin analogy - we would only dig ourselves a deeper grave, as we would then need to package and host not only Mercury, but also a number of its dependencies (which, to be honest, will probably also have some dependencies that are incompatible with Pyodide), for some of which we would also need to create Pyodide packages, and so on and so forth.
All of this was simply not feasible for us, as firstly, the prototype was supposed to be up and running in only a few weeks, and secondly, juggling this type of dependency mess would certainly not be maintainable in the long run.

So we decided to put Mercury aside for the moment, and instead focus our attention on getting the prototype up and running using only JupyterLite (fortunately, this was not a major setback in the project, as Mercury was meant to be the nice "frontend" stuck on top of the JupyterLite instance, and not the core of the tool).

## The Github API

The next step would now be to implement the functionality that would allow us to fetch data from the Github API, which would then be used to create the interactive visualizations.
There are a number of Python implementations of the Github API, but we decided to go with [PyGithub](https://pygithub.readthedocs.io/en/latest/), as it is the most popular one, and also the one that is most actively maintained.
Again, we tried to install the PyGithub package using pip magic, and again, we ran into the problem that the package requires C extensions, in this case regarding cryptography, which is required for API requests such as signing commits.

For the same reasons that we had abandoned Mercury, we had to abandon the idea of using PyGithub (as well as any of its alternatives, which all have the same problem) within JupyterLite as well.
However, not all hope was lost as it was with Mercury, as the interaction with the Github API is not something that *must* be done in the browser to prove the concept of our tool.
At first, the idea was to simply fetch the data using PyGithub in a local environment, [pickle](https://docs.python.org/3/library/pickle.html) it, and then load it again in the JupyterLite instance.
This idea was quickly put to rest however, as we realized that in order to unpickle the data, we would need to still have PyGithub installed in our JupyterLite frontend, which as we know is not possible due to its C extensions.

Instead, we decided to go the simplest route possible, and simply dump the data retrieved using PyGithub to a JSON file, which we would then load in the JupyterLite instance.
This was not too far-fetched, as the Github API natively returns the data in JSON format.
PyGithub then parses this data to Python objects, but still keeps the original JSON data, which we can still access using the `raw_data` attribute of the respective objects.

## What we have now

## What we want to do next
