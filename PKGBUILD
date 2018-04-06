# Maintainer: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=1.0.3
_commit=773c489c9c1b21a6d78b5c538cd395416ec50f88
pkgrel=1
pkgdesc='An open and reliable container runtime'
url='https://containerd.io/'
depends=('runc')
makedepends=('go-pie' 'git' 'btrfs-progs')
arch=('x86_64')
license=("APACHE")
source=("$pkgname-$pkgver.tar.gz::https://github.com/containerd/containerd/archive/v$pkgver.tar.gz")
sha256sums=('299e3a93eac232c1259fe058e724bfc274741b13b9de96116d1f61619bb2789e')

prepare() {
  mkdir -p src/github.com/containerd
  ln -rTsf $pkgname-$pkgver src/github.com/containerd/containerd
  # fix paths in service
  sed -i 's,/sbin,/usr/bin,;s,/usr/local,/usr,' $pkgname-$pkgver/containerd.service
}

build() {
  export GOPATH="$srcdir"
  cd src/github.com/containerd/containerd
  make VERSION=v$pkgver.m REVISION=$_commit.m
}

package() {
  export GOPATH="$srcdir"
  cd src/github.com/containerd/containerd
  make install DESTDIR="$pkgdir/usr"
  install -Dm644 containerd.service "$pkgdir"/usr/lib/systemd/system/containerd.service
}

# vim:set ts=2 sw=2 et:
