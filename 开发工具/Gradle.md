[参考网址：http://wiki.jikexueyuan.com/project/gradle/java-quickstart.html](http://wiki.jikexueyuan.com/project/gradle/java-quickstart.html)

* 初始化一个java项目`gradle init --type java-library`

在编写Gradle脚本的时候，在build.gradle文件中经常看到这样的代码：

*build.gradle*

	buildScript {
	    repositories {
	         mavenCentral()
	    }
	    
	    dependencies {
	        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.2.RELEASE")
	    }
	}
	
	repositories {
	    mavenCentral()
	}

这样子很容易让人奇怪，为什么repositories要声明两次哪？buildscript代码块中的声明与下半部分声明有什么不同？

其实答案非常简单。buildscript中的声明是gradle脚本自身需要使用的资源。可以声明的资源包括依赖项、第三方插件、maven仓库地址等。而在build.gradle文件中直接声明的依赖项、仓库地址等信息是项目自身需要的资源。

gradle是由groovy语言编写的，支持groovy语法，可以灵活的使用已有的各种ant插件、基于jvm的类库，这也是它比maven、ant等构建脚本强大的原因。虽然gradle支持开箱即用，但是如果你想在脚本中使用一些第三方的插件、类库等，就需要自己手动添加对这些插件、类库的引用。而这些插件、类库又不是直接服务于项目的，而是支持其它build脚本的运行。所以你应当将这部分的引用放置在buildscript代码块中。gradle在执行脚本时，会优先执行buildscript代码块中的内容，然后才会执行剩余的build脚本。

举个例子，假设我们要编写一个task，用于解析csv文件并输出其内容。虽然我们可以使用gradle编写解析csv文件的代码，但其实apache有个库已经实现了一个解析csv文件的库供我们直接使用。我们如果想要使用这个库，需要在gradle.build文件中加入对该库的引用。

```
buildscript {
    repositories {
        mavenLocal()
        mavenCentral()
    }

    dependencies {
        classpath 'org.apache.commons:commons-csv:1.0'
    }
}

import org.apache.commons.csv.*

task printCSV() {
    doLast {
        def records = CSVFormat.EXCEL.parse(new FileReader('config/sample.csv'))
        for (item in records) {
            print item.get(0) + ' '
            println item.get(1)
        }

    }
}
```
**build项目**

`gradle build`



**设置resources目录**

```
sourceSets {
    main {
        resources {
            srcDirs "src/main/resources", "src/main/configs"
        }
    }
}
```

