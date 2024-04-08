pkgname=open-ephys-gui
pkgver=0.6.7.r0.g8c64539
pkgrel=1
pkgdesc="Software for processing, recording, and visualizing multichannel electrophysiology data"
arch=(x86_64)
url="https://open-ephys.org/"
license=('GPL3')
source=(
  "git+https://github.com/open-ephys/plugin-GUI.git"
  "no_require_cmake_release.patch"
)
depends=(
  'freeglut'
  'freetype2'
  'libxinerama'
  'libxcursor'
  'alsa-lib'
  'libxrandr'
  'curl'
  'webkit2gtk'
)
makedepends=(
  'git'
  'cmake'
  'gendesk'
)
sha256sums=(
  'SKIP'
  'de8d8d9581b8d4fd54a01d651d9ab4fe2a29223bebcd53fc21ce52a2ca8ffe70'
)

pkgver() {
  cd plugin-GUI
  git describe --tags --long --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  gendesk -n -f \
    --exec '/opt/open-ephys/open-ephys' \
    --icon '/opt/open-ephys/icon-small.png' \
    --pkgname "$pkgname" \
    --pkgdesc "$pkgdesc"

  patch --directory=plugin-GUI --forward --strip=1 --input=../no_require_cmake_release.patch
}

build() {
  cmake -B plugin-GUI/Build -S plugin-GUI \
    -DCMAKE_BUILD_TYPE='None' \
    -DCMAKE_INSTALL_PREFIX='/usr' \
    -Wno-dev
  cmake --build plugin-GUI/Build
}

package() {
  OPTDIR="$pkgdir/opt/open-ephys"
  BINDIR="$pkgdir/usr/bin"
  mkdir -p "$OPTDIR" "$BINDIR"

  cp -a plugin-GUI/Build/None/. "$OPTDIR"
  ln -sr  "$OPTDIR/open-ephys" "$BINDIR"

  install -Dm644 plugin-GUI/LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 plugin-GUI/Resources/Scripts/40-open-ephys.rules "$pkgdir/etc/udev/rules.d/40-open-ephys.rules"
  install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
}
