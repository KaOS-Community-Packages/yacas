pkgname=yacas
pkgver=1.9.1
_docver=1.8.0
pkgrel=1
pkgdesc='Yet another computer algebra system'
url='http://www.yacas.org/'
screenshot='https://dl.dropbox.com/s/dy9evnpl13kdo21/yacas-console.png'
license=('GPL2')
groups=("mathematics")
arch=('x86_64')
depends=('nspr' 'qtwebengine' 'zstd')
makedepends=('gcc' 'cmake' 'perl' 'nodejs')
optdepends=('gnuplot' 'links' 'texmacs' 'okular: Reading EPUB manual')
install=${pkgname}.install
noextract=('yacas.epub')
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/grzegorzmazur/yacas/archive/v${pkgver}.tar.gz"
        "https://buildmedia.readthedocs.org/media/epub/yacas/v${_docver}/yacas.epub"
        "yacas.desktop"
        "yacas-docs.desktop"
        )
md5sums=('f908d52c6095d45bd69bc3e517dbee08'  # yacas source
         'becd320686b3b9aa2e6c39477f37ff06'  # Epub manual
         'eb776002fabe21623716ed2642f6d365'  # yacas.desktop
         '04d2a47c02fba5d88f337a404e02929c'  # yacas-docs.desktop
         )

build() {
  cd ${srcdir}/${pkgname}-${pkgver}
  [ -d build ] && rm -rf build
  mkdir build
  cd build
  # ENABLE_DOCS=ON requires a lot of missing sphinx packages in KaOS
  msg "### cmake" ; cmake .. \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DENABLE_CYACAS_CONSOLE=ON \
    -DENABLE_CYACAS_GUI=ON \
    -DENABLE_CYACAS_UNIT_TESTS=OFF \
    -DENABLE_CYACAS_BENCHMARKS=OFF \
    -DENABLE_JYACAS=OFF \
    -DENABLE_DOCS=OFF \
    -DENABLE_CYACAS_KERNEL=OFF \
    -DCMAKE_BUILD_TYPE=Release
  msg "### make"  ; make
}

package() {
  cd ${srcdir}/${pkgname}-${pkgver}/build
  msg "### install" ; make DESTDIR=$pkgdir install
  cd ..
  _appdir=${pkgdir}/usr/share/applications
  _pngdir=${pkgdir}/usr/share/pixmaps
  _docdir=${pkgdir}/usr/share/doc/${pkgname}
  _licdir=${pkgdir}/usr/share/licenses/${pkgname}
  install -dm755 ${_appdir} ${_pngdir} ${_docdir} ${_licdir}
  install -Dpm644 ${srcdir}/yacas.desktop ${_appdir}/
  install -Dpm644 ${srcdir}/yacas-docs.desktop ${_appdir}/
  install -Dpm644 AUTHORS CODE_OF_CONDUCT.md ChangeLog README.rst TODO ${_docdir}/
  install -Dpm644 ${srcdir}/yacas.epub ${pkgdir}/usr/share/yacas/
  install -Dpm644 COPYING ${_licdir}/
  (cd ${_docdir} ; ln -s ../../yacas/yacas.epub .)
}
