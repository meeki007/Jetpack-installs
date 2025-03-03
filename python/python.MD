# Python

#### Update your system
```
$ sudo apt update
```
#### install pip and pip3 for jetpack's python(s)
Do it now. As if its not installed it will break native pip for jetpack
```
sudo apt-get install python-pip python3-pip -y
```
test install
```
$ python2 -m pip --version
$ python3 -m pip --version
```
upgrade them while were here
```
$ sudo -H python -m pip install --upgrade pip
$ sudo -H python3 -m pip install --upgrade pip
```
#### Install dependencies
```
$ sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev -y
```
#### Make sure your in your home directory
```
$ cd $HOME
```
#### Download Python
Copy the url to the python version you want to install ---> https://www.python.org/downloads/source/

I will be using Python3.9.12 for this guide. Change the link to the version you want if different
```
$ wget https://www.python.org/ftp/python/3.9.12/Python-3.9.12.tgz
```
#### Extract tar
If you have used a different version change ***Python-3.9.12.tgz*** to the version you download
```
$ tar -xf Python-3.9.12.tgz
```
#### Move into dir of extracted tar
If you have used a different version change ***Python-3.9.12*** to the version you download
```
$ cd Python-3.9.12
```
#### Configure Python
```
$ ./configure --enable-optimizations
```
#### Run make with all cores
```
$ make -j $(nproc)
```
#### Install Python
```
$ sudo -H make altinstall
```
### Check that Python installed
If you have used a different version change ***python3.9*** to the version you download
```
python3.9 -c 'import sys; print(sys.version)'
```

#### Clean up after install
Go back to home dir
```
$ cd $HOME
```
If you have used a diffrent version change ***Python-3.9.12*** and ***Python-3.9.12.tgz*** to the version you downloaded
```
$ sudo rm -r Python-3.9.12 && rm Python-3.9.12.tgz
```


# update-alternatives pointing to python

Right now if you or some script tries to use python3 from the command line it will default to the version that came with jetpack. This is fine if you want to call your altinstall of python every time with ***python3.9*** or the version you installed.

To avoid this you can call pip from python3.9 like so.
```
$ python3.9 -m pip --version
```

However you can fix this behavior with update-alternatives

#### Pointing python3 to python version you installed using update-alternatives
```
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 0
$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.9 1
```

Note: if you messed up and need to remove an alturnative ***$ sudo update-alternatives --remove python3 /usr/local/bin/python3.9***

#### configure (select the one you want to have priority, make it auto)
```
$ sudo update-alternatives --config python3
```
Now check that python3 is working with python3.9

It should show the version you installed
```
$ python3 --version
```

However ***Best Practace*** if using pip is to always use python -m to use pip for that python version
```
$ python3.9 -m pip --version
```
# Upgrade and or install all pip version for python3.9
If you have used a diffrent version change ***Python-3.9.12*** and ***Python-3.9.12.tgz*** to the version you downloaded
```
$ sudo -H python3.9 -m pip install --upgrade pip
```

###### END OF DOCUMENT
