# Maintainer: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=0.2.9
pkgrel=2
pkgdesc='An open and reliable container runtime'
url='https://containerd.io/'
depends=('glibc' 'runc')
makedepends=('go-pie' 'git')
arch=('x86_64')
source=("containerd.io::git+https://github.com/containerd/containerd.git#tag=v$pkgver")
license=("APACHE")
md5sums=('SKIP')

build() {
  export GOPATH="$srcdir"
  mkdir -p src/github.com/containerd
  ln -rTsf containerd.io src/github.com/containerd/containerd
  cd src/github.com/containerd/containerd
  LDFLAGS= make
}

package() {
  cd src/github.com/containerd/containerd/bin
  for _file in *; do
    install -Dm755 "$_file" "$pkgdir/usr/bin/$_file"
  done
}

# vim:set ts=2 sw=2 et:
