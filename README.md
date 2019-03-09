# fat-aar
用于android library aar合并
### 使用方法
+ 将插件上传至maven仓库 在工程中引用
```
allprojects {
	repositories {
		maven { url 'https://www.jitpack.io' }
	}
}
```
```
classpath "com.github.top2015:fat-aar-2019:v1.0.0"
```
+ 在library中build.gradle中引用plugin
```
apply plugin: 'fat-aar'
```
+ 需要打包进来的module 用api引用，如
```
 api 'com.top:xxxxx:2.0.0-SNAPSHOT'
```
+ 可以选择添加资源文件过滤，防止打包冲突
```
fataar{
    resourceRegx=""
}
```
+ 执行gradle打包命令，输出的合并后的aar文件

### 原理简介
aar包括
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
1. AndroidManifest 反射android task “com.android.build.gradle.tasks.InvokeManifestMerger”
2. R文件 生成R.jar 将各个module指向当前library 并针对性地过滤处理
### 注意点
+ android plugin版本变化可能导致task或者输出文件变化，从而导致合并失败，本插件支持gradle 3.0.x、3.1.x、3.2.x
+ aar集成的moudle不能在外部引用，否则会引起冲突
+ 考虑灵活性，没有考虑打包工程依赖的子项目，即所有需要集成的module都需要手动引入
+ 本工程仅考虑打包远程maven依赖的方式，本地依赖的方式不建议使用本插件
+ 建议打包的aar不包含任何第三方库，否则容易引起冲突，为了兼容性与扩展性建议全原生实现

### Contract & FeedBack
Author: Li Haitao

Email: haitao.li_2016@163.com

QQ:986086927

GitHub: https://github.com/top2015

任何缺陷、建议，欢迎给我发邮件，或在GitHub上创建问题单。

Any bugs and recommendations, please send emails for me, or create issues on GitHub.