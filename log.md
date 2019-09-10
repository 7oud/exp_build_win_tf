# 09-10

## Windows平台使用c++ load custom op dll

- python 接口 `tf.load_op_library()`在加载库文件的同时，实现了对op及opkernel的动态注册，此过程无法通过简单的 Windows API `LoadLibray()`实现，需要通过调用TF接口 `TF_LoadLibrary` 实现custom op lib的加载

- `TF_LoadLibrary` 位于 `c/c_api.h` 中，属于TF [C API](https://tensorflow.google.cn/install/lang_c)，并未public c++ API，可以包含该头文件，或者手动暴露其内部的C++实现，完成调用；lib的释放存在同样的问题

- 通过 `TF_LoadLibrary` 加载简单的 `zero_out.dll` 失败，返回错误码5，提示找不到dll，从dll加载原理上分析，应该是 `zero_out.dll` 所依赖的某些dll未找到
	- 将 tensorflow_cc.dll 以及一些相关的dll放到 zero_out.dll 同目录下，仍然无效
	- 是否意味这需要通过bazel编译调用custom op lib的程序
	- 尝试在Linux下通过 `TF_LoadLibrary` 加载 `zero_out.so`，由于没有Linux下编译的 tensorflow.so，下载了官方的c版本的 tf1.14 lib， `zero_out.so` 使用 pip 安装的 1.12 版TF编译得到，结果仍然是提示找不到 `zero_out.so`，不清楚是否与TF版本差异有关



## 相关链接

- [Tensorflow是如何注册和调用C++ New Op的](https://zhuanlan.zhihu.com/p/34168765)，解释了op的注册、加载过程 
- [How to load custom op library when using c++?](https://stackoverflow.com/questions/57857325/how-to-load-custom-op-library-when-using-c) StackOverflow的回答，说明需要使用 `TF_LoadLibrary` 加载custom op lib
- [StackOverflow上相关TF_LoadLibrary的回答](https://stackoverflow.com/questions/50475320/executing-frozen-tensorflow-graph-that-uses-tensorflow-contrib-resampler-using-c/50538292#50538292)
- [官方的custom op 示例 repo](https://github.com/tensorflow/custom-op)
- [GitHub上相关TF_LoadLibrary的问题](https://github.com/tensorflow/tensorflow/issues/21251)，能够加载custom op，但register错误
- [load custom op library when using c++?](https://github.com/tensorflow/tensorflow/issues/32361)，等待回复


## 需要留意的回复

- [How to load custom op library when using c++?](https://stackoverflow.com/questions/57857325/how-to-load-custom-op-library-when-using-c)
- [GitHub上相关TF_LoadLibrary的问题](https://github.com/tensorflow/tensorflow/issues/21251)
- [load custom op library when using c++?](https://github.com/tensorflow/tensorflow/issues/32361)




- [关键字搜索 TF_LoadLibrary](https://cn.bing.com/search?q=TF_LoadLibrary&pc=MOZI&form=MOZTSB)
- [搜索tf官方github issue中相关TF_LoadLibrary的内容]