---
title: C模拟实现类
date: '2017-11-18'
tags:
  - cpp
  - algorihm
---
# 由头

之前在《深入理解PHP内核》一书中看到了用函数作为结构体的属性，想到用　C　语言来模拟实现面对对象。

## 具体实现

```c
#include <stdio.h>

typedef struct Class {
    int _param;
    void ( * _construct )();
    void ( * function )();
    void ( * _destory )();
} Class, *Instance;


void init( Instance this, int param ) {
    this->_param = param;
    printf("this is construct function\n");
}

void doSomething(Instance this) {
    printf("this is a function called %d\n", this->_param);
}

void destory(Instance this) {
    this->_param = 0;
    printf("this is detory function\n");
}

int main() {
    struct Class Object = { 233, init, doSomething, destory };

    Object._construct( &Object, 666 );
    Object.function(&Object);
    Object._destory(&Object);
    return 0;
}
```
## 解释

### ＂方法＂
主要的一个知识点就是 C 语言中，函数是可以已指针的形式被传递（**函数指针**）．

比如：

```c
int* sort(int arr[], int *compare( int pre, int next ) ) {
    for( int index = 0; index < len(arr) - 1; ++ index ) {
        if( compare( arr[index], arr[index+1] ) > 0 ) {
            swap( &arr[index], &arr[index+1] );
        }
    }
}
```

这里就是把一个比较函数`compare()`传入`sort()`函数里面．实现了自定义排序过程．这样的做法，动态语言里面比较常见．

＂模拟类＂也是同样的原理．我们把结构体的属性设为函数指针，结构体通过访问函数指针属性来调用函数，由此来模拟＂方法＂．


### "this"

实际上，纯　C 实现＂类＂内部实现`this`是很不现实的．早期的　C++ 没有底层实现 this 的指向，而是将　C++ 代码编译成　C ，在这个过程中使用了一点小把戏：在编译成　C 代码的过程中，给所有的类的＂方法＂的参数列表添加一个参数，而这个参数就是指向实例的一个指针．

## 总结

C 语言，最为强大之处莫过于它的指针了．教材所学，它的一点精髓都没有，同志任需努力呀！
