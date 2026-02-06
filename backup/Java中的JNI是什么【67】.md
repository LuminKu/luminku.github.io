## [什么是JNI（Java Native Interface）](https://blog.csdn.net/scjrw/article/details/132052809)


**JNI（Java Native Interface）** 是 Java 提供的一种接口，允许 Java 代码与本地代码（通常是 C 或 C++）进行交互。它的主要作用是弥补 Java 在性能、硬件操作以及复用已有库方面的不足，从而实现更高效的功能扩展。

通过 JNI，Java 程序可以调用底层的系统功能或库，例如图形处理、音频处理、硬件控制等。这种机制特别适用于需要高性能计算或直接操作硬件的场景。



1.**基本原理**



JNI 的核心是充当 Java 虚拟机（JVM）与本地代码之间的桥梁。Java 代码通过声明 *native* 方法调用本地代码，而本地代码则通过 JNI 提供的接口与 JVM 交互。以下是 JNI 的基本工作流程：

- 在 Java 类中声明 *native* 方法，例如：

```java
public native int add(int a, int b);
```




- 使用 *System.loadLibrary()* 加载包含本地方法实现的共享库（如 *.so* 或 *.dll* 文件）。

- 在本地代码中实现对应的 C/C++ 函数，函数签名需遵循 JNI 规范，例如：

```c
JNIEXPORT jint JNICALL Java_PackageName_ClassName_add(JNIEnv *env, jobject obj, jint a, jint b) {

return a + b;

}
```


- 编译本地代码生成共享库，并在 Java 程序中调用 *native* 方法。

使用场景

- **性能优化**：在计算密集型任务（如图像处理、音视频编解码）中，C/C++ 的性能通常优于 Java。
- **硬件访问**：直接操作底层硬件（如传感器、GPU）需要使用本地代码。
- **复用现有库**：通过 JNI，可以直接调用已有的 C/C++ 库（如 OpenCV、FFmpeg）。
- **平台特定功能**：访问 Java 层未暴露的底层系统功能。

示例代码

以下是一个简单的 JNI 示例，展示如何通过 JNI 调用 C 函数计算两个整数的和：

Java 代码

```java
public class Add {

static {

System.loadLibrary("add"); // 加载共享库

}

public native int add(int a, int b); // 声明 native 方法

}
```




C 代码



```c
#include <jni.h>

#include "Add.h"

JNIEXPORT jint JNICALL Java_Add_add(JNIEnv *env, jobject obj, jint a, jint b) {

return a + b; // 实现加法逻辑

}
```




编译和运行

1. 使用 *javac* 编译 Java 文件。
2. 使用 *javah* 生成头文件。
3. 编写并编译 C 文件生成共享库（如 *libadd.so*）。
4. 运行 Java 程序调用 *add* 方法。

注意事项

- **内存管理**：本地代码需手动管理内存，避免内存泄漏。
- **线程安全**：JNIEnv 是线程绑定的，需确保在正确的线程中使用。
- **异常处理**：本地代码需检查并处理 Java 方法可能抛出的异常。

通过 JNI，开发者可以在享受 Java 开发便利的同时，调用本地代码的强大功能，从而实现更高效、更灵活的应用程序。