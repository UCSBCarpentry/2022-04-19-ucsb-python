---
title: Setup
---

> ## Data
> Data for this lesson is provided by [Santa Barbara Coastal, Long Term Ecological Research](https://sbclter.msi.ucsb.edu/about/) site.
> SBC LTER is based at the University of California, Santa Barbara (UCSB) Marine Science Institute.
> There, they maintain and support research on long-term ecological phenomena; their principal study domain is the northern portion of the Southern
> California Bight.
> You may also read more about the greater [Long Term Ecological Research Network](https://lternet.edu/about/).
>
> Please [download the SBC LTER data files as a zip.](https://ucsbcarpentry.github.io/2022-04-19-ucsb-python/data/lobster-teaching-db.zip)
> This link will give you everything in a compressed file. **You will need to unzip
> this file after downloading it.** Please do the same for the second set of [teaching data files](https://ucsbcarpentry.github.io/2022-04-19-ucsb-python/data/portal-teachingdb-master.zip).
>
> We recommend making a folder on your desktop called "april-2022-python". Inside of this folder, please keep the data files in another folder called "data".
> If you follow these steps, you will have a folder called "april-2022-python". Inside of april 2022 python, you will have a folder called "data". Inside of the data folder, is the data files you
> downloaded from the link above.
{: .prereq}


> ## Installing Python using Anaconda
>
> [Python][python] is a popular language for scientific computing, and great for
> general-purpose programming as well. Installing all of the scientific packages we use in the lesson
> individually can be a bit cumbersome, and therefore recommend the all-in-one
> installer [Anaconda][anaconda].
>
> Regardless of how you choose to install it, please make sure you install **Python
> version 3.x.**
>
> If applicable: please **log out** of any drives on your device before installing the software. (e.g. OneDrive, BoxDrop, etc.)
>
{: .prereq}


## Installing Anaconda

{::options parse_block_html="true" /}
<div>
<ul class="nav nav-tabs" role="tablist">
  <li role="presentation" class="active"><a data-os="windows" href="#anaconda-windows" aria-controls="Windows" role="tab" data-toggle="tab">Windows</a></li>
  <li role="presentation"><a data-os="macos" href="#anaconda-macos" aria-controls="MacOS" role="tab" data-toggle="tab">MacOS</a></li>
  <li role="presentation"><a data-os="linux" href="#anaconda-linux" aria-controls="Linux" role="tab" data-toggle="tab">Linux</a></li>
</ul>

<div class="tab-content">
<article role="tabpanel" class="tab-pane active" id="anaconda-windows">

1.  Open <https://www.anaconda.com/products/individual> in your web browser.
2.  Download the Anaconda Python 3 installer for Windows.
3.  Double-click the executable and install Python 3 using the recommended settings.
    Make sure that **Register Anaconda as my default Python 3.x** option is checked --
    it should be in the latest version of Anaconda.
4.  Verify the installation:
    click Start, search and select `Anaconda Prompt` from the menu.
    A window should pop up where you can now type commands
    such as checking your Conda installation with:

    ~~~
    conda --help
    ~~~
    {: .language-bash}

#### Video Tutorial

<div class="yt-wrapper2">
<div class="yt-wrapper">
<iframe type="text/html" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" src="https://www.youtube-nocookie.com/embed/xxQ0mzZ8UvA?modestbranding=1&playsinline=1&iv_load_policy=3&rel=0" class="yt-frame" allowfullscreen></iframe>
</div>
</div>
</article>

<article role="tabpanel" class="tab-pane" id="anaconda-macos">

1.  Visit <https://www.anaconda.com/products/individual> in your web browser.
2.  Download the Anaconda Python 3 installer for macOS.
    These instructions assume that you use the graphical installer `.pkg` file.
3.  Follow the Anaconda Python 3 installation instructions.
    Make sure that the install location is set to "Install only for me"
    so Anaconda will install its files locally, relative to your home directory.
    Installing the software for all users tends to create problems in the long run
    and should be avoided.
4.  Verify the installation:
    click the Launchpad icon in the Dock, type Terminal in the search field, then click Terminal.
    A window should pop up where you can now type commands
    such as checking your conda installation with:

    ~~~
    conda --help
    ~~~
    {: .language-bash}

#### Video Tutorial

<div class="yt-wrapper2">
<div class="yt-wrapper">
<iframe type="text/html" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" src="https://www.youtube-nocookie.com/embed/TcSAln46u9U?modestbranding=1&playsinline=1&iv_load_policy=3&rel=0" class="yt-frame" allowfullscreen></iframe>
</div>
</div>
</article>

<article role="tabpanel" class="tab-pane" id="anaconda-linux">
Note that the following installation steps require you to work from the terminal (shell).
If you run into any difficulties, please request help before the workshop begins.

1.  Open <https://www.anaconda.com/products/individual> in your web browser.
2.  Download the Anaconda Python 3 installer for Linux.
3.  Install Anaconda using all of the defaults for installation.
    * Open a terminal window.
    * Navigate to the folder where you downloaded the installer.
    * Type `bash Anaconda3-` and press <kbd>Tab</kbd>.
      The name of the file you just downloaded should appear.
    * Press <kbd>Return</kbd>
    * Follow the text-only prompts.  When the license agreement appears (a colon
      will be present at the bottom of the screen) press <kbd>Spacebar</kbd> until you see the
      bottom of the text. Type `yes` and press <kbd>Return</kbd> to approve the license. Press
      <kbd>Return</kbd> again to approve the default location for the files. Type `yes` and
      press <kbd>Return</kbd> to prepend Anaconda to your `PATH` (this makes the Anaconda
      distribution your user's default Python).
4.  Verify the installation:
    this depends a bit on your Linux distribution, but often you will have an Applications listing
    in which you can select a Terminal icon you can click. A window should pop up where you can now
    type commands such as checking your conda installation with:

    ~~~
    conda --help
    ~~~
    {: .language-bash}

</article>
</div>
</div>

[anaconda]: https://www.anaconda.com/
[jupyter]: https://jupyter.org/
[python]: https://www.python.org/


## Required Python Packages

The following are packages needed for this workshop:

* [Pandas](https://pandas.pydata.org/)
* [Jupyter notebook](https://jupyter.org/)
* [Numpy](https://numpy.org/)
* [Matplotlib](https://matplotlib.org/)
* [Plotnine](https://plotnine.readthedocs.io/en/stable/)

All packages apart from `plotnine` will have automatically been installed with Anaconda
and we can use Anaconda as a package manager to install the missing `plotnine` package:
You need to open up a *Terminal*, if you are using Mac OSX, or Linux (see instructions above),
or launch an *anaconda-promt*, if you are using Windows. In your terminal window type the following:

~~~
conda install -y -c conda-forge plotnine
~~~
{: .language-bash}

This will then install the latest version of plotnine into your conda environment.

## Required packages: Miniconda

Miniconda is a lightweight version of Anaconda. If you install Miniconda instead of Anaconda,
you need to install required packages manually in the following way:
~~~
conda install -y numpy pandas matplotlib jupyter
conda install -c conda-forge plotnine
~~~
{: .language-bash}

### _(Alternative)_ Installing required packages with environment file
Download the
[environment.yml](https://raw.githubusercontent.com/datacarpentry/python-ecology-lesson/gh-pages/environment.yml)
file by right-clicking the link and selecting save as.
In the directory where you downloaded the environment.yml file run:

~~~
conda env create -f environment.yml
~~~
{: .language-bash}

Activate the new environment with:
~~~
conda activate python-ecology-lesson
~~~
{: .language-bash}

You can deactivate the environment with:
~~~
conda deactivate
~~~
{: .language-bash}

## Launch a Jupyter Lab

After installing either Anaconda or Miniconda and the workshop packages,
launch JupyterLab. You can launch JupyterLab through Anaconda Navigator, or by typing this command from the terminal:

~~~
jupyter lab
~~~
{: .language-bash}

JupyterLab should open automatically in your browser. If it does not or you
wish to use a different browser, open this link: <http://localhost:8888/lab>.
