# 新版小米智能家庭Android app免安装插件开发手册


**最新修改**
- 新版插件sdk使用Android studio开发

## 插件开发

## 插件开发

### 安装开发版智能家庭app

智能家庭应用商店版的app不支持本地开发调试，需要安装sdk目录下的智能家庭app
 
### 工程目录结构

- [github](https://github.com/MiEcosystem/NewXmPluginSDK)更新SDK代码

- 插件工程放置于plugProject目录下，如下图，可以放置多个插件工程，注意插件工程目录结构
也可以在plugProject目录下直接管理其他git创库工程，比如插件实例工程[NewPluginDemo](https://github.com/MiEcosystem/NewPluginDemo)，在plugProject目录下直接clone即可

```
cd plugProject
git clone https://github.com/MiEcosystem/NewPluginDemo.git
```

![](./md_images/gradle_project.png)

### 配置插件签名文件
所有插件在智能家庭app上运行时需要进行签名验证
- 修改插件工程build.gradle的签名信息

```
    signingConfigs {
        release {
            storeFile new File("${project.projectDir}/keystore/key.keystore")
            storePassword 'mihome'
            keyAlias 'mihome-demo-key'
            keyPassword 'mihome'
        }
    }

    buildTypes {
        debug {
            debuggable true
            signingConfig signingConfigs.release
        }
        release {
            minifyEnabled true
            shrinkResources false
            zipAlignEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }
```

### 配置插件编译脚本

- 修改插件工程build.gradle，末尾添加

```
apply from: '../../plug.gradle'

```

### 插件编译运行

-在插件sdk目录下执行
```
gradle tasks
```
可以看到安装插件task，如下

![](./md_images/gradle_tasks.png)

```
gradle install 安装运行release配置插件
gradle installRelease 安装运行release配置插件
gradle installDebug 安装运行debug配置插件

如果有多个插件工程，上面指令会安装所有插件，指定安装某个插件工程

gradle :plugProject:NewPluginDemo:install 安装运行release配置插件
gradle :plugProject:NewPluginDemo:installRelease 安装运行release配置插件
gradle :plugProject:NewPluginDemo:installDebug 安装运行debug配置插件
```

### 调试插件
安装上插件后，会自动启动智能家庭app，点击android studio 调试按钮，一次点击如下图所示按钮，可以在插件代码中打断点调试

![](./md_images/gradle_debug.png)

### 上传插件到智能家庭后台，申请上线

开发完成后，测试通过，可以申请上线，编译好的安装包插件目录/build/outputs/apk下面
除了功能测试通过，必须要注意进行内存测试，原则上在退出插件页面后，插件需要退出所有的后台线程，释放所有的内存资源，特别是Activity对象的内存泄露
上线审核前，会专门针对这两项测试。


------


```
免安装插件框架实现了一套在Android上面加载apk技术，跳过系统安装程序，只需要把apk上传到后台服务器，app端自动更新插件，具有H5实时更新特性，又具有native app灵活高效率特性。
```


- [框架描述](框架描述.md)
- [服务器部署](服务器部署.md)
- [插件开发](插件开发.md)
- [开发接口描述](开发接口描述.md)
- [蓝牙规范](智能家庭蓝牙规范.md)

- [插件实例工程](https://github.com/MiEcosystem/NewPluginDemo)
- [智能家庭开放平台](https://open.home.mi.com)


开发联系方式
qq:276111321






<!-- create time: 2015-04-17 10:53:01  -->