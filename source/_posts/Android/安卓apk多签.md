---
title: Android打包
categories: Unity
date: 2025-7-07 11:50:08
tags: Android
---

## 环境

- SDK

## 操作步骤

- 准备新旧证书（`keystore` 或者`jks`），`Key store password`;`Key alias`;`Key password`;应用包名。
- 创建沿袭签名，此时文件夹中会生成一个`lineagefile`文件。（apksigner.bat为SDK 文件夹重的批处理执行程序）

```
apksigner.bat rotate --out lineagefile --new-signer --ks a.jks --old-signer --ks b.jks
```

- 生成共同签名的APK

```
@REM @echo off
chcp 65001
setlocal enabledelayedexpansion

:: 切换到当前脚本所在目录
cd /d "%~dp0"

:: 检查是否拖拽了一个文件
if "%~1"=="" (
    echo 请拖拽一个.apk文件到此批处理上
    pause
    exit /b 1
)

:: 获取拖拽的文件路径
set "apk_file=%~1"

:: 检查文件是否为 xxx_signed.apk
if /i not "%apk_file:~-4%"==".apk" (
    echo 文件必须是.apk文件
    pause
    exit /b 1
)

echo 开始执行多签操作，请等待...

:: 执行签名命令 (34.0.0\apksigner.bat 为批处理文件)(** 填写对应证书信息)
34.0.0\apksigner.bat sign --lineage lineagefile --rotation-min-sdk-version 24 --ks a.jks --ks-key-alias ** --ks-pass pass:** --next-signer --ks b.jks --ks-key-alias ** --ks-pass pass:** --v1-signing-enabled true --v2-signing-enabled false --v3-signing-enabled true %apk_file%

if %errorlevel% neq 0 (
    echo 签名失败，请检查配置
    pause
    exit /b 1
)

echo 签名成功！
pause
```

## 验证

```
keytool --print-cert -jarfile J:\launcher-release.apk
```

```
apksigner.bat verify --print-certs J:\launcher-release.apk
```

使用上面的命令行即可验证证书

## 导出`cer`公钥证书

```
keytool -export -alias 包名 -keystore keystore名称带后缀 -rfc -file test.crt
```

