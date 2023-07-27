---
title: "ESP-IDF 最小项目"
description: "编程第一步，Hello World"
date: 2023/03/27 20:52:03
abbrlink: 1eb238e0
#image: "cover.png"
tags: [ESP32C3, ESP-IDF]
categories: ESP32C3
---

## 创建工程

- 文件结构:

```bash
ESP32C3
├── CMakeLists.txt
└── main
    ├── CMakeLists.txt
    └── main.c
```

- `CMakeLists.txt`

```cmake
cmake_minimum_required(VERSION 3.16)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(ESP32C3_Minimal)
```

- `main/CMakeLists.txt`

```cmake
idf_component_register(SRCS "main.c" INCLUDE_DIRS "")
```

- `main/main.c`

```c
#include <stdio.h>
#include "sdkconfig.h"

void app_main(void)
{
    printf("Hello world!\n");
    fflush(stdout);
}
```

## 工程配置与烧写测试

```bash
idf.py set-target esp32c3
idf.py menuconfig
idf.py build
idf.py flash monitor
```