# tensorflow-cpu
测试了tensorflow-cpu whl文件的编译

## 系统环境：
* Windows 10
* Anaconda: 4.7.10
* Python: 3.6.7
* Tensorflow: 1.9.1
* bazel: 0.16.0
* msvc: 2015 update 3

## 编译流程：
cpu版本的编译较为简单，请直接参阅[tensorflow官方文档](https://tensorflow.google.cn/install/source_windows "tensorflow官方文档")

## 编译总结：
cpu版本的编译为下一步编译gpu版本铺垫了基础，在编译过程中需要注意如下问题
* 执行编译的主机用户名中不能包含空格（bazel的编译实际上在`C:/users/用户名`中进行，bazel无法识别含空格的编译路径）
* MSVC2015建议安装完整的Visual Studio， 而不是只安装build tool
* bazel版本一定要注意匹配（参考官方文档），过高或过低都无法完成编译


# tensorflow-gpu
## 系统环境
- Windows 10
- Anaconda: 4.7.10
- Python: 3.6.7
- CUDA: 10.0
- CUDNN: 7.6.1
- Tensorflow: 1.13.1


## 预安装编译环境
1. 在anaconda powershell prompt (以下简称cmd)中创建新的虚拟环境tf_test

```
conda create –n tf_test python=3.6.7
conda activate tf_test
```

2. cmd下安装以下Tensorflow pip软件包依赖项

```
pip install six numpy wheel
pip install keras_applications==1.0.6 --no-deps
pip install keras_preprocessing==1.0.5 --no-deps
```

3. 安装[Bazel](https://github.com/bazelbuild/bazel/releases)作为Tensorflow的编译工具，对于将要安装的Tensorflow_gpu-1.13.1，此流程经测试确认可用的Bazel版本为0.21.0，将安装好的bazel.exe添加到系统环境变量%PATH%中

4. 安装[MSYS2](https://www.msys2.org)作为Tensorflow所需的bin工具，默认的安装路径为C:/msys64，将C:\msys64\usr\bin添加到%PATH%环境变量中。然后，在cmd下运行以下命令。(*下载很慢，容易断掉，如何处理？用手机热点，多试几次*)

```
pacman –S git patch unzip
```

5. 安装[Visual studio C++生成工具](https://visualstudio.microsoft.com/zh-hans/vs/older-downloads)，虽然生成工具可以独立安装，但这里建议安装完整的Visual studio 2015（对于将要安装的Tensorflow_gpu-1.13.1，此流程经测试确认可用的VS版本为VS 2015），安装时注意勾选通用 **Windows应用开发工具**->**Tools和Windows10 SDK** 以及 **通用工具**->**Visual Studio扩展性工具Update3**

6. 4.7及以上版本的Anaconda安装时可以提供`powershell`提示符，可以直接运行`ps1`文件，无需额外设置。


## 编译过程
1. 在powershell中进入源码路径，路径下应有以下文件或文件夹：
文件夹：patches（用于C++ API编译的补丁包），source（tensorflow源代码）
文件：build.ps1（编译脚本），extract-built-tensorflow-cpp-api.ps1（lib和include路径生成脚本）
```
(tf_test) (base) PS C:\Users\Administrator>cd C:\build_tf
```

2. 在当前路径下运行脚本build.ps1

	需激活的参数
- `BuildCppAPI`：编译C++ API时需激活此参数，用于应用C++ API补丁以及调用外部符号
- `ReserveSource`：保留之前编译的结果
- `IgnoreDepsVersionIssues`：编译过程中忽略编译工具版本不对应
- `InstallDefaultDeps`：安装默认版本的编译工具

	需输入的参数
- `BazelBuildParameter`：bazel编译参数，添加在 " " 内，细节参阅 [tensorflow官方文档](https://tensorflow.google.cn/install/source_windows#gpu_support)

	```
	(tf_test) (base) PS C:\build_tf>.\build.ps1 –BuildCppAPI –BazelBuildParameter “--config=opt --config=cuda --define=no_tensorflow_py_deps=true //tensorflow:libtensorflow_cc.so”
	```

	- 以上指令将编译C++ API的libtensorflow_cc.so
	- 编译初始阶段会从github上下载多个第三方文件，需要能够翻墙的网络（挂载vpn后，移动网速较联通更快）
	- 下载失败的package可以反复尝试，已下载的package无需重复下载
	- 该脚本主要修改自 [`build.ps1`](https://github.com/guikarist/tensorflow-windows-build-script/blob/master/build.ps1)，可定期同步更新

3. 经过较长时间的编译后，powershell将输出

	```
	INFO: Build completed successfully
	```
	至此Tensorflow库文件编译完成，输出路径为`C:\build_tf\source\bazel-out\x64_windows-opt\bin\tensorflow\libtensorflow_cc.so`

4. 若需要生成tensorflow_cc.dll, tensorflow_cc.lib及include文件，在当前目录下运行extract-built-tensorflow-cpp-api.ps1，输出的库文件和头文件在新生成的bin\tensorflow文件夹中
	```
	(tf_test) (base) PS C:\build_tf> .\extract-built-tensorflow-cpp-api.ps1
	```

## 编译总结：
此编译脚本引用自[guikarist](https://github.com/guikarist "guikarist")，更完整的编译流程请参阅[ta的原创脚本](https://github.com/guikarist/tensorflow-windows-build-script)</br>
在编译cpu的基础上，编译gpu版本时仍需注意如下问题：
- 测试编译whl文件时未能成功编译，理论上只需将编译选项中`-BazelBuildParameters`的最后一项修改为`//tensorflow/tools/pip_package:build_pip_package`，但编译未通过，提示问题为DLL文件加载失败，可能的原因为python环境的某些依赖未成功加载。
- 在编译过程中需要注意网络连接通畅，需要科学上网
- 利用当前脚本编译生成的include文件并不完全，只能满足最简单的测试功能，如需要更完善的include目录，可拷贝pip在Linux下安装后的`include`目录
- 在visual studio中测试编译出的dll时，需注意修改“属性 - C/C++ - 预处理器 - 预处理器定义”处，添加`NOMINMAX`选项
- 在编译中途失败，或需要重新编译时，注意添加`-ReserveSource`编译选项以保存之前编译的部分结果


# Q&A
- Q: 脚本调用git命令时出现`BUG: run-command.c:500: disabling cancellation: Invalid argument`？</br>
A： 运行MSYS2.exe，无需在MSYS命令行输入任何指令，保持该窗口在后台运行，即可正常使用git命令
- Q：在编译过程中，出现类似`error loading package '': Encountered error while reading extension file 'repositories/repositories.bzl': no such package '@io_bazel_rules_docker//repositories':`的error？</br>
A： 在load package阶段出现类似错误通常原因为网络连接不稳定，建议更换网络或连接VPN后重新尝试编译
- Q：编译出的tensorflow c++ API不能调用第三方库（如libprotobuf)？</br>
A： 当前版本tensorflow环境（1.13.1）下，第三方库文件只能与其源代码一同编译使用，直接链接.lib或.a文件无法使用，[相关链接](https://github.com/guikarist/tensorflow-windows-build-script/issues/21)
