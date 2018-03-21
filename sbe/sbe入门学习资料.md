# 序列化和反序列化-simple-binary-encoding的入门介绍

GitHub地址：https://github.com/real-logic/simple-binary-encoding

# 1. SBE源码编译过程说明及注意事项

Windows和Linux下都可以编译，编译需要依赖Jdk（我用的是Jdk1.8），先下载安装Jdk并配置好Java环境变量（配置环境变量去Google搜一下），另外还需要依赖ant，但是会自动下载。如果没自动下载，去Google搜索下载安装，配置好环境变量后再执行编译。

如果是Windows打开VS命令提示符，cd命令切换到sbe源码目录，然后输入 **gradlew** 开始编译，编译完成后显示如下：

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/build_sbe_tool.jpg)

编译成功后会生成\sbe-all\build\libs\sbe-all-1.7.10-SNAPSHOT.jar，该文件用来将xml的协议文件生成相应的C++代码或者C#代码。同时还生成了SBE的Demo-Car代码，在sbe-benchmarks\build\generated\uk\co\real\_logic\sbe\benchmarks目录下，如果需要看xml的协议文件，该文件在simple-binary-encoding-master\sbe-samples\src\main\resources\example-schema.xml

编译C++文件的命令为（在cmd命令行中执行）：

java -Dsbe.generate.ir=true -Dsbe.target.language= **Cpp** -Dsbe.target.namespace=sbe -Dsbe.output.dir=. -Dsbe.errorLog=yes -jar sbe-all/build/libs/sbe-all-1.7.10-SNAPSHOT.jar example-schema.xml

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/compile_command.jpg)

此处需要注意的是sbe-all-1.7.10-SNAPSHOT.jar文件的路径，注意如果生成的C#代码，-Dsbe.target.language标签值为：-Dsbe.target.language=&quot;uk.co.real\_logic.sbe.generation.csharp.CSharp&quot;   _&lt;引号不能省略&gt;_

sbe-all-1.7.10-SNAPSHOT.jar文件可以放在任何地方，只要指定了具体路径就可以。

生成的CPP文件如下：

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/gen_file_lists.jpg)

以上就是Windows下，编译sbe源码和将Demo-Car的xml文件生成CPP或者C#的过程。

Linux下的处理过程也是一样，作者Windows下和Linux全都试过，都没问题。Linux下唯一遇到的问题就是在编译SBE源码的过程中ANT包没有自动下载，其他一切正常，编译源码主要是配置好JDK和ANT的环境变量就可以，无论是Windows还是Linux，编译源码的最终目的是获得sbe-all-1.7.10-SNAPSHOT.jar文件，因为该文件是将xml的协议文件转成所需语言必须的。

对于源码目录下的，cppbuild和csharp目录，前者是用来生成Cpp版本的lib库文件，后者是用来生成C#的库文件，生成方法如果是Linux下编译比较简单了，直接用网站上提供的命令就可以。如下图所示。

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/cpp_build.jpg)

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/C%23_build.jpg)

如果是Windows下生成库文件，笔者仅试过Cpp版本，需要先安装cmake（gui），安装好之后，进行如下配置（此处需要注意，安装的VS版本与cppbuild-vs2015.cmd中的版本号一致，笔者用的是VS2017，故版本号是15）：

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/config_vs_version_for_gen_cpp_lib.jpg)

修改好配置之后，进行cmake配置，

#

![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/cmake_config.jpg)

注意&quot;source code&quot;和&quot;build the binaries&quot;路径选择，配置后先&quot;Configure&quot;然后再&quot;Generate&quot;，当出现Configure done和Generating done之后表明已经生成成功，此时选择&quot;Open Project&quot;打开VS工程，&quot;sbe工程&quot;就是sbe的库工程，直接生成lib库就可以，至此sbe的库生成成功了。

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/sbe_project.jpg)

# 2. xml原型文件学习介绍

一直提到xml文件，相信用过protobuf的都会明白这个文件的用处，就是sbe自己规定proto原型文件，通过该文件加上相应的编译命令可以生成相应的高级语言代码。对于该文件的学习还是要参考sbe提供的语法介绍，网址为

https://github.com/real-logic/simple-binary-encoding/wiki/FIX-SBE-XML-Primer

SBE总的学习Wiki很不错，有时间的同学一定要看看，因为上面包括了SBE的编码结构，

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/sbe_struct.jpg)

Wiki学习资料地址：https://github.com/real-logic/simple-binary-encoding/wiki

# 3.生成的文件如何在项目中使用

关于如何使用该文件，笔者是根据SBE提供的Demo来学习的，文件在simple-binary-encoding-master\sbe-samples\src\main\cpp\GeneratedStubExample.cpp

将该文件和Demo-Car的Cpp文件放在一起进行使用。

注意此处需要引用sbe的lib库和头文件，头文件在\simple-binary-encoding-master\sbe-tool\src\main\cpp\sbe\sbe.h

sbe的库文件在下图所示路径下（前提是步骤1中已经生成过）

 ![image](https://github.com/QuarkCloud/docs/blob/master/sbe/file/lib_path.jpg)