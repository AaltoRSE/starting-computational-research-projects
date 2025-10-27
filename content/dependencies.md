# Recording dependencies

```{objectives}
- Understand what dependency management tools can be useful for
- Discuss environment/requirements files in the context of reusability and
  reproducibility
```

```{questions}
- How can we communicate different versions of software dependencies?
```

```{instructor-note}
- 10 min teaching
- 10 min exercise/demo
```

Our codes often depend on other codes that in turn depend on other codes ...

- **Reproducibility**: We can version-control our code with Git but how should we version-control dependencies?
  How can we capture and communicate dependencies?
- **Dependency hell**: Different codes on the same environment can have conflicting dependencies.

```{figure} img/dependency.png
:alt: An image showing blocks (=codes) depending on each other for stability
:width: 60%

From [xkcd - dependency](https://xkcd.com/2347/). Another image that might be familiar to some of you working with Python can be found on [xkcd - superfund](https://xkcd.com/1987/).
```

---

## Dependency and environment management

Tools like **Conda, Anaconda, pip, virtualenv, Pipenv, pyenv, Poetry, renv** and files to record dependencies like **requirements.txt** and **environment.yml** try to solve the following problems:

- **Defining a specific set of dependencies**
- **Installing those dependencies** mostly automatically
- **Recording the versions** for all dependencies
- **Isolate environments**
  - On your computer for projects, so they can use different software
  - Isolate environments on computers with many users (and allow self-installations)
- Using **different package versions** per project (also, e.g., Python/R versions)
- Provide tools and services to **share packages**

Isolated environments are also useful because they help you make sure
that you know your dependencies!

**If things go wrong, you can delete and re-create** - much better
than debugging. The more often you re-create your environment, the
more reproducible it is.

---

## Examples of recorded dependencies

``````{challenge} Dependencies-1: Time-capsule of dependencies
Situation: 5 researchers (A, B, C, D, E) wrote code that depends on a couple of libraries.
They uploaded their projects to GitHub. We now travel 3 years into the future
and find their GitHub repositories from the respective publications. We would like to
try to re-run their code before adapting it.
Which of the following do you think you will get to work?

  `````{tabs}
    ````{group-tab} Conda
      **A**:
      You find a couple of library imports across the code but that's it.

      **B**:
      The README file lists which libraries were used.

      **C**:
      You find a `environment.yml` file with:
      ```
      name: student-project
      channels:
        - conda-forge
      dependencies:
        - scipy
        - numpy
        - sympy
        - click
        - python
        - pytorch
        - pip
        - pip:
          - git+https://github.com/someuser/someproject.git@master
          - git+https://github.com/anotheruser/anotherproject.git@master
      ```

      **D**:
      You find a `environment.yml` file with:
      ```
      name: student-project
      channels:
        - conda-forge
      dependencies:
        - scipy=1.3.1
        - numpy=1.16.4
        - sympy=1.4
        - click=7.0
        - python=3.8
        - pytorch=1.10
        - pip
        - pip:
          - git+https://github.com/someuser/someproject.git@d7b2c7e
          - git+https://github.com/anotheruser/anotherproject.git@sometag
      ```

      **E**:
      You find a `environment.yml` file with:
      ```
      name: student-project
      channels:
        - conda-forge
      dependencies:
        - scipy=1.3.1
        - numpy=1.16.4
        - sympy=1.4
        - click=7.0
        - python=3.8
        - pytorch=1.10
        - someproject=1.2.3
        - anotherproject=2.3.4
      ```
    ````

    ````{group-tab} Python virtualenv
      **A**:
      You find a couple of library imports across the code but that's it.

      **B**:
      The README file lists which libraries were used.

      **C**:
      You find a `requirements.txt` file with:
      ```
      scipy
      numpy
      sympy
      click
      python
      git+https://github.com/someuser/someproject.git@master
      git+https://github.com/anotheruser/anotherproject.git@master
      ```

      **D**:
      You find a `requirements.txt` file with:
      ```
      scipy==1.3.1
      numpy==1.16.4
      sympy==1.4
      click==7.0
      python==3.8
      git+https://github.com/someuser/someproject.git@d7b2c7e
      git+https://github.com/anotheruser/anotherproject.git@sometag
      ```

      **E**:
      You find a `requirements.txt` file with:
      ```
      scipy==1.3.1
      numpy==1.16.4
      sympy==1.4
      click==7.0
      python==3.8
      someproject==1.2.3
      anotherproject==2.3.4
      ```
    ````

    ````{group-tab} R
      **A**:
      You find a couple of `library()` or `require()` calls across the code but that's it.

      **B**:
      The README file lists which libraries were used.

      **C**:
      You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
      ```
      Imports:
          dplyr,
          tidyr
      ```
      In addition you find these:
      ```r
      remotes::install_github("someuser/someproject@master")
      remotes::install_github("anotheruser/anotherproject@master")
      ```

      **D**:
      You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
      ```
      Imports:
          dplyr (== 1.0.0),
          tidyr (== 1.1.0)
      ```
      In addition you find these:
      ```r
      remotes::install_github("someuser/someproject@d7b2c7e")
      remotes::install_github("anotheruser/anotherproject@sometag")
      ```

      **E**:
      You find a [DESCRIPTION file](https://r-pkgs.org/description.html) which contains:
      ```
      Imports:
          dplyr (== 1.0.0),
          tidyr (== 1.1.0),
          someproject (== 1.2.3),
          anotherproject (== 2.3.4)
      ```
    ````

  `````

  `````{solution}
  **A**: It will be tedious to collect the dependencies one by one. And after
  the tedious process you will still not know which versions they have used.

  **B**: If there is no standard file to look for and look at, it might
  become very difficult to create the software environment required to
  run the software. At least we know the list of libraries, but we don't
  know the versions.

  **C**: Having a standard file listing dependencies is definitely better
  than nothing. However, if the versions are not specified, you or someone
  else might run into problems with dependencies, deprecated features,
  changes in package APIs, etc.

  **D** and **E**: In both of these cases exact versions of all dependencies are
  specified and one can recreate the software environment required for the
  project. One problem with the dependencies that come from GitHub is that
  they might have disappeared (what if their authors deleted these
  repositories?).

  **E** is slightly preferable because version numbers are easier to understand than Git
  commit hashes or Git tags, but is most often out of scope for research projects, as it
  requires a significant overhead to submit these versions to the respective repositories

  `````
