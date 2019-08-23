### docker
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "e513c0ac6534810eb7a14bf025a0f159726753f97f74ab7863c650d26e01d677",
    strip_prefix = "rules_docker-0.9.0",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.9.0/rules_docker-v0.9.0.tar.gz"],
)
load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load(
    "@io_bazel_rules_docker//python3:image.bzl",
    _py_image_repos = "repositories",
)

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_pull(
    name = "py3_base",
    registry = "index.docker.io",
    repository = "library/python",
    tag = "3.6",
)

_py_image_repos()

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

register_toolchains("//:python_toolchain")

git_repository(
    name = "rules_python",
    remote = "https://github.com/mmmknt/rules_python.git",
    # NOT VALID!  Replace this with a Git commit SHA.
    commit = "c87051f901d2b03188546f1e79e02e396e9db927",
)

# Only needed for PIP support:
load("@rules_python//python:pip.bzl", "pip_repositories", "pip_import")

pip_repositories()

# This rule translates the specified requirements.txt into
# @my_deps//:requirements.bzl, which itself exposes a pip_install method.
pip_import(
    name = "mysite_deps",
    requirements = "//:requirements.txt",
)

load("@mysite_deps//:requirements.bzl", mysite_deps_install = "pip_install")

mysite_deps_install()

