## link
- [official](https://tensorflow.google.cn/install/source)
- [csdn](https://blog.csdn.net/qq_37674858/article/details/81095101)
- [csdn](https://blog.csdn.net/luanpeng825485697/article/details/81152615)
- [csdn](https://blog.csdn.net/shuzfan/article/details/78516542)
- [jianshu](https://www.jianshu.com/p/d46596558640)
- [cnblogs](https://www.cnblogs.com/seniusen/p/9756302.html)
- [cnblogs](https://www.cnblogs.com/seniusen/p/9756481.html)
- [others](http://www.manongjc.com/article/114533.html)


```
# build pip package
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg

# build cc library
bazel build --config=opt --config=cuda //tensorflow:libtensorflow_cc.so

# build c library
bazel build --config=opt --config=cuda //tensorflow:libtensorflow.so

```

## problem

## config
- monolithic meaning?

### build pip package
- tf.Session() was failed



# some problems in building AI applications in Linux

[eclipse c++11]
- https://www.cnblogs.com/mcginn/p/5899275.html
- https://blog.csdn.net/xinqingwuji/article/details/81019843

[TensorBlock.h:393:35: error: parse error in template argument list]
- https://stackoverflow.com/questions/36274171/missing-template-arguments-before/36275150#36275150

[recompile with -fPIC]
- https://blog.csdn.net/shenchen8274/article/details/7822822
- (propertise->c/c++ build->settings->GCC **C++** Compiler->Command)
