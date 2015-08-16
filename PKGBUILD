pkgname=mingw-w64-libmpc-static
pkgver=1.0.1
pkgrel=1
pkgdesc="Library for the arithmetic of complex numbers with arbitrarily high precision (mingw-w64, static)"
arch=(any)
url="http://www.multiprecision.org/"
license=("LGPL")
makedepends=(mingw-w64-gcc)
depends=(mingw-w64-crt "mingw-w64-mpfr-static>=3.0.0")
conflicts=(mingw-w64-libmpc)
provides=(mingw-w64-libmpc)
options=(!libtool !strip !buildflags)
source=("http://www.multiprecision.org/mpc/download/mpc-${pkgver/_/-}.tar.gz")
md5sums=('b32a2e1a3daa392372fbd586d1ed3679')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  unset LDFLAGS
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/${pkgname#mingw-w64-lib}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch}
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.a' | xargs -rtl1 ${_arch}-strip -g
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}