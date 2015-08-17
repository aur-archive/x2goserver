# Contributor: Gerhard Brauer <gerbra@archlinux.de>
# Contributor: Richard Murri <admin@richardmurri.com>
# Contributor: Markus Opitz <mastero23 at gmail dot com>
# Maintainer: Milan Knížek <knizek@volny.cz>
pkgname=x2goserver
pkgver=4.0.0.0
pkgrel=1
pkgdesc="Open source terminal server"
arch=('i686' 'x86_64')
url="http://www.x2go.org/"
license=('GPL')
depends=('openssh' 'perl-config-simple' 'perl-dbd-sqlite' 'python' 'x2goagent' 'xorg-xauth')
makedepends=('man2html')
optdepends=('cups-x2go: printing support')
# x2gofmbindings are already included in the source, however the executable and mime type
# is not installed by "make install" on purpose - it would conflict x2gognomebindings, etc.
#conflicts=('x2gofmbindings')
options=(emptydirs)
install=x2goserver.install
backup=('etc/x2go/x2goserver.conf' 'etc/x2go/x2gosql/sql')
groups=('x2go' 'alts')
source=('x2goserver.rc.d'
        'x2goserver.service'
        "http://code.x2go.org/releases/source/${pkgname}/${pkgname}-${pkgver}.tar.gz")

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  # -r option does not exist in Arch linux
  # (However, html man pages do not get installed anyway...)
  for Makefile in $(find . -type f -name Makefile); do
    sed -i 's@(MAN2HTML_BIN) -r @(MAN2HTML_BIN) < @g' $Makefile
    sed -i 's@ \$(MAN2HTML_SRC)/@ < \$(MAN2HTML_SRC)/@g' $Makefile
  done
#  sed -i 's@share/x2gofeature@share/x2go/x2gofeature@g' x2goserver-fmbindings/Makefile
#  sed -i 's@VERSION.x2goserver-extensions@VERSION.x2goserver-fmbindings@g' x2goserver-fmbindings/Makefile
#  patch -p0 < ../x2gostartagent-xauthority.patch
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make PREFIX=/usr DESTDIR="$pkgdir" install

  install -D -m 755 "$srcdir/x2goserver.rc.d" "$pkgdir/etc/rc.d/x2goserver"
  install -D -m 644 "$srcdir/x2goserver.service" "$pkgdir/usr/lib/systemd/system/x2goserver.service"

  install -m 755 -d "${pkgdir}/usr/share/doc/${pkgname}"
  install -m 644 "debian/changelog" "${pkgdir}/usr/share/doc/${pkgname}/changelog.DEBIAN"
  install -m 644 "debian/copyright" "${pkgdir}/usr/share/doc/${pkgname}/copyright.DEBIAN"
}
md5sums=('8083819ac423401440c0ad5feef1a128'
         '3bdfe6a4f2c3847bb8d021e88a6706e5'
         '1fc38566832c30897c6583e7ffa1fda3')
