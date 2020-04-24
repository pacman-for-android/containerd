# Maintainer: Morten Linderud <foxboron@archlinux.org>
# Maintainer: Santiago Torres-Arias <santiago@archlinux.org>
# Contributor: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=1.3.4
_commit=d76c121f76a5fc8a462dc64594aea72fe18e1178
pkgrel=1
pkgdesc='An open and reliable container runtime'
url='https://containerd.io/'
depends=('runc')
makedepends=('go' 'git' 'btrfs-progs' 'libseccomp')
arch=('x86_64')
license=("Apache")
source=("$pkgname-$pkgver.tar.gz::https://github.com/containerd/containerd/archive/v$pkgver.tar.gz"
        bbolt-1.3.4.tar.gz::https://github.com/etcd-io/bbolt/archive/v1.3.4.tar.gz)
sha256sums=('374d0c944985a64a7f31da4018aaa6d7a0b123122633ce28ab04cf1203f785b8'
            '536029610c6cd08e4a9fe2e1b0857ae539b0736850132cbbe1c18f0fbed8a4da')


prepare() {
  # Update bbolt to fix tests with go 1.14
  rm -r $pkgname-$pkgver/vendor/go.etcd.io/bbolt
  ln -rTsf bbolt-1.3.4 $pkgname-$pkgver/vendor/go.etcd.io/bbolt

  mkdir -p src/github.com/containerd
  ln -rTsf $pkgname-$pkgver src/github.com/containerd/containerd

  # fix paths in service
  sed -i 's,/sbin,/usr/bin,;s,/usr/local,/usr,' $pkgname-$pkgver/containerd.service
}

build() {
  export GOPATH="$srcdir"
  cd src/github.com/containerd/containerd
  export CGO_LDFLAGS="$LDFLAGS"
  export GOFLAGS="-trimpath"
  make VERSION=v$pkgver.m REVISION=$_commit.m
}

check() {
  cd src/github.com/containerd/containerd
  make test
}

package() {
  export GOPATH="$srcdir"
  cd src/github.com/containerd/containerd
  make install DESTDIR="$pkgdir/usr"
  install -Dm644 containerd.service "$pkgdir"/usr/lib/systemd/system/containerd.service
}
