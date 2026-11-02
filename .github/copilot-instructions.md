# Project Instructions

## Project

- Beskin is a Qt 6 widget style and settings utility, with a KDE KDecoration3 window decoration.
- The style and KDE configuration module are separate Qt plugins. Keep the installed `beskin.themerc`'s `ConfigPage` aligned with the installed module path and test that the module loads when changing either.
- Follow the existing C++17, Qt, and CMake patterns. Keep changes focused and preserve unrelated user changes.

## Build and test

- Configure with `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release`; use `-DBUILD_KWIN_DECORATION=OFF` only when KDecoration3 is unavailable and the change does not require that plugin.
- Build with `cmake --build build --parallel 2` and run `ctest --test-dir build --output-on-failure`.
- Before packaging on Arch Linux, cap the whole build at 6 GiB and disable swap: `systemd-run --user --scope -p MemoryMax=6G -p MemorySwapMax=0 makepkg -f --noconfirm`. For installation, use `-si` instead of `-f --noconfirm` as appropriate.
- The PKGBUILD already limits compilation to two jobs and runs CTest. Verify package contents when changing install rules, plugin metadata, or dependencies.
- Arch packages must be named `beskin` and `beskin5`, keep Beskin plugin paths distinct from Breeze, and be checked for file conflicts with the installed `breeze` package.
- When changing build or packaging behavior, update the relevant CMake/PKGBUILD configuration and user-facing instructions together.
- Bump the version in `CMakeLists.txt` and `PKGBUILD` together, and update the changelog in `PKGBUILD` with a short summary of user-facing changes. Especially before making  packages.
- Remove any unused build or packaging dependencies, and verify that the package still builds and runs.
- Keep the code clean, and up to the kde coding standards. Run `clang-format` and `clang-tidy` on changed files, and fix any warnings or errors.

## Project-specific conventions

- Keep Qt style plugins, KDE style-config plugins, and KDecoration3 plugins in their appropriate Qt/KDE plugin directories; check the staged package for expected files.
- When changing build or packaging behavior, update the relevant CMake/PKGBUILD configuration and user-facing instructions together.
- Add or update a focused smoke test for plugin loading or metadata changes, then run the narrowest applicable build and test commands.
