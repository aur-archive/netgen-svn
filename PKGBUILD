# Maintainer: Oliver Goethel <godeezy at gmail.com>
# Contributor: Thomas Zervogiannis <tzervo@gmail.com>
# Contributor: Michael Schubert <mschu.dev at gmail>

pkgname=netgen-svn
pkgver=947
pkgrel=1
pkgdesc="An automatic 3D tetrahedral mesh generator"
url="http://www.hpfem.jku.at/netgen/"
depends=('tk' 'opencascade' 'libjpeg' 'ffmpeg' 
'tk-togl-legacy-netgen' 'tix' 'openmpi')
arch=('i686' 'x86_64')
md5sums=('SKIP'
         'b963a08af5414da040f337ac863b93e5'
         '203efc3ba8b462af2f7c4ccd14a1b9d2')
license=('LGPL')
provides=('netgen')
conflicts=('netgen')
source=("netgen-code::svn://svn.code.sf.net/p/netgen-mesher/code/netgen" 
"netgen"
"netgen5_ffmpeg.patch")
options=('docs' '!libtool')

pkgver() {
  cd netgen-code
  svnversion | tr -d [A-z]
}

build() {
  cd netgen-code

  patch -Np1 -i ${srcdir}/netgen5_ffmpeg.patch
  
  autoreconf --install
  
  CXXFLAGS+=" -D__STDC_CONSTANT_MACROS" LDFLAGS+=" -L/usr/lib/openmpi -lmpi -lmpi_cxx -ldl -L/usr/lib/Togl1.7 -lTogl1.7" ./configure --prefix=/usr \
    --enable-occ -with-occ=/opt/opencascade --with-gnu-ld --enable-nglib \
    --enable-jpeglib --enable-parallel --enable-ffmpeg --datadir=/usr/share

  make || return 1
}

package () {
  cd netgen-code
  make DESTDIR="$pkgdir" install
  install -D -m644 "$srcdir/netgen-code/doc/ng4.pdf" \
    "$pkgdir/usr/share/doc/$pkgname/ng4.pdf"
  mv "$pkgdir/usr/bin/netgen" "$pkgdir/usr/bin/netgen.bin"
  install -m 755 "$srcdir/netgen" "$pkgdir/usr/bin/netgen"
}
