pkgname=lib32-mingw-w64-pangomm
pkgver=2.28.4
pkgrel=2
pkgdesc="C++ bindings for pango (mingw-w64, 32-bit)"
arch=(any)
url="http://gtkmm.sourceforge.net"
license=("LGPL")
makedepends=(mingw-w64-gcc mingw-w64-pkg-config)
depends=(mingw-w64-crt
mingw-w64-cairomm
mingw-w64-glibmm
mingw-w64-pango)
options=(!libtool !strip !buildflags)
source=("http://ftp.gnome.org/pub/GNOME/sources/pangomm/${pkgver%.*}/pangomm-${pkgver}.tar.xz")
md5sums=('f4fe0905ee56e1ff0205005e61d2a66f')

_architectures="i686-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    export CFLAGS="-O2 -pipe"
    export CXXFLAGS="$CFLAGS"
    export CPPFLAGS="$CPPFLAGS -D_REENTRANT"
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    ${srcdir}/pangomm-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --enable-static \
      --enable-shared \
      --disable-documentation
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' -o -name '*.bat' -o -name '*.def' -o -name '*.exp' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip -x
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
  done
}