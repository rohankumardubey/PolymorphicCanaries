# To-Detect-Stack-Buffer-Overflow-With-Polymorphic-Canaries.  
=======================================================.  
A High Efficient Protection against Brute-force Attacks
=======================================================. 

## Authors
- Zhilong Wang <mg1633081@smail.nju.edu.cn>
- Xuhua Ding <xhding@smu.edu.sg>
- Chengbin Pang <mg1733051@smail.nju.edu.cn>
- Jian Guo <mf1733018@smail.nju.edu.cn>
- Jun Zhu <clearscreen@163.com>
- Bing Mao <maobing@nju.edu.cn>


## Publications
If you used our code, please cite our paper.

```
To Detect Stack Buffer Overflow with Polymorphic Canaries

@inproceedings{polymorphiccanaries,
  author = {Z. Wang and X. Ding and C. Pang and J. Guo and J. Zhu and B. Mao},
  booktitle = {2018 48th Annual IEEE/IFIP International Conference on Dependable Systems and Networks (DSN)},
  title = {To Detect Stack Buffer Overflow with Polymorphic Canaries},
  year = {2018},
  volume = {00},
  number = {},
  pages = {243-254},
  keywords={Security;Runtime;Instruments;Force;Tools;Instruction sets},
  doi = {10.1109/DSN.2018.00035},
  url = {doi.ieeecomputersociety.org/10.1109/DSN.2018.00035},
  ISSN = {2158-3927},
  month={Jun}
}
```

## Installation

### Compiler based PSSP
For program with source code.

#### Build Runtime Environment
~~~~{.sh}
# build runtime environment
$ cd /Runtime_Environment/Compiler_Based_Version/
$ make
$ cd /Runtime_Environment/Binary_Based_Version/
$ make
~~~~

#### Build Compiler Plugin
Make your choose according your needs.
~~~~{.sh}
# build LLVM pass 
$ cd Compiler_based_Implementation/P-SSP
$ mkdir build && cd build
$ cmake ..
$ make

# build gcc plugin(if your compiler is GCC
$ cd GCC_PLUGIN
$ make
~~~~

#### Compile your Program

##### GCC
~~~~{.sh}
# For small program, compile your application with the following (GNU GCC) flags: 
$ gcc -fstack-protector -fplugin=<PROJECT_SOURCE_DIR>/GCC_PLUGIN/PolymorphicCanaries.so test.c -o test

# For larger projects, adding `-fstack-protector',`-fno-omit-frame-pointer', and `-fplugin=<PROJECT_SOURCE_DIR>/GCC_PLUGIN/PolymorphicCanaries.so' to `CFLAGS'.
~~~~

##### LLVM
~~~~{.sh}
# For small program, compile your application with the following flags: 
$ clang -Xclang -load -Xclang <PROJECT_SOURCE_DIR>/Compiler_based_Implementation/P-SSP/libStackDoubleProtector.so test.c -o test


# For larger projects, adding `-Xclang -load -Xclang <PROJECT_SOURCE_DIR>/Compiler_based_Implementation/P-SSP/libStackDoubleProtector.so' to `CFLAGS'.
~~~~

#### Run your program with PSSP
~~~~{.sh}
# run 
$ export LD_PRELOAD=<PROJECT_SOURCE_DIR>/Runtime_Environment/Compiler_Based_Version/LIBPolymorphicCanaries.so
$ ./yourprogram
~~~~


### Binary rewriter
For program without source code. 


#### Customize glibc.    
1. Download a version of [glibc](https://www.gnu.org/software/libc/) which is compatible with your OS.
2. Customize the stack_chk_fail.c file in glibc according the template in [/Binary_based_implementation/dynamic linked proram/stack_chk_fail.c](https://github.com/zhilongwang/PolymorphicCanaries/blob/master/Binary%20based%20implementation/dynamic%20linked%20proram/stack_chk_fail.c) // Your may simply replace the file with our one.
3. Build and install the modified glibc.


#### Build Instrumentor
~~~~{.sh}
# Build instrumentor
$ cd Binary_based_implementation/dynamic linked proram/
$ make
~~~~   

#### Rewrite your programs
~~~~{.sh}
# Rewrite your programs
$ ./Binary_based_implementation/dynamic linked proram/InstrumentationCode yourprogram
~~~~    

#### Run your program with PSSP
~~~~{.sh}
$ export LIB_LIBRARY_PATH=<CUSTOMIZED_GLIBC_LIB_DIR>/*.so
$ export LD_PRELOAD=<PROJECT_SOURCE_DIR>/Runtime_Environment/Binary_Based_Version/LIBPolymorphicCanaries.so
$ ./yourprogram
~~~~
