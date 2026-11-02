# Maintainer: Beskin contributors

pkgbase=beskin
pkgname=(beskin beskin5)
pkgver=6.8.80.r3418.g56088368
pkgrel=3
arch=(x86_64)
pkgdesc='Beskin widget style and window decoration based on Breeze'
url='https://kde.org/plasma-desktop/'
license=(LGPL-2.0-or-later)
depends=(breeze
         breeze-icons
         frameworkintegration
         glibc
         kcmutils
         kcolorscheme
         kconfig
         kcoreaddons
         kdecoration
         kguiaddons
         ki18n
         kiconthemes
         kirigami
         kwidgetsaddons
         kwindowsystem
         libstdc++
         qt6-base
         qt6-declarative
         qt6-svg)
makedepends=(extra-cmake-modules
             frameworkintegration5
             git
             kconfigwidgets5
             kiconthemes5
             kwindowsystem5)
optdepends=('breeze-gtk: Breeze widget style for GTK applications')
source=()
sha256sums=()

pkgver() {
  cd "$startdir"
  local version revision
  version=$(sed -n 's/^set(PROJECT_VERSION "\([^"]*\)")/\1/p' CMakeLists.txt)
  revision=$(git rev-list --count HEAD)
  printf '%s.r%s.g%s\n' "$version" "$revision" "$(git rev-parse --short HEAD)"
}

build() {
  cmake -B build -S "$startdir" \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_TESTING=ON \
    -DBUILD_QT5=OFF
  cmake --build build --parallel 2
  ctest --test-dir build --output-on-failure

  cmake -B build5 -S "$startdir" \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_TESTING=ON \
    -DBUILD_QT6=OFF
  cmake --build build5 --parallel 2
  ctest --test-dir build5 --output-on-failure
}

package_beskin() {
  DESTDIR="$pkgdir" cmake --install build

  find "$pkgdir" \( -type f -o -type l \) \
    ! -path "$pkgdir/usr/lib/qt6/plugins/styles/beskin6.so" \
    ! -path "$pkgdir/usr/lib/qt6/plugins/kstyle_config/beskinstyleconfig.so" \
    ! -path "$pkgdir/usr/lib/qt6/plugins/org.kde.kdecoration3/org.kde.beskin.so" \
    ! -path "$pkgdir/usr/lib/qt6/plugins/org.kde.kdecoration3.kcm/kcm_beskin_decoration.so" \
    ! -path "$pkgdir/usr/share/kstyle/themes/beskin.themerc" \
    -delete
  find "$pkgdir" -depth -type d -empty -delete
}

package_beskin5() {
  pkgdesc='Qt5 Beskin widget style'
  depends=(beskin
           glibc
           frameworkintegration5
           kconfig5
           kconfigwidgets5
           kguiaddons5
           kiconthemes5
           kwindowsystem5
           libstdc++
           qt5-base
           qt5-declarative)
  optdepends=()

  DESTDIR="$pkgdir" cmake --install build5
  find "$pkgdir" \( -type f -o -type l \) \
    ! -path "$pkgdir/usr/lib/qt/plugins/styles/beskin5.so" \
    -delete
  find "$pkgdir" -depth -type d -empty -delete
}
