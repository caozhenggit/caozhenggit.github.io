---
title: 【Android异常】关于Android方法数量限制的问题
date: 2017-10-11 11:44:58
categories: "Android"
tags: "异常"
---

## 异常错误信息 ##
错误信息：

``` bash
Error:Errorconverting bytecode to dex:
Cause:com.android.dex.DexIndexOverflowException:field ID not in [0, 0xffff]: 65536
:XXXXXX:transformClassesWithDexForDebug FAILED
Error:Executionfailed for task ':XXXXXX:transformClassesWithDexForDebug'.
>com.android.build.api.transform.TransformException:com.android.ide.common.process.ProcessException:java.util.concurrent.ExecutionException:com.android.ide.common.process.ProcessException:org.gradle.process.internal.ExecException: Process 'command 'C:\ProgramFiles\Java\jdk1.8.0_73\bin\java.exe'' finished withnon-zero exit value 2

```

## 限制Android方法数量的原因 ##

Android应用以DEX文件的形式存储字节码文件，在Dalvik字节码规范里，方法引用索引method referenceindex只有16位,即65536个。
注意是method reference，这里限制的是自己代码、Android框架、第三方库三者方法数量的总和。

## 解决方法 ##

[Google官方出的分包方案](http://developer.android.com/intl/es/tools/building/multidex.html)，采用MultiDex支持库

1，配置building.gradle，开启MultiDex
  
``` bash
android {
    defaultConfig {
        multiDexEnabled true
    }
}
dependencies{
    compile'com.android.support:multidex:1.0.0'
}
}

```

2，配置应用

方法1：在AndroidManifest.xml的application中声明android.support.multidex.MultiDexApplication；

``` bash
<?xmlversion="1.0" encoding="utf-8"?>
<manifestxmlns:android="http://schemas.android.com/apk/res/android"
   package="com.example.android.multidex.myapplication">
    <application
        ...
       android:name="android.support.multidex.MultiDexApplication">
        ...
    </application>
</manifest>
```

方法2：让你自己的Application类继承MultiDexApplication

方法3：让你自己的Application类重写attachBaseContext 方法，
``` bash
@Override
protected void attachBaseContext(Context base) {
super.attachBaseContext(base);
MultiDex.install(this);
}
```

