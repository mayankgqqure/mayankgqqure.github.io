---
layout: post
title:  "Python Wheels"
date:   2021-02-24 19:21:00 +0530
categories: jekyll update
---

Let's say you have a set of common Python utilites that you use across a number of different projects. Copying these scripts across multiple code bases would be nightmare from the perspective of maintenance. Hence, the solution is to `package` your utilites into a `Python package`. That way, you can easily control versioning of these utilities across all projects that have a dependency on them.

`Wheel` is a built-package format for python. After completing this tutorial, you'll have better understanding of Python `Wheel` .whl files. 

<h1>{{ "What is a Python Wheel?" }}</h1>
There are two primary distribution types in use today, `Built` and `Source` Distributions. `Wheel` is a type of <B>Built distribution</B>. A `wheel` is a ZIP-format archive with a specially formatted filename and the .whl extension. In this, `built` means that the wheel comes in ready-to-install format and allows you to skip the build stage required with source distributions. 

A `wheel` filename is broken down into parts separated by hyphens:

{% highlight Text %}
{dist}-{version}(-{build})?-{python}-{abi}-{platform}.whl
{% endhighlight %}
Each section in {brackets} has some meaning about what the `wheel` contains and where the `wheel` will or will not work. 

For example, 
{% highlight Text %}
opencv_python-4.5.1.48-cp36-cp36m-macosx_10_13_x86_64.whl
{% endhighlight %}

* <B>opencv_python</B> : package-name.
* <B>4.5.1.48</B> : package version.
* <B>cp36</B> : for CPython version 3.6
* <B>macosx_10_13_x86_64</B> : platform tag.
    * <B>macosx</B> is the macOS operating system.
    * <B>10_13</B> is the macOS developer tools SDK version used to compile the Python that in turn built this wheel. 
    * <B>x86_64</B> is a reference to x86-64 instruction set architecture.


<h1>{{ "Advantages of Python Wheels" }}</h1>

* <B>Wheels install faster</B> than source distributions for both pure-Python packages and extension modules. 
* <B>Wheels are smaller</B> than source distributions. for example, the `six` wheel is about `one-third the size` of the corresponding source distributions
* There's no need for a compiler to install wheels that contain compiled extension modules. 

<h1>{{ "Comparison between Python wheels and Source Distribution(sdist)" }}</h1>
A source distribution contains source code. That includes not only Python code but also the source code of any extension modules (usually in C or C++) bundled with the package.

We can start this experiment for better understanding by installing a python package into enviroment. 
In this case, let's install `uWSGI` version 2.0.x:

{% highlight shell %}
$ python -m pip install 'uwsgi==2.0.*'
Collecting uwsgi==2.0.*
  Downloading uwsgi-2.0.18.tar.gz (801 kB)
     |████████████████████████████████| 801 kB 1.1 MB/s
Building wheels for collected packages: uwsgi
  Building wheel for uwsgi (setup.py) ... done
  Created wheel for uwsgi ... uWSGI-2.0.18-cp38-cp38-macosx_10_15_x86_64.whl
  Stored in directory: /private/var/folders/jc/8_hqsz0x1tdbp05 ...
Successfully built uwsgi
Installing collected packages: uwsgi
Successfully installed uwsgi-2.0.18
{% endhighlight %}

In above example, pip progresses through several steps:
* It downloads a TAR file(tarball).
* It takes the tarball and builds a `.whl` file through a call to `setup.py`.
* Install the actual package after having built the wheel.

Now try installing a different package, `chardet`:

{% highlight shell %}
$ python -m pip install 'chardet==3.*'
Collecting chardet
  Downloading chardet-3.0.4-py2.py3-none-any.whl (133 kB)
     |████████████████████████████████| 133 kB 1.5 MB/s
Installing collected packages: chardet
Successfully installed chardet-3.0.4
{% endhighlight %}

Installing `chardet` downloads a `.whl` file directly from PyPI. There's no build stage when `pip` finds a compatible wheel from PyPI.
The reason why `pip` found sdist for `uWSGI` and wheel for `chardet` is that `uWSGI` provides only `source distribution`, while chardet provides both. 

Above, we can see that, Installing from `wheels` directly avoids the intermediate step of building packages off the sdist. 

One can make `pip` ignore its inclination towards wheel by passing `--no-binary` flag option.

From the developer’s perspective, a source distribution is what gets created when you run the following command:
{% highlight shell %}
$ python setup.py sdist
{% endhighlight %}

and, a wheel is the result of running the following command:
{% highlight shell %}
$ python setup.py bdist_wheel
{% endhighlight %}