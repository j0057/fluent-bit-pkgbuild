# Maintainer: Joost Molenaar <jjm@j0057.nl>
# Contributor: Tim Meusel <tim@bastelfreak.de>

pkgname=fluent-bit

pkgver=1.6.1
pkgrel=1
epoch=

pkgdesc='Collect data/logs from different sources, unify and send them to multiple destinations.'
arch=(x86_64)
url='https://fluentbit.io/'
license=('Apache')
groups=()

depends=('gcc-libs')
# PostgreSQL_TYPE_INCLUDE_DIR is provided by postgresql, this is currently a bug
makedepends=('cmake' 'postgresql-libs' 'postgresql' 'python' 'valgrind' 'systemd-libs')
checkdepends=('gtest' 'doxygen' 'graphviz')
optdepends=()

provides=()
conflicts=()
replaces=()

backup=('etc/fluent-bit/fluent-bit.conf'
        'etc/fluent-bit/parsers.conf')
options=()
install=
changelog=
source=("https://fluentbit.io/releases/${pkgver:0:3}/$pkgname-$pkgver.tar.gz")
noextract=()
validpgpkeys=()

build() {
    cd $pkgname-$pkgver/build
    cmake \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_SYSCONFDIR=/etc \
        -DFLB_TESTS_INTERNAL=Yes \
        -DFLB_IN_MQTT=Yes \
        -DFLB_TLS=Yes \
        -DFLB_ALL=Yes \
        -DFLB_OUT_NATS=Yes \
        -DFLB_HTTP_SERVER=Yes \
        ..
    make
}

check() {
    cd $pkgname-$pkgver/build
    make test || true
}

package() {
    cd $pkgname-$pkgver/build

    # install binaries and libraries
    make DESTDIR="$pkgdir/" install

    # put systemd unit files in the right spot (make install puts them in /lib/systemd/system)
    mkdir -p "$pkgdir/usr/lib/systemd/system"
    mv "$pkgdir/lib/systemd/system/fluent-bit.service" "$pkgdir/usr/lib/systemd/system"
    rm -rf "$pkgdir/lib"

    # install license file and documentation
    cd "${srcdir}/${pkgname}-${pkgver}"
    install -Dm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
    install -Dm 644 *.md Dockerfile* -t "$pkgdir/usr/share/doc/$pkgname/"
}

sha512sums=('cb4f049c87929c27dacfa9d17c9c6bc87b3a316774f777cf214539bc205f9c6d714a42ad40625e949d776b2417e1f6c00416fed394977d7ceef8d244a9ada2a3')
