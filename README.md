# HBApp
组件化相关链接：
https://github.com/CocoaPods/CocoaPods/
https://www.cnblogs.com/dins/p/ios-zu-jian-hua-fang-an.html
https://www.dazhuanlan.com/2019/12/12/5df15dd68b8e0/
https://www.jianshu.com/p/f218fe3baff8
https://github.com/Lede-Inc/LDBusMediator



组件化：

>1、模块管理：cocoapods+router+解耦工具
>2、基础库：三方库+工具库
>3、业务库：主页+消息+我的
>4、app：模块管理+基础库+业务库+APP通用库

可抽出模块：账户中心、意见反馈、商品详情

## 一、创建应用HBApp

1、Git账号设置
git config --global user.name yahibo
git config --global user.email yahibo@qq.com

出现问题：iOS git push 提示 denied to
解决：钥匙串删除GitHub项重新提交
注意：修改user.name会不生效需要，打开钥匙串找到github连接删除记录重新修改即可

2、创建空仓库
GitHub上创建一个私有空库

3、创建应用并提交到github
* 创建APP工程，进入工程目录
* git init
* git add .
* git commit -m "first commit"
* git branch -M main
* git remote add origin https://github.com/yahibo/HBApp.git
* git push -u origin main 

git使用：
git tag 1.0.0
git push --tags
git tag -d 1.0.0 //删除本地
git push origin :refs/tags/1.0.0 //删除远端


## 二、创建Git私有仓库

用来管理其他库的`spec`文件，通过该仓库找到对应的库地址，在`podfile`文件中直接使用`pod 'HBBase’` 引入即可。
Git 仓库地址：
https://github.com/swift-temp-app/hb-base-repo.git
git@github.com:swift-temp-app/hb-base-repo.git

## 三、创建模块

```
pod lib create 模块名称 --template-url=https://github.com/swift-temp-app/hb-pod-template.git
```
1、提交至git，同上
进入模块，验证库：`pod lib lint`，`pod lib lint --allow-warnings`。

2、提交Spec到私有库

提交tag，tag和s.version要保持一致
```
git tag 0.1.0
git push --tags
```

将模块添加到私有库中
```
pod repo add swift-temp-app git@github.com:swift-temp-app/hb-base-repo.git

```

将spec文件推送到远端
```
pod repo push swift-temp-app HBBase.podspec --allow-warnings
```
或
```
pod repo push swift-temp-app *.podspec --allow-warnings
```

在主工程HBApp中Podfile添加pod `'HBBase'`即可引入远端库



## 四、组件通信

public open的区别


## 五、遇到的问题

问题1：
“[!] [Xcodeproj] Generated duplicate UUIDs:”

创建重复文件导致，警告描述较多

解决：
https://github.com/CocoaPods/CocoaPods/issues/4370#issuecomment-602368518
1、创建duplicateUUIDs.txt文件，将警告复制到文件中，筛选出重复的文件名称，使用命令：
2、grep -E '[a-zA-Z+]+\.(h|m|swift)' -o duplicateUUIDs.txt | sort | uniq -d
3、找到重复的文件删除即可。






