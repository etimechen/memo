# Java调用C++库文件例子

>本例使用JNI(Java Native Interface)技术调用C++库文件
>* 系统: Mac OS
>* IDE: Eclipse

* 创建Java工程命名为jniTest,新建一个类,名为MyDLL.java,代码如下:

```
public class MyDLL {
	static{
		System.loadLibrary("JavaCallCpp");
	}
	//定义一个native方法
	public native int addition(int a, int b);
}
```

eclipse保存后将在项目创建路径的bin目录下生成对应的.class文件

* 进入.class文件所在目录,在命令行里输入 `javah -cp /Users/chenliang/Works/Develop/MyProject/WorkSpace/jniTest/bin/ com.chen.jni.MyDLL`,将在当前目录生成.class文件对应的.h文件(C++头文件). _*注意:-cp后的路径和类名(类名前必须包含包名)*_
* 在相同目录下创建.cpp文件,命名为MyDLL.cpp,代码如下:

```
#include "com_chen_jni_MyDLL.h"

JNIEXPORT jint JNICALL Java_com_chen_jni_MyDLL_addition(JNIEnv *env, jobject object, jint a, jint b){
	return a + b;
}
```

* 命令行在当前目录输入 `g++ -I /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home/include -I /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home/include/darwin -shared -o JavaCallCpp.dylib JavaCallCpp.cpp` ,两个 -I 是指定JDK目录下的jni.h和jni_md.h文件路径, -shared 为生成共享库, -o 为指定库文件名为MyDLL.dylib(Mac OS下为 lib文件名.dylib,Windows下为 文件名.dll,linux下为 文件名.so).

* 将MyDLL.dylib文件拷贝到jniTest java工程根目录下,并新建一个类,名为test.java,代码如下:

```
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		MyDLL mydll = new MyDLL();
		//打印出3和4相加的结果
		System.out.println(mydll.addition(3, 4));
	}

}
```

* 在eclipse里右键点击jniTest工程,选择 `Build Path` - `Configure Build Path...` - 展开 `JRE System Library` - `Native library location` ,点击右边的 `Edit...` ,将dylib文件路径输入
* 运行Test,显示结果为7,调用动态库中的addition方法成功!