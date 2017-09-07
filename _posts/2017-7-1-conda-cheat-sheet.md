---
layout: post
sitemap: true
noindex: false
title: Anaconda (conda) / Jupyter notebook cheat sheet
categories: anaconda jupyter notebook python cheatsheet
---

Anaconda® is a package manager, an environment manager, a Python distribution, and a collection of over 720 open source packages. Here we have a basic _cheatsheet_ for ```conda``` command line tool... enjoy!


## Environments

### Create environment
The default environment, the environment used when you aren't in one, is called root.

```
conda create -n my_env python=3
```

Alternative:
```
conda create -n env_name list of packages
```

### Create an environment from an _environment file_
```
conda env create -f environment.yaml
```

### Listing environments
```
conda env list
```

### Activate environment
```
source activate my_env
```

**or** just _activate_ on windows

```
activate my_env 
```

### To **leave** the environment on OSX/Linux
```
source deactivate
```

**or** just _deactivate_ on windows

```
deactivate
```

### Removing environments
```
conda env remove -n env_name 
```

----

## Install packages
```
conda install numpy scipy pandas matplotlib
conda install jupyter notebook
```

specify which version of a package
```
conda install numpy=1.10
```

## List packages

```
conda list
```

## Update conda and packages
```
conda upgrade conda
conda upgrade --all
```

## Saving and loading environments (use **env**, not _your_env_name_)
```
conda env export > environment.yaml
```

## Update package
 - To update a package **conda update package_name**
 - If you want to update all packages in an environment, which is often useful, use **conda update --all**

## Uninstall package
 - To uninstall, use **conda remove package_name**

## Search packages
 - Search packages: **conda search beautifulsoup**
 

## Sharing environments
When sharing your code on GitHub, it's good practice to make an environment file and include it in the repository. This will make it easier for people to install all the dependencies for your code. I also usually include a pip requirements.txt file using **_pip freeze_** for people not using conda.

### Saving and loading environments

 - Saving an environment to a YAML file

```
conda env export > environment.yaml
```

- Loading an environment from a YAML file

  _This will create a new environment with the same name listed in environment.yaml._

```
conda env create -f environment.yaml
```

### Listing environments
```
conda env list
```

### Removing environments
```
conda env remove -n env_name
```


One thing that’s helped me tremendously is having separate environments for Python 2 and Python 3. I used ```conda create -n py2 python=2``` and ```conda create -n py3 python=3``` to create two separate environments, py2 and py3. Now I have a general use environment for each Python version. 


# Jupyter Notebook

## Install Jupyter notebooks

```
conda install jupyter notebook
```

_Jupyter notebooks are also available through pip with pip install jupyter notebook._

## Launching the notebook server
```
jupyter notebook
```

## Notebook Conda (nb_conda)

Allow us to create new environments, install packages, update packages, export environments and more. Additionally, with nb_conda installed you will be able to access any of your conda environments when choosing a kernel. 

```
conda install nb_conda
```

## Tips and tricks working with Jupyter Notebooks

### Magic keywords / commands

 > List of built-in magic commands [here](http://ipython.readthedocs.io/en/stable/interactive/magics.html)

Rendering matplotlib inline
```
%matplotlib inline
%config InlineBackend.figure_format = 'retina'
```

time how long it takes for a whole cell to run:
```
%%timeit
```

### Debugging in the Notebook
 using %pdb. When you cause an error, you'll be able to inspect the variables in the current namespace.
To quit the debugger, simply enter q in the prompt
 ```
 %pdb
 ```

## Converting Notebooks

to convert a notebook to an HTML file, in your terminal use
```
jupyter nbconvert --to html notebook.ipynb
```