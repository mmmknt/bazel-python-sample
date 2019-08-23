# bazel-python

## How to build

```
bazel build //mysite:mysite
```

## How to execute

```
bazel-out/darwin-fastbuild/bin/mysite/mysite runserver
```

## How to execute on image

```
bazel run //mysite:mysite_image
```

## How to push image

Change `mysite/BUILD.bazel` and execute bazel.

```
bazel run //mysite:push
```

