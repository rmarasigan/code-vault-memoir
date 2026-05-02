# 💭 Linux Python Installation

### Prerequisites

#### Update your package lists
```bash
$ sudo apt update
```

#### Install required build dependencies
```bash
$ sudo apt install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

These ensure smooth compilation of Python versions like 3.14.4.

#### Install Pyenv
Run the official installer.

```bash
$ curl -fsSL https://pyenv.run | bash

...
WARNING: seems you still have not added 'pyenv' to the load path.

# Load pyenv automatically by appending
# the following to 
# ~/.bash_profile if it exists, otherwise ~/.profile (for login shells)
# and ~/.bashrc (for interactive shells) :

export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"

# Restart your shell for the changes to take effect.

# Load pyenv-virtualenv automatically by adding
# the following to ~/.bashrc:

eval "$(pyenv virtualenv-init -)"
```

This clones pyenv and plugins (`pyenv-doctor`, `pyenv-update`, `pyenv-virtualenv`).

### Shell Configuration

Append to `~/.bashrc` (interactive shells).

```bash
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
```

Also append the first three lines (without virtualenv-init) to `~/.profile` for login shells.

```bash
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
$ echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
$ echo 'eval "$(pyenv init - bash)"' >> ~/.profile
```

Reload shell.

```bash
$ exec "$SHELL"
```

### Install Python Version

List available versions:

```bash
$ pyenv install --list
```

Install Python 3.14.4 (confirmed available and installs successfully).

```bash
$ pyenv install 3.14.4

Downloading Python-3.14.4.tar.xz...
-> https://www.python.org/ftp/python/3.14.4/Python-3.14.4.tar.xz
Installing Python-3.14.4...
Installed Python-3.14.4 to /home/dev/.pyenv/versions/3.14.4
```

Verify.

```bash
$ pyenv versions

* system (set by /home/dev/.pyenv/version)
  3.14.4
```

Shows `* system` and `3.14.4`.

### Set Local Version
In your project directory, set local Python version (creates `.python-version` file).

```bash
$ pyenv local 3.14.4
```

This auto-activates the version in that directory and subdirectories.

Verify with `python --version` and `which python` (points to `~/.pyenv/versions/3.14.4/bin/python`).

### Optional: Virtual Environment
Create and activate project-specific `virtualenv`.

```bash
$ pyenv virtualenv 3.14.4 myproject-env
$ pyenv local myproject-env
```

Install packages with `pip install <package>`—they stay isolated.


### Run Python Scripts
Execute files with the active version.

```bash
$ python filename.py
```

## 📋 Related Articles
* [pyenv](https://github.com/pyenv/pyenv/tree/master)
* [Pyenv Commands](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)
* [Suggested build environment](https://github.com/pyenv/pyenv/wiki#suggested-build-environment)
* [How to Install and Use Pyenv in Ubuntu?](https://itslinuxfoss.com/install-use-pyenv-ubuntu/)
* [Pyenv in Ubuntu 22.04: ERROR: The Python ssl extension was not compiled. Missing the OpenSSL lib?](https://stackoverflow.com/questions/72842089/pyenv-in-ubuntu-22-04-error-the-python-ssl-extension-was-not-compiled-missing)