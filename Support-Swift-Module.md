# 支持Swift组件化（基于Cocoapods)

## Swift组件结构

### Swift native module

- swiftmodule  
  
  二进制定义的swift公开接口

- swiftdoc  

    二进制化的swift公开接口文档

- swiftinterface

    文本定义的swift公开接口，用来保证模块格式稳定性(Module Format Stability)

如果Swift组件包含ObjC代码，还将包含Clang Module的描述结构

### Clang Module

关于Clang Module的详细介绍，参见附录2。
关于Clang module和Swift module的比较， 参见附录3.

- modulemap

    定义Objc公开头文件到模块逻辑结构的映射关系

- umbrella header

    包含所有允许外部访问的Objc公开头文件

如果Swift接口需要暴露给外部ObjC使用，还包含

### Product-Swift.h

定义允许Objc访问的Swift桥接类，编译时自动生成

## 各种形式组件的区别

|      |  Swift Module  | Swift+ObjC Module |  ObjC Module | 不适用Module的Objc组件 |
| ---- | ------- | ------ | ------- | ------ |
| 打包格式 |  XCFramework | XCFramework | static .a | static .a |
| 接口定义 | Swift native module, Product-Swift.h (如有给ObjC引用的接口） | Swift native module, Clang module |  Clang module | C/C++ public headers |

## Swift组件化的几大问题

### 源码编译

### 静态库编译

### Gundam对组件化的支持

### Swift组件与其他组件的互操作

首先回顾下Swift组件的元素，`ProductName.modulemap` 包含模块子模块定义，并且引入伞型文件`ProductName.h`， `ProductName.h`包含所有允许外部访问的公开头文件，`ProductName-Swift.h`包含允许OBJC访问的Swift类桥接定义，
它是在Swift模块编译过程中生成的中间文件。

- 外部组件Swift文件访问Swift
通过`import ProductName`引用modulemap里的swift module

- 外部组件ObjC文件访问Swift
通过 `#import <ProductName-Swift.h>`访问Swift类桥接定义

- 外部组件Swift文件访问Objc
通过`@import ProductName`来访问modulemap引用的伞型文件`ProductName.h`包含的ObjC文件，

- 外部组件ObjC文件访问Objc
如果该模块Enable Module, 和Swift文件访问Objc一样，通过 `@import <ProductName>`访问Obj文件
如果该模块没有Enable Module，通过`#import <ProductName.h>`，来访问Objc文件

## References

1. https://medium.com/@mail2ashislaha/swift-objective-c-interoperability-static-libraries-modulemap-etc-39caa77ce1fc
2. https://clang.llvm.org/docs/Modules.html
3. https://forums.swift.org/t/module-system/144
   