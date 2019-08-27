# Maintainer: Chih-Hsuan Yen <yan12125@archlinux.org>
# Contributor: Dave Reisner <d@falconindy.com>

_pkgname=pyalpm
pkgname=$_pkgname-git
pkgver=0.8.4.r100.gbca42ad
pkgrel=1
pkgdesc="Libalpm bindings for Python 3 (Git version)"
arch=('x86_64')
url="https://git.archlinux.org/pyalpm.git/"
license=('GPL3')
depends=('python>=3.6' 'pacman>=5')
makedepends=('git' 'python-setuptools')
checkdepends=('python-pytest')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
source=('git+https://git.archlinux.org/pyalpm.git'
        'https://github.com/archlinux/pyalpm/commit/93af5e0d4602ae328d0dbd3bf42bd1e4e6f6354c.patch')
sha256sums=('SKIP'
            'b184f423c7bdde69ed98fd1d90b2ddc23d9f808bb3035e22ce5bec53e5c0aee5')

prepare() {
  cd $_pkgname
  if pacman -Qq pacman-git; then git apply -v ${srcdir}/*.patch; fi
}

pkgver() {
  cd $_pkgname
  ( set -o pipefail
    git describe --long --tags 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
  )
}

build() {
  cd $_pkgname

  python setup.py build
}

check() {
  cd $_pkgname

  PYTHONPATH="$(ls -d build/lib.linux-$CARCH-*)" pytest -v test
}

package() {
  cd $_pkgname

  if pacman -Qq pacman-git; then depends=('python>=3.6', 'pacman-git' ); fi
  python setup.py install --root="$pkgdir" --optimize=1 --skip-build
}
