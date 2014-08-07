# Theano and CUDA 

## Theano on the CPU

Install Theano with

    pip install theano

Then try the mandatory MNIST tutorial from http://github.com/lisa-lab/DeepLearningTutorials/blob/master/code/DBN.py

It seems to works (still training), but takes time

## CUDA 

Trying to make things faster

First, which card do I have?

    $> lspci
    (and search for your card there)
    GeForce GTX 560 Ti

I found some info there:

    http://www.nvidia.com/object/product-geforce-gtx-560ti-gtx-550ti-us.html

    CUDA Cores  384
    Graphics Clock  (MHz)    822
    Processor Clock (MHz)   1645
    Memory Specs:   
     Standard Memory Config  1GB
    Feature Support:    
     NVIDIA CUDA Technology 
     OpenGL  4.1 

## Install & configure CUDA on Ubuntu 12.04 LTS


From https://developer.nvidia.com/cuda-downloads#linux

    wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1204/x86_64/cuda-repo-ubuntu1204_6.0-37_amd64.deb

From http://www.r-tutor.com/gpu-computing/cuda-installation/cuda6.0-ubuntu

    $ sudo dpkg -i cuda-repo-ubuntu1204_6.0-37_amd64.deb
    $ sudo apt-get update
    $ sudo apt-get install cuda 

Testing:

    richarde@SV-38-059:~$ nvcc --version
    nvcc: NVIDIA (R) Cuda compiler driver
    Built on Thu_Mar_13_11:58:58_PDT_2014
    Cuda compilation tools, release 6.0, V6.0.1

Parenthesis: This did not work

    wget http://developer.download.nvidia.com/compute/cuda/6_0/rel/installers/cuda_6.0.37_linux_64.run
    sudo sh cuda_6.0.37_linux_64.run

    * Please make sure your PATH includes /usr/local/cuda/bin
    * Please make sure your LD_LIBRARY_PATH
    *   includes /usr/local/cuda/lib64:/usr/local/cuda/lib
    * OR
    *   add /usr/local/cuda/lib64 and /usr/local/cuda/lib
    * to /etc/ld.so.conf and run ldconfig as root

    vim .bashrc
    PATH=$PATH:/usr/local/cuda/bin
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/lib


## Configure for Theano

From http://deeplearning.net/software/theano/install.html#gpu-linux
Well, not much to configure...

Testing config: http://deeplearning.net/software/theano/tutorial/using_gpu.html#using-gpu

    cd /home/richarde/dev//theano
    THEANO_FLAGS='mode=FAST_RUN,cuda.root=/usr/local/cuda,device=gpu,floatX=float32' python check1.py

Woops:

    ERROR (theano.sandbox.cuda): Failed to compile cuda_ndarray.cu: libcublas.so.6.0: cannot open shared object file: No such file or directory
    Looping 1000 times took 9.06630682945 seconds
    Used the cpu

    sudo ldconfig /usr/local/cuda/lib64

Now we're good:

    Using gpu device 0: GeForce GTX 560 Ti
    Looping 1000 times took 0.494688987732 seconds
    Used the gpu

Acceleration factor `9.0663 / 0.4946 = 18x`, nice!

