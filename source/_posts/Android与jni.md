---
title: Android与jni
date: 2017-11-21 16:55:26
tags: Android
categories: Android
thumbnail: http://p2.so.qhimgs1.com/t019f9129d3d99e30d2.jpg
---

创建工程选择引入ndk，再加入构建模块
![这里写图片描述](http://img.blog.csdn.net/20171119105730824?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
程序跑出来就有一个默认的调用c的方法，
下面是一个完整的c的方法
```
extern "C"
JNIEXPORT jstring

JNICALL
Java_com_example_administrator_testnative_MainActivity_stringFromJNI(
//        JNIEnv 类型代表了Java环境。通这个JNIEnv指针，就可以对Java断的代码进行操作，例如，创建Java类的对象
//        调用Java对象的方法，获取Java对象的属性等等，JNIEnv的指针会被jni传入到本地方法的实现函数中来对Java断的代码进行操作
        JNIEnv *env,
//        如果在Java中不是静态方法，该方法对应的类的实例；如果是静态方法传入的是该类的.class对象
        jobject /* this */) {
    std::string hello = "Hello from C++";
    return env->NewStringUTF(hello.c_str());
}
```
JNIEnv中提供的方法

![JNIEnv中提供的方法](http://img.blog.csdn.net/20171119110105519?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Java中类型在c中的对应关系

![这里写图片描述](http://img.blog.csdn.net/20171119110347704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在jni中获取jclass类

![这里写图片描述](http://img.blog.csdn.net/20171119110713660?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在jni中访问Java的方法与属性

![这里写图片描述](http://img.blog.csdn.net/20171119110836628?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

示例：

```
extern "C"
JNIEXPORT void

JNICALL
Java_com_example_administrator_testnative_MainActivity_test(
        JNIEnv *env,
        jobject obj) {
    // 得到jclass对象
    jclass mainActivity = env->GetObjectClass(obj);
    // 得到方法的id
    jmethodID id = env->GetMethodID(mainActivity,"jniCall","(I)I");
    // 调用方法
    env->CallIntMethod(obj,id,100L);
}
```

在上面用到了签名，签名是对应的方法的参数和返回值类型设置的，上面的示例中`"(I)I"`对应的是有int类型的参数返回值为`int`类型如果有多个类型`"(int i, boolean b,String s,int j)"`对应的是`""(IZLjava/lang/String;I)`，如果没有返回值没有参数`"()V"`或`"()V"`
![这里写图片描述](http://img.blog.csdn.net/20171119143318902?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在jdk中提供了javap工具签名
![这里写图片描述](http://img.blog.csdn.net/20171119111838555?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171119112925088?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

为了方便调试在jni中打印安卓日志
```
#include  <android/log.h>
#define  TAG    "HHH"   // 这是安卓的Tag  ANDROID_LOG_ERROR是日志级别 
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR,TAG,__VA_ARGS__)

    // 将java字符串转换成c字符串后再打印
    const char * str =  env->GetStringUTFChars(javaString,NULL);

    LOGE("%s",str);
```

### JNI中获取到java的字符串
```
// 在java中传入字符串到jni然后再返回java
extern "C"
JNIEXPORT

jstring
JNICALL
Java_com_example_administrator_testnative_MainActivity_callJniMethod(
        JNIEnv *env,
        jobject,jstring javaString){

    // 将java字符串转换成c字符串
    const char * str =  env->GetStringUTFChars(javaString,NULL);

    jstring javaStr = env->NewStringUTF(str);
    return javaStr;
}
```

jni数组操作

```
	// 在jni中创建数组
    // 定义c数组
    jint nativeArr[3] = {21,22,23};
    // 定义java数组，可以返回给java使用的数组
    jintArray javaArray = env->NewIntArray(3);
    // 将c数组的内容复制给java
    env->SetIntArrayRegion(javaArray,0,3,nativeArr);


// 复制数组再操作 在java中传入一个数组在c中计算完成后返回值
extern "C"
JNIEXPORT jint JNICALL
Java_com_example_administrator_testnative_MainActivity_getSum(
        JNIEnv *env,
        jobject obj,
        jintArray javaArr) {

    // 得到数组的长度
    jsize len = env->GetArrayLength(javaArr);
    // 定义一个数组
    jint cArr[len];
    // 将java数组复制到c中
    env->GetIntArrayRegion(javaArr,0,len,cArr);
    jint sum = 0;
    for (int i = 0; i < len; i++) {
        sum +=cArr[i];
    }
    return sum;
}


// 第二种中对直接指针的操作
// 在java中传入一个数组在c中计算完成后返回值
extern "C"
JNIEXPORT jint JNICALL
Java_com_example_administrator_testnative_MainActivity_getSum(
        JNIEnv *env,
        jobject obj,
        jintArray javaArr) {

    jint result = 1;
    jsize length = env->GetArrayLength(javaArr);
    // 等到c数组的int指针
    jint* nativeDirectArray = env->GetIntArrayElements(javaArr,NULL);
    for(int i=0;i<length;i++){
        result += nativeDirectArray[i];
//        nativeDirectArray[i] = i;
    }

    // 排序
    std::sort(nativeDirectArray,nativeDirectArray+length);

    // 0：将内容复制回来并释放原生数组 在c中改变数组，java中也会改变
    // JNI_COMMIT：将内容复制回来但是不释放原生数组，一般用于周期性的更新一个Java数组。
    // JNI_ABORT释放原生数组但是不将内容复制回来。
    env->ReleaseIntArrayElements(javaArr,nativeDirectArray,0);
    return result;
}
```
### 在c中全局引用、局部引用、弱引用
>从java虚拟机创建的对象传到本地c代码是会产生引用。根据java的垃圾回收机制，只要有引用存在就不会触发该引用指向的java对象的垃圾回收。
>这些引用在jni中分三种
	>全局引用（GlobalReference）
	局部引用（LocalReference）
	弱全局引用（WeakGlobalReference）
	
![这里写图片描述](http://img.blog.csdn.net/20171119215243084?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20171119215350192?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20171119215442316?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

由于jfielId和jmethodId在创建时会消耗，所以可以缓存起来
第一种在方法内使用static，在多次调用该方法时只会创建一次id
```
    static jmethodID initId;
    if (initId==NULL) {
        initId= env->GetMethodID(dateClass,"<init>","()V");
    }
```
第二种使用全局变量存储,在任何native函数调用之前初始化
```
// 全局变量
jfieldID gId=0;

extern "C"
JNIEXPORT void JNICALL Java_com_example_administrator_testnative_MainActivity_init(
        JNIEnv* env,
        jobject obj) {
    jclass cl = env->GetObjectClass(obj);
    gId = env->GetFieldID(cl,"age","I");
}
```
