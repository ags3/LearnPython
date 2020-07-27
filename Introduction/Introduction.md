## 1. Introduction to Python

### 1.1 About Python
_Python_ is a general purpose, high-level programming language.
It puts a lot of emphasis on code readability and productivity.  
It is very beginner friendly but also used for large scale, real world projects.  
The official site is http://www.python.org. Here we can find the official documentation and python distributions for download.

### 1.2 About this course and prerequisites
This is an introduction to the Pyhon language for beginners.
Some familiarity with programming will help, but is generaly not mandatory. 
An exception to this is the chapter about embedding and extending the Python interpreter, which assumes a good familiarity with the C programming language.  

### 1.3 Setting-up
#### 1.3.1 Install Python
This course uses _Python 3.7_. 
In order to follow it you will need to install _Python 3.7.x_ (or greater).
Python is available on the official site (http://www.python.org), but I recommend to use the [_miniconda_ distribution](http://conda.pydata.org/miniconda.html).  
After installing it, verify that everything works fine. 
Open a _terminal_ (also known as _Command Prompt_ on Windows systems) and execute:

    python

You should see something similar to:

    Python 3.7.3 (default, Mar 27 2019, 22:11:17) 
	[GCC 7.3.0] :: Anaconda, Inc. on linux
	Type "help", "copyright", "credits" or "license" for more information.
	>>> 

You have invoked the Python _interactive shell_.
Here you can introduce commands at the interactive prompt (>>>) and the Python _interpreter_ will execute them.  
For example, you can enter some arithmetic expressions and use Python as a calculator:

    >>> 2 * 8 + 4
    20
    >>> 2**16 + 1
    65537

So, the Python interactive shell makes a good calculator.  

Take a while to see a set of core principles which guides Python. Execute:

    >>> import this

After finished playing with the Python interactive shell, call the quit function to exit:

    >>> quit()

#### 1.3.2 Install IPython
_IPython_ it is an _improved_ interactive shell.
In order to install it, we will use the _pip_ package management utility (_pip_ is the official package manager for Python).  
From a _terminal_, execute:

    pip install ipython

Note: on Windows systems you will also need to install the _pyreadline_ package in order to benefit from color highlighting and auto-complete when using _ipython_:

    pip install pyreadline ipython

Note: you have tons of third-party packages that you can install using _pip_. 
Take a look at the _Python Package Index_ (a.k.a _PyPI_): https://pypi.python.org/pypi 

#### 1.3.3 Install code checking tools
During this course we will use the _pylint_ static code checking tool. Install it as usually:

    pip install pylint flake8

### 1.4 Text editors and Integrated Development Environments
You will need a _text editor_ or an _Integrated Development Environment_ (_IDE_) with Python support.
A good option is [_Visual Studio Code_](https://code.visualstudio.com/). 
Other popular options are [_PyCharm_](https://www.jetbrains.com/pycharm/download/) or [_PyDev_](http://www.pydev.org/).
