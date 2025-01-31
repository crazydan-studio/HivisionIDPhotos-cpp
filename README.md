<div align="center">
<h1>HivisionlDPhoto-cpp</h1>
​

[English](README-EN.md) / 中文

[![release](https://img.shields.io/badge/release-black)](https://github.com/zjkhahah/HivisionIDPhotos-cpp/releases/tag/file)
[![issue](https://img.shields.io/badge/issue-black)](https://github.com/zjkhahah/HivisionIDPhotos-cpp/issues)
[![stars](https://img.shields.io/badge/stars-green)](https://github.com/zjkhahah/HivisionIDPhotos-cpp/stargazers)
[![forks](https://img.shields.io/badge/forks-blue)](https://github.com/zjkhahah/HivisionIDPhotos-cpp/forks)

 </div>

# 目录

- [项目简介](#项目简介)
- [准备工作](#准备工作)
- [权重文件下载](#权重文件下载)
- [编译源码](#源码编译)
- [如何使用](#使用)
- [引用项目](#引用项目)
- [联系我们](#联系我们)

# 项目简介

​ **HivisionIDPhoto 旨在开发一种实用、系统性的证件照智能制作算法。HivisionIDPhoto_cpp 是对 HivisionIDPhoto C++的重构，目的利用边缘设备的计算资源，以实现嵌入式设备和手机端本地部署。**

**HivisionIDPhoto_cpp 可以做到。**

​ **1.离线手机端部署。arm 设备部署运行。**

​ **2.轻量级抠图**

​ **3.根据不同尺寸规格生成不同的标准证件照**

​ **4.六寸排版照**

​ **5.美颜**

​ **6.安卓 apk（waitting）**

# 准备工作

windows 版和 Linux 版程序压缩放在[release](https://github.com/zjkhahah/HivisionIDPhotos_cpp/releases/tag/file)中，解压后需将执行文件和依赖环境放入 HivisionIDPhotos_cpp 根目录下，权重文件放入 model 文件夹下。

# 权重文件下载

存到项目的`model`目录下

- modnet_photographic_portrait_matting.mnn: https://github.com/zjkhahah/HivisionIDPhotos-cpp/releases/tag/v1.0/modnet_photographic_portrait_matting.mnn
- hivision_modnet.mnn: https://github.com/zjkhahah/HivisionIDPhotos-cpp/releases/tag/v1.0/mnn_hivision_modnet.mnn
- symbol_10_320_20L_5scales_v2_deploy.mnn: https://github.com/zjkhahah/HivisionIDPhotos-cpp/releases/tag/v1.0/symbol_10_320_20L_5scales_v2_deploy.mnn
- symbol_10_320_20L_8scales_v2_deploy.mnn: https://github.com/zjkhahah/HivisionIDPhotos-cpp/releases/tag/v1.0/symbol_10_320_20L_8scales_v2_deploy.mnn

# 源码编译

## **1. 克隆项目**

```
https://github.com/zjkhahah/HivisionIDPhotos_cpp.git
cd  HivisionIDPhotos_cpp
```

## 2.编译平台

​ **其中 MNN 版本为 2.9.0**

​ **opencv 版本为 4.7.0**

### **1.windows**

1.1 环境要求

- Microsoft Visual Studio >= 2017
- cmake >= 3.13
- powershell
- Ninja

编译 MNN 库并把编译后的.a 静态库文件放入 lib 文件夹下

编译 MNN windows 版教程为[主库编译 — MNN-Doc 2.1.1 documentation](https://mnn-docs.readthedocs.io/en/latest/compile/engine.html)

编译 opencv windows 版.a 静态库文件放入 lib 文件夹下

1.2 编译代码

在设置中找到 vcvars64.bat（适用于 VS 2017 的 x64 本机工具命令提示）并单击，打开 VS 编译 x64 架构程序的虚拟环境

```
cd  HivisionIDPhotos_cpp
mkdir build && cd build
cmake .. -G Ninja  #命令行编译cmake .. -G Ninja    api编译 cmake .. -G Ninja -DCOMPILE_LIBRARY=ON
ninja
```

### **2.arm64 编译**

​以 Linaro 工具链为例。首先从[Linaro](https://releases.linaro.org/components/toolchain/binaries/latest-7/)网页中按照宿主机以及交叉编译目标设备来选择合适的工具链。这里我们以`arm-linux-gnueabi`为例，点击网页上的链接，进入[arm-linux-gnueabi](https://releases.linaro.org/components/toolchain/binaries/latest-7/arm-linux-gnueabi/)页面。 按照宿主机类型(这里以 X64 Linux 为例)选择下载链接, 文件名形如 gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabi.tar.xz 下载后解压到任意目录

编译 arm64 版本的 opencv 和 mnn

编译 MNN ARM 版教程为[点击这里](https://mnn-docs.readthedocs.io/en/latest/compile/engine.html)

编译好的静态 opencv 文件和 mnn 文件放入 lib 文件夹下

```
cd  HivisionIDPhotos_cpp
mkdir build && cd build
cmake .. \
-DCMAKE_SYSTEM_NAME=宿主系统，例如Linux \
-DCMAKE_SYSTEM_VERSION=1 \
-DCMAKE_SYSTEM_PROCESSOR=交叉编译目标处理器的信息。例如aarch64 \
-DCMAKE_C_COMPILER=交叉编译器中C编译器的路径 \
-DCMAKE_CXX_COMPILER=交叉编译器中C++编译器的路径
make -j8
```

### **3.安卓编译**API

1. 编译 MNN 安卓版教程为[[主库编译 — MNN-Doc 2.1.1 documentation (mnn-docs.readthedocs.io)](https://mnn-docs.readthedocs.io/en/latest/compile/engine.html)]
2. 下载 opencv 安卓版本[链接](https://github.com/opencv/opencv/releases/download/4.7.0/opencv-4.7.0-android-sdk.zip)

编译好的静态 opencv 文件和 mnn 文件放入 lib 文件夹下

```
cd  HivisionIDPhotos_cpp
mkdir build && cd build

cmake ..     -DCMAKE_TOOLCHAIN_FILE=#CMake工具链文件的路径     -DCMAKE_ANDROID_NDK=#Android NDK（Native Development Kit）的安装路径    -DANDROID_PLATFORM=android-21     -DANDROID_NATIVE_API_LEVEL=21     -DANDROID_ABI=arm64-v8a     -DANDROID_NATIVE_API_LEVEL=21 -DCOMPILE_LIBRARY=ON -DCOMPILE_ANDROID=ON

make -j8
```

# 命令行使用

核心指令：

- `-e`: 人像在图片比例
- `-i`：输入照片路径
- `-o`：输出照片路径
- `-k`：输出照片大小
- `-f`：人脸模型选择
- ...

更多指令通过`./HivisionIDPhotos_cpp.exe --help`查看

## 1.打开程序

以 windows 系统为例，使用工具 powershell 跳转到根目录下,例：

` cd D:\HivisionIDPhotos-cpp`

## 2.证件照制作

输入一张照片，输出一张标准证件照 png

`./HivisionIDPhotos_cpp.exe -i demo/images/test.jpg  -o 1  -r 255  -g 0  -b 0 -h 413 -w 295 `

## 3.人像抠图

输入 1 张照片，获得 1 张 4 通道透明 png

## 4.生成一张六寸排版照

输入一张照片，得到一张六寸排版照 png

`./HivisionIDPhotos_cpp.exe -i demo/images/test.jpg  -o 1  -r 255  -g 0  -b 0 -h 413 -w 295 -l 1`

<br>

# API 使用

利用 SpringBoot+JNA 调用 dll 文件生成 API（linux 环境调用 `.so` 文件）
下面以 `HivisionIDphotos` 中的 `human_mating` 方法为例

## 1.创建 SpringBoot 项目并在 pom.xml 中添加依赖

```
        <dependency>
            <groupId>net.java.dev.jna</groupId>
            <artifactId>jna</artifactId>
            <version>5.12.1</version>
            <scope>compile</scope>
        </dependency>
```

## 2.放入 dll/so 文件

将需要调用的 dll 文件放在 `src\main\resources\win32-x86-64\` 中

## 3.构建需要传参的类

这里需要知道方法的具体传参，根据 JNA 映射规则进行编写，下面三个类根据 `HivisionIDphotos.dll` 内函数的传参结构体编写。

- `Hivision_config.java`

```
public class Hivision_config extends Structure implements Structure.ByValue {
    /** 人像抠图模型文件 */
    public String model_path;
    /** 人脸检测模型目录，由 {@link #model_scale} 确定模型文件名称 */
    public String face_model_path;
    /** 源图片的文件路径 */
    public String image_path;
    /** 输出图片文件的存放目录 */
    public String out_image_path;
    /** 输出图片的文件类型 */
    public String out_image_type = "jpg";
    /** 输出图片背景色 */
    public Hivision_color background_color = new Hivision_color(255, 0, 0);
    /** 并发线程数：默认为 CPU 核心数 */
    public int thread_num = Runtime.getRuntime().availableProcessors();
    /**
     * 人脸检测模型类型
     * <p/>
     * 可选值：5, 8，其分别对应模型文件
     * <code>symbol_10_320_20L_5scales_v2_deploy.mnn</code>
     * 和 <code>symbol_10_560_25L_8scales_v1_deploy.mnn</code>
     */
    public int model_scale = 8;
    /** 人像在图片中的比例 */
    public float head_measure_ratio = 0.35f;
    /** 输出图片宽度 */
    public int out_image_width = 295;
    /** 输出图片高度 */
    public int out_image_height = 413;

    @Override
    protected List<String> getFieldOrder() {
        return Arrays.asList(
            "model_path", "face_model_path", "image_path", "out_image_path",
            "out_image_type", "background_color", "thread_num", "model_scale",
            "head_measure_ratio", "out_image_width", "out_image_height"
        );
    }
}
```

> 注意，必须实现接口 `Structure.ByValue`，其表示结构体以**值**
> 方式传入接口（另一种为 `Structure.ByReference` 引用），
> 该接口的实现中的 `String` 将会自动通过 `com.sun.jna.Pointer` 做 `char*` 转换，
> 否则，会出现指针引用等问题

- `Hivision_color.java`

```
public class Hivision_color extends Structure implements Structure.ByValue {
    public int r;
    public int g;
    public int b;

    /** Note: 必须定义无参构造函数 */
    public Hivision_color() {
        super();
    }

    public Hivision_color(int r, int g, int b) {
        this();

        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```

## 4.编写对应的 Library 接口

一个 `Library` 接口对应一个 dll，内抽象方法与 dll 中的函数一一对应。`Native.loadLibrary()` 第一个参数为 dll 文件路径，不加 `.dll` 后缀。

- `HivisionIDphotosLibrary.java`

```
public interface HivisionIDphotosLibrary extends Library {
    /** 动态库的名称：文件名去掉开头的 <code>lib</code> 和结尾的 <code>.so</code> 便是其库名称 */
    String NATIVE_LIBRARY_NAME = "HivisionIDphotos";

    void human_mating(Hivision_config config);

    int ID_photo(Hivision_config config,int out_size_kb,boolean layout_phot);
}
```

## 5.编写 Controller 类

- `JnaDemoController.java`

```
@RestController
@RequestMapping("/JNA")
public class JnaDemoController {
    private static final HivisionIDphotosLibrary INSTANCE;

    static {
        INSTANCE = Native.load(HivisionIDphotosLibrary.NATIVE_LIBRARY_NAME, HivisionIDphotosLibrary.class);
    }

    @PostMapping("/human_mating")
    public String human_mating(@RequestParam("config_json") String config_json){
        try{
            Hivision_config config = JSONObject.parseObject(config_json, config.class);

            INSTANCE.human_mating(config);

            return "success";
        }catch (Exception e){
            System.out.println(e.getMessage());
            return "fail";
        }
    }
}
```

若是动态库未安装到标准的系统库中，则需要在启动 Java 应用之前设置环境变量
`LD_LIBRARY_PATH=/path/to/dll/or/so`。具体可参考
[crazydan-studio/lib-graphic-java-opencv](https://github.com/crazydan-studio/lib-graphic-java-opencv)
中的说明。

# 引用项目

1. [MNN](https://github.com/alibaba/MNN):

```
@software{Alibaba_MNN_2024,
    author = {Alibaba},
    title = {{MNN}},
    url = {https://github.com/alibaba/MNN},
    year = {2024},
    publisher = {GitHub}
}
```

2. [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos)

```
@software{Zeyi-Lin_HivisionIDPhotos_2024,
    author = {Zeyi-Lin},
    title = {{HivisionIDPhotos}},
    url = {https://github.com/Zeyi-Lin/HivisionIDPhotos},
    year = {2024},
    publisher = {GitHub}
}
```

3. **[LFFD-MNN](https://github.com/SyGoing/LFFD-MNN)**

```

@software{LFFD-MNN_2024,
    author = {SyGoing},
    title = {{LFFD-MNN}},
    url = {https://github.com/SyGoing/LFFD-MNN},
    year = {2024},
    publisher = {GitHub}
}

```

<br>

# 联系我们

如果您有任何问题，请提[issue](https://github.com/zjkhahah/HivisionIDPhotos-cpp/issues)或发邮件至junkangzhou@stu.xidian.edu.cn
