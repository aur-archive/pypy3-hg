# $Id: PKGBUILD 95035 2013-08-04 09:44:24Z svenstaro $
# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>

#_hgrev=63547

pkgname=pypy3-hg
#pkgver=2.1beta1
#_pkgver=2.1-beta1
#[[ -n $_hgrev ]] && pkgver=2.0beta2.$_hgrev
pkgrel=1
pkgver=r70272.5cba21f3d2be
pkgdesc="A Python3 implementation written in Python, JIT enabled, hg version"
url="http://pypy.org"
arch=('i686' 'x86_64')
depends=('libffi')
provides=('pypy3')
conflicts=('pypy3')
options=(!buildflags)
makedepends=('python' 'mercurial' 'python2' 'tk')
optdepends=('openssl: openssl module'
            'expat: pyexpat module'
            'ncurses: ncurses module'
            'zlib: zlib module'
            'bzip2: bz2 module'
            'tk: tk module')
license=('custom:MIT')
source=("hg+https://bitbucket.org/pypy/pypy#branch=py3k")
#source=("https://bitbucket.org/pypy/pypy/downloads/$pkgname-$_pkgver-src.tar.bz2")
md5sums=('SKIP')

pkgver() {
  #set -xv
  cd "$srcdir/pypy"
  printf "r%s.%s" "$(hg identify -n)" "$(hg identify -i)"
}

build() {
  cd "${srcdir}"/pypy/pypy/goal

  #python2 ../../rpython/bin/rpython -Ojit targetpypystandalone
  python2 ../../rpython/bin/rpython -Ojit --shared --gcrootfinder=shadowstack targetpypystandalone
}

package() {
  cd "${srcdir}"/pypy/pypy/tool/release

  LD_LIBRARY_PATH=../../goal/ python2 package.py ../../../ pypy3 pypy-c "${srcdir}"/${pkgname}.tar.bz2

  mkdir -p "${pkgdir}"/opt
  tar x -C "${pkgdir}"/opt -f "${srcdir}"/${pkgname}.tar.bz2

  mkdir -p "${pkgdir}"/opt/pypy3/lib
  cp ../../goal/libpypy-c.so "${pkgdir}"/opt/pypy3/lib/
  mkdir -p "${pkgdir}"/usr/lib
  ln -s /opt/pypy3/lib/libpypy-c.so "${pkgdir}"/usr/lib/libpypy-c.so

  mkdir -p "${pkgdir}"/usr/bin
  ln -s /opt/pypy3/bin/pypy-c "${pkgdir}"/usr/bin/pypy3

  install -Dm644 "${pkgdir}"/opt/pypy3/LICENSE "${pkgdir}"/usr/share/licenses/pypy3/LICENSE
}

