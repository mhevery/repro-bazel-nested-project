load("@npm_core//@bazel/typescript:index.bzl", "ts_project")

package(default_visibility = ["//visibility:public"])

ts_project(
    name = "core",
    srcs = glob(
        [
            "*.ts",
        ],
    ),
    declaration = True,
    preserve_jsx = True,
    tsconfig = "//:tsconfig.json",
    deps = [
        "@npm_core//:node_modules",
    ],
)
