
# Scripting the MP3 Library with Python

This Workshop will introduce python as a scripting language for you. Scripting in here is referred to as small tasks, often used for system administration and filesystem organisation, which do not really require a full blown program but just some little script to automate tasks. In this particular case we'll be working on organsing MP3-Files depending on their embedded tags. As such it is assumed you are having a small batch of mp3 files you'd like to work with. If you got not, ask a person around you to help you out with that.

## Install and Setup

### Python
It is assumed, that you have an installation of Python 2.7 on your system. If you have a recent Mac or Linux, you are covered. In case you are running a Winows Operation System, please refer to the Setup page of the [Python on Windows](http://docs.python.org/2/using/windows.html#excursus-setting-environment-variables).

### PyID3
We'll be using an ID3 Helper library to read and temper with the tags. A library is just a bunch of code files, which encapsulate a group of tasks and are packaged togehter to be distributed. Python has plenty of libraries all kinds of tasks, almost all of them beeing listed on [pypi](http://pypi.python.org/pypi). The Library we'll be using is called "PyID3". In order to install it for this project, either download this [ZIP-File](https://github.com/OpenTechSchool/pyid3/archive/master.zip) and unpack it or clone the [repository from here](https://github.com/OpenTechSchool/pyid3) with git. If you do not know how to do the later, just click on first link and download that file :) .

You should now have a directory called "pyid3". Open a Command Line and go into that directory.

#### Setup Test Environment
In order to not temper with our own library while we are still working on our code, we'll start by setting up a local tag copy directory that we can develop our code against. In order to do that start the following programm giving the Path to your current library (replacing "PATH_TO_YOUR_LIB")::

	python test/findtags.py test PATH_TO_YOUR_LIB

This will create a directory called "sample-tags" with a bunch of files just containing the tag-data from your library. **If we screw these files up, you can always simply remove this directory and restart the command in order to re-create it.**

## First Steps 

### Accessing the first tags

Alright, let's get started then. Use the Command Line from before, staying in the same directory and start a python shell by typing `python` and hit enter. Now import the library with the following command:

	import id3

And parse the first file (named 'sample-tags/test-0000000.tag") by calling the ID3v1 class the library exposes to us:

	id3tags = id3.ID3v1('sample-tags/test-0000000.tag')

You now have an object, which encapsulates the tags of that particular file for you. As such it has a bunch of properties representing the different tags in the file, as 'artist', 'album', 'title' and others.

*Excercise*: Print artist, album, title and genre of this file. If they are empty, try a different file until you have at least one file of which you know the tags now. Remember this one.

### Changing Tags
You can't only read but also write files via this library. Just assign a different value to the attribute and call the save method of the object. This example changes sets the artist to Frank Sinatra and saves it to the file:

	id3tags.artist = "Frank Sinatra"
	id3tags.save()

You can re-open the file and print the value to verify it worked:

	id3tags = id3.ID3v1('sample-tags/test-0000000.tag')
	print(id3tags.artist)

Congratulations, you just changed the ID3-Tags of that file. And it works the same way on all other files as well.

## File Renamer
Now, let's make ourself a little script that renames the file depending on its tags. For that we also need the build-in "os" library, which exposes a function called "rename", with the two parameters of the current filename and the new filename. So after you `import os`, you need to compile the filename from the tags like this:

	new_filename = "sample-tags/{0}-{1}.tag".format(id3tags.artist, id3tags.title)

Once we have done that, we can use "os.rename" to rename the file:

	os.rename('sample-tags/test-0000000.tag', new_filename)

You can now see that the file has been renamed. Of course to access its tags again, you now need to use the `ID3v1` command with that new file first.

*Excercise*: rename another file to the schema "Artist-Album-Track-Title".


## Make it into a Script

In order to have a real script, let's put this into a file now. Open your favourite editor and paste the following in there:

	import os
	import id3

	id3tags = id3.ID3v1('sample-tags/test-0000000.tag')
	new_filename = "{0}-{1}-{2}.tag".format(id3tags.artist, id3tags.album, id3tags.title)
	os.rename('sample-tags/test-0000000.tag', new_filename)

If you now save it in the same directory under the name "renamer.py" and call it via `python renamer.py`, it will rename the file. But it will only do it once because then the file has already been renamed. So let's make it a bit more flexible by giving it a filename via the commandline. In order to do that, we'll be using the great [argparse module](http://docs.python.org/2/library/argparse.html#module-argparse) that gets shipped with python. Just put this at the top of your file (after the other imports):

	import argparse

	parser = argparse.ArgumentParser()
	parser.add_argument("filename")

	args = parser.parse_args()

If you now execute the file again, you'll notice that it quits and tells you:

	usage: [-h] filename
	: error: too few arguments

That is the argument parser we've just set up, stopping execution at parse_args because the argument "filename" is missing on the commandline. Great, exactly what we want. Now we also need replace every place where we have the static filename right now with the new filename. We can access it as "filename" on the args that got passed. After that your file should look something like this:

	import os
	import id3
	import argparse

	parser = argparse.ArgumentParser()
	parser.add_argument("filename")

	args = parser.parse_args()

	id3tags = id3.ID3v1(args.filename)
	new_filename = "{0}-{1}-{2}.tag".format(id3tags.artist, id3tags.album, id3tags.title)
	os.rename(args.filename, new_filename)


And if you execute it now, it should rename any given file for you depending on its tags. Try it :) .


:: Notes from the Email
The scripting workshop will show people how Python can be used as a scripting language by processing mp3 id3 tags. Possible Libraries to use are:
* PyID3 http://pypi.python.org/pypi/PyID3/
* eyeD3 http://pypi.python.org/pypi/eyeD3/

We thought about possible steps which are:
✔ * Print id3 tags 
✔ * Rename files based on id3 tag
* Change id3 data based on file/folder name
* List directory / walk directories / batch processing
* Statistics about genre, total number of albums, ...
* Bonus: Explain exceptions (corrupt file)
The workshop will probably be in the middle of March. Ben, Stephan and Andreas will work on this (again, everyone is very welcome to join)
