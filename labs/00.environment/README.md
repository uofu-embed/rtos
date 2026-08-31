# Lab 0: Build ALL THE THINGS!
In this lab, we'll be setting up the development environment. This environment will allow you to build, test, deploy, and manage your code.
## Learning objective:
- Navigate documentation
- Setup toolchain
- Create a project.
- Commit to source control
- Build project
- Run tests
- Flash a program
- Use the debugger
- Setup CICD
# Prelab
Complete the following tasks before lab. **This will require a large download and long installation, so do it before lab**. At the end of this section, you should have the tools installed and a working project skeleton.

## Reading
Read the documentation about pull requests.

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests

https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request


## Tool installation
Follow the [installation guide](../../SETUP.md) in the top directory of this repository.

# Pico blink project
### Overview

Build tools use a set of conventions to setup a project. This usually defines a consistent organization for source code, header, and test code. The configuration of the project, library dependencies, along with any custom build tasks, will be located in the build file. The Raspberry Pi Pico uses the CMake tool to configure and run the build process. The build file `CMakeLists.txt` is located at the root of the project. The build file will be committed to your git repository with the rest of your source code. Anyone else working on the project will be able to use the same project configuration by simply checking out the code.

### Tasks
Clone the template repository. https://github.com/uofu-embed/rtos.template

To build the code, you will use CMake to setup the build configuration and then execute the build process.
There is some brief docs [here on using CMake](../../CMAKE.md).
Try to load the example code onto the board following the instructions.

- Appendix C in the Pico getting started guide has some additional information. https://datasheets.raspberrypi.com/pico/getting-started-with-pico.pdf

# Lab
# Commit project
## Overview
In this activity, practice creating and manipulating git repositories.
## Tasks
1. Create repository on github
    1. Do *not* initialize the repository with a README, gitignore, or license.
    1. You only need one repository for your group.
1. Initialize the local repository.
    1. From your project directory, run `git init`. This sets up the repository metadata.
    1. Create a file named `.gitignore` to tell git not to track IDE and build files (see below for example contents)
    1. Stage all of the files in the current working directory `git add .`
    1. Commit the staged files with a descriptive commit message `git commit -m "Initialize Pico project"`
    1. Create a main branch `git branch -M main`
    1. Add your github repository as a remote. `git remote add origin git@github.com:username/lab0.git`
    1. Push the main branch and the commit to the remote. `git push -u origin main`

### .gitignore sample
```
.vscode/
**/build/
```
This exercise is to help get familiar with the git workflow. In the next activities, we'll start with an existing project template.

# Referencing libraries.
If you haven't already, clone the template repository. https://github.com/uofu-embed/rtos.template

We will be using external libraries over the course of the labs.

1. https://github.com/ThrowTheSwitch/Unity
1. https://github.com/FreeRTOS/FreeRTOS-Kernel
1. https://github.com/raspberrypi/pico-sdk

If you try and build the project without these, you will get an error indicating the library cannot be found.

## Tasks
1. Check the CMakeLists.txt file in the top level of your repository. Look for the environment variables which specify the locations for the libraries.
1. The template repo constains git submodules for FreeRTOS and for pico-sdk. Initialize the submodules and see what happens. See https://git-scm.com/book/en/v2/Git-Tools-Submodules
1. You can either check out a single global copy somewhere in your filesystem and set the environment variables in your `.profile`, or you can use the submodules. It is recommended you use a global version to save time and disk space.
1. The template repo does not contain a submodule for Unity test framework. Add the submodule into `lib`.

Submodules are one way of referencing library code, particularly for C based projects. Modern languages will have a library management system (e.g. rust has cargo, python has pip, java has gradle).

# Making changes to a project

## Tasks
1. Create a new git branch. You can call it whatever you like, but it is useful to use a descriptive name for what is being developed in the branch.`git checkout -b compilation-demo`
1. Make the modifications in section 4.2 of the getting started datasheet.
1. Build and deploy the changes.
1. Commit the changes to the main file, and push the branch to your github remote.

# Setup Continuous Integration
## Overview
Continuous Integration (CI), often paired with Continuous Delivery (CI/CD), is a development pattern to rapidly deliver consistent working software. The basic principle is to always keep your project in a working state, with small incremental changes. The project should be monitored with automated tests and performance instrumentation. When code is pushed to the central repository, a build system will run the automated tests. If the tests pass, the code is ready to be reviewed and then deployed.

Github provides an automated build system called Github Actions. We will set up your project to build and run tests on push. We will then show the passing or failing status of your tests on your repository README.
## Tasks
1. Create the metadata directories. In the root of your repository, create a directory `.github` (note the leading dot). Inside that directory, create another directory `workflows`.
1. Create a workflow.
    1. Workflows are defined in yaml configuration files, which define a series of steps that will be executed in response to an event, such as a push.
    1. Create a file `.github/workflows/main.yml`. Add the configuration in the [main.yml](main.yml) file inside this directory of the class repo.

https://docs.github.com/en/actions/learn-github-actions

1. Commit the file and push.
1. Add test status badge to your repo README.
    1. Create a file named `README.md` in the root of your project.
    1. Add a brief description of you project.
    1. Add a badge image showing the current status of the workflow
```
![example workflow](https://github.com/<OWNER>/<REPOSITORY>/actions/workflows/main.yml/badge.svg)
```
https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge
    1. Commit your README and push.
    1. Open a pull request to merge your branch into main branch.

# Pull requests.

As you should have reviewed in the prelab, a (pull request)[https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests] is a procedure for having your code reviewed before being merged into the main branch.
Members of the team responsibles for maintaining the repository will (review your changes)[https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests].
This is why you do your work in a branch or a fork. Your work is done in isolation, and then brought into the "official" branch of the project (usually called _main_ or _master_).
Once the changes are reviewed and the code is ready, the changes will be (incorporated into the main branch)[https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request].

For open source projects, this is a way for community members who are not part of the team to propose bug fixes and new features.
In a corporate environment, the review process could be required as part of the coprorate policy.
This may be legally required as part of company policy or by government regulation.

In this class, you will do all your work on a branch. Once your work is ready, check the assignment page on Canvas, which will list the other team your team will be doing reviews with. You'll create a pull request on the Github website. Add all the members of the other team as reviewers. Once *all* members of the other team have approved the pull request, you'll merge the branch using the pull request page on Github. Once this is done, submit the URL to your repo on Canvas.

# Reference implementation
A reference implementation is available at https://github.com/uofu-embed/rtos.00
