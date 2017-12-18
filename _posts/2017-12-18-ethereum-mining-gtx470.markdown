---
layout: post
title:  "Ethereum mining on GTX470"
date:   2017-12-18 20:18:00 +0200
categories: mining ethereum 
---

# TL;DR

GTX470 with 1.2GB of VRAM cannot hold the DAG.
OpenCL or CUDA, it doesn't matter.


# Building ethminer from the sources

`git clone https://github.com/ethereum-mining/ethminer.git` 

got commit 6fbf438 from the 15th of december :-)

```
mkdir build
cd build
cmake ..
```

which used default config:

```
---------------------------------------------------------- components
-- ETHASHCL         Build OpenCL components                  ON
-- ETHASHCUDA       Build CUDA components                    OFF
-- ETHSTRATUM       Build Stratum components                 ON
-- ETHDBUS          Build D-Bus components                   OFF
-- APICORE          Build API Server components              ON
```

`cmake --build .`

# Running in benchmark mode

`build$ ./ethminer/ethminer -M -G`


```
  ℹ  20:10:53|ethminer  Found suitable OpenCL device [ GeForce GTX 470 ] with 1277886464  bytes of GPU memory
Benchmarking on platform: CL
Preparing DAG for block #0
 cl  20:10:53|cl-0      No work. Pause for 3 s.
Warming up...
 cl  20:10:56|cl-0      New work: header #50c856ae… target 0000000000000002000000000000000000000000000000000000000000000000
 cl  20:10:56|cl-0      New seed #00000000…
 cl  20:10:56|cl-0      Platform: NVIDIA CUDA
 cl  20:10:56|cl-0      Device:   GeForce GTX 470  / OpenCL 1.1 CUDA
 cl  20:10:56|cl-0      OpenCL 1.1  not supported - minimum required version is 1.2
  ✘  20:10:56|cl-0      OpenCL Error: clEnqueueWriteBuffer -36
Trial 1... 0
Trial 2... 0
Trial 3... 0
Trial 4... 0
Trial 5... 0
min/mean/max: 0/0/0 H/s
inner mean: 0 H/s
```

Well, not very impressive :-(  It seems to fail at finding the right openCL version, because clinfo states OpenCL 1.2 and 2.1 are available. Never mind since a more obvious problem is the VRAM amount : only 1.2GB available where at least 2GB are required to host the DAG... have to wait for another video card, and better switch to another currency for tonight


# Retry with CUDA configuration 

`cmake .. -DETHASHCUDA=ON`

`cmake --build .`

```
[  0%] Built target BuildInfo.h
Scanning dependencies of target devcore
[  3%] Building CXX object libdevcore/CMakeFiles/devcore.dir/RLP.cpp.o

....

[ 54%] Built target ethcore
[ 57%] Building NVCC (Device) object libethash-cuda/CMakeFiles/ethash-cuda.dir/ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o
nvcc fatal   : Unsupported gpu architecture 'compute_60'
CMake Error at ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o.cmake:207 (message):
  Error generating
  /home/fribes/mining/ethminer/build/libethash-cuda/CMakeFiles/ethash-cuda.dir//./ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o


libethash-cuda/CMakeFiles/ethash-cuda.dir/build.make:63 : la recette pour la cible « libethash-cuda/CMakeFiles/ethash-cuda.dir/ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o » a échouée
make[2]: *** [libethash-cuda/CMakeFiles/ethash-cuda.dir/ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o] Erreur 1
CMakeFiles/Makefile2:407 : la recette pour la cible « libethash-cuda/CMakeFiles/ethash-cuda.dir/all » a échouée
make[1]: *** [libethash-cuda/CMakeFiles/ethash-cuda.dir/all] Erreur 2
Makefile:149 : la recette pour la cible « all » a échouée
make: *** [all] Erreur 2
``` 


removed 

`-gencode arch=compute_60,code=sm_60;-gencode arch=compute_61,code=sm_61;-gencode arch=compute_62,code=sm_62;-gencode arch=compute_70,code=sm_70`

from 

`ethash-cuda_generated_ethash_cuda_miner_kernel.cu.o.cmake`

new error: 

```
/usr/include/string.h: In function ‘void* __mempcpy_inline(void*, const void*, size_t)’:
/usr/include/string.h:652:42: error: ‘memcpy’ was not declared in this scope
   return (char *) memcpy (__dest, __src, __n) + __n;
```

added `-D_FORCE_INLINES`

in the same file, for nvcc flags

`set(nvcc_flags -m64;-DETH_ETHASHCL;-DETH_ETHASHCUDA;-DETH_STRATUM;-DAPI_CORE;-D_FORCE_INLINES ) # list`

HUNTER dependency manager might get in the path and rewrite file, but not always

Finally got executable with CUDA support:

```
build$ ./ethminer/ethminer -M -U
 cu  20:58:47|ethminer  Using grid size 8192 , block size 128
Benchmarking on platform: CUDA
Preparing DAG for block #0
Warming up...
  ℹ  20:58:47|CUDA0     set work; seed: #00000000, target:  #0000000000000002000000
  ℹ  20:58:47|CUDA0     Initialising miner...
 cu  20:58:48|CUDA0     Using device: GeForce GTX 470  (Compute 2.0)
CUDA error in func 'init' at line 247 : out of memory.
Cuda error in func 'set_header' at line 178 : invalid device symbol.
``` 

Out of memory => GAME OVER