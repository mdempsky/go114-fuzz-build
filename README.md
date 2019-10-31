# Intro

go114-fuzz-build is a mostly-drop-in replacement for
github.com/dvyukov/go-fuzz-build's -libfuzzer build mode, but uses
cmd/compile's native libfuzzer instrumentation (expected for Go 1.14)
instead of source-to-source transformation.

# Example

1. Install go114-fuzz-build:

$ go get github.com/mdempsky/go114-fuzz-build

2. Checkout and build Go with patch set 5 of https://go-review.googlesource.com/c/go/+/203887:

$ git clone https://go.googlesource.com/go go-wip
$ cd go-wip
$ git fetch origin refs/changes/87/203887/5
$ git checkout FETCH_HEAD
$ cd src
$ ./make.bash

3. Build Kubernetes fuzz target with go114-fuzz-build and link against libFuzzer:

$ git clone --depth=1 git clone --depth 1 https://github.com/kubernetes/kubernetes.git
$ cd kubernetes
$ PATH=path/to/go-wip/bin:$PATH go114-fuzz-build -o yaml_FuzzSigYaml.a -func FuzzSigYaml ./test/fuzz/yaml
$ clang -o yaml_FuzzSigYaml yaml_FuzzSigYaml.a -fsanitize=fuzzer
