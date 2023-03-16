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

- The tool *must* be implemented using Jupyter notebooks, to test their feasability as a more interactive replacement for the current implementation.
- The tool *must not* require users to install or download any software. Most notably, users *must not* be required to have a Python installation on their machine. This is due to the fact that the tool is intended to be used by non-technical users, who are not to be expected to have any programming experience.
- The tool *must* be able to run in a browser.
- The tool *must* be able to access data required for the analysis using the Github API, and not require users to provide more than the name of the repository in question to get started. As a minimum requirement, the commit history for a given repository should be accessed.
- The tool *must* provide at least one data visualization. (Note: This was chosen to be a heatmap of weekly commit activity)
- The tool *must* offer at least one interactive feature to influence the way the data is visualized using the previously mentioned visualization. (Note: Among other, this was chosen to be the ability to switch between an hourly and time-of-day based visualization).
- The abovementioned interactive feature *must* not require the user to refresh the page or re-run a code cell, but instead update the visualization in real-time.

## Tools we wanted to use

## Problems with the initial approach

## Compromises we had to make

## What we have now

## What we want to do next


[^1]: https://github.com/hpi-epic/repositoryguide, https://repositorygui.de/