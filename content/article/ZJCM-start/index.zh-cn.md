---
title: "成功启动 ZJCM 项目"
description: "记录一次 ZJCM 项目从拉取代码到 IDEA 打包启动的完整过程"
date: 2026-05-09
draft: false
tags: ["ZJCM", "Gradle", "IDEA", "Java"]
---

## 介绍

这篇文章记录一下成功启动 `ZJCM` 项目的过程。这个项目启动时比较容易遇到依赖包、IDEA 打包、编译内存和 Java 版本不匹配的问题，所以这里把完整流程整理下来，方便之后重新部署或者排查问题。

## 拉取项目

首先使用 `git` 命令直接拉取项目，不建议使用 IDEA 自带的拉取工具。

```bash
git clone 项目地址
```

拉取完成后，再使用 IDEA 打开项目目录。


## 修改 build.gradle

项目中有一部分依赖包已经放在项目自带的 `lib` 目录中，所以需要修改 `build.gradle`，让 Gradle 使用项目 `lib` 里的本地包。

可以在依赖配置中加入类似下面的配置：

```gradle
dependencies {
    implementation fileTree(dir: 'lib', include: ['*.jar'])
}
```

如果项目中原来已经有 `dependencies` 配置，不需要重复创建，只需要把 `fileTree` 这一行加进去即可。

{{< figure
src="img/修改builde-gradle-lib.png"
alt="修改 build.gradle 使用 lib 目录依赖"
caption="修改 build.gradle，让项目使用 lib 目录中的 jar 包"
>}}

添加以下配置
```gradle
tasks.withType(JavaCompile) {
    options.incremental = true
    options.compilerArgs += ["-nowarn", "-XDenableSunApiLintControl"]
}

//定义目录结构
sourceSets {
    main {
        java {
            srcDirs = ['src/main/java']
        }
        resources {
            srcDirs = ['src/main/config', 'src/main/resources', 'src/main/webapp']
        }
    }
}
```

## 使用 IDEA 打包

修改完成后，回到 IDEA 中重新加载 Gradle 配置，等待依赖刷新完成。

然后使用 IDEA 对项目进行打包。这里建议直接使用 IDEA 的 Gradle 面板或者项目已有的打包任务进行构建，确认打包过程没有依赖缺失或者编译错误。

{{< figure
src="img/idea打包.png"
alt="使用 IDEA 打包 ZJCM 项目"
caption="使用 IDEA 执行项目打包"
>}}

## 修改 IDEA 编译内存

如果打包过程中出现内存不足、编译卡住或者 Gradle 构建异常，可以调整 IDEA 的编译共享堆内存。

打开 IDEA 设置：

```plaintext
Settings/Preferences -> Build, Execution, Deployment -> Compiler -> Shared heap size
```

将 `Shared heap size` 修改为 `2G` 或者 `4G`。

如果电脑内存比较充足，可以直接设置为 `4G`；如果机器内存一般，可以先设置为 `2G`。

{{< figure
src="img/修改idea编译内存.png"
alt="修改 IDEA Compiler Shared heap size"
caption="修改 IDEA 编译共享堆内存为 2G 或 4G"
>}}

## 处理 Java 版本问题

如果启动或打包时提示 Java 版本不对，需要把项目的 Java 版本修改为项目要求的对应版本。

重点检查这些位置：

1. **Project SDK**
2. **Module SDK**
3. **Gradle JVM**
4. **build.gradle 中的 sourceCompatibility 和 targetCompatibility**

修改完成后，如果 IDEA 仍然提示版本异常，可以清除 IDEA 缓存后重新打开项目。

```plaintext
File -> Invalidate Caches...
```

清除缓存后，重新加载 Gradle 配置，再次执行打包或启动。

{{< figure
src="img/修改java版本.png"
alt="修改 IDEA Java 版本配置"
caption="修改项目 Java 版本为对应版本"
>}}

{{< figure
src="img/清除idea缓存.png"
alt="清除 IDEA 缓存"
caption="清除 IDEA 缓存后重新加载项目"
>}}

## 启动成功

完成上面的配置后，再次启动 `ZJCM` 项目。如果控制台没有继续出现依赖缺失、Java 版本错误或者内存不足的问题，就说明项目已经成功启动。

{{< figure
src="img/启动成功.png"
alt="ZJCM 项目启动成功"
caption="ZJCM 项目启动成功"
>}}

## 总结

这次启动 `ZJCM` 项目主要需要注意以下几点：

1. 使用 `git` 命令直接拉取项目，不使用 IDEA 工具拉取。
2. 修改 `build.gradle`，让项目使用 `lib` 目录中的本地依赖包。
3. 使用 IDEA 完成项目打包。
4. 将 IDEA 的 `Compiler -> Shared heap size` 修改为 `2G` 或 `4G`。
5. 如果提示 Java 版本不对，修改为对应版本，并清除 IDEA 缓存后重新加载项目。

按照这个流程处理后，项目就可以正常打包并启动。
