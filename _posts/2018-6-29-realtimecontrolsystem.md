---
layout: post
title: Real-time Control System
date: 2018-06-29
categories: blog
tags: [技术总结]
description: RCS Module detail
---

**千万不要以rcs-test等短划线的方式命名RCS工程**

## RCS Structure

- \{project-name\}.nml //配置buffer文件，使用\#注释
- \{project-name\}svr.cc //server源文件，编译后变为可执行文件
- \{project-name\}main.cc //main程序，其中定义了module对象，并循环执行module中的××_process\(\)
- \{module-name\}_module.cc // 模块代码，工程的功能模块
- \{module-name\}.hh // 对应上面的module.cc，是它的头文件
- \{module-name\}n.hh //模块的status和command通道
- \{aux-channel\}n.hh //工程的辅助通道
- GNUmakefile // makefile文件

## RCS Config

RCS中主要的配置都在.nml文件中，nml文件可以分为三部分：

> buffer

> module-name

> srv

如果使用的buffer不是该工程的srv建立的，但该工程使用了这个buffer，那么在buffer和module-name部分都要声明这个buffer，但**srv**部分没有这个buffer，对应**\{project-name\}svr.cc**中也没有关于该buffer的构建的code。
\{Note\} 该工程的nml文件中buffer部分声明的使用的buffer需要和定义该buffer的（srv申请的buffer）工程中的buffer MP必须相同，TCP貌似也应该相同。

## RCS Module

在声明辅助通道时候，如果该module是input该辅助通道，那么一般会这样写

```
//<module-name>.hh
NML * ×××_CHANNEL;//
×××_MSG * ×××_data;//这是一个指针
//<module-name>_module.cc
×××_CHANNEL = new NML(×××Format, "<buffer-name>", "<moudle-name>", "×××.nml");
×××_data = (×××_MSG *) ×××_CHANNEL->get_address();
switch(SLAMPOSAZIM_CHANNEL->read())
{
  case ×××_MSG_TYPE:
          do-someting....;
	break;
}
```

如果该module是output该辅助通道，那么一般会这样写

```
NML * ×××_CHANNEL;//
×××_MSG ×××_data;//这是一个对象
×××_CHANNEL = new NML(×××Format, "<buffer-name>", "<moudle-name>", "×××.nml");
...赋值操作。。。
×××_CHANNEL->write(&×××_data);
```

## 命令通道

TODO

## 两个文件夹在一台电脑上共用一个辅助通道

nml配置：对于这个通道的而言，其buffer number/MP/TCP port保持一致，否则diag_NB.jar会报错。

## diag_NB.jar

```
java -jar /usr/local/diag_NB.jar
在对话框里open **.cfg
这个过程需要打开cfg文件中提到的channel的P_**n.hh，否则diag_NB无法确定channel的类型
```

## 修改nml文件注意事项

1. 在nml文件中增加删除某个buffer，注意更改相应的××svr.cc中的该buffer对应的channel相关代码
2. 由codegen.jar将{aux_channel}n.hh得到的{aux_channel}n_n.cc中会自动生成一个关于该通道最小是多少数值，供参考。


## RCS struct类型的声明

DECLARE_NML_DYNAMIC_LENGTH_ARRAY({类型名}, 数组变量名, 数组的最大长度);//声明一个数组

在某个{aux_channel}n.hh中需要声明一个struct{结构体};，需要在该文件中同时声明```extern void nmlupdate(CMS *cms, {结构体名称} *x);```，这将通过codegen产生结构体的构造函数
