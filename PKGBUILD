
# Maintainer: Jordy van Wolferen <jordy@kluisip.nl>

pkgname=c2c-git
_pkgbase=c2c
pkgver=r637.710be0b
pkgrel=1
pkgdesc="C2 compiler"
arch=('i686' 'x86_64')
url="http://c2lang.org"
license=('Apache')
depends=()
makedepends=('git' 'llvm-c2' 'cmake')
source=("c2compiler::git+https://code.kluisip.nl/c2compiler#branch=master")
options=(!buildflags)
md5sums=('SKIP')

build() {
  cd "$srcdir"/c2compiler/c2c

  # force llvm-c2, if llvm is installed
  [ -f /usr/bin/llvm-config ] && export PATH=/opt/llvm-c2/bin:$PATH

  # fix CmakeLists.txt to use llvm-c2
  sed -i 's/llvm-config/\/opt\/llvm-c2\/bin\/llvm-config/g' CMakeLists.txt
  sed -i 's/clang++/\/opt\/llvm-c2\/bin\/clang++/g' CMakeLists.txt

  rm -rf build
  mkdir build
  cd build

  cmake . ..
  make

  # run unit tests
  cd "$srcdir"/c2compiler/tools/tester
  make
  cd ../../c2c/build
  make test
}

package() {
  cd "$pkgdir"
  mkdir -p etc/profile.d
  echo 'export PATH=$PATH:/opt/c2c/bin' > etc/profile.d/${_pkgbase}.sh
  chmod 775 etc/profile.d/${_pkgbase}.sh

  mkdir -p opt/c2c/bin
  mkdir -p opt/c2c/doc

  cd opt/c2c/bin

  cp "$srcdir"/c2compiler/c2c/build/c2c .
  cp "$srcdir"/c2compiler/tools/tester/tester .

  cd "$pkgdir"/opt/c2c
  cp -r "$srcdir"/c2compiler/c2c/examples .
  cp "$srcdir"/c2compiler/*.md doc
}


pkgver() {
  cd "$srcdir"/c2compiler/c2c
  echo r$(git rev-list --count master).$(git rev-parse --short master)
}

# vim:set ts=2 sw=2 et:
