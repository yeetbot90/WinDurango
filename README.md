# WinDurango

> [!IMPORTANT]
> We are rewriting WinDurango, due to many of the old codebase's flaws coming to light.

## Building
```
git clone https://codeberg.org/WinDurango/WinDurango.git

vcpkg install

mkdir build && cd build

cmake ..

cmake --build .
```

## Releases

WinDurango includes automated build and release workflows on GitHub Actions.

- **Build validation** runs on push, pull request, and manual dispatch for both `x64-windows` and `x64-windows-static` triplets.
- **Release publishing** is triggered by tags in the form `v*` (for example, `v0.1.0`) and creates per-triplet zip packages.
- **Manual release workflow runs** upload zip artifacts even when no tag is present, so maintainers can test packaging before a real release.

The release workflow builds in `Release` mode with Ninja and vcpkg, then publishes release assets on tagged runs.

