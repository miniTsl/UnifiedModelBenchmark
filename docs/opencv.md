# opencv编译

## opencv for android
```bash
# 下载opencv 4.7.0
export https_proxy="http://172.16.101.124:7890"
wget https://codeload.github.com/opencv/opencv/zip/refs/tags/4.7.0
```
### 环境准备
#### 环境
```bash
# 可能需要java环境
apt install openjdk-11-jdk
# 编译需要python3
apt install python3.8
ln -s /usr/bin/python3.8 /usr/bin/python3
# cmake环境
wget https://github.com/Kitware/CMake/releases/download/v3.26.0/cmake-3.26.0-linux-x86_64.sh
# 配置环境变量即可...
```

#### 安装Android SdkManager
1. 进入[Android Studio 网址](https://developer.android.com/studio),下载"Command line tools only"
```bash
wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip
unzip commandlinetools-linux-9477386_latest.zip
cp cmdline-tools/ ~/android_sdk



# 安装 android 环境
sdkmanager --install "platforms;android-29"
sdkmanager --install "ndk;25.0.8775105"
```

### 编译脚本
```bash
unzip opencv-4.7.0.zip
cd opencv-4.7.0/
mkdir build
cd build/
```

```bash
# 环境变量
export https_proxy="http://172.16.101.124:7890"
export ANDROID_NDK=/root/android_sdk/ndk/25.0.8775105
# cmake命令
cmake -DCMAKE_TOOLCHAIN_FILE=$ANDROID_NDK/build/cmake/android.toolchain.cmake \
-DCMAKE_ANDROID_NDK=ANDROID_NDK \
-DANDROID_NATIVE_API_LEVEL=29 \
-DBUILD_ANDROID_PROJECTS=OFF \
-DBUILD_ANDROID_EXAMPLES=OFF \
-DCMAKE_BUILD_TYPE=Release  \
-DBUILD_JAVA=OFF  \
-DANDROID_ABI=arm64-v8a \
-DANDROID_STL=c++_shared \
-DBUILD_SHARED_LIBS=ON \
-DCMAKE_INSTALL_PREFIX=/workspace/opencv-4.7.0/build/install ..
# 编译好的库再install目录里面
make -j8
make install
```