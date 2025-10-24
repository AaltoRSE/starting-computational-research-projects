# Recording environments

```{objectives}
- Understand what containers are and what they are useful for
- Discuss container definitions files in the context of reusability and
  reproducibility
```

```{instructor-note}
- 10 min teaching/discussion
- 10 min demo
```

## What is a container?

Imagine if you didn't have to install things yourself, but instead you could
get a computer with the exact software for a task pre-installed. Containers
effectively do that, with various advantages and disadvantages. They are
**like an entire operating system with software installed, all in one file**.

```{figure} img/docker_meme.jpg
:alt: He said, then we will ship your machine. And that's how Docker was born.
:width: 60%

From [reddit](https://www.reddit.com/r/ProgrammerHumor/comments/cw58z7/it_works_on_my_machine/).
```


## From definition files to container images to containers

- Containers can be built to bundle _all the necessary ingredients_ (data, code, environment, operating system).
- A container image is like a piece of paper with all the operating system on it. When you run it,
  a transparent sheet is placed on top to form a container. The container runs and writes only on
  that transparent sheet (and what other mounts have been layered on top). When you are done,
  the transparent sheet is thrown away. This can be repeated as often as you want, and base is always the same.
- Definition files (e.g., Dockerfile or Singularity definition file) are text
  files that contain a series of instructions to build container images.


## You may have use for containers in different ways

- **Installing a certain software is tricky**, or not supported for your operating system? - Check if an image is available and run the software from a container instead!
- You want to make sure your colleagues are using the **same environment** for running your code? - Provide them an image of your container!
  - If this does not work, because they are using a different architecture than you do? - Provide a definition file for them to **build the image suitable for their computers**. This does not create the exact environment you have, but in most cases a similar enough one.

---

## Popular container implementations

- [Docker](https://www.docker.com/)
- [Singularity](https://sylabs.io/docs/) (popular on high-performance computing systems)
- [Apptainer](https://apptainer.org) (popular on high-performance computing systems, fork of Singularity)
- [podman](https://podman.io/)

They are to some extent interoperable:

- podman is very close to Docker
- Docker images can be converted to Singularity/Apptainer images
- [Singularity Python](https://singularityhub.github.io/singularity-cli/) can convert Dockerfiles to Singularity definition files

---

## Pros and cons of containers

Containers are popular for a reason - they solve a number of
important problems:

- Allow for seamlessly **moving workflows across different platforms**.
- Can solve the **"works on my machine"** situation.
- For software with many dependencies, in turn with its own dependencies,
  containers offer possibly the only way to preserve the
  computational experiment for **future reproducibility**.
- A mechanism to "send the computer to the data" when the **dataset is too large** to transfer.
- **Installing software into a file** instead of into your computer (removing
  a file is often easier than uninstalling software if you suddenly regret an
  installation).

However, containers may also have some drawbacks:

- Can be used to hide away software installation problems and thereby
  **discourage good software development practices**.
- Instead of "works on my machine" problem: **"works only in this container"** problem?
- They can be **difficult to modify**.
- Container **images can become large**.

```{danger}
Use only **official and trusted images**!  Not all images can be trusted! There
have been examples of contaminated images, so investigate before using images
blindly. Apply the same caution as when installing software packages from untrusted
package repositories.
```

---


````{exercise} (optional) Containers-3: Explore two really useful Docker images
You can try the below if you have Docker installed. If you have
Singularity/Apptainer and not Docker, the goal of the exercise can be to run
the Docker containers through Singularity/Apptainer.

1. Run a specific version of *Rstudio*:
   ```console
   $ docker run --rm -p 8787:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
   ```

   Then open your browser to [http://localhost:8787](http://localhost:8787)
   with login rstudio and password "yourpasswordhere" used in the previous
   command.

   If you want to try an older version you can check the tags at
   [https://hub.docker.com/r/rocker/rstudio/tags](https://hub.docker.com/r/rocker/rstudio/tags)
   and run for example:
   ```console
   $ docker run --rm -p 8787:8787 -e PASSWORD=yourpasswordhere rocker/rstudio:3.3
   ```

2. Run a specific version of *Anaconda3* from
   [https://hub.docker.com/r/continuumio/anaconda3](https://hub.docker.com/r/continuumio/anaconda3):
   ```console
   $ docker run -i -t continuumio/anaconda3 /bin/bash
   ```
````

## Resources for further learning

- [Carpentries incubator lesson on Docker](https://carpentries-incubator.github.io/docker-introduction/)
- [Carpentries incubator lesson on Singularity/Apptainer](https://carpentries-incubator.github.io/singularity-introduction/)

```{keypoints}
- Containers can be helpful if complex setups are needed to run a specific software.
- They can also be helpful for prototyping without "messing up" your own computing environment, or for running software that requires a different operating system than your own.
```
