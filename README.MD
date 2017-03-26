## Caffe Installation In Linux Mint 18 with CUDA, cuDNN
#### Introduction
This is only a personal log which contains the problems I encountered when I was installing the Caffe Deep Learning framework.
My computer contains a Nvidia GTX980 Graphic Card, so it is suitable for me to install Caffe with CUDA and cuDNN supports.

I also added python3 support in the configuration file.

The installation guides of the Caffe official website are pretty well written, but sometimes it is still a tedious experience to installing it.
So I decide to share the problems that I have encountered and how I solved them.

For your convenience, I have attached my Makefile.config in this repo and you should modify it to match your own env if you want to use it. :)
##### make:protoc: command not found
 Solution:
 ```
 sudo apt-get install protobuf-c-compiler protobuf-compiler
 ```

##### fatal error: gflags/gflags.h: file or directory not found
 Solution:
 ```
 sudo apt-get install libgflags-dev
 ```
 
##### fatal error: glog/logging.h: file or directory not found
 Solution:
 ```
 sudo apt-get install libgoogle-glog-dev
 ```
 
#### fatal error: hdf5.h: file or directory not found
 Solution:
 ```
 sudo apt-get install libhdf5-*
 ```
 or if you use zsh use the following:
 ```
 sudo apt-get install libhdf5-\*
 ```
 Wait， it is still not done yet. You have to add the hdf5 include files to the "Makefile.config" file:
 ```
 # find this line:
 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
 
 # modify it to:
 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
 ```
 
##### fatal error: lmdb.h: file or directory not found
 Solution:
 ```
  sudo apt-get install liblmdb-dev
 ```
  

##### make: *** [.build_release/lib/libcaffe.so.1.0.0-rc3] Error 1
 The complete outputs maybe like the following:
 ```
  AR -o .build_release/lib/libcaffe.a
  LD -o .build_release/lib/libcaffe.so.1.0.0-rc3
  /usr/bin/ld: cannot find -lboost_python3
  collect2: error: ld returned 1 exit status
  Makefile:563: recipe for target '.build_release/lib/libcaffe.so.1.0.0-rc3' failed
  make: *** [.build_release/lib/libcaffe.so.1.0.0-rc3] Error 1
 ```
 
 Solution:
 Still you have to modify the config file:
 ```
  # find the following lines:
  INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial

  LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
    
  # modify them to the following:
  # if your machine is 64 bit:
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial

    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial
  # if your machine is 32 bit:
    INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial

    LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/i386-linux-gnu /usr/lib/i386-linux-gnu/hdf5/serial
 ```
 
 ##### LD ...atlas or blas: not found
  In the linking stage the compiler can not find atlas or blas library files:
  
  Solution:
  ```
    sudo apt-get install libopenblas-dev
  ```
  Then modify the config file:
  ```
  # find this:
  BLAS := atlas
  
  # modify it to:
  BLAS := open
  ```
  
  
 ##### LD: ...cuDNN lib: file not found:
  Solution:
  I have installed the cuDNN using the .deb file. It is also need the cudnn-8.0-linux-x64-v?.?.tgz file and extract it to the "/usr/local/cuda" directory and merge those files into it.
  (one include file to the include directory and lib64 directory to the lib64 directory)
  
