# 发布Lib到Jcenter
![](https://img.shields.io/badge/Gradle-v2.14.1-red.svg)
![](https://img.shields.io/badge/Studio-v2.1.3-green.svg)
![](https://img.shields.io/badge/Java-7-blue.svg)

## 准备
+ 注册账号：https://bintray.com (可以用github账号直接授权).
+ 注册完毕之后，记住用户名.
+ 在 `Edit Your Profile` -> `API Key` 中获取Key.

## 如何使用
### 1.创建 `Android Library Project`
### 2.修改在 `local.properties` 
在最后添加如下两个属性：
``` script
bintray.apikey=你的API Key
bintray.user=你的用户名
```
### 3. 修改 `根目录(Project)`build.gradle
+ 添加工具库插件
``` script
plugins {
    id "com.github.dcendents.android-maven" version "1.5"
    id "com.jfrog.bintray" version "1.7"
}
```
+ 添加本地maven仓库路径
``` script
allprojects {
    repositories {
    	// 新添加开始
    	maven {
	    	Properties properties = new Properties()
			properties.load(project.rootProject.file('local.properties').newDataInputStream())
            url properties.getProperty("sdk.dir")+"/extras/android/m2repository"
        }
        mavenLocal()
        // 新添加结束
        jcenter()
    }
}
```

### 4.修改`Module的`build.gradle ，在最后添加：
``` script
ext {
	name = '你的lib名称'			        // lib名称，比如：My_Lib
	desc = '库的描述'   			        // 库的描述尽量不要用中文
	
	groupId = '你的groupId'			     // 填写groupId， 一般是包名，比如：com.android.support
	//artifactId = '你的aritfactId'	 	 // 这里不需要再填写，自动以Model的名字作为aritfactId
	version = '版本号'			            // 版本号，比如：22.2.1

	websiteUrl = '库的网站链接'		       // 可以填写github上的库地址.
	issueTrackerUrl = '库的issue链接'	    // 可以填写github库的issue地址.
	vcsUrl = '库的版本控制地址'		         // 可以填写github上库的地址.
}
// 下面这行请勿修改
apply from: 'https://raw.githubusercontent.com/andforce/release-android-lib-to-jcenter/master/bintray.gradle'
```

### 5.编译发布
``` script
gradle jcenter
```
### 6.Add to JCenter
执行完上面的步骤，你只是在bintray中创建了一个Package，要发布到JCenter还需要你手动去网站点一下`Add to JCenter`.之后等待审核就好了.

-------------------------
### 完整使用例子
https://github.com/andforce/AsyncOkHttp

### 可能遇到的问题
#### 1. 如果你的库又引用了别的库怎么办？
> 不需要特殊处理，直接在 `dependencies` 中引用就可以了

#### 2. 因为现在是以Module的名字作为 `aritfactId` 如果想换怎么办？
> 只能修改 `Module` 的名称，具体需要修改3处：Module 路径名称，Module 名称， 以及settings.gradle中对应Module名称，都改成一致即可.



## 感谢:
https://github.com/dcendents/android-maven-gradle-plugin
https://github.com/bintray/gradle-bintray-plugin
http://theartofdev.com/2015/02/19/publish-android-library-to-bintray-jcenter-aar-vs-jar-and-optional-dependency/
http://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en
