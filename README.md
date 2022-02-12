# Bazel rules_nodejs issue reproduction

Steps to reproduce:

```
cd child
bazel build packages/core
```

Above succeeds. This proves that the `./child` `local_repository` is correctly configured.

Next:

```
cd ..
bazel build @child//packages/core
```

The above command fails with:

````
INFO: Analyzed target @child//packages/core:core (0 packages loaded, 0 targets configured).
INFO: Found 1 target...
ERROR: /private/var/tmp/_bazel_misko/7cb4da9d45aa96b44a2c9ccbd78ae6c8/external/child/packages/core/BUILD:5:11: Compiling TypeScript project @child//packages/core:core [tsc -p ../child/tsconfig.json] failed: (Exit 2): tsc.sh failed: error executing command bazel-out/darwin_arm64-opt-exec-2B5CBBC6/bin/external/npm_child/typescript/bin/tsc.sh --project external/child/tsconfig.json --outDir bazel-out/darwin_arm64-fastbuild/bin/external/child/packages/core ... (remaining 5 arguments skipped)

Use --sandbox_debug to see verbose messages from the sandbox
external/child/packages/core/main.ts(1,19): error TS2307: Cannot find module 'is-odd' or its corresponding type declarations.
Target @child//packages/core:core failed to build
Use --verbose_failures to see the command lines of failed build steps.
INFO: Elapsed time: 1.287s, Critical Path: 0.56s
INFO: 2 processes: 2 internal.
FAILED: Build did NOT complete successfully```
````

The core issue is this:

```
external/child/packages/core/main.ts(1,19): error TS2307: Cannot find module 'is-odd' or its corresponding type declarations.
```

It seems that the typescript compiler can't find `node_modules` to resolve `is-odd`.

Running with `--sandbox_debug` reveals this layout:

```
execroot/parent
  - bazel-out
  - external
    - child
      - tsconfig.json
      - packages
        - core
          - main.ts
  - node_modules <= @npm
    - typescript
  - packages
    - core
      - node_modules <= @npm_child_core
        - is-odd
```

The core issue is that from typescript perspective the `node_module` which contains `is-odd` is buried in such a way that it can't be found. I am not aware of any way one could update `tsconfig.json` so that it can find the buried `node_module` with `is-odd`
