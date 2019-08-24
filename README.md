# bazel-python
## Application
Djangoのこのドキュメントを上からいくつかやった状態。
https://docs.djangoproject.com/ja/2.2/intro/tutorial01/

## Execution
### How to build

```
bazel build //mysite:mysite 8080
```

### How to execute

```
bazel-out/darwin-fastbuild/bin/mysite/mysite runserver
```

### How to execute on image

```
bazel run //mysite:mysite_image runserver 0.0.0.0:8080
```

### How to push image

Change `mysite/BUILD.bazel` and execute bazel.

```
bazel run //mysite:push
```

## 所感
* requirements.txtを元にdependenciesを定義できるから、dependency managementはgazelleを使うより楽そう。
```
pip_import(
    name = "mysite_deps",
    requirements = "//:requirements.txt",
)

load("@mysite_deps//:requirements.bzl", mysite_deps_install = "pip_install")

mysite_deps_install()
```

* gcrにpushするのにpython2が必要だけどアプリがpython3じゃないといけなかったりしてて、pythonつらい。toolchainでいい感じに設定できるらしいけど、いまいち動きが分かってない。
```
load("@bazel_tools//tools/python:toolchain.bzl", "py_runtime_pair")

py_runtime(
    name = "python-3.6.8",
    interpreter_path = "~/.pyenv/versions/3.6.8/bin/python3",
    python_version = "PY3",
)

py_runtime(
    name = "python-2.7.16",
    interpreter_path = "~/.pyenv/versions/2.7.16/bin/python2",
    python_version = "PY2",
)

py_runtime_pair(
    name = "py_runtime_pair",
    py2_runtime = ":python-2.7.16",
    py3_runtime = ":python-3.6.8",
)

toolchain(
    name = "python_toolchain",
    toolchain = ":py_runtime_pair",
    toolchain_type = "@bazel_tools//tools/python:toolchain_type",
)
```
* デフォルトだとpip3が動かないっぽい。python3必須のアプリ作る時にはどうにかしないとダメっぽい。
https://github.com/mmmknt/rules_python/commit/c87051f901d2b03188546f1e79e02e396e9db927
* py3_imageで作成したimageでは、 `/usr/bin/python` がentry pointになってるので、このpathにpythonがないと動かない。ベースイメージを指定する時には気をつけよう。
  python:3.6-slimはこのpathにpythonがないからエラーになる。
https://github.com/bazelbuild/rules_docker/blob/master/python3/image.bzl#L104

