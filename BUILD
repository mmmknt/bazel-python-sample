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