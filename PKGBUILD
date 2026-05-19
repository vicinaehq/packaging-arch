# Maintainer: cilgin <cilgincc@outlook.com>
# Maintainer: Arjix <me@arjix.dev>
# Maintainer: Aurelle <gh@aurelle.dev>

# shellcheck disable=SC2034
# shellcheck disable=SC2154
# shellcheck disable=SC2128
pkgname=vicinae-bin
pkgver=0.21.0
pkgrel=3
pkgdesc="Raycast like FOSS app on Linux"
arch=('x86_64')
url="https://github.com/vicinaehq/vicinae"
license=('GPL3')
depends=(
  nodejs
  qt6-base
  qt6-declarative
  qt6-svg
  layer-shell-qt
  libqalculate
  qtkeychain-qt6
  libxml2
  minizip
  syntax-highlighting
)
install=vicinae-bin.install
provides=("vicinae")
conflicts=("vicinae")

noextract=("vicinae-${arch}-v${pkgver}-${pkgrel}.tgz")
source=(
  "vicinae-${arch}-v${pkgver}-${pkgrel}.tgz::${url}/releases/download/v${pkgver}/${pkgname%-bin}-linux-${arch}-v${pkgver}.tar.gz"
  "vicinae.hook"
)

sha256sums=('d19099a094507184fcf9388e85ef74839a99fc3666b41bd6cc3bf116e1b01ca9'
            '7b4715a9b3b25c55255824b171780dbb760406cb43ea8e3622bb9de867fd0ec7')

prepare() {
  mkdir -p vicinae
  tar -xzf "vicinae-${arch}-v${pkgver}-${pkgrel}.tgz" -C vicinae
}

package() {
  install -d "$pkgdir/usr"
  for item in ./vicinae/*; do
    cp -a "$item" "$pkgdir/usr/"
  done

  find "$pkgdir/usr/" -type f -name '*.in' -delete

  # have to be installed manually due to non standard locations
  install -Dm644 ./vicinae/share/vicinae/native-host/chromium/com.vicinae.vicinae.json "$pkgdir/etc/chromium/native-messaging-hosts/com.vicinae.vicinae.json"
  install -Dm644 ./vicinae/share/vicinae/native-host/firefox/com.vicinae.vicinae.json "$pkgdir/usr/lib/mozilla/native-messaging-hosts/com.vicinae.vicinae.json"

  # Pacman hook
  install -Dm644 "$srcdir/${pkgname%-bin}.hook" "$pkgdir/usr/share/libalpm/hooks/${pkgname%-bin}.hook"
}
