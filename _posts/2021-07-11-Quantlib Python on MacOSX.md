---
title: Quantlib Python on MacOSX
author: Flowerowl
layout: post
permalink: /4060.html
views:
  - 667
categories:
  - note
tags:
  - note
---

### 整体流程：

1. 安装quantlib

2. 重编译quantlib-swig

3. 定制 quantlib-swig

4. 重编译quantlib-swig

5. 安装quantlib

源码下载：https://www.quantlib.org/download.shtml

### 1. Quantlib编译安装

boost/swig安装可以brew安装或者编译安装

./configure --with-boost-include=/usr/local/include/ \
        --with-boost-lib=/usr/local/lib/ --prefix=/usr/local/ \
        CXXFLAGS='-O2 -std=c++11 -stdlib=libc++ -mmacosx-version-min=10.9' \
        LDFLAGS='-stdlib=libc++ -mmacosx-version-min=10.9'

make

sudo make install

### 2. Quantlib-SWIG重编译

xcode-select --install

export CXXPATH=/Library/Developer/CommandLineTools/usr/include/c++/v1

export LDFLAGS=-L/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib

./configure --with-boost-include=/usr/local/include/ \
        --with-boost-lib=/usr/local/lib/ --prefix=/usr/local/ \
        CXXFLAGS='-O2 -std=c++11 -stdlib=libc++ -mmacosx-version-min=10.9' \
        LDFLAGS='-stdlib=libc++ -mmacosx-version-min=10.9'

make -C Python

make check

sudo make install

###	3. 定制.i文件

### 4. 重编译quantlib-swig

	make clean -C Python

	make -C Python

### 5. 安装quantlib python版本

	make -C Python install

