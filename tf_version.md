## tf version
- [official](https://tensorflow.google.cn/install/source)
- [3rd build](https://github.com/fo40225/tensorflow-windows-wheel)
- [3rd build](https://github.com/guikarist/tensorflow-windows-build-script)

### 1.10.0
- official compile envionment
  - `Cmake v3.6.3` | Bazel 0.15.0 (last official release using cmake on Windows)
  - MSVC 2015 update 3 | GCC 4.8
  - CUDA 9, cuDNN 7
- tf 1.10.1, No new binaries will be built for Windows(???)

### 1.12.0
- official compile envionment
  - Bazel 0.15.0
  - MSVC 2015 update 3 | GCC 4.8
  - CUDA 9, cuDNN 7

### 1.13.1
- official compile envionment
  - Bazel 0.19.0-0.21.0 | Bazel 0.19.2
  - MSVC 2015 update 3 | GCC 4.8
  - `CUDA 10`, cuDNN 7.4 (1st release using CUDA 10)
  - `TensorRT 5.0` (optional)
  
### 1.14.0
- 1st release contraining `compat.v2` module
- turn on `MKL-DNN` by default
- **Non-Windows system libraries** are now versioned. This should be a no-op for most users as it affects only package maintainers or those building extensions to TF
  - Pip packages contain one library file(Linux: libtensorflow_framework.so.1)
  - 译, Windows pip 包中不含有 tf 相关 dll(Linux pip 包中含有 libtensorflow_framework.so.1), 这意味着在 Windows 下无法通过链接 lib 方式添加 custom op
- official compile envionment
  - Bazel 0.24.1-0.25.2 | Bazel 0.24.1
  - `MSVC 2017` | GCC 4.8
  - CUDA 10, cuDNN 7.4
- [build cxx lib for Windows](https://github.com/fo40225/tensorflow-windows-wheel/issues/117#issuecomment-559360181)

### 1.15.0
- `last 1.x` release for TF
- add `fftshift` op
- auto `Mixed-Precision graph optimizer` simplifies converting models to float16. 
  - enabled by wrapping an optimizer class with `tf.train.experimental.enable_mixed_precision_graph_rewrite()`
- should be last release has `contrib` directory in release code
- [some TensorRT support](https://github.com/tensorflow/tensorflow/releases/tag/v1.15.0)
- NO official compile envionment on official web

### 2.0.0
- eager, keras, ..., and all features in tf 1.x
- removed `freeze_graph` command line tool; SavedModel should be used in place of frozen graphs
- still has `contrib` directory in release code, but not in branch `r2.1` or `master`
  - maybe tf2 can be built with cmake
  - tf.contrib moved to `tensorflow/addons` or `tensorflow/io`, or removed
- official compile envionment
  - Bazel 0.26.1
  - MSVC 2017 | `GCC 7.3.1`
  - CUDA 10, cuDNN 7.4

### 2.1.0
- pip package is built with `CUDA 10.1 and cuDNN 7.6`
- Windows pip package is built with `VS2019`
- experimental support for [mixed precision](https://tensorflow.google.cn//guide/keras/mixed_precision)
- many keras implemented [models](https://github.com/tensorflow/models/tree/master/official)
- `tf.debugging.enable_check_numerics()` to help debugging the root causes of issues involving Inf and NaN
- `TensorRT 6.0` is supported and enabled by default
- from v2.1.0, `contrib` folder was removed from src

### 2.2.0
- replace `std::string` with `tensorflow::tstring`, now ABI is stable
- add a new `Profiler` of TF2, for CPU/GPU/TPU
- export C++ using `pybind11`, SWIG is deprecated
- add `SyncBatchNormalization` layer for multi-GPUs
- **custom training logic** by overriding `Model.train_step`
- XLA now builds and works on Windows


### To be built on Windows
- tf 1.10.0, CUDA 9
  - 官方支持的最后一个cmake版本, 可代替目前针对 MR 发布的 tf lib
- tf 1.12.0, CUDA 9
  - 尝试cmake编译, 如成功，可成为支持 CUDA 9 的最后一个版本
- tf 1.13.1, CUDA 10
  - 尝试 cmake 编译, 如成功, 可成为支持 CUDA 10 的第一个版本
  - 尝试 bazel 编译, 已有成功案例, 但有较多的不确定因素
- tf 1.14.0, CUDA 10
  - 尝试 cmake 编译, 如成功, 可成为第一个支持 vs2017 版本
  - 尝试 bazel 编译, 尚无可参考的案例
- tf 1.15.0, CUDA 10
  - 尝试 cmake 编译, 如成功, 可成为最后一个 tf1.x 版本
  - 尝试 bazel 编译, 尚无可参考的案例
- tf 2.0.0, CUDA 10
  - 尝试 cmake 编译, 如成功, 可成为第一个 tf2 版本
  - 尝试 bazel 编译, 尚无可参考的案例
- tf 2.1.0, CUDA 10.1
  - 尝试 bazel 编译, 尚无可参考的案例


- 在无需 custom op 的情况下, 使用 cmake 构建或成为首选


### To be done
- tf2 overview, 看一遍官网guide
- SavedModel
- 看fftshift具体实现
- use clion bazel to view tf code
