<div id="top"></div>

# Atoms

This repository contains the de.STAIR atoms that are incorporated in the
[Galaxy workflow generator](https://github.com/destairdenbi/galaxy-workflow-generator),
to assist users in the analysis of RNA-Seq and BS/RRBS-Seq data.

- [How it works](#how-it-works)
- [How to contribute](#how-to-contribute)
  - [Set up Docker](#set-up-docker)
  - [When some tools are missing](#when-some-tools-are-missing)
  - [When every tool is there](#when-every-tool-is-there)
  - [Test new atoms](#test-new-atoms)


## How it works

Atoms are interactive Galaxy tours that illustrate one or more tools within the
context of an experimental setup.  
Alternative atoms are therefore provided to describe the usage and
parameterization of alternative tools to solve the same biocomputational
problem.  

In the Galaxy workflow generator, each atom describes a tool (or series
thereof) to solve a *task* of the desired data analysis.  

For instance, to carry out the tasks of an RNA-Seq analysis:
1. Quality control and data preprocessing
2. Genome alignment
3. Transcript quantification and differential gene expression

alternative atoms can be built using the following Galaxy tools:

<p align="center">
  <img align="center"
    src="web/atoms.png"
    width="600px"
    alt="Sample alternative atoms to complete a task in an RNA-Seq analysis"
    valign="top"/>
  <br />
  <div align="center">
Where <sup>X,Y,Z,W</sup> are alternative parameterizations.
  </div>
</p>

Doing so, users can be informed about the availability of alternative
strategies to carry out the desired analysis, and select the most appropriate
atom within the context of their experimental setup.
<p align="right"><a href="#top">&#x25B2; back to top</a></p>


## How to contribute

Atoms can be tested, modified, and extended. The following sections will help
you set up the Galaxy workflow generator to contribute with new atoms.
<p align="right"><a href="#top">&#x25B2; back to top</a></p>


### Set up Docker

Set up Docker by following the same [installation requirements](https://github.com/destairdenbi/galaxy-workflow-generator#installation-requirements)
needed to run the Galaxy workflow generator.
<p align="right"><a href="#top">&#x25B2; back to top</a></p>


### When some tools are missing

New tools can be added in the Galaxy workflow generator by installing them...
1. From the admin panel (may load to unexpected errors upon execution)
2. Via re-building the docker image

Clone the [Galaxy workflow generator](https://github.com/destairdenbi/galaxy-workflow-generator):
```
$ git clone https://github.com/destairdenbi/galaxy-workflow-generator.git
```

Enter the cloned repository, and edit the ``tools.yaml`` file. New entries are
described in the Markdown format.  
The minimum set of metadata to be provided for each tool comprise:
- name
- owner
- tool_panel_section_label
- revisions

If unsure, please refer to the [Galaxy Toolshed](https://toolshed.g2.bx.psu.edu/).  

Once edited, you can build the Docker container locally:
```
$ docker build -t destair-local:latest .
```

Now run your local image and access it via a web browser:
```
$ docker run -d -p 8080:80 --name destair -v /absolute/path/to/local/directory/:/export destair-local:latest
```

for more parameters and further help, consult
[these instructions](https://github.com/destairdenbi/galaxy-workflow-generator#run-the-container).

<p align="right"><a href="#top">&#x25B2; back to top</a></p>


### When every tool is there

Atoms are interactive tours that illustrate one or more Galaxy tools.  
For ease of use within the context of specific experimental setups, we suggest
to build atoms that solve entire tasks of a target analysis, *i.e.* Adapter
clipping, Quality control, Genome alignment, Transcript quantification, and so
on.  

You can create atoms by leveraging on the [Galaxy Tour Builder](https://github.com/TailorDev/galaxy-tourbuilder).
However, be aware that the Tour Builder cannot identify elements with a missing
``tour_id`` attribute, which means that the resulting tour's bubbles will not
always be placed on the HTML elements of interest. This limitation is
particularly visible on those elements that require an explicit user input,
such as dropdown menu selectors, checkboxes, etc.  
To overcome this problem, we defined some templates that can be used to place
the interactive tour's bubble on the actual HTML elements of interest.  
Check our [TEMPLATES.md](https://github.com/destairdenbi/galaxy-atoms/blob/master/TEMPLATES.md)
file for examples and use cases.  

If your atom needs to be placed within an analysis which is not yet included in
the Galaxy workflow generator, you need to create a new *linker file*. Look at
the examples in the repository.  
Otherwise, name the contributed atom with the label ``dgea_Xx.yaml``, or
``bs_Xx.yaml``. Where ``X`` is the task number, and ``x`` an identifier of your
choice.

<p align="right"><a href="#top">&#x25B2; back to top</a></p>


### Test new atoms

New atoms can be tested by running a Galaxy Docker container using a *bind
mount*, or by running the [Galaxy workflow generator](https://github.com/destairdenbi/galaxy-workflow-generator)
using the interactive [plugin](https://github.com/destairdenbi/galaxy-webhooks).

<p align="right"><a href="#top">&#x25B2; back to top</a></p>

#### Option 1: Docker bind mount method

Run the Galaxy Docker container by creating a bind mount:
```
$ docker run -d -p 8080:80 --name destair -v /absolute/path/to/local/directory/:/export/ quay.io/destair/galaxy-workflow-generator:latest
```

for more parameters and further help, consult
[these instructions](https://github.com/destairdenbi/galaxy-workflow-generator#run-the-container).  

Once the container is ready, you will be able to copy the new atom into
```
/absolute/path/to/local/directory/export/galaxy-central/config/plugins/tours/
```
Now, either restart the running Galaxy instance and access it via a web browser
locally ``localhost:8080``:
```
$ docker exec destair supervisorctl restart galaxy:
```
or login to your Galaxy instance as administrator and run our de.STAIR plugin
once with ``Update tours DB`` admin option checked.

New atoms will be automatically offered by our plugin and can be found by
navigating the Galaxy interface under the header section ``Help -> Interactive
Tours``.

<p align="right"><a href="#top">&#x25B2; back to top</a></p>

#### Option 2: Local Galaxy setup method

- [Set up the Galaxy environment](https://github.com/destairdenbi/galaxy-webhooks#set-up-the-galaxy-environment)
- [Set up the de.STAIR atoms](https://github.com/destairdenbi/galaxy-webhooks#set-up-the-destair-atoms)
- Contribute new atoms as stated above
- [Set up the de.STAIR plugin](https://github.com/destairdenbi/galaxy-webhooks#set-up-the-destair-plugin)
- [Run the Galaxy framework](https://github.com/destairdenbi/galaxy-webhooks#run-the-galaxy-framework)

<p align="right"><a href="#top">&#x25B2; back to top</a></p>
