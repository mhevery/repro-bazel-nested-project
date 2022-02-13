workspace(
    name = "parent",
    managed_directories = {
        "@npm": ["node_modules"],
    },
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "d63ecec7192394f5cc4ad95a115f8a6c9de55c60d56c1f08da79c306355e4654",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/4.6.1/rules_nodejs-4.6.1.tar.gz"],
)

load("@build_bazel_rules_nodejs//:index.bzl", "node_repositories", "yarn_install")
node_repositories(
    node_version = "16.6.2",
)

yarn_install(
    name = "npm",
    package_json = "//:package.json",
    yarn_lock = "//:yarn.lock",
)

#####################
##   child
#####################

local_repository(
    name = "child",
    path = "./child",
    repo_mapping = {
        "@npm": "@npm_child",
        "@npm_core": "@npm_child_core",
    },
)

yarn_install(
    name = "npm_child",
    package_json = "@child//:package.json",
    yarn_lock = "@child//:yarn.lock",
)

yarn_install(
    name = "npm_child_core",
    package_json = "@child//packages/core:package.json",
    yarn_lock = "@child//packages/core:yarn.lock",
)

#####################
##   /child
#####################
