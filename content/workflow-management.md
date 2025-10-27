# Recording computational steps

```{objectives}
- Know what is the minimal amount of information necessary to reproduce your results
- How can you make it easier for others (or your later self) to reproduce your work
- Understand why and when a workflow management tool can be useful
```

```{questions}
- You have some steps that need to be run to do your work.  How do you
  actually run them?  Does it rely on your own memory and work, or is it
  reproducible? **How do you communicate the steps** for future you and others?
- How can we create a reproducible workflow?
```

```{instructor-note}
- 5 min teaching
- 15 min exercise/demo
```

## Several steps from input data to result

_The following material is partly derived from a [HPC Carpentry lesson](https://hpc-carpentry.github.io/hpc-python/)._

In this episode, we will use an [example
project](https://github.com/coderefinery/word-count) which finds frequent
words in books and plots the result from those statistics. In this example we
wish to:

1. Analyze word frequencies using [code/count.py](https://github.com/coderefinery/word-count/blob/main/code/count.py)
   for 4 books
   (they are all in the [data](https://github.com/coderefinery/word-count/tree/main/data) directory).
2. Plot a histogram using [plot/plot.py](https://github.com/coderefinery/word-count/blob/main/plot/plot.py).

```{figure} img/word-count/arrows.png
:alt: From book to word counts to plot
:width: 100%
```

Example (for one book only):

```console
$ python code/count.py data/isles.txt > statistics/isles.data
$ python code/plot.py --data-file statistics/isles.data --plot-file plot/isles.png
```

Another way to analyze the data would be via a graphical user interface (GUI), where you can for example drag and drop files and click buttons to do the different processing steps.

We can also express the workflow for all books with a script. The repository includes such script called `run_all.sh`.

We can run it with:

```console
$ bash run_all.sh
```

These approaches are fine, when only a few steps are needed, or only a few repetitons are required, but become infeasible when large amounts of data need to be processed.
E.g. you don't want to do either approach with 500 different books. Clicking 500 times, or having 500 copies of lines with small modifications is bound to introduce typos or other errors.
How could we deal with this?

- Loops with automated argument lists or other approaches to specify the inputs
- Workflow managers

The simpler way, to just get reproducible results is to have tools generate the inputs automatically e.g. using "one folder/file per input" approaches.
This will lead to reproducible results, but requires that for every change everything has to be re-run. E.g. when adding another 10 datapoints, if your script just checks
what is there, it will re-run the analysis for all 1000 other elements as well, and if you start adding more arguments to your script, you risk the forgetting elements or
having typos again.
The advantage of this approach, however, is that you can easily build a executable manuscript file from such a script (like jupyter notebooks, or matlab live code).

## Workflow managers

Workflow managers in contrast create flows that, in general, keep track on what needs to be executed. E.g. [snakemake](https://snakemake.readthedocs.io/)
will keep track of what has been processed and only re-run those parts of it's flow that need updates.
If properly defined (e.g. source files being inputs of steps), it will re-run all analysis starting from a modified step, which makes
results more dependable, since you can't forget to "run that one new pre-processing step" for some old input data.
An example of how it can be used on triton (which is also generally applicable) can be found [here](https://github.com/AaltoRSE/snakemake-triton-example)

---

## Additional Tools

- [Make](https://www.gnu.org/software/make/)
- [Nextflow](https://www.nextflow.io/)
- [Task](https://taskfile.dev/)
- [Common Workflow Language](https://www.commonwl.org/)
- Many [specialized frameworks](https://github.com/common-workflow-language/common-workflow-language/wiki/Existing-Workflow-systems) exist.
- [Book on building reproducible analytical pipelines with R](https://raps-with-r.dev/)
- [{targets} R package - make-like pipeline tool for R](https://books.ropensci.org/targets/)

```{keypoints}
- Computational steps can be recorded in many ways.
- Workflow tools can help if there are many steps to be executed and/or many datasets to be processed.
```
