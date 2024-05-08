pkgname=open-ephys-gui
pkgver=0.6.7.r0.g8c64539
pkgrel=1
pkgdesc="Software for processing, recording, and visualizing multichannel electrophysiology data"
arch=(x86_64)
url="https://open-ephys.org/"
license=('GPL3')
source=(
  "git+https://github.com/open-ephys/plugin-GUI.git"
  "allow_build_type_none.patch"
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
  '72b21c45a0a9eccb2be0be311b678d6a3687c855d5b3f5fd64c805341b517e05'
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

  patch --directory=plugin-GUI --forward --strip=1 --input=../allow_build_type_none.patch
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
