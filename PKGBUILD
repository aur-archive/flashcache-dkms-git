# Maintainer: mindcat <444303908@qq.com>
pkgname=flashcache-dkms-git
pkgver=1.0.r223.g304bbc1
pkgrel=1
pkgdesc="FlashCache is a general purpose writeback block cache for Linux."
arch=('x86_64')
url="https://github.com/facebook/flashcache"
license=('GPL')
makedepends=('git' 'dkms')
source=('flashcache_install'
        'flashcache_hook'
        'git+https://github.com/facebook/flashcache.git')
md5sums=('35da1ce02628669061ad6883f58cb690'
         '24630e6cb1b1159e7494c3b963322ce3'
         'SKIP')
install="flashcache.install"
conflicts=('flashcache-git')

_gitroot=git://github.com/facebook/flashcache.git
_pkgname=flashcache

pkgver() {
  cd "$srcdir/${_pkgname}"
  git describe --long | sed -r 's/([^-]*-g)/r\1/;s/-/./g'
}

build() {
  cd "$srcdir/${_pkgname}"
  make -C src/utils
}

package() {
  # cd "./${_pkgname}"
  cd "$srcdir/${_pkgname}"
  
  mkdir -p ${pkgdir}/usr/src/${_pkgname}-${pkgver}

  sed -i "s/PACKAGE_VERSION=/PACKAGE_VERSION=${pkgver}/g" ./src/dkms.conf # add version

  cp -RLv ./src/* "${pkgdir}/usr/src/${_pkgname}-${pkgver}"
  mkdir -p "${pkgdir}/usr/bin/"
  install -D -m755 ./utils/flashstat "${pkgdir}/usr/bin/"
  # rm -Rfv ${_pkgname}/src/utils
  install -D -m644 "${srcdir}/flashcache_install" "${pkgdir}/usr/lib/initcpio/install/flashcache"
  install -D -m644 "${srcdir}/flashcache_hook" "${pkgdir}/usr/lib/initcpio/hooks/flashcache"
  make DESTDIR="$pkgdir/" -C src/utils install
}

# vim:set ts=2 sw=2 et:
