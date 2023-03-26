---
title: Typora图片共享（Github+Picgo+Typora）
date: 2023-01-05 13:39:00
tags:
---

# Typora图片共享（Github+Picgo+Typora）

1、安装PicGo

右下角有安装，然后转跳到主页即可根据流程进行安装。

安装路径：`D:\typora\Typora`

![image-20230104173203948](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415761.png)



## 2、配置Github

### 建立仓库

![image-20230104173406460](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415933.png)



### 获得token

右上角头上setting => 左下角Developer settings => Personal access tokens (classic) =>generate new token =>

得到token



## 3、配置Picgo

![image-20230104172612597](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415261.png)

![image-20220328081536617](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415656.png)

![image-20230105133448055](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415008.png)



### 4、测试验证即可

![image-20230104173804757](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415582.png)

![image-20230105133305869](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051415647.png)



## 5、图片上传配置

对typora进行图片配置，如下图所示

![image-20230105141748428](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051417484.png)

* 首先在本地写typora文件时，要打开Picgo

* 将图片插入方式改为以上形式后，当粘贴图片进入typora时，图片会自动上传Github，且图片路径会从本地路径更改为Github建立的图床中的路径。如：上图为`(https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051417484.png)`

* 这样，当md文件上传到Github中，图片自动引用Github图床中的图片。

  ![image-20230105142120130](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051421175.png)

  Github中md文件如下：

  ![image-20230105142142496](https://raw.githubusercontent.com/BiAJiii/imgsBed/main/imgs/202301051421539.png)

* 大功告成~

## 6、md上传hexo博客

因为上述图片引用的都是GitHub图床的内容，可以直接访问，所以，在hexo上传md时，只需要将需要的md文件，复制到blog对应的md文件即可。
