# Maintainer: Dan McGee <dan@archlinux.org>

pkgbase=postgresql
pkgname=('postgresql-libs' 'postgresql-docs' 'postgresql')
pkgver=9.2.0
_majorver=${pkgver%.*}
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.postgresql.org/"
license=('custom:PostgreSQL')
makedepends=('krb5' 'libxml2' 'python2' 'perl' 'tcl' 'openssl>=1.0.0')
source=(http://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2
        postgresql.rcd postgresql.confd postgresql.pam postgresql.logrotate
        postgresql.service postgresql-check-db-dir)
md5sums=('8c4c32a4abe8cf61b02c8366181ede50'
         '1ddd1df8010549f237e7983bb326025e'
         'a54d09a20ab1672adf08f037df188d53'
         '96f82c38f3f540b53f3e5144900acf17'
         'd28e443f9f65a5712c52018b84e27137'
         'f0d46e63198db0a1e51dcd4a0599cd33'
         '505e0e4abfc746cae9558584d471a03c')
sha256sums=('3731c607df492bbb57f37917b49f57719c0d6f823720426431fff10d82b1fe33'
            '9f6307b1358892e304f9474a456f0cb9160cfb8812a9da0430abe647f8a9cf45'
            '3de5c059eead8816db15c2c5588e6196d6c4b0d704faf1a20912796cf589ba81'
            '57dfd072fd7ef0018c6b0a798367aac1abb5979060ff3f9df22d1048bb71c0d5'
            '6abb842764bbed74ea4a269d24f1e73d1c0b1d8ecd6e2e6fb5fb10590298605e'
            'af41dd8c1e6b124880fb4347c9fa4adabdef5b6e6bd13601cac25eb9e7bc7774'
            '3a3279d290f556bf7a362670e32b491794f47ed218f6b8c6acef366a3291f669')

build() {
  cd "${srcdir}/postgresql-${pkgver}"

  ./configure --prefix=/usr \
  --mandir=/usr/share/man \
  --datadir=/usr/share/postgresql \
  --with-krb5 \
  --with-libxml \
  --with-openssl \
  --with-perl \
  --with-python PYTHON=/usr/bin/python2 \
  --with-tcl \
  --with-pam \
  --with-system-tzdata=/usr/share/zoneinfo \
  --enable-nls \
  --enable-thread-safety

  make world
}

package_postgresql-libs() {
  pkgdesc="Libraries for use with PostgreSQL"
  depends=('krb5' 'openssl>=1.0.0' 'readline>=6.0')
  provides=('postgresql-client')
  conflicts=('postgresql-client')

  cd "${srcdir}/postgresql-${pkgver}"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/postgresql-libs/LICENSE"

  # install libs
  for dir in src/interfaces src/bin/pg_config src/bin/psql; do
    make -C ${dir} DESTDIR="${pkgdir}" install
  done

  install -D -m644 doc/src/sgml/man1/pg_config.1 "${pkgdir}/usr/share/man/man1/pg_config.1"
  install -D -m644 doc/src/sgml/man1/psql.1 "${pkgdir}/usr/share/man/man1/psql.1"

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
  depends=("postgresql-libs>=${pkgver}" 'krb5' 'libxml2' 'readline>=6.0' 'openssl>=1.0.0')
  optdepends=('python2: for PL/Python support'
              'perl: for PL/Perl support'
              'tcl: for PL/Tcl support'
              'postgresql-old-upgrade: upgrade from previous major version using pg_upgrade')
  install=postgresql.install

  cd "${srcdir}/postgresql-${pkgver}"

  # install
  make DESTDIR="${pkgdir}" install
  make -C contrib DESTDIR="${pkgdir}" install
  make -C doc/src/sgml DESTDIR="${pkgdir}" install-man

  # we don't want these, they are in the -libs package
  for dir in src/interfaces src/bin/pg_config src/bin/psql; do
    make -C ${dir} DESTDIR="${pkgdir}" uninstall
  done
  rm "${pkgdir}/usr/share/man/man1/pg_config.1"
  rm "${pkgdir}/usr/share/man/man1/psql.1"

  # install license
  install -D -m644 COPYRIGHT "${pkgdir}/usr/share/licenses/${pkgbase}/LICENSE"

  # clean up unneeded installed items
  rm -rf "${pkgdir}/usr/include/postgresql/internal"
  rm -rf "${pkgdir}/usr/include/libpq"
  find "${pkgdir}/usr/include" -maxdepth 1 -type f -execdir rm {} +
  rmdir "${pkgdir}/usr/share/doc/postgresql/html"

  # install launch script
  install -D -m755 "${srcdir}/postgresql.rcd" "${pkgdir}/etc/rc.d/postgresql"
  install -D -m644 "${srcdir}/postgresql.service" \
    "${pkgdir}/usr/lib/systemd/system/postgresql.service"
  install -D -m755 "${srcdir}/postgresql-check-db-dir" \
    "${pkgdir}/usr/bin/postgresql-check-db-dir"

  # install conf file
  install -D -m644 ${srcdir}/postgresql.confd \
    "${pkgdir}/etc/conf.d/postgresql"

  install -D -m644 ${srcdir}/postgresql.pam \
    "${pkgdir}/etc/pam.d/postgresql"

  install -D -m644 ${srcdir}/postgresql.logrotate \
    "${pkgdir}/etc/logrotate.d/postgresql"
}
