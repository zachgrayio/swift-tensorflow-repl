# swift-tensorflow-repl

Dockerized [Swift for TensorFlow](https://github.com/tensorflow/swift) REPL based on [swift-tensorflow](https://github.com/zachgrayio/swift-tensorflow). This image is available now on Docker Hub at `zachgray/swift-tensorflow-repl:4.2`.

## Motivation

With this container, you can run the Swift for TensorFlow REPL with a single command.

## Overview

This image will allow you to easily take the official [Swift for TensorFlow](https://github.com/tensorflow/swift) for a test drive without worrying about installing dependencies, changing your path, and interfering with your existing Swift/Xcode config.

## Run

*Note: when running this interactive container with the standard `-it`, we also must [run without the default seccomp profile](https://docs.docker.com/engine/security/seccomp/) with `--security-opt seccomp:unconfined` to allow the Swift REPL access to `ptrace` and run correctly.*

#### Run the `swift-tensorflow-repl` container:

```bash
docker run --rm --security-opt seccomp:unconfined -it zachgrayio/swift-tensorflow-repl:4.2
```

#### Observe the output:
```
Welcome to Swift version 4.2-dev (LLVM 04bdb56f3d, Clang b44dbbdf44). Type :help for assistance.
  1> 
```

#### Interact with TensorFlow:

```
  1> import TensorFlow
  2> var x = Tensor([[1, 2], [3, 4]])
2018-04-27 04:30:17.505272: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
x: TensorFlow.Tensor<Double> = [[1.0, 2.0], [3.0, 4.0]]
  3> x + x
$R0: TensorFlow.Tensor<Double> = [[2.0, 4.0], [6.0, 8.0]]
  4> :exit
```

## Run with Volume

To do real work with TensorFlow, you'll likely want to access your local disk. This is accomplished easily in Docker by mounting your current working directory into the container.

#### Run the `swift-tensorflow-repl` container:

```bash
docker run --rm --security-opt seccomp:unconfined -itv ${PWD}:/usr/src \
    zachgrayio/swift-tensorflow-repl:4.2
```

Files in your current working directory are now available; the file `file.txt` will be accessible at path `/usr/src/file.txt`.

## Create a run script (optional)

```bash
nano tfrepl.sh
```

Paste:

```bash
#!/usr/bin/env bash
docker run --rm --security-opt seccomp:unconfined -itv ${PWD}:/usr/src \
    zachgrayio/swift-tensorflow-repl:4.2
```

- Press Ctrl + O
- Press ENTER
- Press Ctrl + X

```bash
chmod +x ./tfrepl.sh
```

Run with `./tfrepl.sh`.


## Advanced Usage

You should make use of the parent image [swift-tensorflow](https://github.com/zachgrayio/swift-tensorflow) and it's deeper documentation for advanced use cases such as:

* [Compiling executables](https://github.com/zachgrayio/swift-tensorflow#run-the-compiler)
* [Passing scripts to the interpreter](https://github.com/zachgrayio/swift-tensorflow#run-the-interpreter)
* [Importing third-party libraries](https://github.com/zachgrayio/swift-tensorflow#run-with-dependencies-advanced) such as [RxSwift](https://github.com/ReactiveX/RxSwift) into your REPL session
* Creating and building SPM projects

## License

This project is [MIT Licensed](https://github.com/zachgrayio/swift-tensorflow-repl/blob/master/LICENSE).