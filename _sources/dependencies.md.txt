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

## Exercise / Demo

``````{challenge} Dependencies-1: Time-capsule of dependencies
Situation: 5 students (A, B, C, D, E) wrote a code that depends on a couple of libraries.
They uploaded their projects to GitHub. We now travel 3 years into the future
and find their GitHub repositories and try to re-run their code before adapting
it.

Answer in the collaborative document:

- Which version do you expect to be easiest to re-run? Why?
- What problems do you anticipate in each solution?

  `````{tabs}
    ````{group-tab} Conda
      **A**:
      You find a couple of library imports across the code but that's it.

      **B**:
      The README file lists which libraries were used but does not mention
      any versions.

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
        - someproject=1.2.3
        - anotherproject=2.3.4
      ```
    ````

    ````{group-tab} Python virtualenv
      **A**:
      You find a couple of library imports across the code but that's it.

      **B**:
      The README file lists which libraries were used but does not mention
      any versions.

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
      The README file lists which libraries were used but does not mention
      any versions.

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

    ````{group-tab} Matlab
    Can you please contribute an example?
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
  commit hashes or Git tags.
  `````
``````


```{keypoints}
- Recording dependencies with versions can make it easier for the next person to execute your code.
- There are many tools to record dependencies and separate environments.
```
