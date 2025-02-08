# Maintainer: Jelle van der Waa <jelle@archlinux.org>
# Contributor: Bruno Pagani <archange@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>

pkgname=nodejs-lts-jod
pkgver=22.13.1
pkgrel=2
pkgdesc="Evented I/O for V8 javascript (LTS release: Jod)"
arch=(x86_64)
url="https://nodejs.org/"
license=(MIT)
# maybe revert back to openssl-1.1 or internal openssl
# https://github.com/nodejs/node/issues/47852
depends=(openssl zlib icu libuv c-ares brotli libnghttp2) # http-parser v8)
makedepends=(python procps-ng)
optdepends=('npm: nodejs package manager')
options=(!lto)
provides=("nodejs=$pkgver")
conflicts=(nodejs)
source=(https://nodejs.org/dist/v${pkgver}/node-v${pkgver}.tar.xz)
# https://nodejs.org/download/release/latest-jod/SHASUMS256.txt.asc
sha256sums=('cfce282119390f7e0c2220410924428e90dadcb2df1744c0c4a0e7baae387cc2')

build() {
  cd node-v${pkgver}

  # this uses malloc_usable_size, which is incompatible with fortification level 3
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  ./configure \
    --prefix=/usr \
    --with-intl=system-icu \
    --without-npm \
    --shared-openssl \
    --shared-zlib \
    --shared-libuv \
    --experimental-http-parser \
    --shared-brotli \
    --shared-cares \
    --shared-nghttp2
    # --shared-v8
    # --shared-http-parser

  make
}

check() {
  cd node-v${pkgver}
  make test-only || :
}

package() {
  cd node-v${pkgver}

  # this uses malloc_usable_size, which is incompatible with fortification level 3
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  make DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE -t "${pkgdir}"/usr/share/licenses/${pkgname}/
}
