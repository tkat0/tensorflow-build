# tensorflow-build

build c api.

## Setup

```bash
git clone git@github.com:tensorflow/tensorflow.git
cd tensorflow
git checkout 3727302f03a495b665efda24a845c4b30a7cffe9
```

```bash
commit 3727302f03a495b665efda24a845c4b30a7cffe9 (HEAD -> master, origin/master, origin/HEAD)
Author: A. Unique TensorFlower <gardener@tensorflow.org>
Date:   Sun Dec 13 10:18:02 2020 -0800

    Removing run_deprecated_v1 and run_v1_only tags on tests for array_ops
    
    PiperOrigin-RevId: 347267934
    Change-Id: Id8d1edfa7ff51c1ba3d9fc496d51a981a19c2ba9
```

## Build

```bash
docker-compose build android

cd tensorflow

# set manualy
./configure

# android: libtensorflowlite_c.so
# config -> see tensorflow/tensorflow/BUILD
bazel build -c opt --config=android_arm64 \
  --define tflite_with_xnnpack=true \
  //tensorflow/lite/c:tensorflowlite_c

# android: libtensorflowlite_gpu_gl.so
bazel build -c opt --config=android_arm64 \
  --define tflite_with_xnnpack=true \
  //tensorflow/lite/delegates/gpu:libtensorflowlite_gpu_gl.so

# x86-linux: libtensorflowlite_c.so
bazel build -c opt --config=linux_x86_64 \
  --define tflite_with_xnnpack=true \
  //tensorflow/lite/c:tensorflowlite_c

cp bazel-out/arm64-v8a-opt/bin/tensorflow/lite/c/libtensorflowlite_c.so ../lib/android
cp bazel-out/arm64-v8a-opt/bin/tensorflow/lite/delegates/gpu/libtensorflowlite_gpu_gl.so ../lib/android
cp bazel-out/k8-opt/bin/tensorflow/lite/c/libtensorflowlite_c.so ../lib/linux
```