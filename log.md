# 09-10

## Windows平台使用c++ load custom op dll

- python 接口 `tf.load_op_library()`在加载库文件的同时，实现了对op及opkernel的动态注册，此过程无法通过简单的 Windows API `LoadLibray()`实现，需要通过调用TF接口 `TF_LoadLibrary` 实现custom op lib的加载

- `TF_LoadLibrary` 位于 `c/c_api.h` 中，属于TF [C API](https://tensorflow.google.cn/install/lang_c)，并未public c++ API，可以包含该头文件，或者手动暴露其内部的C++实现，完成调用；lib的释放存在同样的问题

- 通过 `TF_LoadLibrary` 加载简单的 `zero_out.dll` 失败，返回错误码5，提示找不到dll，从dll加载原理上分析，应该是 `zero_out.dll` 所依赖的某些dll未找到, 最终通过Process Manager发现需要依赖`python36.dll`，可能是Win平台下TF lib的编译不规范，导致链接了并不需要的python lib，但随后的操作发现尽管能够找`zero_out.dll`，但`TF_GetOpList`返回的buff为空，没有op被注册，原因未知，是否类似Win TF lib编译后缺少导出符号是相同的原因。
- 在Linux尝试上述过程，相对顺利，注意编译`libzero_out.so`与编译`tf_cc.so`的gcc版本需要统一（ABI一致），否则仍出现找不到lib的情况；随后解析`TF_GetOpList`返回值，能够看到`ZeroOut`字符串，但问题是如何调用该op，目前仍无解决方案，可参考[tf src](https://github.com/tensorflow/tensorflow/blob/04256c89d8783c5cfd7e550f9512e9478beb6454/tensorflow/c/c_api_test.cc#L2345), 有个绕开直接调用该op的办法，就是将改op作为模型的一部分，类似目前AI应用的cc inference过程，因为sense过程并没有可训练参数，需要额外定义一个变量以便保存模型，经尝试该方法可行。


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