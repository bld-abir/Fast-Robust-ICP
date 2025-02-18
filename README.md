# Fast-Robust-ICP (Welsch)

This repository includes the source code the paper [Fast and Robust Iterative Closet Point](https://arxiv.org/abs/2007.07627).

Authors: [Juyong Zhang](http://staff.ustc.edu.cn/~juyong/), Yuxin Yao, [Bailin Deng](http://www.bdeng.me/).

This code is protected under patent. It can be only used for research purposes. If you are interested in business purposes/for-profit use, please contact Juyong Zhang (the author, email: juyong@ustc.edu.cn).

This code was written by Yuxin Yao. If you have questions, please contact yaoyuxin@mail.ustc.edu.cn. 


## Compilation

The code is compiled using [CMake](https://cmake.org/) and requires [Eigen](eigen.tuxfamily.org). It has been tested on Ubuntu 16.04 with gcc 5.4.0 and on Windows with Visual Studio 2015. 

Follow the following steps to compile the code:

1) Make sure Eigen is installed. We recommend version 3.3+.
	- Download Eigen from eigen.tuxfamily.org and extract it 
	into a folder 'eigen' within the 'include' folder. Make sure the files 'include/eigen/Eigen/Dense' and 'include/eigen/unsupported/Eigen/MatrixFunctions' can be found 
	- Alternatively: On Ubuntu, use the command "apt-get install libeigen3-dev" to install Eigen. 

2) Create a build folder 'build' within the root directory of the code

3) Run cmake to generate the build files inside the build folder, and compile the source code:
    - On linux, run the following commands within the build folder:
    ```
   $ cmake -DCMAKE_BUILD_TYPE=Release ..
   $ make
    ```
    - On windows, use the cmake GUI to generate a visual studio solution file, and build the solution.

4) Afterwards, there should be an exectuable file 'FRICP' generated.

## Usage

The program is run with four input parameters:

1. an input file storing the source point cloud;
2. an input file storing the target point cloud;
3. an output path storing the registered source point cloud and transformation;
4. registration method:
```
0: ICP
1: AA-ICP
2: Ours (Fast ICP)
3: Ours (Robust ICP)
4: ICP Point-to-plane
5: Our (Robust ICP point-to-plane)
6: Sparse ICP
7: Sparse ICP point-to-plane
```
You can ignore the last parameter, in which case `Ours (Robust ICP)` will be used by default. 

Example:
```
$ ./FRICP ./data/target.ply ./data/source.ply ./data/res/ 3
```
But obj and ply (Non-binary encoding) files are supported.

### Initialization support 
If you have an initial transformation that can be applied on the input source model to roughly align with the input target model, you can set `use_init=true` and set `file_init` to the initial file name in `main.cpp` . The format of the initial transformation is a 4x4 matrix(`[R, t; 0, 1]`), where `R` is a 3x3 rotation matrix and `t` is a 3x1 translation vector. These numbers are stored in 4 rows, and separated by spaces in each row. This format is the same as the output transformation of this code. It is worth mentioning that this code will align the center of gravity of the initial source and target models by default before starting the registration process, but this operation will be no longer used when the initial transformation is provided. In our experiment, we directly use the output file of transformation matrix generated by [Super4PCS](https://github.com/nmellado/Super4PCS) as the initial file.

## Citation

Please cite the following papers if it helps your research:

```
@article{zhang2021fast,
  author={Juyong Zhang and Yuxin Yao and Bailin Deng},
  title={Fast and Robust Iterative Closest Point}, 
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence}, 
  year={2021},
  volume={},
  number={},
  pages={1-1}}
```



## Acknowledgements
The code is adapted from the [Sparse ICP implementation](https://github.com/OpenGP/sparseicp) released by the authors.
