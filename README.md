# Intro

go114-fuzz-build is a mostly-drop-in replacement for
[go-fuzz-build](github.com/dvyukov/go-fuzz-build)'s `-libfuzzer` build mode, but uses
cmd/compile's native libfuzzer instrumentation (included
experimentally in Go 1.14) instead of source-to-source transformation.

# Example

1. Install go114-fuzz-build:

``` sh
$ go get -u github.com/mdempsky/go114-fuzz-build
```

2. Checkout and build latest Go pre-release version:

``` sh
$ git clone https://go.googlesource.com/go go-wip
$ cd go-wip/src
$ ./make.bash
```

3. Build Kubernetes fuzz target with go114-fuzz-build and link against libFuzzer:

``` sh
$ git clone --depth=1 git clone --depth 1 https://github.com/kubernetes/kubernetes.git
$ cd kubernetes
$ PATH=path/to/go-wip/bin:$PATH go114-fuzz-build -o yaml_FuzzSigYaml.a -func FuzzSigYaml ./test/fuzz/yaml
$ clang -o yaml_FuzzSigYaml yaml_FuzzSigYaml.a -fsanitize=fuzzer
```
