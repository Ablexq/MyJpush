

参考：
[三分钟帮你集成极光推送——和那些可能你不知道的事](https://blog.csdn.net/zj_blog/article/details/80247920)

# 问题一：

AndroidManifest.xml missing required intent filter for PushActivity: cn.jpush.android.ui.PushActivity

解决方式：

动态权限所致，测试时将targetSdkVersion降低到22即可。


# 问题二：

Permission is only granted to system apps

解决：

setting搜索Using system app permission，然后将Error改成Warning


# 问题三：

App is not indexable by Google Search; consider adding at least one Activity with an ACTION-VIEW

解决方式：

```
Android{
    lintOptions {
        disable 'GoogleAppIndexingWarning'
    }
}
```


# 问题四

JCoreInterface init failed，

解决：

1.注意build.gradle的applicationId，和Androidmanifest中的包名是否一致

2.appkey是否填写


# 集成指南：


### 导入库：

方式一：下载的demo中，将so文件,jar文件拷贝进来

注意：so文件放在lib中，要添加以下：
```
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
```

方式二：
```
buildscript {
    repositories {
        jcenter()
    }
    ......
}

allprojets {
    repositories {
        jcenter()
    }
}
```

```
android {
    ......
    defaultConfig {
        applicationId "com.xxx.xxx" //JPush 上注册的包名.
        ......

        ndk {
            //选择要添加的对应 cpu 类型的 .so 库。
            abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a'
            // 还可以添加 'x86', 'x86_64', 'mips', 'mips64'
        }

        manifestPlaceholders = [
            JPUSH_PKGNAME : applicationId,
            JPUSH_APPKEY : "你的 Appkey ", //JPush 上注册的包名对应的 Appkey.
            JPUSH_CHANNEL : "developer-default", //暂时填写默认值即可.
        ]
        ......
    }
    ......
}

dependencies {
    ......

    compile 'cn.jiguang.sdk:jpush:3.1.6'  // 此处以JPush 3.1.6 版本为例。
    compile 'cn.jiguang.sdk:jcore:1.2.5'  // 此处以JCore 1.2.5 版本为例。
    ......
}

```

### 下载的demo中，将权限,四大组件，并修改自己的包名和appkey


### 初始化
```
JPushInterface.setDebugMode(true); 	// 设置开启日志,发布时请关闭日志
JPushInterface.init(this);     		// 初始化 JPush
```












