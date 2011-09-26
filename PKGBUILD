# Maintainer: Dan McGee <dan@archlinux.org>

pkgbase=postgresql
pkgname=('postgresql-libs' 'postgresql-docs' 'postgresql')
pkgver=9.1.1
_majorver=${pkgver%.*}
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.postgresql.org/"
license=('custom:PostgreSQL')
makedepends=('libxml2' 'python2' 'perl' 'openssl>=1.0.0')
source=(ftp://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2
        postgresql postgresql.confd postgresql.pam postgresql.logrotate)

build() {
  cd "${srcdir}/postgresql-${pkgver}"

  ./configure --prefix=/usr --mandir=/usr/share/man \
  --datadir=/usr/share/postgresql \
  --with-libxml --with-openssl --with-perl \
  --with-python PYTHON=/usr/bin/python2 --with-pam \
  --with-system-tzdata=/usr/share/zoneinfo --enable-nls \
  --enable-thread-safety

  make world
}

package_postgresql-libs() {
  pkgdesc="Libraries for use with PostgreSQL"
  depends=('openssl>=1.0.0')
  provides=('postgresql-client')
  conflicts=('postgresql-client')

  cd "${srcdir}/postgresql-${pkgver}"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/postgresql-libs/LICENSE"

  # install libs
  for dir in src/interfaces src/bin/pg_config; do
    pushd ${dir}
    make DESTDIR="${pkgdir}" install
    popd
  done

  install -D -m644 doc/src/sgml/man1/pg_config.1 "${pkgdir}/usr/share/man/man1/pg_config.1"

  cd src/include

  mkdir -p "${pkgdir}"/usr/include/{libpq,postgresql/internal/libpq}

  # these headers are needed by the public headers of the interfaces
  install -m644 pg_config.h "${pkgdir}/usr/include/"
  install -m644 pg_config_os.h "${pkgdir}/usr/include/"
  install -m644 postgres_ext.h "${pkgdir}/usr/include/"
  install -m644 libpq/libpq-fs.h "${pkgdir}/usr/include/libpq/"
  install -m644 pg_config_manual.h "${pkgdir}/usr/include/"

  # these headers are needed by the not-so-public headers of the interfaces
  install -m644 c.h "${pkgdir}/usr/include/postgresql/internal/"
  install -m644 port.h "${pkgdir}/usr/include/postgresql/internal/"
  install -m644 postgres_fe.h "${pkgdir}/usr/include/postgresql/internal/"
  install -m644 libpq/pqcomm.h "${pkgdir}/usr/include/postgresql/internal/libpq/"
}

package_postgresql-docs() {
  pkgdesc="HTML documentation for PostgreSQL"
  options=(docs)

  cd "${srcdir}/postgresql-${pkgver}"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/postgresql-docs/LICENSE"

  make -C doc/src/sgml DESTDIR="${pkgdir}" install-html
  chown -R root:root "${pkgdir}/usr/share/doc/postgresql/html/"

  # clean up
  rmdir "${pkgdir}"/usr/share/man/man{1,3,7}
  rmdir "${pkgdir}"/usr/share/man
}

package_postgresql() {
  pkgdesc="A sophisticated object-relational DBMS"
  backup=('etc/conf.d/postgresql' 'etc/pam.d/postgresql' 'etc/logrotate.d/postgresql')
  depends=("postgresql-libs>=${pkgver}" 'libxml2' 'readline>=6.0' 'openssl>=1.0.0')
  optdepends=('python2: for PL/Python support'
              'perl: for PL/Perl support'
              'postgresql-old-upgrade: upgrade from previous major version using pg_upgrade')
  install=postgresql.install

  cd "${srcdir}/postgresql-${pkgver}"

  # install
  make DESTDIR="${pkgdir}" install
  make -C contrib DESTDIR="${pkgdir}" install
  make -C doc/src/sgml DESTDIR="${pkgdir}" install-man
  # we don't want this, it is in the -libs package
  make -C src/bin/pg_config DESTDIR="${pkgdir}" uninstall
  rm "${pkgdir}/usr/share/man/man1/pg_config.1"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/${pkgbase}/LICENSE"

  # clean up unneeded installed items
  rm -rf "${pkgdir}/usr/include/postgresql/internal"
  rm -rf "${pkgdir}/usr/include/libpq"
  find "${pkgdir}/usr/include" -maxdepth 1 -type f -execdir rm {} +
  rmdir "${pkgdir}/usr/share/doc/postgresql/html"

  # install launch script
  install -D -m755 "${srcdir}/postgresql" "${pkgdir}/etc/rc.d/postgresql"

  # install conf file
  install -D -m644 ${srcdir}/postgresql.confd \
    "${pkgdir}/etc/conf.d/postgresql"

  install -D -m644 ${srcdir}/postgresql.pam \
    "${pkgdir}/etc/pam.d/postgresql"

  install -D -m644 ${srcdir}/postgresql.logrotate \
    "${pkgdir}/etc/logrotate.d/postgresql"
}

md5sums=('061a9f17323117c9358ed60f33ecff78'
         '4a6e9b63f2aa50195735f2b46aba040b'
         '2bef962971bac882022c41a72efabb96'
         '96f82c38f3f540b53f3e5144900acf17'
         'd28e443f9f65a5712c52018b84e27137')
sha256sums=('c794016bcf3fc1f561bc86796c6c9d050e3d8d2d55356515126e048275ae56f2'
            'b2931d7a719e765f14811b9109310b2418d3064bfcedef699573fc25854a2201'
            '92fe999c34824a9b8fe4b7212d58f60247e3d0514dd38807758eac64c4198190'
            '57dfd072fd7ef0018c6b0a798367aac1abb5979060ff3f9df22d1048bb71c0d5'
            '6abb842764bbed74ea4a269d24f1e73d1c0b1d8ecd6e2e6fb5fb10590298605e')
