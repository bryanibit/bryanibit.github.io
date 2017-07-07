---
layout: post
title: 2018-Alibaba-coding-online
date: 2017-7-5
categories: blog
tags: [考试经验总结]
description: c++问题
---

该题目的介绍以及初步程序代码已经上传[GitHub](https://github.com/bryanibit/2018-alibaba-CodeTest-online)

在此整理一些编程问题

7月6日前的代码没有考虑几个特殊情况，例如3个点，4个点时候的情况，以及num_all_point == num_hull 

博主有空将尽快更新

首先这道算法题应该就是先找到凸包点，之后遍历点对，查询其余点在该点对生成直线的两侧的数量，

进而得到两侧中较大的数字，加上2（脑补一下）就是实际上其能看到的城堡数量

## CMakeLists.txt

```
project(point_group)
cmake_minimum_required(VERSION 2.8)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -march=native -O3 -pthread")
＞　# OpenCV is not install in default location, if not delete that.
set(OpenCV_DIR "/home/inin/OpenDroneMap/SuperBuild/install/share/OpenCV")
# Find OpenCV at the default location
find_package(OpenCV REQUIRED)
set(OpenCV_LIBS opencv_core opencv_imgproc opencv_highgui)
include_directories(${OpenCV_INCLUDE_DIRS})
# Add source directory
aux_source_directory("./src" SRC_LIST)
# Add exectuteable
add_executable(${PROJECT_NAME} ${SRC_LIST})
target_link_libraries(point_group ${OpenCV_LIBS})
```

这里主要想说的是OpenCV如果作死不装在默认路径上（/usr/local/），需要指定.cmake 位置

有篇CMakeLists的博文（好像是7月5号更新的）里面写了指定${CMAKE_MODULE_PATH},

然后把.cmake copy 到工程根目录下

## C++数据结构 (*). == ->


### c++ 文件结构

在编程快结束后，博主感觉还是需要找到凸包上的点，于是在网上找了一个程序，
凸包查找（找到后附上链接），该链接将一些数学问题很好的整理，只要
include<cmath>即刻，但是存在复制的代码和自己的代码数据结构不同的情况。

此时新建一个.cpp和.h，将函数具体实现放在.cpp中并include**.h;
头文件和struct和class放在.h中，并

//#ifndef __hull__
//#define __hull__ #endif

在自己的.cpp(main(int argc,char** argv)在此)加上include ".h"


### 读取文件C++

      #include<fstream>
      // read-mode
      ifstream infile;
	 
	  infile.open("./1.txt");
	  string s;

	  // get a line
	  getline(infile, s);

### C++ split()

```
std::vector<std::string> split(const std::string& s, char seperator)
{
	std::vector<std::string> output;

	std::string::size_type prev_pos = 0, pos = 0;

	while ((pos = s.find(seperator, pos)) != std::string::npos)
	{
		std::string substring(s.substr(prev_pos, pos - prev_pos));

		output.push_back(substring);

		prev_pos = ++pos;
	}

	output.push_back(s.substr(prev_pos, pos - prev_pos)); // Last word

	return output;
}
```
### 遍历一个vector<double> v

```
for(auto iterator = v.begin(); iterator != v.end(); ++iterator)
{
		double a = *iterator
}
```

### 遍历一个vector< vector<double> > v

```
for(auto iterator = v.begin(); iterator != v.end(); ++iterator)
{
	double b = (*iterator)[3]//取每个vector中的第四个元素
	for(auto iter = (*iterator).begin(); iter != (*iterator).end(); ++iter)
	{
		double a = *iter//取每个vector中的每个vector的每个元素
    }
}
```

### 平面点集表示 vector<pair<double, double>> 

使用std::vector<string> point(每个string的
形式为 "3.5,12.3" 形式)填满point_num

```
for (vector<string>::iterator ite = point.begin(); ite != point.end(); ++ite)
	{
		// xy=split(*ite, ',');
		a = *ite;
		xy = split(a, ',');
		double x = atof(xy.at(0).c_str());
		double y = atof(xy.at(1).c_str());
		point_num.push_back(std::make_pair(x, y));

	}
```

```
遍历读取point_num的数字

for (auto iter1 = point_num.begin(); iter1 != point_num.end(); ++iter1)
	{
		double first_number = iter1->first
		double second_number = iter2->first
	}
```

### struct的遍历和表示

```
struct POINT
{
	double x;
	double y;
	bool value = false;
	POINT(double a = 0, double b = 0) { x = a; y = b; } //constructor   
};

POINT ch[100];
std::vector< pair<double,double> > point_hull;
...
得到一个ch，赋值的ch的value变为true，此时遍历ch，
注意此时的指针使用
for (auto ite = ch; ite->value != false; ++ite)
	{
		point_hull.push_back(std::make_pair(ite->x, ite->y));
		//make_pair 会将后面接的数字强制转化为double型
	}
```