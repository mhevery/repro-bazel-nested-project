workspace(
    name = "parent",
    managed_directories = {
        "@npm": ["node_modules"],
    },
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "6b951612ce13738516398a8057899394e2b7a779be91e1a68f75f25c0a938864",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/5.0.0/rules_nodejs-5.0.0.tar.gz"],
)

load("@build_bazel_rules_nodejs//:repositories.bzl", "build_bazel_rules_nodejs_dependencies")
build_bazel_rules_nodejs_dependencies()

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
