Step 1:  Update and Upgrade your system:
	 $ sudo apt-get update
	 $ sudo apt-get upgrade

Step 2:  Verify You Have a CUDA-Capable GPU:
	 $ lspci | grep -i nvidia
	 Note: GPU model. eg GeForce GTX 1060

	 If you don't see any settings.Update the PCI hardware database that linux maintains by entering update-pciids(generall found in /sbin) at the
	 command line and return the previous lspci command.
	 If your graphics card is from NVIDIA then goto https://developer.nvidia.com/cuda-gpus and verify if listed in CUDA enabled gpu list.

Step 3:  Verify You Have a Supported Version of Linux:
	 To detemine which distribution and release number you're runing,type the following at the command line:
	 $ uname -m && cat /etc/*release
	 The x86_64 line indicates you have are run on a 64-bit system which os supported by cuda9.1

Step 4:  Install Dependencies:
	 Required to compile from source:
	 $ sudo apt-get install build-essential
	 $ sudo apt-get install cmake git unzip zip
	 $ sudo apt-get install python2.7-dev python3.5-dev pylint

Step 5:  Install linux kernel header:
	 Goto terminal and type:
	 $ uname -r
	 You can get like "4.10.0-42-generic". Note down linux kernal version.
	 To install linux header supported by your linux kernal do following:
	 $ sudo apt-get install linux-headers-$(uname -r)

Step 6:  Download the NVIDIA CUDA Toolkit:
	 Go to https://developer.nvidia.com/cuda-downloads and download installer for linux 16.04 x86_64 deb[network]. I highly recommand 
	 network installer to get update gpu driver supported by your linux kernal.
	 for,direct download
	 $ wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.1.85-1_amd64.deb
	
	 Installation instruction:
	 $ sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
	 $ sudo dpkg -i cuda-repo-ubuntu1604_9.1.85-1_amd64.deb
	 $ suda apt-get update
	 $ sudo apt-get install cuda

Step 7:  Reboot the system to load the NVIDIA drivers.

Step 8:  Go to terminal and type:
	 $ vi ~/.bashre
	
	 In the end of the file,add:
	 $ export PATH=/usr/local/cuda-9.1/bin${PATH:+:${PATH}}
	 $ export LD_LIBRARY_PATH=/usr/local/cuda-9.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
	 Save and exit.

	 $ source ~/.bashrc
	 $ sudo ldconfig
	 $ nvidia-smi
	 Check driver version probably Driver Versin:390.30
	 (not likely) If you got nvidia-smi is not found then you have unsupported linux kernel installed. 
	 Comment your linux kernel version noted in step 5.

Step 9:  Goto https://developer.nvidia.com/cudnn and download Membership required.
	 After login
	 Download the following:
	 cuDNN v7.0.5 Runtime Library for Ubuntu16.04 (Deb)

	 cuDNN v7.0.5 Developer Library for Ubuntu16.04 (Deb)

	 cuDNN v7.0.5 Code Samples and User Guide for Ubuntu16.04 (Deb)

	 Goto downloaded folder and in terminal perform following:
	 $ sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.1_amd64.deb
	 $ sudo dpkg -i libcudnn7-dev_7.0.5.15-1+cuda9.1_amd64.deb
	 $ sudo dpkg -i libcudnn7-doc_7.0.5.15-1+cuda9.1_amd64.deb
	
	 Verifying cuDNN installation:
	 $ cp -r /usr/src/cudnn_samples_v7/ $HOME
	 $ cd  $HOME/cudnn_samples_v7/mnistCUDNN
	 $ make clean && make
	 $ ./mnistCUDNN
	 If cuDNN is properly installed and runing on your linux system. you will see a message similar to the following:
	 Test passed!

Step 10: Install Dependencies;
	 libcupti(required)
	 $ sudo apt-get install libcupti-dev
	 $ echo 'export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
	
	 Bazel (required)
	 $ sudo apt-get install openjdk-8-jdk
	 $ wget https://github.com/bazelbuild/bazel/releases/download/0.9.0/bazel_0.9.0-linux-x86_64.deb
	 $ sudo dpkg -i bazel_0.9.0-linux-x86_64.deb

	 To install these packages for Python 2.7, issue the following command:
	 $ sudo apt-get install python-numpy python-dev python-pip python-wheel

	 To install these packages for Python 3.n, issue the following command:
	 $ sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel

Step 11: Configure Tensorflow from source:
	 Reload environment variables:
	 $ source ~/.bashrc
	 $ sudo ldconfig

	 Start the process of building TensorFlow by downloading latest tensorflow 1.4.1 :
	 $ wget https://github.com/tensorflow/tensorflow/archive/v1.4.1.zip
	 $ unzip v1.4.1.zip
	 $ cd tensorflow-1.4.1
	 $ ./configure

	 Give a python path in
	 Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3

	 Please enter two times

	 Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: Y
	 Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: Y
	 Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: Y
	 Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: Y
	 Do you wish to build TensorFlow with XLA JIT support? [y/N]: N
	 Do you wish to build TensorFlow with GDR support? [y/N]: N
	 Do you wish to build TensorFlow with VERBS support? [y/N]:N
	 Do you wish to build TensorFlow with OpenCL support? [y/N]:N
	 Do you wish to build TensorFlow with CUDA support? [y/N]: Y
	 Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 8.0]: 9.1
	 Please specify the location where CUDA 9.1 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda
	 Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 6.0]: 7.0.5
	 Please specify the location where cuDNN 7.0.5 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/lib/x86_64-linux-gnu

	 Now we need compute capability which we have noted at step 1 eg. 5.0

	 Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 5.0] 5.0

	 Do you want to use clang as CUDA compiler? [y/N]: N
	 Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: /usr/bin/gcc
	 Do you wish to build TensorFlow with MPI support? [y/N]: N
	 Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: -march=native

	 If you are building Tensorflow 1.5.0 then you will get following.
	 Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]:N

	 Configuration finished.

Step 12: Build Tensorflow using bazel
	 Do following to create symbolic link to cuda/include/math_functions.hpp 
	 from cuda/include/crt/math_functions.hpp to fix math_functions.hpp is not found error.
	 $ sudo ln -s /usr/local/cuda/include/crt/math_functions.hpp /usr/local/cuda/include/math_functions.hpp

	 If it says file already exits then ignore it.

	 The next step in the process to install tensorflow GPU version will be to build tensorflow using bazel. 
	 This process takes a fairly long time.
	
	 CPU-only support!!!
	 To build a pip package for TensorFlow with CPU-only support, you would typically invoke the following command:
	 $ bazel build --config=opt --incompatible_load_argument_is_label=false //tensorflow/tools/pip_package:build_pip_package

	 Note:
	 add "--config=mkl" if you want Intel MKL support for newer intel cpu for faster training on cpu

	 GPU-support!!!
	 To build a pip package for TensorFlow with GPU support, invoke the following command:
	 $ bazel build --config=opt --config=cuda --incompatible_load_argument_is_label=false //tensorflow/tools/pip_package:build_pip_package

	 This process will take a lot of time. It may take 1 – 2 hours or maybe even more.

	 The bazel build command builds a script named build_pip_package. 
	 Running this script as follows will build a .whl file within the tensorflow_pkg directory:
	 $ bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg

	 To install tensorflow with pip:
	 for python2:
	 $ pip2 install tensorflow*.whl

	 for python3:
	 $ pip3 install tensorflow*.whl

Step 13: Verify Tensorflow installation
	 $ python

	 import tensorflow as tf
	 hello = tf.constant('Hello, TensorFlow!')
	 sess = tf.Session()
	 print(sess.run(hello))

refer to the website:

http://www.python36.com/install-tensorflow141-gpu/
