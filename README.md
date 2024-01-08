# bazel-registry

Bazel registry for WFA projects. See https://bazel.build/external/registry

## Adding a module version

This can be partially automated using the `add_module.py`
[tool from the Bazel Central Registry](https://github.com/bazelbuild/bazel-central-registry/tree/main/tools).
If doing updates manually, the `calc_integrity.py` tool can be used to calculate
integrity values.

`MODULE.bazel` files from module repositories typically do not specify the
correct module version. The common practice is to modify this file to add the
version when adding it to the registry, adding a patch file for the
modification.

### Using `add_module.py`

1.  Copy the `MODULE.bazel` for the module, specifying the version.
2.  Run the tool, following the input prompts.
    *   Use the file from (1) when prompted.
    *   Choose No for the remaining Yes/No prompts.
    *   When asked for build targets, enter any string as the WFA registry does
        not use this.
    *   Warnings related to BCR maintainer review and use of GitHub source
        archives can be ignored.
3.  Create a pull request for the generated files, excluding files that are
    ignored by `.gitignore`.

If you get an error related to `presubmit.yml` not being found, you can add an
empty file to the specified location and re-run the tool specifying the
generated `.json` file using the `--input` option.

### Manual testing

Update a project that depends on the module to use the newly-added version.
Build the project with Bazel, using the `--registry` option to specify the local
clone of the registry using the `file://` scheme. For example,

```shell
bazel build --registry="file://$HOME/git/bazel-registry" //...
```
