# Maintainer: cilgin <cilgincc@outlook.com>
# Maintainer: Arjix <me@arjix.dev>
# Maintainer: Aurelle <gh@aurelle.dev>
# Contributor: enchanteddev <code.enchanted@gmail.com>

# shellcheck disable=SC2034,SC2154,SC2128,SC2128,SC2164

pkgname=vicinae-bin
pkgver=0.21.1
pkgrel=1
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
  "99-vicinae.rules"
)

sha256sums=('1f1dd85854eaf8c269fea506071d4da38ac8ef0eb8c80c2c3d1e7ce537353b36'
            '7b4715a9b3b25c55255824b171780dbb760406cb43ea8e3622bb9de867fd0ec7'
            '85abd3fb5c0351281a3e4a6001f138c251d791c92c0c45baf984fefa1bdb58c7')

prepare() {
  mkdir -p vicinae
  tar -xzf "vicinae-${arch}-v${pkgver}-${pkgrel}.tgz" -C vicinae

  mv vicinae/share/vicinae/native-host/*.in "$srcdir"
  rm -rf vicinae/share/vicinae/native-host

  mv com.vicinae.vicinae.chromium.json{.in,}
  mv com.vicinae.vicinae.firefox.json{.in,}

  sed -i \
    -e 's|@NATIVE_HOST_BIN@|/usr/libexec/vicinae/vicinae-browser-link|' \
    -e 's|@CHROME_EXTENSION_ID@|kcmipingpfbohfjckomimmahknoddnke|' \
    ./*.json
}

package() {
  install -d "$pkgdir/usr"
  for item in ./vicinae/*; do
    cp -a "$item" "$pkgdir/usr/"
  done

  # have to be installed manually due to non standard locations
  install -Dm644 "$srcdir"/com.vicinae.vicinae.chromium.json "$pkgdir/etc/chromium/native-messaging-hosts/com.vicinae.vicinae.json"
  install -Dm644 "$srcdir"/com.vicinae.vicinae.firefox.json "$pkgdir/usr/lib/mozilla/native-messaging-hosts/com.vicinae.vicinae.json"

  # udev rules
  install -Dm644 "$srcdir/99-${pkgname%-bin}.rules" "$pkgdir/usr/lib/udev/rules.d/99-${pkgname%-bin}.rules"

  chown root:input "$pkgdir/usr/libexec/vicinae/vicinae-input-server"
  chmod 2755 "$pkgdir/usr/libexec/vicinae/vicinae-input-server"

  # Pacman hook
  install -Dm644 "$srcdir/${pkgname%-bin}.hook" "$pkgdir/usr/share/libalpm/hooks/${pkgname%-bin}.hook"
}
