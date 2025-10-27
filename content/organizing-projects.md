# Organizing your projects

```{objectives}
- Understand how to organize research projects
- Get an overview of tools for collaborative and version controlled manuscripts
```

```{instructor-note}
- 10 min teaching incl. discussions
```

## Reproducible publications

### Tools for collaborative writing and version control of manuscripts

On the very top of the pyramid we intoduced rests the paper that is the published result of your work. Even if it is based on a solid foundation, additional
safeguards are important.
-> Consider using **version control for manuscripts** as well. It may help you when keeping track of edits + if you sync it online then you don't have to worry about losing your work.

Git **can** be used to collaborate on manuscripts written in, e.g., LaTeX and other text-based formats. However it might not always be the most convenient.
Other tools exist to make the process more enjoyable:

You can **collaboratively gather notes** using self-hosted or public instances of tools like [HedgeDoc](https://hedgedoc.org/) and [Etherpad](https://etherpad.org)
or use online options like [HackMD](https://hackmd.io/), [Google Docs](https://docs.google.com) or the Microsoft online tools for easy and efficient collaboration.
These tools are convenient for brainstorming or getting a quick place to share ideas also with external collaborators.

At Aalto we also have [Sharepoint](https://aaltofi.sharepoint.com/), which allows collaborative editing of MS Office documents, if this is your preferred format.
Aalto also provides an [Overleaf](https://www.overleaf.com) (LaTeX) subscription. Overleaf is a collaborative latex editor and Aalto provides several templates on the platform (e.g. Thesis templates, base Article templates etc).

The huge advantages:

- You can work in parallel without duplicating work (if using a collaborative editor)
- You have time stamps of when ideas have been put forward (when there is a contention of originality)

### Executable manuscripts

You may also want to consider writing an executable manuscript using tools like [Jupyter Notebooks](https://jupyter.org) hosted on [Binder](https://mybinder.org), [Quarto](https://quarto.org/), [Authorea](https://www.authorea.com) or [Observable](https://observablehq.com/), to name a few.

One of the first steps to make your work reproducible is to organize your projects well.
Let's go over some of the basic things which people have found to work (and not to work).

## Organizing projects

### What to get when you start

The following chart gives an overview of what data storage places you should set up / request if you start a new project

```{figure} img/data_flow_chart.svg
:alt: Flow chart, showing that teamwork is strictly needed, a version.aalto.fi repository needed, when code is involved and a triton data folder if hpc is required
:width: 100%
```

It's important to note, that files in personal folders of students might be difficult to recover if the student leaves. Therefore always have project folders that are
owned by the principal investigator and have your staff/students use these folders for storing project data or code.

### Directory structure for projects

- Project files in a **single directory**, this is your teamwork folder.
- **Different projects** should have **separate directories**, different teamwork folders, different gitlab repositories
- Use **consistent and informative directory structure**
  - Avoid spaces in directory and file names – use `-`, `_` or CamelCase instead (nicer for computers to handle)
- If you need to separate public/private data in gitlab:
  - For secrets (e.g. API keys, access tokens or similar information):
    - use `.gitignore` to exclude the private information from being tracked
  - For code you don't want to have public yet:
    - create a separate public repository, which only contains the code that is publicly available.
    - Do NOT create a fork for this but a separate repository. Note, that this public repository is your "ground truth"
      for anyone else and should reflect the version in use for publications
- Add a **README file** to describe the project and instructions on reproducing the results
- If you want to use the **same code in multiple projects**, host it on GitHub (or similar) and clone it into each of your project directories

A project directory can look something like this:

```shell
project_name/
├── README.md             # overview of the project
├── data/                 # data files used in the project
│   ├── README.md         # describes where data came from
│   └── sub-directory/       # may contain subdirectories
├── processed_data/       # intermediate files from the analysis
├── manuscript/           # manuscript describing the results
├── results/              # results of the analysis (data, tables, figures)
├── src/                  # contains all code in the project (tracked with gitlab)
│   ├── LICENSE           # license for your code
│   ├── requirements.txt  # software requirements and dependencies
│   └── ...
└── doc/                  # documentation for your project
    ├── index.rst
    └── ...
```

---

## Tracking source code, data, and results

- All code is version controlled and goes in the `src/` or `source/` directory
- Include appropriate LICENSE file and information on software requirements
- You can also version control data files or input files under `data/`
  - If data files are too large (or sensitive) to track, untrack them using `.gitignore`
- Intermediate files from the analysis are kept in `processed_data/` and should be in `.gitignore`
- Consider using Git tags to mark specific versions of results (version
  submitted to a journal, dissertation version, poster version, etc.):
  ```console
  $ git tag -a thesis-submitted -m "this is the submitted version of my thesis"
  ```

Check the [Git-intro lesson](https://coderefinery.github.io/git-intro/) for a reminder.

## Some tools and templates

- [R devtools](https://devtools.r-lib.org/)
- [Python cookiecutter template](https://github.com/Materials-Data-Science-and-Informatics/fair-python-cookiecutter)
- [Reproducible research template](https://github.com/the-turing-way/reproducible-project-template) by the Turing Way

More tools and templates in [Heidi Seibolds blog](https://heidiseibold.ck.page/posts/setting-up-a-fair-and-reproducible-project).

### Resources on research compendia

- [About research compendia at the Turing Way](https://book.the-turing-way.org/reproducible-research/compendia)
- ["Research compendia"](http://inundata.org/talks/rstd19/#/): a set of good practices for reproducible data analysis in R, but much is transferable to other languages.
- [rrtools](https://github.com/benmarwick/rrtools): instructions, templates, and functions for writing a reproducible article or report with R.
- ...

```{keypoints}
- An organized project directory structure helps with reproducibility.
- Also think about version control for writing your academic manuscripts.
```
