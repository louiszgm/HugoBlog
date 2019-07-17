+++
title = "Keyboard Setting"
lastmod = 2019-07-17T17:43:20+08:00
draft = false
weight = 2001
author = "louiszgm"
+++

先用一下命令查询键盘设备的ID

```shell
xinput list
```


## 交换Win键和Alt键的位置 {#交换win键和alt键的位置}

```shell
setxkbmap -option altwin:swap_alt_win -device $DEVICE_ID
```


## CapsLock 和 Ctrl调换(适用于Emacs的。Mac上可以设置，Windows上和Linux上就需要用这个命令了，除非键盘自身支持，比如HHKB) {#capslock-和-ctrl调换--适用于emacs的-mac上可以设置-windows上和linux上就需要用这个命令了-除非键盘自身支持-比如hhkb}

```shell
setxkbmap -option ctrl:nocaps -device $DEVICE_ID
```
