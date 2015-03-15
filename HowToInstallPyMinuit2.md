# Introduction #
Minuit has become the default minimization package in High Energy Physics. In its latest incarnation, it is called [Minuit2](http://project-mathlibs.web.cern.ch/project-mathlibs/sw/html/Minuit2.html) and being maintained by the ROOT development team, but available independently of it.

# Install Minuit2 #
First you need to [download the latest sources of Minuit2](http://project-mathlibs.web.cern.ch/project-mathlibs/sw/Minuit2.tar.gz), extract the source and change into the Minuit2-

&lt;RELEASE&gt;

 directory, e.g.
```
wget http://project-mathlibs.web.cern.ch/project-mathlibs/sw/Minuit2.tar.gz
tar xzf Minuit2.tar.gz
cd Minuit2-5.18.00
```
configure with the configure script. See ./configure --help for more options.

```
./configure --prefix=/opt/Minuit2
make
```

In order to make sure that the libraries are found, you will have to add them to your LD\_LIBRARY\_PATH. If you are using bash, execute the following command now and add the same line to your ~/.bashrc
```
export LD_LIBRARY_PATH=/opt/Minuit2/lib:${LD_LIBRARY_PATH}
```
If you are using a csh derivative, execute the following line instead and add it to your .cshrc
```
setenv LD_LIBRARY_PATH /opt/Minuit2/lib:${LD_LIBRARY_PATH}
```

To build the test do:
```
make check
```

and to run them going in the subdirectory. Do for example:
```
cd test/MnSim
./test_Minuit2_DemoGaussSim
```


# Install [PyMinuit2](http://code.google.com/p/pyminuit2/) #
First, change the directory back to the one where you put minuit2 and mathcore in order to avoid confusion. If you continue from the previous step, that would be
```
cd ../../..
```

Next, you can obtain the source from
```
svn checkout http://pyminuit2.googlecode.com/svn/trunk/ pyminuit2-read-only
cd pyminuit2-read-only
```

Then you will have to modify the setup.py to show the location of your Minuit2 includes and libraries. If you have specified the prefix in the configure step of Minuit2 as I have shown, then it should look like this:

```
#!/usr/bin/env python

from distutils.core import setup, Extension
import os, subprocess

##################################################################################################
incdir='/opt/Minuit2/include'
libdir='/opt/Minuit2/lib'

setup(name="pyMinuit2",
      version="1.0.0",
      description="pyMinuit2: Minuit2 interface for minimizing Python functions",
      author="Johann Cohen-Tanugi",
      author_email="johann.cohentanugi@gmail.com",
      url="http://?",
      package_dir={"": "lib"},
      ext_modules=[Extension(os.path.join("minuit2"),
                             [os.path.join("minuit2.cpp")],
                             library_dirs=[libdir],
                             libraries=["Minuit2Base"],
                             include_dirs=[incdir]
                             )])
```

Then you can see if the package builds like so:
```
python setup.py build_ext --inplace
cd build
```
If you then run python from within this directory, you should be able to run the examples on the pyminuit page
http://code.google.com/p/pyminuit/