# Installation & technical matters

Here are some [slides on Python Installation & environments](https://docs.google.com/presentation/d/1aTYSvpuYaE_dPIWwT_HEcYve7fsYpDtzix74d5-NvC4/edit?usp=sharing).

## 1. Python

**Note**: 

Instructions to follow for:
- Linux: included;
- MacOS: [download the installer](https://www.python.org/downloads/macos/)
- [Windows](https://docs.python.org/3/using/windows.html)

The overall [download](https://www.python.org/downloads/) page.

## Miniconda/Anaconda/Miniforge/Mamba

For managing Python environment (and more), use conda! Works on Linux/MacOS/Windows.

- [Miniforge](https://github.com/conda-forge/miniforge), recommended for Mac M1/2/3 users (this currently includes `mamba`, just below).
- [Mamba](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html), a C++ reimplementation of `conda`, the package manager of Anaconda. Still in development, but resolves issues of speed found in Anaconda.
- [Miniconda](https://docs.conda.io/en/latest/miniconda.html), the minimal install, CLI only, for Anaconda below
- [Anaconda](https://www.anaconda.com/), the full install (only recommended if you use it elsewhere or are only comfortable with GUI software)

Installation instructions in the above links.

### Checking your installation

For Python, open a terminal and type `python --version` and/or `python3 --version`.

Note: to locate programs, and see if they are installed, use `which` on Linux/MacOs, `where` on Windows.

In a terminal, type `where/which conda`.

If it says something to the effect of `conda not found`, then you are good to continue with your installation.

Note: if you install things, often you need to reset or close the current terminal, then reopen a new one.

#### Post-install note

If you don't want to have the `(base)` environment automatically activated, run this:

```bash
conda config --set auto_activate_base false
```

## 2.1 Environments (Conda)

A [conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html) is a directory that contains a specific collection of conda packages that you have installed. This allows you to work cleanly on your different projects that may require different versions of libraries or Python itself. You can easily activate or deactivate environments, which is how you switch between them. 

**NOTE: when using `conda`, it is *strongly* recommended never to use the `base` environment, the one coming out of the box with `conda`, and that `conda` needs to update itself (installing things in this environment runs the risk of slowing down any update you want to make to `conda` itself). Just leave it as is and always work in an environment _you_ created.**

### Creating the environment

Open a terminal/console and type (`--name` and `-n` are equivalent, you can list flags with `conda --help`): 

```bash
$ conda create --name dmlcp python
```

This will specifies the Python version and its name 'dmlcp' ('Data and Machine Learning for Creative Practice'). Feel free to choose some other name if you prefer. 

Activate it by typing:

```bash
$ conda activate dmlcp
```

Now your console *should* indicate the environment in some way or other. We can install a package (the `-c conda-forge` specifies a *channel*, again a way of controlling dependencies: a bit like getting your version of the program from one supermarket). You can add the `-y` flag if you don't want conda to wait for your approval.

```bash
(dmlcp) $ conda install -c conda-forge jupyter
```

The package is now installed **only** in your environment `dmlcp`. From now on, make sure you always activate this environment when working on the lab/project activities of this module!

### Checking what packages are installed in your environment

```bash
(dmlcp)  $ conda list
```

Search for something in particular, using Unix pipes:

```bash
$ conda list | grep torch # pytorch, torchvision, etc.
```

### Removing a package from your environment

Remove the package `scipy` from the currently-active environment:

```bash
$ conda remove scipy
```

Remove a list of packages from an environemnt 'myenv':

```bash
$ conda remove -n myenv scipy curl wheel
```

### Listing all your conda environments

```bash
$ conda env list
```

### Removing an existing conda environment

Removing an environment 'myenv': 

```bash
$ conda remove --n myenv --all
```

### More information:

- [Getting Started with Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)
- [Conda Cheatsheet](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)

## 2.2 Environments (Pip)

Pip is Python's package manager (like `npm` for JavaScript)

To create an environment *only for a specific project*:
```bash
$ cd my-project # go to that folder
$ mkdir env # create a (usually hidden) folder for the env files
$ python3 -m venv env --prompt my-project # creates your pip environment
$ source env/bin/activate                 # without --prompt, my environment will be called 'env'
(my-project) $ which pip  # pip will now be in my-project/bin/pip, a local copy
```

Everything relating to this environment is contained in the `env` folder.

To deactivate:
```bash
(my-project) $ deactivate
```

To delete all traces of your environment, start from zero, etc.
```bash
rm -rf env # inside `my-project``, simply delete the folder with env files
```

### **!! NOTE: When you run python3 -m venv, it will create an environment with the same version of Python as the one used when invoking the command! To use another version, you need to have it installed, then use python3.11 -m venv...**

A nice way to work with various versions of Python outside conda (to work with pip and venv) is to install [pyenv](https://github.com/pyenv/pyenv), which allows you to select and install the python version you like without endangering your system's Python (see Rachele Guzzon's nice [Notion page](https://rachypan.notion.site/Creating-a-PIP-environment-for-Apple-silicon-MAC-without-GPU-acceleration-3a808e6c2690472aa73d94572ed123ea) installing all dependencies only with pip)!

## 2.3 Conda vs Pip: What is the difference?

You might often come across the commands `pip install` and `conda install` when installing packages.

Pip and conda are both used to install packages but:

### Pip

- is used *only for Python*!
- *will only check the dependencies of the package you are currently installing (not the consistency of the whole environment)*, you can easily break things, but it's *super* fast when downloading.

### Conda

- is used for Python *and many other things*, for instance installing low-level GPU libraries (that means you can use pip inside conda, nice!)
- *will check every possible dependency conflict*, which can take ages if you have many things installed.
- this previous thing is one of the reasons why you are advised *never to use the `base` environment* in conda, the one that comes out of the box, as conda needs it to update itself. Just leave it untouched and create another environment straight away.

Both create environments, however pip works *project-wise*, whereas conda works *system-wise*.

Often, what you want to do is create a conda environment, then use `pip` to install things quickly (and if everything is messed up, you can just remove the env entirely and start from scratch).

## 3. Jupyter & Google Colab

The [Jupyter Notebook/Lab](https://jupyter.org) is a web interface around [IPython](https://ipython.org/) (an improved interactive Python interpreter) for creating and sharing computational documents. Note that this works for other languages as well, such as[JavaScript](https://github.com/n-riesco/ijavascript).

[Google Colab](https://colab.research.google.com) are Jupyter Notebooks hosted online by Google, that run on cloud machines (with GPU/TPU access)! A lot of packages come installed out of the box, and it's easy enough to wipe the whole machine and start afresh!

### Jupyter Notebooks / Lab

Use the terminal/console and type:

```bash
$ conda activate dmlcp # or the name of your env
$ (dmlcp) jupyter lab
```

Then use the file explorer that opens in the browser to locate your notebook (explained below).

You have now launched the Jupyter Notebook programme from your terminal, and it has opened in the browser at localhost:8888. It is important to understand that the programme is running in the terminal, and we have a view of it through the browser. This client – server set up means that if we close the browser, the notebook doesn’t close.

### Colab

Three ways:

1. Open [Google Colab](https://colab.research.google.com), then select the option "Upload" from the window that pops up or go to (File --> Upload notebook), then drag and drop in your (downloaded) iPython notebook.
2. Go to your [Google Drive], upload the `.ipynb` file somewhere, then double click on it and select "Google Colaboratory" as the app to open it.
3. Open the link to an existing notebook. Go to "File", then "Save a copy in Drive". A copy should open in a new tab (you can then go to your drive, in "Colab Notebooks", and relocate it if you wish).

It is as simple as that. You don't need to install anything to use Google Colab. When you start working with it, you will notice that it is quite useful to connect it to your Google Drive to allow it to read and write files instead of re-uploading your data every time you start a new colab session.

In Python, this is how you do it (in a code cell):

```python
from google.colab import drive
drive.mount('/content/drive/')
```

### **!! NOTE: Never forget to check the _Runtime_ (CPU/GPU/TPU) in `Runtime → Runtime Type`**

## 4. ML Software

### PyTorch

Select the correct set-up [here](https://pytorch.org/get-started/locally/) and use the appropriate installation command.

#### Installing PyTorch on Mac M1/M2

If you are working on a Mac M1/2/3 (nice!), **Anaconda/Miniconda will not work, you will need to use Miniforge instead!**

If you need to uninstall Anaconda/Miniconda, use [these instructions](https://docs.anaconda.com/anaconda/install/uninstall/).

[Here](https://developer.apple.com/metal/pytorch/) are installations for PyTorch and Silicon acceleration.

### Huggingface/Gradio

Huggingface [recommends pip for their installation](https://huggingface.co/docs/transformers/installation)

```bash
# all the Huggingface things
pip install --upgrade transformers diffusers datasets accelerate
```

And [so does Gradio](https://www.gradio.app/guides/quickstart):

```bash
pip install gradio
```

## 5. Other dependencies

Here are instructions to install the other required dependencies for the DMLCP course. This assumes you followed the Python and Conda setup instructions and installed PyTorch by following the instructions above. Repeating these steps if all or some of the dependencies are already satisfied should cause no problem.

Conda is known for having speed issues, and against that there are two possibilities:
- use pip inside your conda environment: pip does not check dependencies, but it's fine, since you can just delete the environment and rebuild one if things go wrong.
- use something like `mamba`, that is much faster.

Now first make sure your environment is active `conda activate dmlcp`, then you can use `conda install -c conda-forge` to install the following packages:

- essentials
```bash
    jupyter numpy matplotlib pillow
```
- required by canvas:
```bash
    pycairo
```
-  image manipulation library
```bash
    opencv
```
- for `scikit-image`, use the Anaconda channel:
```bash
     conda install -c anaconda scikit-image 
```
- for `Py5Canvas` you also need pyglet:
```bash
pip install pyglet
```
- for the scraping notebooks:
```bash
    beautifulsoup4 selenium
```
-  for creating gifs in the DCGAN notebooks
```bash
    imageio
```

#### Note! That you may not need all of them. One good habit to gain is not to freak out when seeing an error saying a package isn't present, and install it when you need it.

### Canvas

If you have the `canvas.py` file in the same folder as a notebook importing it (and you've installed `pycairo`), everything will work. If you want to install [py5canvas](https://github.com/colormotor/py5canvas) on your system, open a terminal and write:

```
git clone https://github.com/colormotor/py5canvas.git
cd py5canvas
pip install -e .
```

(See [here](https://stackoverflow.com/a/59562571)). To update py5canvas to the latest version, navigate to the `py5canvas` directory in the terminal (using `cd`) and then:  `git pull`.


## 6. Git

A great way to get the code everytime the repository is updated is to use the Github App (but the Terminal is nice, too). It's also a wonderful tool for your projects and for collaborations, even if learning it well takes some investment.

### The GitHub App

Get the app [here](https://desktop.github.com/), and sign in using your GitHub credentials, and follow [these steps](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-and-forking-repositories-from-github-desktop) for `https://github.com/jchwenger/DMLCP`.

Getting updates in the App, click `Pull origin`.

### The Terminal:

```bash
$ git clone https://github.com/jchwenger/DMLCP
$ cd DMLCP # voilà
$ git:(main) # if you have oh my zsh, the branch will appear on the left in some form
```

To pull, just type `git pull`.

### VS Code

There is a little branch icon on the left, below the magnifying glass for searching. You can use Git directly within the editor.

#### A note on the GIT workflow

It is **highly** recommended, when working with a repo, that you create a separate *branch*, for instance `dev`, and do your work on this branch, commit things, etc. Then, you can pull new changes from the `upstream` branch (mine) without difficulty in your `master` branch. 

In the terminal, right after you cloned:

```bash
$ git:(main) git checkout -b dev # creates a branch dev and checkout ('moves') into it
$ git:(dev)
$ git:(dev) git checkout main # to come back (this will work *only* if your changes are committed or stashed)
$ git:(main) # here you can pull
```

#### A note on the Huggingface workflow

When working with [Huggingface Spaces](https://huggingface.co/spaces), if you want to work with your code locally, you will need to work with Git in the same way as with GitHub, which means:

- you need to log into your computer using the `huggingface_hub` CLI utility, and use your [personal token](https://huggingface.co/settings/tokens) (in the environment where you installed `transformers`, the utility should already be installed. After that, in a terminal, do:
  ```bash
  python -m pip install huggingface_hub # this should tell you it's already installed
  huggingface-cli login # this will ask for your token, found on your profile under settings > tokens
  ```
- you may need to save your ssh public key to your Huggingface account, and create one if you haven't already (this allows an utility like Git to send the public key to say "hey, it's me", and be recognised as such, like a password), see [this tutorial](https://huggingface.co/docs/hub/security-git-ssh).

Once this is done, you can push to your space from your machine using git commands.

#### Highly recommended: [Dan Shiffman's Git and GitHub for Poets](https://www.youtube.com/watch?v=BCQHnlnPusY&list=PLRqwX-V7Uu6ZF9C0YMKuns9sLDzK6zoiV)

## 7. Terminal things 💀💝

### Mac Users: Install Brew

A real nice [package manager](https://brew.sh/) for the Terminal.

```bash
/bin/bash -c “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
It's nice. Do it. 💖

### Windows Users: Install Bash on your machine!

Bash is the command line interface running on Linux and Mac. There are many tutorials on how to install this on Windows. Send me your favourite!

It's nice. Do it. 💖

### Bash users: Install the Z shell & Oh My ZSH!

The Z Shell and [Oh My ZSh](https://ohmyz.sh/) (a wrapper around the former, with lots of functionalities) are super nice and make life much easier on the terminal.

Bash (& Zsh) keyboard shortcuts for [PC/Linux](https://gist.github.com/tuxfight3r/60051ac67c5f0445efee), [MacOS](https://gist.github.com/simonsmith/c725d03d5bc5c3898e6a) & [Z shell tutorial](https://github.com/hmml/awesome-zsh/blob/master/README.md).

It's nice. Do it. 💖

## 8. ffmpeg tricks

Install ffmpeg with [conda](https://anaconda.org/conda-forge/ffmpeg):

`conda install -c conda-forge ffmpeg`

Visit [ffmpeg.org](https://ffmpeg.org) for reference on how to record, convert and stream audio and video.

Useful practice:

- Extract images from a video:

`ffmpeg -i foo.mp4 -r 25 folder/foo-%03d.jpeg`

`-i` for input file url

`-r` for rate. You could write `fps=25` instead.

`foo-%03d.jpeg` specifies to use decimal numbers composed of 3 digits padded with zeros to express the sequence number. The images will be saved in the `folder`.

This will extract 25 video frames per second from the video and will output them in files named foo-001.jpeg, foo-002.jpeg, etc. You can add `-s WxH` so that images are rescaled to fit new WxH values.
