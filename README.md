<div align="center">
<img src="https://raw.githubusercontent.com/top2015/fat-aar-2019/master/logo.png"  height="200" width="500">
</div>

# fat-aar
[![](https://www.jitpack.io/v/top2015/fat-aar-2019.svg)](https://www.jitpack.io/#top2015/fat-aar-2019)  
Merging Librarys From Maven eventually generates an AAR

### USE
Step1. quote plugin address in project
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
Step2. apply plugin in moudle`s build.gradle
```
apply plugin: 'fat-aar'
```
Step3. use api front in library you want to refre,E.g:
```
 api 'com.top:xxxxx:2.0.0-SNAPSHOT'
```
Step4. Select resource files that need to be packaged to prevent packaging conflicts (support regular expressions, such as customization based on the prefix of the project)
```
fataar{
    resourceRegx=""
}
```
Step5. Execute the gradle packaging command to output the merged AAR file

### PRINCIPLE
aar include:
```
/AndroidManifest.xml（Required）
/classes.jar（Required）
/res/（Required）
/R.txt（Required）
/assets/（optional）
/libs/*.jar（optional）
/jni/<abi>/*.so（optional）
/proguard.txt（optional）
/lint.jar（optional）
```
The above items are merged one by one.

1. Android packages through a series of tasks in gradle to generate apks. The process of merging AAR is to add the directory to the attributes of the corresponding task after downloading the relevant files in the library.
2. AndroidManifest reflects android task “com.android.build.gradle.tasks.InvokeManifestMerger”
3. Each moudle generates its own R file and maps it to the ID (int) of the final AAR generated R file. R(libary) >>>>>>>> R(module)

### ATTENTION
+ Android plugin version changes may cause task or output file changes, leading to merge failure. This plug-in supports gradle 3.0.x, 3.1.x, and 3.2.x.
+ This project only considers packaging remote maven dependencies. Local dependencies do not recommend using this plug-in. Directory differentiation can be considered.
+ It is suggested that the packaged AAR does not contain ** third-party libraries **, otherwise it is prone to version conflicts. Android studio will not deal with local and Maven reference version dependencies. For compatibility and extensibility, it is recommended that ** native ** implementations be implemented. ** The resulting AAR package can not be excluded from implementation references by adding excludes, because the warehouse name Library Name: Version is the way Maven refers to, and also the way Google recommends referencing libraries to resolve version conflicts.
+ Considering flexibility, we do not consider the sub-projects that the packaging engineering depends on, that is, all modules that need to be integrated need to be introduced manually.


### THANKS
[android-fat-aar](https://github.com/adwiv/android-fat-aar)  
[fataar-gradle-plugin](https://github.com/Mobbeel/fataar-gradle-plugin)

### Contract & FeedBack
Author:HiTop

Email: haitao.li_2016@163.com

QQ:986086927

GitHub: https://github.com/top2015

任何缺陷、建议，欢迎给我发邮件，或在GitHub上创建问题单。

Any bugs and recommendations, please send emails for me, or create issues on GitHub.
