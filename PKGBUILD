# Maintainer: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=0.2.2
pkgrel=1
pkgdesc='A daemon to control runC, built for performance and density'
url='https://containerd.tools/'
depends=('glibc' 'runc')
makedepends=('go' 'git')
arch=('x86_64')
source=("git+https://github.com/docker/containerd.git#tag=v$pkgver")
license=("APACHE")
md5sums=('SKIP')

build() {
  export GOPATH="$srcdir"
  mkdir -p src/github.com/docker
  ln -rsf containerd src/github.com/docker
  cd src/github.com/docker/containerd
  LDFLAGS= make
}

package() {
  cd src/github.com/docker/containerd/bin
  for file in $(find . -type f -print); do
    install -Dm755 $file $pkgdir/usr/bin/$file
  done
}

# vim:set ts=2 sw=2 et:
