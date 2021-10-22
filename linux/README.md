##### Linux

1. Install libgmp-dev
2. Install CUDA 11.0
3. Replace the Main.cpp and Rotor.cpp files in the Rotor folder with these.

 - Edit the makefile and set up the appropriate CUDA SDK and compiler paths for nvcc. 
 - Or pass them as variables to `make` command.
 - Install libgmp: ```sudo apt install -y libgmp-dev```


    ```make
    CUDA       = /usr/local/cuda-11.0
    CXXCUDA    = /usr/bin/g++
    ```
 - To build CPU-only version (without CUDA support):
    ```sh
    $ make all
    ```
 - To build with CUDA: pass CCAP value according to your GPU compute capability
    ```sh
    $ cd Rotor-Cuda
    $ make gpu=1 CCAP=75 all
    ```

