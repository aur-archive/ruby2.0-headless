# Maintainer: Adam Gradzki <adam.gradzki@gmail.com>
# Contributor: Jonne Haß <me@mrzyx.de>
# Contributor: Thomas Dziedzic <gostrc@gmail.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: John Proctor <jproctor@prium.net>
# Contributor: Jeramy Rutley <jrutley@gmail.com>

pkgname=ruby2.0-headless
pkgver=2.0.0_p451
pkgrel=0
arch=('i686' 'x86_64')
pkgdesc='An object-oriented language for quick and easy programming'
url='http://www.ruby-lang.org/en/'
license=('BSD' 'custom')
depends=('gdbm' 'openssl' 'libffi' 'libyaml')
makedepends=('gdbm' 'openssl' 'libffi' 'libyaml')
provides=('rubygems2.0' 'rake2.0')
options=('!emptydirs' '!makeflags' 'staticlibs')
source=("http://cache.ruby-lang.org/pub/ruby/${pkgver%.*}/ruby-${pkgver//_/-}.tar.bz2"
         0001-merge-revision-s-r45220.patch
         0002-merged-partially-from-r42781.patch
         0003-merge-revision-s-r45187-r45205-r45212-r45213-Backpor.patch
         0004-merge-revision-s-r45178-r45179-r45180-r45183-Backpor.patch
         0005-merge-revision-s-r45225-r45240-Backport-9578.patch)

build() {
  cd ruby-${pkgver//_/-}

  patch -p1 < ../0001-merge-revision-s-r45220.patch
  patch -p1 < ../0002-merged-partially-from-r42781.patch
  patch -p1 < ../0003-merge-revision-s-r45187-r45205-r45212-r45213-Backpor.patch
  patch -p1 < ../0004-merge-revision-s-r45178-r45179-r45180-r45183-Backpor.patch
  patch -p1 < ../0005-merge-revision-s-r45225-r45240-Backport-9578.patch

  PKG_CONFIG=/usr/bin/pkg-config ./configure \
    --prefix=/opt/$pkgname \
    --sysconfdir=/etc \
    --enable-shared \
    --disable-rpath \
    --with-dbm-type=gdbm_compat

  make
}

check() {
  cd ruby-${pkgver//_/-}

  make test
}

package() {
  cd ruby-${pkgver//_/-}

  make DESTDIR="${pkgdir}" install-nodoc

  install -dm755 $pkgdir/usr/bin
  install -dm755 $pkgdir/usr/lib

  for i in erb irb rdoc ri ruby testrb rake gem; do
    ln -s /opt/$pkgname/bin/$i $pkgdir/usr/bin/$i-2.0
  done

  ln -s /opt/$pkgname/lib/libruby.so.2.0 $pkgdir/usr/lib/libruby.so.2.0

  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"
  install -D -m644 BSDL "${pkgdir}/usr/share/licenses/$pkgname/BSDL"
}

sha256sums=('5bf8a1c7616286b9dbc962912c3f58e67bc3a70306ca90b0882ef0bd442e02f5'
            '95fff1f37f565ef9055af1c15d537c31e83441eb8f636851ba023d8f21254091'
            '4bba4b0bfd7e4ea86c012bddfc34244d3a09fc6e5235c91f0435d5f440ec8b3a'
            'a56f308add1900c2cf2a025e628448c7f77d97246c9811fb80e49c850eb60c1e'
            '8af971cee9e47c95e0f8672a30770b7005122a6524660c0803a0a830a829ee73'
            'c1a910bb17b13de5ae42951419cf14398b03f44b76985ac6b5db6f62515c4211')
