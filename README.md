# Caffe2 AICamera tutorial example

This is an example for using Caffe2 on Android. See [the original README](README.orig.md) for details.

## Using caffe2 from PyTorch master

Newer android seems to prefer clang, at least I couldn't find the gnu stl lib.

To compile libs for armeabi-v7a I used
```
ANDROID_NDK=~/Android/Sdk/ndk-bundle/ 
./scripts/build_android.sh -DANDROID_TOOLCHAIN=clang
```

Note: On OS X, this could be /Users/<username>/Library/Android/sdk/ndk-bundle instead.

For x86 (useful to debug android apps) I needed to disable AVX. To do this, I inserted `if (NOT DISABLE_AVX)` and `endif()` before and after the AVX check in cmake/MiscCheck.cmake.

I then built with
```
ANDROID_NDK=~/Android/Sdk/ndk-bundle/ BUILD_ROOT=$(pwd)/build_android_x86   
./scripts/build_android.sh -DANDROID_TOOLCHAIN=clang  -DANDROID_ABI=x86 -DDISABLE_AVX=1
```

Then I copied the resulting `build_android*/lib/lib*` into the corresponding `x86` and `armeabi-v7a` subdirectories of `app/src/main/jniLibs`.

You would likely want to replace the headers in app/src/main/cpp with those from torch/lib/include or so (possibly after building PyTorch).
