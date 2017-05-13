# Maintainer: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=0.2.8
pkgrel=1
pkgdesc='An open and reliable container runtime'
url='https://containerd.io/'
depends=('glibc' 'runc')
makedepends=('go' 'git')
arch=('i686' 'x86_64')
source=("git+https://github.com/containerd/containerd.git#tag=v$pkgver")
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
  for _file in *; do
    install -Dm755 "$_file" "$pkgdir/usr/bin/$_file"
  done
}

# vim:set ts=2 sw=2 et:
