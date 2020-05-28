## preparation
0. install office link of paraview: https://github.com/Kitware/ParaView/blob/master/Documentation/dev/build.md
<pre>
sudo apt-get install git cmake build-essential libgl1-mesa-dev libxt-dev qt5-default libqt5x11extras5-dev libqt5help5 qttools5-dev qtxmlpatterns5-dev-tools libqt5svg5-dev python3.7-dev python3-numpy libopenmpi-dev libtbb-dev ninja-build
</pre>
1. install git:
<pre>
// install git
sudo apt install git
//check the version of git
git --version
// set the user name and email of git
git config --global user.name "Lubos Rendek"
git config --global user.email "web@linuxconfig.org"
// verify the user name and email
git config --list
</pre>
2. install gcc: 
<pre>
sudo apt install build-essential
gcc --version
</pre>
3. install qt5
<pre>
sudo apt-get install qt5-default
//find the location of the package
dpkg -L qt5-default
</pre>
4. install cmake
<pre>
sudo apt-get -y install cmake
sudo apt-get -y install cmake-qt-gui
</pre>
5. install pyenv
<pre>
curl https://pyenv.run | bash
//copy the content  to ~/.bashrc, save
source ~/.bashrc
</pre>
6. install python
<pre>
//// the way to install python will causet the confustion of the different versions.
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.7
python ––version
//// correct way to install and mange python version
// add some related packages if the system does not have
sudo apt-get install zlib1g-dev
sudo apt-get update
sudo apt-get install build-essential libffi-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev
// check which version can be installed 
pyenv install --list
//install python version
pyenv install 3.7.4
// change the default version of python in the system
pyenv global 3.7.4
// check the default version
pyenv versions
</pre>

## install paraview
<pre>
// clone and enter repo
git clone https://github.com/Kitware/ParaView paraview
cd paraview
// checkout version 5.7.0
git checkout v5.7.0
// pull all submodules
git submodule update --init --recursive
// create and enter build folder
mkdir build
cd build
// configure build
ccmake .. // install cmake-gui, cmake-gui .. can show the UI to set the configuration.
set the following options:
CMAKE_INSTALL_PREFIX = pathToYourParaViewRepo/install
CMAKE_BUILD_TYPE=Release
PARAVIEW_ENABLE_PYTHON=ON
PARAVIEW_PYTHON_VERSION=3
PARAVIEW_INSTALL_DEVELOPMENT_FILES=ON
(some options are only visible in the advanced mode, press 't' to display the advanced mode.)
// build and install with N cores.
/************* Notice!: in macos, you have to change python 3.8.0 to other version, like python 3.7.7. otherwise, the you can build successfully **********/
make -jN install
</pre>
