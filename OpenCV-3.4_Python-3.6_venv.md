# Installing OpenCV 3.4 in a Python 3.6 virtual environment
Originally published: 2018-02-05
Last Edited: 2020-04-28

## Contents
1. [Background](#background)
1. [Preparation](#preparation)
1. [Build & Install Python 3.6.4]()
1. [Virtual Environment](#virtual-environment)
1. [Building & Installing OpenCV]()
1. [Testing](#testing)
1. [Sources](#sources)

## Background
If you do research in computer vision, you probably want OpenCV. You also probably want to keep all software installations as local as possible (so you don't interfere with other projects), which may spur you to use a virtual environment. I've spent the last few weeks trying to get OpenCV installed under such conditions, and I'd like to share what I've found.
**Please note that my machine is running Ubuntu 14.04, so if you're using something more modern, YMMV.**

## Preparation
First, let's set up some important environment variables to make it go smoothly. Add the following lines to your ~/.bashrc (or similar) and then reload it:

```
export LD_LIBRARY_PATH=$HOME/.python3.6.4/lib <del>/usr/local/bin/python</del>
export PYTHON=$HOME/.python3.6.4/bin/python3.6
export PIP=$HOME/.python3.6.4/bin/pip3
```
What do these do? Firstly, `LD_LIBRARY_PATH` tells your future (local) Python installation where to look for dynamically-linked libraries (**note, this may muck with future installs of Python, so beware**). The `PYTHON` and `PIP` variables are simply going to help us a little bit later, and don't serve any real purpose besides saving us a few keystrokes. Feel free to leave these out. Please note that `$HOME/.python3.6.4` is simply where I decided to install my local Python files. You can choose whatever you want here. I made it hidden because I like to keep a clean home :)

## Build & Install Python 3.6.4

Next, we're going to download, build, and install the Python3.6 source code. As of this writing (Feb 2, 2018), the following uses the most updated stable Python source code:

```
$ wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz
$ tar xzvf Python-3.6.4.tgz
$ cd Python-3.6.4
$ ./configure --prefix=$HOME/.python3.6.4 --enable-shared --enable-optimizations
```

Let's break down these configuration options:

- `--prefix` simply tells the script where to put our new Python files. Note that this is the root of the locations we specified in `.bashrc` above
- `--enable-shared` allows us to use custom modules, and is very important
- `--enable-optimizations` makes Python faster, at the expense of a slower build time

Now let's make and install it:

```
$ make
$ make install
```

Now, you may encounter some issues on that last command. If you do, try:

```
$ sudo apt-get install zlib1g-dev
```

## Virtual Environment
Now that Python (and implicitly Pip) is installed locally, let's get some tools for the virtual environment setup. VirtualEnv is a nice tool for creating and managing virtual environments, and VirtualEnvWrapper is a nice tool that abstracts away some of the details. Abstraction for your abstraction, if you will. We'll also grab NumPy, since we'll need it for OpenCV anyway.

```
$ $PIP install virtualenv virtualenvwrapper numpy
```

Note that we're using the environment variable we created [above](#preparation).


Next we'll add some more important things to our `.bashrc` (make sure you reload it when you're done):

```
export VIRTUALENVWRAPPER_PYTHON=$HOME/.python3.6.4/bin/python3.6
export VIRTUALENVWRAPPER_VIRTUALENV=$HOME/.python3.6.4/bin/virtualenv
export WORKON_HOME=$HOME/.virtualenvs
source $HOME/.python3.6.4/bin/virtualenvwrapper.sh
```

The first two lines tell the VirtualEnvWrapper where to look for Python and VirtualEnv, respectively. The next line is where we'll keep our virtual environments (again, hidden in home). Finally, the last line runs the VirtualEnvWrapper when we open a terminal so that we have access to all its useful functionality.


Now it's time to create the virtual environment to which we'll install all this stuff. I called mine `code` but you can specify it to whatever you choose. The later instructions do assume you called it `code`, however, so watch that.

```
$ mkvirtualenv code
```

## Building & Installing OpenCV

We'll start by cloning both the [official](https://github.com/opencv/opencv) and [contrib](https://github.com/opencv/opencv_contrib) repos of OpenCV. You don't _need_ `opencv_contrib`, strictly speaking, but there are some very useful tools in it that are absent in OpenCV. Please note that below are the instructions that are up-to-date as of Feb 5, 2018. The tag number you checkout may be different when you read this. I'm also providing instructions for installing directly to your home directory, so if you want this source code elsewhere, you'll have to specify that.

```
$ cd
$ git clone https://github.com/opencv/opencv
$ git clone https://github.com/opencv/opencv_contrib
$ cd opencv_contrib
$ git checkout 3.4.0
$ cd ~/opencv
$ git checkout 3.4.0
```

Next, we'll configure our build. You'll notice I've set some optional flags to customize my build and to build it faster. You may want to tweak these. You'll also notice that I'm using my `code` virtual environment created above. If you chose a different name, you'll have to change that as well.

```
$ mkdir ~/opencv/build
$ cd ~/opencv/build
$ cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=$WORKON_HOME/code/lib/python3.6/site-packages \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON \
    -D PYTHON3_PACKAGES_PATH=$WORKON_HOME/code/lib/python3.6/site-packages \
    -D PYTHON3_LIBRARIES=$WORKON_HOME/lib/libpython3.6m.so \
    -D PYTHON3_EXECUTABLE=$WORKON_HOME/code/bin/python3.6 \
    -D PYTHON_DEFAULT_EXECUTABLE=$WORKON_HOME/code/bin/python3.6 \
    -D BUILD_opencv_java=OFF \
    -D WITH_MATLAB=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=$HOME/opencv_contrib/modules \
..
```

Please note that I specify no MATLAB or Java, since I have no need for either in my work. I turned off those flags to speed up the build time. If you anticipate using either one, change the value to `ON`


Now we can actually build the darn thing! This step will take some time, though it's made significantly faster by setting those Java and MATLAB flags `OFF`, and asking CMake to use almost all your machine's processors. My machine has eight cores, so I use `-j7` so my machine can still do other stuff

```
$ make -j7
```

Go make yourself a cup of coffee, read some interesting articles, or go for a run. This will probably take 30-90 minutes based on your machine, and longer if you are building with more functionality or with fewer jobs.


It may be the case that this fails the first time you try it. I don't know why this is the case, but try it again - it may just work.


. . .


Welcome back. Let's install it now. This should take significantly less time, no more than five or ten minutes.

```
$ make install
```

## Testing

You should be all done! To make sure everything worked, try the following:

```
$ workon code
$ python -c "import cv2; print(cv2.__version__)"
```

The first line will activate your virtual environment, and the second will run a couple Python commands - namely importing OpenCV and printing the version number.


Hope you found this helpful!


## Sources
- Adrian Rosebrock's [tutorial](https://www.pyimagesearch.com/2015/07/20/install-opencv-3-0-and-python-3-4-on-ubuntu/) was immensely helpful here, and if you're new to GNU/Linux or Python, I'd recommend starting there.

