
# Scripting the MP3 Library with Python

This Workshop will introduce python as a scripting language for you. Scripting in here is referred to as small tasks, often used for system administration and filesystem organisation, which do not really require a full blown program but just some little script to automate tasks. In this particular case we'll be working on organsing MP3-Files depending on their embedded tags. As such it is assumed you are having a small batch of mp3 files you'd like to work with. If you got not, ask a person around you to help you out with that.

## Install and Setup

### Python
It is assumed, that you have an installation of Python 2.7 on your system. If you have a recent Mac or Linux, you are covered. In case you are running a Winows Operation System, please refer to the Setup page of the [Python on Windows](http://docs.python.org/2/using/windows.html#excursus-setting-environment-variables).

### PyID3
We'll be using an ID3 Helper library to read and temper with the tags. A library is just a bunch of code files, which encapsulate a group of tasks and are packaged togehter to be distributed. Python has plenty of libraries all kinds of tasks, almost all of them beeing listed on [pypi](http://pypi.python.org/pypi). The Library we'll be using is called "PyID3". In order to install it for this project, either download this [ZIP-File](https://github.com/myers/pyid3/archive/master.zip) and unpack it or clone the [repository from here](https://github.com/myers/pyid3) with git. If you do not know how to do the later, just click on first link and download that file :) .

You should now have a directory called "pyid3". Open a Command Line and go into that directory.

#### Setup Test Environment
In order to not temper with our own library while we are still working on our code, we'll start by setting up a local tag copy directory that we can develop our code against. In order to do that start the following programm giving the Path to your current library (replacing "PATH_TO_YOUR_LIB")::

	python test/findtags.py test PATH_TO_YOUR_LIB

This will create a directory called "sample-tags" with a bunch of files just containing the tag-data from your library. If we screw these files up, you can always simply remove this directory and restart the command in order to re-create it.

## 