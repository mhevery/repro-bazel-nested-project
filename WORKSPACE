workspace(
    name = "parent",
    managed_directories = {
        "@npm": ["node_modules"],
    },
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "c077680a307eb88f3e62b0b662c2e9c6315319385bc8c637a861ffdbed8ca247",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/5.1.0/rules_nodejs-5.1.0.tar.gz"],
)

load("@build_bazel_rules_nodejs//:repositories.bzl", "build_bazel_rules_nodejs_dependencies")

build_bazel_rules_nodejs_dependencies()

# fetches nodejs, npm, and yarn
load("@build_bazel_rules_nodejs//:index.bzl", "node_repositories", "yarn_install")
node_repositories()

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
