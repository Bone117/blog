# Android studio中遇到的问题

> 首先声明只是Android studio使用中遇到的问题纯属个人学习笔记，有什么不对的可以留言。
> 将脱壳后的java文件拖入到Android studio
> android studio 首先提示是`ERROR: Gradle version 2.2 is required. Current version is 5.1.1`
> 首先，确认Bild,Execution,Deployment>Build Tools>Gradle中的配置
> ![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/iPic/2019-09-02-024921.png)
> 选中第三个，配置`Gradle home`为本地安卓中的gradle版本，macOS可以在 应用程序中右击Android studio 显示包内容，在gradle目录下就可以看到。

> 再次build 提示`Could not find com.android.tools.build:gradle:2.2.3.`
> 再次查看当前系统的gradle的版本，这次指的是仓库中的gradle编译工具版本，不是之前的gradle版本，
> 我的路径在`/Applications/Android Studio.app/Contents/gradle/m2repository/com/android/tools/build/gradle`
> ![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/iPic/2019-09-02-031711.png)
> 修改build.gradle里的classpath

```java
dependencies {
        classpath 'com.android.tools.build:gradle:3.4.1'
}
```

还是不行的话，在gradle中添加

```java
allprojects {
    repositories {
        google()
        jcenter()
    }
}
```

> 再次build碰到了`The specified Android SDK Build Tools version (23.0.1) is ignored, as it is`
> 大致意思是目前使用的build工具版本26.0.2不合适，
> 修改`buildToolsVersion`为指定的版本(错误提示中有)
> 要是遇到编译的时候碰到了`Error:Failed to open zip file`,可以执行以下操作
> 首先删除下面的文件
> macOS: ~/.gradle/wrapper/dists
> Linux: ~/.gradle/wrapper/dists
> Windows: C:\Users\your-username\.gradle\wrapper\dists

然后重启`File Invalidate Caches / Restart`
参考:https://stackoverflow.com/questions/44071080/could-not-find-com-android-tools-buildgradle3-0-0-alpha1-in-circle-ci
