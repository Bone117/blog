# xposed的基本使用

## 一、原理

> Android运行的核心是zygote进程，所有app的进程都是通过zygote fork出来的。通过替换**system/bin/下面的app_process等文件**，相当于替换了zygote进程，实现了控制手机上的所有APP。基本原理是修改了ART/Davilk虚拟机，将需要hook的函数注册为Native层函数，当执行到该函数时，虚拟机会先执行Native层函数，然后执行Java层函数，这样完成hook。

更详细的可以参考：https://blog.csdn.net/wxyyxc1992/article/details/17320911

## 二、Xposed安装

环境：网易mumu、Android Studio3.3.1

github地址：https://github.com/rovo89/XposedInstaller

Xposedinstaller的apk：https://repo.xposed.info/module/de.robv.android.xposed.installer

> 在网易mumu中安装好xposedinstaller apk后，关闭应用兼容性(不关闭的话安装xposed框架会出错)，进去之后点击小云彩即可安装完成。

![image-20190619104940343](http://ww3.sinaimg.cn/large/006tNc79ly1g469wd667cj31am0u0gs3.jpg)

## 三、Xposed代码编写

> 新建项目，选择`empty activity`，创建成功后，在`AndroidManifest.xml`中添加如下代码

```xml
<meta-data
    android:name="xposedmodule"
    android:value="true" />   <!--告诉xposed框架这是一个xposed模块-->
<meta-data
    android:name="xposeddescription"
    android:value="这是一个Xposed例程" /> <!--模块描述-->
<meta-data
    android:name="xposedminversion" <!--模块支持的最低版本-->
    android:value="30" />
```

> 在gradle中配置XposedbridgeApi，build.gradle中配置

```gradle
repositories {
    jcenter()
}
dependencies {
		...
    compileOnly 'de.robv.android.xposed:api:82'
    compileOnly 'de.robv.android.xposed:api:82:sources'
    ...
}
```

> 这是在网络通畅的情况下进行的， 网络不通畅的话，可以手动下载XposedBridgeApi-82.jar，拖动到/app/libs中，删除上述gradle中配置的 `jcenter`，右键"Add As Library"添加这个jar包。

> 在界面上画个按钮，并在`MainAcitiviy`中编写如下代码(单纯写hook的话前面新建项目的时候可以add no activity)

```java
package com.example.myapplication;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;
import android.view.View;
public class MainActivity2 extends AppCompatActivity {
    private Button button;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                Toast.makeText(MainActivity2.this, toastMessage(), Toast.LENGTH_SHORT).show();
            }
        });
    }
    public String toastMessage() {
        return "我未被劫持";
    }
}
```

> 编写Hook代码，在`MainActivity`同级目录下新建`HookTest.java`，并且继承接口IXposedHookLoadPackage和重写handleLoadPackage方法

```java
package com.example.myapplication;
import java.lang.reflect.Array;
import java.security.PublicKey;
import java.util.Arrays;
import java.util.Map;
import de.robv.android.xposed.IXposedHookLoadPackage;
import de.robv.android.xposed.XC_MethodHook;
import de.robv.android.xposed.XposedBridge;
import de.robv.android.xposed.XposedHelpers;
import de.robv.android.xposed.callbacks.XC_LoadPackage;
public class HookTest implements IXposedHookLoadPackage {

    private static final String HOOK_APP_NAME = "APP名字";

    public void handleLoadPackage(XC_LoadPackage.LoadPackageParam lpparam) throws Throwable {

        //性能优化，避免操作无关app
        if (!lpparam.packageName.equals(HOOK_APP_NAME))
            return;
        if (lpparam.packageName.equals("HOOK_APP_NAME")) {
            XposedBridge.log(" 劫持成功！！!");
          
        XposedBridge.log("XposedMainInit handleLoadPackage 执行");
        XposedBridge.log("Loaded app: " + lpparam.packageName);            XposedHelpers.findAndHookMethod("APP名字.MainActivity",//hook的类
                    lpparam.classLoader,
                    "toastMessage", // 被Hook的函数
                    //Map.class, 被Hook函数的第一个参数  (此处没有，只是举个例子)
                    //String.class, 被Hook函数的第二个参数String
                    new XC_MethodHook() {
                        protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
                            super.beforeHookedMethod(param);
                            // 参数获取
                            XposedBridge.log("入口函数执行");
                            //参数1
                            XposedBridge.log("beforeHookedMethod map:" + param.args[0]);
                            //参数2
                            XposedBridge.log("beforeHookedMethod hash_key:" + param.args[1]);
                            //函数返回值
                            XposedBridge.log("beforeHookedMethod result:" + param.getResult());
                        }
                        protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                            XposedBridge.log("afterHookedMethod result:" + param.getResult());
                          param.setResult("你已被劫持");
                        }
                    });        
        }
    }
}
```

> 在src/mian目录下添加一个assets目录，目录下添加一个xposed_init文件，里面的代码是你的Hook类的包名+类名。
>
> `com.example.myapplication.HookTest`

> 最后选择禁用 Instant Run： 单击 File -> Settings -> Build, Execution, Deployment -> Instant Run，把勾全部去掉。

![image-20190619133257899](http://ww3.sinaimg.cn/large/006tNc79ly1g46em60th9j30s40jcjt2.jpg)

> 这个时候钩子已经执行了，具体想钩什么，就看自己的需求了。

注：实际操作中，需要对APP先进行反编译(反编译了才能知道要钩那个函数)，反编译工具有很多，这里就不细说了。

## 四、面对加了壳的APP

> 直接用反编译工具打开apk，查看加的是哪种壳，寻找对应的函数，类似`attachBaseContext`这样的方法。
>
> 参考链接：https://www.cnblogs.com/xiaobaiyey/p/6442417.html

```java
public class EncryptHook implements IXposedHookLoadPackage {

    public void handleLoadPackage(LoadPackageParam loadPackageParam) throws Throwable {
        if (!loadPackageParam.packageName.equals("app包名")) { return; }
        XposedBridge.log("Start hook " + loadPackageParam.packageName);

        XposedHelpers.findAndHookMethod("com.stub.StubApp", loadPackageParam.classLoader,
                 //com.stub.StubApp 加壳的类
                "attachBaseContext", Context.class, new XC_MethodHook() {
                 // attachBaseContext 加壳的方法 
                @Override
                protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                    super.afterHookedMethod(param);
                    Context context = (Context) param.args[0];
                    ClassLoader classLoader = context.getClassLoader();
                    XposedBridge.log("Enter stubApp");

                    XposedHelpers.findAndHookMethod("com.huijiemanager.utils.t", classLoader,
                        "a", byte[].class, PublicKey.class, new XC_MethodHook() {
                            @Override
                            protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
                                XposedBridge.log("rsa before params: " + new String(
                                    (byte[]) param.args[0]) + "," + param.args[1]);
                            }
                            @Override
                            protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                                XposedBridge.log("rsa after params: " + new String(
                                    (byte[]) param.args[0]) + "," + param.args[1]);
                            }
                        });
                }
            });
    }
}
```

> 注：反编译的代码不一定准确，逆向的时候最好对每个关键函数都挂上钩子，查看参数是否正确。

附上xposedAPI文档：https://api.xposed.info/reference/packages.html
