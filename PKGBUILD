# Maintainer: Morten Linderud <foxboron@archlinux.org>
# Maintainer: Santiago Torres-Arias <santiago@archlinux.org>
# Maintainer: Robin Candau <antiz@archlinux.org>
# Contributor: Sébastien "Seblu" Luttringer
# Contributor: Iwan Timmer <irtimmer@gmail.com>

pkgname=containerd
pkgver=1.7.5
pkgrel=1
pkgdesc='An open and reliable container runtime'
url='https://containerd.io/'
depends=('runc')
makedepends=('go' 'git' 'btrfs-progs' 'libseccomp' 'containers-common' 'go-md2man')
provides=('container-runtime')
arch=('x86_64' 'aarch64')
license=("Apache")
source=("git+https://github.com/containerd/containerd.git#tag=v${pkgver}?signed"
        containerd-change-prefix.patch)
validpgpkeys=("8C7A111C21105794B0E8A27BF58C5D0A4405ACDB") # Derek McGowan
sha256sums=('SKIP'
            'a8d10806dfd05deb5c581724fb1631016e42d203d3650d9dfcadfcae2a863548')

prepare() {
  # fix paths in service
  sed -i 's,/sbin,/data/usr/bin,;s,/usr/local,/data/usr,' $pkgname/containerd.service
  patch -Np1 -i containerd-change-prefix.patch
}

build() {
  cd "${pkgname}" 
  export GOFLAGS="-trimpath -mod=readonly -modcacherw"
  make PREFIX=/data/usr VERSION=v$pkgver GO_BUILD_FLAGS="-trimpath -mod=readonly -modcacherw" GO_GCFLAGS="" EXTRA_LDFLAGS="-buildid="
  make PREFIX=/data/usr VERSION=v$pkgver man
}

check() {
  cd "${pkgname}" 
  # Ugly, but they are trying to do priviledged operations during testing
  GOFLAGS="-trimpath" make test || true
}

package() {
  cd "${pkgname}" 
  make PREFIX=/data/usr DESTDIR="$pkgdir/" SHELL=/data/usr/bin/sh install
  install -Dm644 containerd.service "$pkgdir"/data/usr/lib/systemd/system/containerd.service
  install -Dm644 man/*.8 -t "$pkgdir/data/usr/share/man/man8"
  install -Dm644 man/*.5 -t "$pkgdir/data/usr/share/man/man5"
}
