title: "Set Up Development Environment On a New Mac"
date: 2015-03-30 23:05:15
categories: [Mac]
tags: [tutorial]

---

In this tutorial you will set up the development environment on a new Macbook for some major languages like [Python](https://www.python.org/), [Ruby](https://www.ruby-lang.org/en/), [Java](http://www.oracle.com/technetwork/java/index.html) and [Node](https://nodejs.org/) environments. Even if you don't program in all four, it is good to have them as many command-line tools use one of them.

<!-- more -->

# System Update

First of all, update your system. For that: **Apple Icon -> About This Mac -> Software Update**


# System Preferences

In **Apple Icon -> System Preferences:**

- Trackpad -> 
	- Tap to click
	- Secondary click -> Click in bottom right corner
- Keyboard ->
	- Key Repeat -> Fast(all the way to the right)
	- Delay Until Repeat -> Short(all the way to the right)
- Dock -> Automatically hide and show the Dock, and make the size of Icons small

In **Battery Icon -> Show Percentage**


# Homebrew

The most popular package manager for OS X is [Homebrew](http://brew.sh/).

## Install

An important dependency before Homebrew can work is the **Command Line Tools** for Xcode. These include compilers that will allow you to build things from source.

If you're Iphone or Mac apps developer, you can install [Xcode](https://developer.apple.com/xcode/) from the **App Store** or the **Apple developer website**.
   
For installing **Xcode command line tools** run the command in terminal:
   
```
xcode-select --install
```
   
If you don't need the **Xcode** which weights something like 5GB+, and your OS X is 10.9 or newer. Just run the command in case 1. 
      
If you don't need the **Xcode** and your OS X is 10.8 or older, go to [Apple developer website](http://developer.apple.com/downloads), sign in with your Apple ID to search and download the **Xcode command line tools**.

Finally, we can install Homebrew.

```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Add **'/usr/local/bin'** to your **$PATH** environment variable:

```
$ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
```

Run the following command to make sure everything works:

```
$ brew doctor
```

## Usage

To search a package:

```
$ brew search <formula>
```

To install a package:

```
$ brew install <formula>
```

To see infomation of a package:

```
$ brew info <formula>
```

To update Homebrew's directory of fomula:

```
$ brew update
```

To see if any of your packages need to be updated:

```
$ brew outdated
```

To update all package:

```
$ brew upgrade --all
```

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

```
$ brew cleanup
```

To see what you have installed (with their version numbers):

```
$ brew list --versions
```

## Cask

Your software is just one command away from being ready and raring to go.

Forget all about babysitting the install process step by step, from website to cleanup.

### Installation

Install [Homebrew-cask](http://caskroom.io/) is really straight forward. We just need to add the cask tap and then install brew cask.

```
$ brew install caskroom/cask/brew-cask
```

### Usage

Brew cask's command just likes the brew's command, you can use following command to see all the brew cask's commands:

```
$ brew cask --help
```

## Homebrew Services

On most unixes, alongside the package manager, there’s a tool to start daemon services, install and uninstall them. There was no such thing with Homebrew until [Homebrew services](https://github.com/Homebrew/homebrew-services) command was added.

### Installation

```
$ brew tap homebrew/services
```

### Usage

Start service:

```
$ brew services start <formula>
```

Stop service:

```
$ brew services stop <formula>
```

To see all the loaded services:

```
$ brew services list
```

Show all the commands:

```
$ brew services --help
```


# GFW

## Shadowsocks

[Shadowsocks](http://shadowsocks.org/en/index.html) is a secure socks5 proxy.

### Installation

```
$ brew cask install shadowsocksx
```

### Server

You can use a free VPS or you can set up your own VPS like me.

BTW, I personally prefer DigitalOcean and my referral link is [https://www.digitalocean.com/?refcode=4b9b1935127e](https://www.digitalocean.com/?refcode=4b9b1935127e​). If you use my referral link to sign up, you will ONLY be charged $15( $10 will be reimbursed to you after you pay $25), and I'll be rewarded as well.

Buy a server with **Ubuntu** I suggest. And add your ssh-key.

#### Log in Your VPS

```
$ ssh root@<your_server_ip>
```

#### Install Shadowsocks

```
apt-get install python-pip
pip install shadowsocks
```

#### Configure Shadowsocks

Create a file named **'shadowsocks.json'** in **/etc/**.

```
{
    “server”:”<your_server_ip>”,
    “server_port”:<your_port>,
    “local_address”: “127.0.0.1″,
    “local_port”:1080,
    “password”:”<your_password>”,
    “timeout”:300,
    “method”:”aes-256-cfb”,
    “fast_open”: false
}
```

### Usage

**Shadowsocksx Icon -> Servers -> Open Server Preference**

Add a new server. Then turn the shadowsocks on.

```
Address: <your_server_ip> : <your_port>
Encryption: <your_encryption>
Password: <your_password>
```

OK, enjoy your VPS now.

## Proxychains-Ng

[proxychains ng](https://github.com/rofl0r/proxychains-ng) (new generation) - a preloader which hooks calls to sockets in dynamically linked programs and redirects it through one or more socks/http proxies. continuation of the unmaintained proxychains project.

### Installation

```
$ brew install proxychains-ng
```

### Configuration

Edit the **'proxychains.conf'**:

```
$ mvim /usr/local/etc/proxychains.conf
```

Add the proxy ip to **[ProxyList]**:

```
socks5  127.0.0.1 1080
```

And turn on the **'dynamic_chain'**.

### Usage

```
$ proxychains4 curl www.twitter.com
```


#Git

I can't imagine that a developer without [Git](http://git-scm.com/)

##Installation

Install Git:

```
$ brew install git
```

Test:

```
$ git --version
```

##Configuration

Define your git user:

```
$ git config --global user.name <your_user_name>
$ git config --global user.email <your_email>
```

##SSH Config for Github

You can see more details in [Github help](https://help.github.com/articles/generating-ssh-keys/).

##.gitignore

Create the file ~/.gitignore as shown below to remove the files that are almost always ignored in all git repositories.

**NOTICE**: On a Mac, it is important to remember to add **.DS_Store** (a hidden OS X system file that's put in folders) to your .gitignore files.

```
# Folder view configuration files
.DS_Store
Desktop.ini

# Thumbnail cache files
._*
Thumbs.db

# Files that might appear on external disks
.Spotlight-V100
.Trashes

# Compiled Python files
*.pyc

# Compiled C++ files
*.out

# Application specific files
venv
node_modules
.sass-cache
```


# iTerm2

## Installation

Let's install a better terminal than the default one. Download and install [iTerm2](http://www.iterm2.com).

## Configuration

Open the Preference.

- General: 
	- Uncheck **Open profiles window**	
- Profiles:
	- Colors: I personally prefer the [Solarized dark](https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized), and  the **cursor text** and **cursor color** to yellow make it more visible.
	- Text: I choose **14pt Menlo**.
	- Window: Adjust the **transparency** to 30%, and set the **columns** to 100, **rows** to 25.
- Keys: Set hotkey to open and close the terminal to **'command + option + \ '**, or else you like. 

## ZSH

### Step 1: Check for Zsh

```
$ cat /etc/shells
```

If you don't have zsh, please go to **Install zsh**. Otherwise, you can skip to **Step 3

### Step 2: Install Zsh

```
$ brew install zsh
```

### Step 3: Change shell to zsh for current user

```
$ chsh -s /bin/zsh
```

### Step 4: Oh My ZSH

Install oh-my-zsh:

```
$ curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
```

Edit the **.zshrc** by opening the file in a text editor:

```
# Path to your oh-my-zsh installation.
export ZSH=/Users/<your_username>/.oh-my-zsh

# Set name of the theme to load.
# Look in ~/.oh-my-zsh/themes/
# Optionally, if you set this to "random", it'll load a random theme each
# time that oh-my-zsh is loaded.
ZSH_THEME="agnoster"

# set name of default user
DEFAULT_USER="<your_username>"

# some useful plugins
plugins=(git textmate ruby autojump osx mvn gradle colored-man colorize github jira vagrant virtualenv pip python brew osx zsh-syntax-highlighting)
[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh
```


# Python

## Python 2.x

```
$ brew install python
```

Test with:

```
$ which python
```

and the terminal should output **'/usr/local/bin/python'**. If output **'/usr/bin/python'**, please:

Edit **'.bash_profile'**:

```
#path
export PATH=$PYTHON:$PATH

#python
export PYTHON=/usr/local/share/python
```

Enter following in your Terminal: 

```
cd /System/Library/Frameworks/Python.framework/Versions
sudo rm Current
sudo ln -s /usr/local/Cellar/python/2.7.9/Frameworks/Python.framework/Versions/Current
```

Test with:

```
$ which python
```

and the terminal should output **'/usr/local/bin/python'**.

## Python 3.x

```
$ brew install python3
```

Test it with:

```
$ which python3
```

and the terminal should output **'/usr/local/bin/python3'**.

## Pip

Upgrade [Pip](https://pypi.python.org/pypi/pip) and [Setuptools](https://pypi.python.org/pypi/setuptools) first:

```
# Installs with root permissions: 'sudo'
# Python uses pip while Python3 uses pip3.
$ sudo pip install --upgrade setuptools
$ sudo pip install --upgrade pip
```

To intall a python package:

```
$ sudo pip install <package_name>
```

To upgrade a package:

```
$ sudo pip install --upgrade <package_name>
```

To see what's installed:

```
$ pip freeze
```

To uninstall a package:

```
$ sudo pip uninstall <package_name>
```

## Virtualenv

[Virtualenv](https://virtualenv.pypa.io/en/latest/) is a tool to create isolated Python environments.

### Install

```
# python3 uses pip3
$ sudo pip install virtualenv
```

Create some directories to store our projects and virtual environments:

```
$ sudo mkdir -p <your_projects_path>/<your_virtual_environments>
```

We’ll then open the **'~/.bashrc'** file (which may be created if it doesn’t exist yet):

```
$ mvim ~/.bashrc
```

and add:

```
# pip should only run if there is a virtualenv currently activated
export PIP_REQUIRE_VIRTUALENV=true

# cache pip-installed packages to avoid re-downloading
export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache

# global python
gpip(){
   PIP_REQUIRE_VIRTUALENV="" sudo pip "$@"
}

gpip3(){
   PIP_REQUIRE_VIRTUALENV="" sudo pip3 "$@"
}
```

Reload the bash environment:

```
$ source ~/.bash_profile
```

Now, we can just use pip/pip3 to install <package> in virtual environment, and use gpip/gpip3 to install global package.

### Usage

To create a new virtual environment

```
$ cd <your_virtual_environments>
$ sudo virtualenv <your_venv_name>
```

If you have both Python 2.x and 3.x and want to create a Python 3.x virtualenv:

```
$ sudo virtualenv-3.x <your_venv_name>
```

**NOTICE**: Remember to add **'your_venv_name'** to your project's **'.gitignore'** file so you don't include all of that in your source code!

##Numpy, Scipy and Matplotlib

[NumPy](http://www.numpy.org/) is the fundamental package for scientific computing with Python.  
[SciPy](http://www.scipy.org/) is a Python-based ecosystem of open-source software for mathematics, science, and engineering.  
[Matplotlib](http://matplotlib.org/) is a python 2D plotting library which produces publication quality figures in a variety of hardcopy formats and interactive environments across platforms.

```
# First of all, we need to install some dependencies.
# Add these formulae to Homebrew.
$ brew tap samueljohn/python
$ brew tap homebrew/science

# Install GCC dependency for the libraries.
$ brew install gcc

# For the libraries.
$ sudo pip install nose
$ sudo pip install pyparsing
$ sudo pip install python-dateutil
$ sudo pip install pep8

# Finally, We can install them!
$ brew install numpy
$ brew install scipy
$ brew install matplotlib
```

## IPython

[IPython](http://ipython.org/) is an awesome project which provides a much better Python shell than the default one.

To install:

```
# Install some dependencies.
$ brew update
$ brew install zeromq # Necessary for pyzmq
$ brew install pyqt # Necessary for the qtconsole
$ sudo pip install pyzmq
$ sudo pip isntall pygments
$ sudo pip install jinja2
$ sudo pip install tornado

# Install ipython.
$ sudo pip install ipython
```


# Ruby

We're going to use [rbenv](https://github.com/sstephenson/rbenv) to install and manage our Ruby versions.  
Run the following commands in your Terminal:

```
$ brew install rbenv ruby-build rbenv-gem-rehash rbenv-default-gems

# Add rbenv to bash so that it loads every time you open a terminal
$ echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
$ source ~/.bash_profile

# Install Ruby
$ rbenv install 2.2.x
$ rbenv global 2.2.x
$ rbenv versions
```

Install bundler. Bundler manages an application's dependencies:

```
$ gem install bundler
$ echo 'bundler' >> "$(brew --prefix rbenv)/default-gems"
```

Skip r-doc generation if you use Google for finding your Gem documentation:
```
$ echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc
```

Install Rails:

```
$ gem install rails
$ echo 'rails' >> "~/.rbenv/default-gems"
```


# Java

Download jdk from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html) and install it.

If you have more than one jdk.

You can add following in your **'bash_profile'**:

```
export JAVA_6_HOME=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home  
export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7/Contents/Home  
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8/Contents/Home  
export JAVA_HOME=$JAVA_8_HOME
```

Command following:
```
$ alias jdk8='export JAVA_HOME=$JAVA_8_HOME' 
$ alias jdk7='export JAVA_HOME=$JAVA_7_HOME' 
$ alias jdk6='export JAVA_HOME=$JAVA_6_HOME'
```

Now, you can configure your jdk version with command jdk6, jdk7, jdk8 in terminal.


# Node.js

Install [node.js](https://nodejs.org/) with Homebrew:

```
$ brew update
$ brew install node
```

## Npm Usage

```
# Remember the "sudo"!
# Install a package.
$ sudo npm install [-g] <package_name> # With -g means global.

# Uninstall a package.
$ sudo npm uninstall <package_name>

# List what's installed.
$ npm list [-g]

# Find outdated packages.
$ npm outdated [-g]

# Upgrade all or a particular package.
$ sudo npm upgrade [<package_name>]
```