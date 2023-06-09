# Installing Python using Linux Command Line


- [Installing Python using Linux Command Line](#installing-python-using-linux-command-line)
  - [Installing Python from Source](#installing-python-from-source)
    - [The libraries and dependencies necessary to build Python:](#the-libraries-and-dependencies-necessary-to-build-python)
    - [Get latest Python release and build:](#get-latest-python-release-and-build)
    - [Change python version system-wide with update-alternatives python](#change-python-version-system-wide-with-update-alternatives-python)
  - [Installing Python using apt](#installing-python-using-apt)
  - [Installing Packages using pip](#installing-packages-using-pip)
  - [Virtual environments and packages](#virtual-environments-and-packages)


## Installing Python from Source

### The libraries and dependencies necessary to build Python:
- [build-essential](https://packages.debian.org/search?lang=en&searchon=names&keywords=build-essential): The build-essentials packages are the form of meta-packages that are essential to compile software. They contain the GNU/g++ compiler collection, GNU debugger, and a few more libraries and tools that are needed for compiling a program. A few other packages, like :
  - **g++:** It is a GNU compiler for C++ language.
  - **gcc:** It is a GNU compiler for C language.
  - **make:** It is a helpful utility that is used to direct the program's compilation.
  - **libc6-dev:** It is a GNU C library. It includes the header files and development directories used for compiling general C++ and C scripts.
  - **dpkg-dev:** This package is used to upload, build, and unpack Debian source packages.
- [zlib1g-dev](https://packages.debian.org/en/sid/zlib1g-dev)
- [libncurses5-dev](https://packages.debian.org/en/sid/libncurses5-dev)
- [alibgdbm-dev](https://packages.debian.org/en/sid/libgdbm-dev)
- [libnss3-dev](https://packages.debian.org/en/sid/libnss3-dev)
- [libssl-dev](https://packages.debian.org/en/sid/libssl-dev)
- [libreadline-dev](https://packages.debian.org/en/sid/libreadline-dev)
- [libffi-dev](https://packages.debian.org/en/sid/libffi-dev)
- [libsqlite3-dev](https://packages.debian.org/en/sid/libsqlite3-dev)
- [wget](https://packages.debian.org/en/sid/wget)
- [libbz2-dev](https://packages.debian.org/en/sid/libbz2-dev)

```sh
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev

```

### Get [latest Python release](https://www.python.org/downloads/source/) and build:

```sh
wget https://www.python.org/ftp/python/3.11.3/Python-3.11.3.tgz
tar -xf Python-3.11.3.tgz
cd Python-3.11.3
# --enable-optimizations: optimizes the Python binary by running multiple tests
./configure --enable-optimizations


# For faster build time, -j nproc
# nproc: number of cores in your processor
make -j 8
# Install the Python binaries
sudo make altinstall
python3.11 --version

```
### Change python version system-wide with update-alternatives python
```sh
# Setting Default Python
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 2

#Confirm the new setting.
sudo update-alternatives --list python
sudo update-alternatives --config python

python --version
#Ensuring pip is available
python -m pip -V

#If you see an error, there are 2 mechanisms to install pip:
# 1) Python comes with an ensurepip module, which can install pip in a Python environment.
python -m ensurepip --upgrade
# 2) Python script that uses some bootstrapping logic to install pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py --user

```


## Installing Python using apt 

``` sh 
sudo apt update && sudo apt upgrade -y

# sudo apt show software-properties-common
sudo apt install software-properties-common

sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.11
python3.11 --version
python -m pip -V
#If you see an error
apt install python3-pip

```

## [Installing Packages using pip](https://packaging.python.org/en/latest/tutorials/installing-packages/)

```sh
# Install ansible using pip
# --user will cause them to be installed inside the user base’s binary directory
# You can find the user base binary directory by running python -m site --user-base and adding bin to the end.
python -m pip install  --user ansible
ansible --version
which ansible
# Show information about one or more installed packages
python -m pip show ansible
```

## [Virtual environments and packages](https://docs.python.org/3/tutorial/venv.html)
Applications will sometimes need a specific version of a library.virtual environment, a cooperatively isolated runtime environment that allows Python users and applications to install and upgrade Python distribution packages without interfering with the behaviour of other Python applications running on the same system.

```sh
sudo apt install python3.11-dev python3.11-venv -y
# Creating Virtual Environments
mkdir ~/envs && cd ~/envs
python -m venv ansible-env

# Activating the virtual environment 
source ~/envs/ansible-env/bin/activate

# Deactivating the virtual environment 
deactivate

# Installing ansible version 7.5.0
python -m pip install ansible==7.5.0
# Upgrading the ansible to the latest version
python -m pip install --upgrade ansible

# Displaying information about ansible
python -m pip show ansible

# Displaying all of the packages installed in the virtual environment
python -m pip list

# Producing list of the installed packages in requirements.txt
python -m pip freeze > requirements.txt
cat requirements.txt

# Installing collected packages
python -m pip install -r requirements.txt

```