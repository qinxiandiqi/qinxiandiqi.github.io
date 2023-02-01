---
# type: posts 
title: "C语言的fopen文件操作相对路径到底相对哪？"
date: 2023-02-02T00:13:45+08:00
authors: ["Jianan"]
summary: 
series: []
categories: []
tags: []
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: true
read_num: 0
comment_num: 0 
---

```c
#include <errno.h>
#include <stdio.h>
#include <string.h>

int main() {
  FILE* pFile;
  pFile = fopen("data/hello.txt", "r");
  if (pFile == NULL) {
    int errorCode = errno;
    char* errorMsg = strerror(errorCode);
    printf("Open file fail, errorCode:%d, errorMsg:%s\n", errorCode, errorMsg);
    return -1;
  }

  int c;
  while (1) {
    c = fgetc(pFile);
    if (feof(pFile)) {
      printf("\n");
      break;
    }
    printf("%c", c);
  }
}
```

