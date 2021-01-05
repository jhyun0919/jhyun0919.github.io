---
layout: post
title: (Python-Dev Tips) Virtual Environment
categories: [Research/CS]
tags: [CS, Dev, Python, virtualenv, Anaconda]
---

In this article, we are going to explain what is virtual environment in python and why do we need it. Also, we will show you how to set up a virtual environment and introduce basic commands.

This posting is one of a series of tips for developing python following the previous post.

1. [Git Basic](https://jhyun0919.github.io/research/cs/2020/10/01/git-tips.html)
2. **Virtual Environment**
3. [PEP8](https://jhyun0919.github.io/research/cs/2020/10/03/pep8.html)
4. [Jupyter](https://jhyun0919.github.io/research/cs/2020/10/04/jupyter.html)

This post was written with reference to the following materials.

- [Python Docs: Virtual Environments](https://docs.python.org/3/tutorial/venv.html)
- [Real Python: Python Virtual Environments](https://realpython.com/python-virtual-environments-a-primer/)

---

# What is Virtual-Env & Why Do We Need it?

Python **virtual environment** is an isolated environment for a Python project. This allows each project can have its **own dependencies**, regardless of what dependencies every other project has [1].

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-04-virtualenv/virtualenv_concept.png" width="555" />
  <figcaption>Figure 1. Examples of Python Virtual Environment [2].</figcaption>
</figure>


<br>

# How to Set Up a Virtual-Env

<br>

## Anaconda

[Anaconda](https://www.anaconda.com) is a package manager, an environment manager, a Python/R data science distribution, and a collection of over 7,500+ open-source packages [3]. Through this, we can set up and manage a virtual emvironment for each projects.

In this article we will introduce how to create a virtual environment and how to export the dependencies of the environment. For more details, you can refer to the following Documentation or Cheat-Sheet.

- [Anaconda Documentation](https://docs.anaconda.com/anaconda/)
- [Conda Cheat-Sheet](https://conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)



- Create a new virtual environmnet

    ```shell
    $ conda create --name ENVNAME
    ```

- Exporting the environment.yml file

    ```shell
    $ conda env export --name ENVNAME > envname.yml
    ```

- Creating an environment from an envname.yml file

    ```shell
    $ conda env create --file envname.yml
    ```

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-04-virtualenv/anaconda_gui.png" width="900" />
  <figcaption>Figure 2. Example of Anaconda GUI.</figcaption>
</figure>

<br>


---
# Reference
[1] Real Python, “Python Virtual Environments: A Primer,” Real Python, 17-Jul-2020. [Online]. Available: https://realpython.com/python-virtual-environments-a-primer/. [Accessed: 05-Jan-2021]. 

[2] S. Shakya, “Virtual Environment in Python,” Medium, 02-May-2019. [Online]. Available: https://medium.com/incwell-bootcamp/virtual-environment-in-python-54db665b9939. [Accessed: 05-Jan-2021]. 

[3] “Anaconda Individual Edition¶,” Anaconda Individual Edition - Anaconda documentation. [Online]. Available: https://docs.anaconda.com/anaconda/. [Accessed: 05-Jan-2021]. 