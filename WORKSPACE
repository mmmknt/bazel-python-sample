load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

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
