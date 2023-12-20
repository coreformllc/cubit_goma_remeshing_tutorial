# cubit_goma_remeshing_tutorial

## Instructions
1.  Clone the [Goma](https://github.com/goma/goma) project, build and install the `goma` executable
    * Goma's [new build instructions](https://github.com/goma/goma/blob/main/BUILD.md), including Spack package instructions
    * Alternatively, I used their ["deprecated" build instructions](https://github.com/goma/goma/blob/main/scripts/README.md)
2.  

## Detailed Instructions for Ubuntu 22.04

I often find it helpful to see what someone else did, in addition to official documentation, so below I've included a log of my build, installation, and execution process on my Ubuntu 22.04 system.

### Step 1: Build and Install Goma
I personally wasn't able to figure out Goma's ["new build instructions"](https://github.com/goma/goma/blob/main/BUILD.md) nor their Spack-based installation.
Instead I used their ["deprecated" build instructions](https://github.com/goma/goma/blob/main/scripts/README.md):

```
mkdir ~/apps
cd ~/apps

git clone https://github.com/goma/goma.git

cd ~/apps/goma
sudo apt update
sudo apt install -y git build-essential m4 zlib1g-dev libx11-dev gfortran pkg-config autoconf
bash scripts/build-goma-dependencies.sh -j 16 ~/apps/goma/tpls/

export PATH="~/apps/goma/tpls/openmpi-4.1.1/bin:$PATH"
export PATH="~/apps/goma/tpls/trilinos-13.0.1/bin:$PATH"
source ~/apps/goma/tpls/config.sh

mkdir ~/apps/goma/install -p
cmake -B install/ .
make -j 16 -C install 
```

## Step 2: Build and Install SEACAS
Following the instructions from the [SEACAS GitHub page](https://github.com/sandialabs/seacas)

```
cd ~/apps
git clone https://github.com/sandialabs/seacas.git

cd ~/apps/seacas 
export ACCESS=`pwd`

# Install Third Party Libraries
apt install -y libaec-dev zlib1g-dev automake autoconf libcurl4-openssl-dev libjpeg-dev wget curl bzip2 m4 flex bison cmake libzip-dev openmpi-bin libopenmpi-dev
JOBS=16 FORCE=YES bash ./install-tpl.sh

cd $ACCESS
mkdir build
cd build
../cmake-config
make -j 16
make install
```

## Step 3: Download and Install Coreform Cubit

```
cd ~
wget https://f002.backblazeb2.com/file/cubit-downloads/Coreform-Cubit/Releases/Linux/Coreform-Cubit-2023.11%2B43088-Lin64.deb

sudo apt install 

sudo dpkg -i Coreform-Cubit-2023.11+43088-Lin64.deb
sudo apt --fix-broken install
sudo apt install -y libgl1
sudo dpkg -i Coreform-Cubit-2023.11+43088-Lin64.deb

coreform_cubit -nog -activate XXXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXXX
```

## Step 4: Clone this repository

```
mkdir ~/projects
cd ~/projects
git clone https://github.com/coreformllc/cubit_goma_remeshing_tutorial.git
```

## Step 5: Setup Python Environment

```
cd ~/projects/cubit_goma_remeshing_tutorial

sudo apt install python3.10-venv
python3 -m venv cubit-goma-remesh
source cubit-goma-remesh/bin/activate

pip install pyvista[all] meshio[all] ipykernel
```