``````

## The necessary information for reproducibility of computational results

When it comes to computational results there are several factors that can come into play

1. Software
2. Hardware

### Hardware

If possible (e.g. the code was run on a local machine), provide the make/model of the CPU and GPU used for the computations. unfortunately those can have an effect
on the actual results, and it allows others to estimate run times on their hardware.

### Software

For all of the following assume that we also want version numbers.

- Operating System (e.g. Windows 10, Windows 11, Ubuntu Linux 22.04, Red Hat Linux 9.3, Mac OS X 15... )
- Compilers/Interpreters (e.g. gcc, python, matlab etc)
- Libraries (what we talked about above).

If you use conda/mamba, there are some very useful functions that can extract what is currently in your environment.

1. `conda env export`
   This command will export the precise list of conda controlled packages in your environment down to the build version.
   For reproducibility I always recommend to provide this file, since it is the only version that shows someone precisely what you used.
   However, this export is often OS specific, if third-language libraries are used because the builds need to be created per operating system.
2. `conda env export --no-builds`
   This command lists the environment stripping the build version and should (often) be installable on any operating system
   However some dependencies are operating system dependent and thus even this version might not be installable on a differen OS.
3. `conda env export --from-history`
   This will list everything that you actively installed into the environment, but not the dependencies of what you wanted.
   This often leads to a minimal environment file, which is most likely installable anywhere. However, this does NOT
   list versions, as it simply repeats what you had in your environment file and any addiitonal `conda install xyz` or `pip install xyz` commands
   you ran.

In conclusion, We recommend to provide:

1. The environment file generated from `conda env export`.
2. The environment file generated from `conda env export --from-history` modified such, that the versions (not builds) listed in the full export
   are added to the file.

The former contains all information available about what you were running, the latter is a simple way to reproduce your code with the minimal amount of
information required to obtain all relevant dependencies.

```{keypoints}
- Recording dependencies with versions can make it easier for the next person to execute your code.
- There are many tools to record dependencies and separate environments.
```
