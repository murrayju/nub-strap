# nub-strap

Use [nub](https://nubjs.com) in your project without installing globally.

## Why

[nub](https://nubjs.com) makes it easier to work with modern typescript projects using node.js as the runtime. For many projects, it is important to ensure that all developers (and production) use the same version of the runtime to avoid inconsistencies and bugs. This can also lead to conflicts between different projects on the same machine requiring different versions. This script provides a simple solution.

This also enables bootstrapping truly zero-dependency development environments, that work regardless of globally installed runtimes.

## How it works

The script checks if nub is installed in a local `download` directory. If not, it downloads and installs the specified version of nub there. Finally, it runs nub with any provided arguments. The result is a transparent proxy to the desired version of nub.

When a specific version is requested, the installed binary's version is compared against it and nub is reinstalled if they differ. When the version is `latest` (the default), any already-installed binary is used as-is, so no version check or download happens. To pick up a newer release, delete the `download/nub` directory (or pin an explicit version).

## Usage

1. Copy the `nub` script to your project directory
2. Ensure it is executable: `chmod +x nub`
3. Run nub commands using `./nub <command>`, e.g., `./nub install`

## Use a specific nub version

The default behavior is to install the current latest version of nub. For most projects, we recommend pinning to a specific version by editing the `version` variable in the script or setting the `NUB_VERSION` environment variable before running the script.

Example using the environment variable:

```bash
NUB_VERSION="0.6.0" ./nub --version
# 0.6.0
```

## Optional alias

To simplify usage, you can create a function in your shell configuration (e.g., `.bashrc`, `.zshrc`). This falls back to using the globally installed nub if no local version is defined.

```bash
nub() {
  if [ -x ./nub ]; then
    ./nub "$@"
  else
    command nub "$@"
  fi
}
```
