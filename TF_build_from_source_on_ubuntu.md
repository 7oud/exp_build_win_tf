## build tensorflow commands
```
# build pip package
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

# build cc library
bazel build --config=opt --config=cuda //tensorflow:libtensorflow_cc.so

# build c library
bazel build --config=opt --config=cuda //tensorflow:libtensorflow.so

```

## links of build tensorflow c++ library
- [official](https://tensorflow.google.cn/install/source)
- [csdn](https://blog.csdn.net/qq_37674858/article/details/81095101)
- [csdn](https://blog.csdn.net/luanpeng825485697/article/details/81152615)
- [csdn](https://blog.csdn.net/shuzfan/article/details/78516542)
- [jianshu](https://www.jianshu.com/p/d46596558640)
- [cnblogs](https://www.cnblogs.com/seniusen/p/9756302.html)
- [cnblogs](https://www.cnblogs.com/seniusen/p/9756481.html)
- [others](http://www.manongjc.com/article/114533.html)


## building parameters

### --config=monolithic
- build only **ONE** library `libtensorflow_cc.so`
- [stackoverflow](https://stackoverflow.com/questions/53705582/what-is-meant-by-static-monolithic-build-when-building-tensorflow-from-source)
- DO NOT use `--config=monolithic` unless you don't need custom op


### build pip package succeed but `tf.Session()` failed
- DO Not install built pip package in source code folder, install it in `~` is OK [Not Sure reason]


## build AI library on Linux

### eclipse setting
- **c++11** [cnblog](https://www.cnblogs.com/mcginn/p/5899275.html)
- **c++11** [csdn](https://blog.csdn.net/xinqingwuji/article/details/81019843)
- **-fPIC** [csdn](https://blog.csdn.net/shenchen8274/article/details/7822822)
- **-fPIC** `propertise->c/c++ build->settings->GCC **C++** Compiler->Command`

### cmake setting
- **release or debug** [stackoverflow](https://stackoverflow.com/a/7725055/9873377)
- **link dynamic and static lib** [csdn](https://blog.csdn.net/KYJL888/article/details/85109782)
- **link lib** [csdn](https://blog.csdn.net/KYJL888/article/details/85109782)
- **cmake cuda**[nvidia](https://devblogs.nvidia.com/building-cuda-applications-cmake/)

### TensorBlock.h:393:35: error: parse error in template argument list
- [stackoverflow](https://stackoverflow.com/questions/36274171/missing-template-arguments-before/36275150#36275150)
- DO NOT use "using namespace std;"

### No session factory registered
- [github/tensorflow](https://github.com/tensorflow/tensorflow/issues/3308#issuecomment-233799915)
- [csdn](https://blog.csdn.net/wd1603926823/article/details/92843830)


## build custom op on Linux

### -D_GLIBCXX_USE_CXX11_ABI=0
- The binary pip packages available on the TensorFlow website are built with gcc4 that uses the older ABI. If you compile your op library with gcc>=5, add `-D_GLIBCXX_USE_CXX11_ABI=0` to the command line to make the library compatible with the older abi. [tensorflow](https://tensorflow.google.cn/guide/extend/op)
	- compile simple custom op library `libzero_out.so` using gcc with pip package from tf website, `-D_GLIBCXX_USE_CXX11_ABI=0` need to be set.
	- compile simple custom op library `libzero_out.so` using gcc with self-built pip package, DO NOT set D_GLIBCXX_USE_CXX11_ABI=0, and in this situation, c++ code can call the custom op library with `TF_LoadLibrary`



