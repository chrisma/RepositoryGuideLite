# Interactive Data visualization using browser-based Jupyter notebooks

You can find the current deployment of this prototype at [nikkelm.github.io/RepositoryGuide-Python](https://nikkelm.github.io/RepositoryGuide-Python), along with all required files and information needed to run through the prototyped workflow.

TODO: Quickstart/How to use

## The idea/why we are doing this

The Enterprise System and Integration Concepts (EPIC) chair at the Hasso-Plattner-Institute (HPI) owns and develops the RepositoryGuide[^1] tool, which "aims at helping development teams gain insights into their teamwork based on the produced GitHub project data."
The tool accesses data that is publicly available through the Github API to provide a variety of data analytics and visualizations to inform development teams about their work processes.
Users can customize the tool by providing time intervals (sprints) and teams (Github users) which will be used during the analysis.

The major downside of the current implementation of the tool (written in Javascript) is its static nature.
Aside from the input provided through the configuration files, users cannot quickly make changes to the way the data is analyzed or visualized.
However, given that the way in which teams work differs greatly not only between organizations and teams, but even between projects, it is important to enable users to customize the tool to fit their needs.

Such customization could range from on-the-fly changes to the date range a visualization displays, to adding completely new types of data analytics not previously contained in the tool, all of which is currently not possible.

## What the tool should offer/goal of the project/Requirements

The goal of this project was to create a prototypical re-implementation of the RepositoryGuide tool using Jupyter notebooks, known for their interactive nature.
The tool should meet the following requirements:

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
9. The tool *should* offer the possibility to set a number of sprints and teams to be used during the analysis. 
10. These sprints and teams *should* be configurable interactively, without the need to provide extensive configuration files.
11. The tool *should* be presented in a way that is easy to understand and use for non-technical users.
Most importantly, the tool *should* only expose code cells when actively requested by the user, and otherwise only expose the interactive elements and visualizations.

## Tools we wanted to use

Two external tools prompted the idea for this prototype, as they alone would allow us to meet a majority of the requirements:

- [JupyterLite](https://github.com/jupyterlite/jupyterlite)
- [Mercury](https://github.com/mljar/mercury)

### JupyterLite

JupyterLite is a JupyterLab distribution that runs entirely in the browser, using a [Pyodide](https://pyodide.org/en/stable/) backed Python kernel running in a Web Worker.
Through the use of JupyterLite, we would be able to meet the requirements 1 through 4, as we can host the tool's Jupyter notebooks using JupyterLite, which allows users to access, run and modify them from within their browser, without the need to install any software.

### Mercury

Mercury is a tool that enables users to turn Jupyter notebooks into interactive web applications.
Using Mercury, we would be able to take the notebooks created or modified in JupyterLite, and turn them into an interactive app that would hide all code cells, and only expose the interactive elements and visualizations, as required by requirement 11.

The ideal workflow would be to first create the initial notebooks with the tool's basic functionality locally on a developer's machine, which would then be hosted on a server using Mercury.
This would be the tool the user initially sees, and would allow them to use all of the tool's functionalities.
If needed or desired, the users would then be able to access the underlying notebooks and thereby add, change or remove functionality using JupyterLite, which would allow them to edit and modify the tool's code to their liking without the need to download any files or external tools, before again converting it to an interactive app using Mercury.


## Problems with the initial approach

## Compromises we had to make

## What we have now

## What we want to do next


[^1]: https://github.com/hpi-epic/repositoryguide, https://repositorygui.de/