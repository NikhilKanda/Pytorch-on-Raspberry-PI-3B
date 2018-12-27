# *How to install PyTorch on the Raspberry Pi 3B+ (RPI)*

## A quick overview of PyTorch:  

PyTorch is an open-source machine learning library for Python, based on Torch,used for applications such as natural language

processing. It is primarily developed by Facebook's artificial-intelligence 

research group, and Uber's "Pyro" software for probabilistic programming is built on it.

To install PyTorch on the RPI you have to compile it from source, because there is no pre-compiled binary for ARMv7 / ARMhf

available. The following are the required steps to install PyTorch.


### Step 1: Install dependencies 

The very first step is to install dependencies for PyTorch. This step could be skipped if dependencies are already installed

    sudo apt-get install libopenblas-dev cython libatlas-dev m4 libblas-dev
    
### Step 2: Set compiler flags

The following Environmental variables are to be set
    
    export NO_CUDA=1            #CUDA is required for GPU access, since RPI doesn't have a GPU, we set No cuda to 1
    
    export NO_DISTRIBUTED=1     #distributed process parallelizes the computations across processes and clusters of machines
    
    export NO_MKLDNN=1          #MKLDNN expedites the computations, but NO_MKLDNN has to be set for error free compilation (don't know why)
    
### Step 3: Clone repository

Now, select/make directory of your choice as to where the pytorch source has to be cloned on RPI.

    Ex: mkdir pytorch_RPI

    cd pytorch_RPI

It's time to clone the pytorch repository. The latest stable version of pytorch 1.0 will be cloned using the below command

    git clone --recursive https://github.com/pytorch/pytorch

It hardly takes 10-15 min


### Step 4: Swap memory 

I don't exactly know the reason behind memory swap, but there has to be atleast 2 GB of swap memory for (16 GB SD card) and 4 GB of swap memory for 32 GB SD card to install pytorch. Select the swap memory accordingly

There are two ways to do that

1. /etc/fstab

    sudo dd if=/dev/zero of=/swap1 bs=1M count=2048

Make this empty file into a swap-compatible file

    sudo mkswap /swap1

Then disable the old swap space and enable the new one

    sudo nano /etc/fstab

This above command will open a text editor on your `/etc/fstab` file. The file should have this as the last line: `/swap0 swap swap`. In this line, please change the `/swap0` to `/swap1`. Then save the file with <key>CTRL</key>+<key>o</key> and <key>ENTER</key>. Close the editor with <key>CTRL</key>+<key>x</key>.

Now your system knows about the new swap space, and it will change it upon reboot, but if you want to use it right now, without reboot, you can manually turn off & empty the old swap space and enable the new one:

    sudo swapoff /swap0
    sudo swapon /swap1

2. Through dphys-swapfile

    vi /etc/dphys-swapfile
    
Now, change the variable CONF_SWAPSIZE value to 2048 and save it.

To apply the change

    sudo /etc/init.d/dphys-swapfile stop
    
    sudo /etc/init.d/dphys-swapfile start


Personally, I prefer the second way i.e., through dphys-swapfile. 

You can check the swap size by using the below command

    htop

### Step 5: PyTorch Compilation

Voila, Now open the directory into which the pytorch is cloned. In our case, it is 

    cd pytorch_RPI/pytorch

    python setup.py build

This takes almost 6-7 hours for compilation. 

### Step 6: Test

If everything went as expected, you can directly use the pytorch through Interpreter. 

Here's a quick link for tutorials on how to use pytorch 

    https://pytorch.org/tutorials/advanced/cpp_export.html
