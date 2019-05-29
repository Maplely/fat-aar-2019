<div align="center">
<img src="https://raw.githubusercontent.com/top2015/fat-aar-2019/master/logo.png"  height="200" width="500">
</div>

# fat-aar
[![](https://www.jitpack.io/v/top2015/fat-aar-2019.svg)](https://www.jitpack.io/#top2015/fat-aar-2019)  
用于将maven引用的aar包合并

### 使用方法
Step1. 将插件上传至maven仓库 在工程中引用
```
allprojects {
	repositories {
		maven { url 'https://www.jitpack.io' }
	}
}
```
```
classpath "com.github.top2015:fat-aar-2019:1.0"
```
Step2. 在library中build.gradle中引用plugin
```
apply plugin: 'fat-aar'
```
Step3. 需要打包进来的module 用api引用，如
```
 api 'com.top:xxxxx:2.0.0-SNAPSHOT'
```
Step4. 选择需要打包进来的资源文件，防止打包冲突(支持正则表达式,例如根据项目的中前缀定制)
```
fataar{
    resourceRegx=""
}
```
Step5. 执行gradle打包命令，输出的合并后的aar文件

### 原理简介
aar文件包括
```
/AndroidManifest.xml（必须）
/classes.jar（必须）
/res/（必须）
/R.txt（必须）
/assets/（可选）
/libs/*.jar（可选）
/jni/<abi>/*.so（可选）
/proguard.txt（可选）
/lint.jar（可选）
```
上面几项内容逐一合并

1. Android通过gradle一系列task打包最终生成apk。合并aar的过程也就是将库中的相关文件下载后，把目录添加到相应task的属性中。
2. AndroidManifest 反射android task “com.android.build.gradle.tasks.InvokeManifestMerger”
3. 各个moudle生成自己的R文件 映射到最终aar生成R文件的id(int 值) R(libary) >>>>>>>> R(module)

### 注意点
+ android plugin版本变化可能导致task或者输出文件变化，从而导致合并失败，本插件支持gradle 3.0.x、3.1.x、3.2.x
+ 本工程仅考虑打包远程maven依赖的方式，本地依赖的方式不建议使用本插件，可以考虑目录区分
+ 建议打包的aar不包含<font color=#FF0000 >第三方库</font>，否则容易引起版本冲突，android studio不会处理本地与maven引用版本依赖的问题，为了兼容性与扩展性建议全原生实现
+ 考虑灵活性，没有考虑打包工程依赖的子项目，即所有需要集成的module都需要手动引入


### 感谢
[android-fat-aar](https://github.com/adwiv/android-fat-aar)  
[fataar-gradle-plugin](https://github.com/Mobbeel/fataar-gradle-plugin)

### Contract & FeedBack
Author:HiTop

Email: haitao.li_2016@163.com

QQ:986086927

GitHub: https://github.com/top2015

任何缺陷、建议，欢迎给我发邮件，或在GitHub上创建问题单。

Any bugs and recommendations, please send emails for me, or create issues on GitHub.
