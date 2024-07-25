---
title: Protobuf-Csharp
date: 2023-02-11 12:36:26
categories: Protobuf
tags: Csharp
---

## 学习环境

- VS or Rider
- Unity

## 步骤

分别下载：

- [GitHub - Protobuf](https://github.com/protocolbuffers/protobuf)

- [GitHub - Protobuf - Releases](https://github.com/protocolbuffers/protobuf/releases)

在Releases中找到C# 的Assets资源，根据不同平台下载对应的压缩包

### Google.Protobuf

- 使用Rider编辑器打开下载好的[GitHub - Protobuf](https://github.com/protocolbuffers/protobuf)中的**csharp/src/Google.Protobuf.sln**
- 随后点击Build->Builf Sulution生成项目
- 新建目录Libraries将上面生成的(路径：csharp\src\Google.Protobuf\bin\Debug\net45)文件夹中的所有内部一起导入unity中Libraries文件夹当中

### Protoc

- 解压Protoc文件夹

- 创建批处理文件

  createProto.bat和exportProto.bat。第一个为批量生成脚本和PB，第二个为查看日志

```
@echo off

set cSharpPath = protoc-22.0-rc-1-win64\bin\cSharpProto
set pbPath = protoc-22.0-rc-1-win64\bin\pbProto

if not exist %pbPath (
    md protoc-22.0-rc-1-win64\bin\pbProto
) else (
    echo pbProto is Exist
)

if not exist %cSharpPath (
    md protoc-22.0-rc-1-win64\bin\cSharpProto
) else (
    echo cSharpProto is Exist
)

cd protoc-22.0-rc-1-win64\bin
for %%i in (*.proto) do (
    protoc --csharp_out=./ %%i
    protoc -o %%~ni.pb %%i    
    echo From %%i To %%~ni.cs Successfully!
    echo From %%i To %%~ni.pb Successfully!
    move %%~ni.cs cSharpProto
    move %%~ni.pb pbProto
)
```

```
@echo off

setlocal enabledelayedexpansion 
set curPath=I:\protobuf\protobufCreateCSharpSourcePackages
cd /d %curPath%
call createProto.bat
pause
```

- 创建需要的proto将其放到protoc.exe同目录下（bin） 
- 随后将生成的C# 脚本导入unity即可使用