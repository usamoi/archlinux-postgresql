# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Dan McGee <dan@archlinux.org>

pkgbase=postgresql
pkgname=('postgresql-libs' 'postgresql-docs' 'postgresql')
pkgver=16.1
_majorver=${pkgver%.*}
pkgrel=5
pkgdesc='Sophisticated object-relational DBMS'
url='https://www.postgresql.org/'
arch=('x86_64')
license=('custom:PostgreSQL')
makedepends=('krb5' 'libxml2' 'python' 'perl' 'tcl' 'openssl' 'pam' 'zlib'
             'icu' 'systemd' 'libldap' 'llvm' 'clang' 'libxslt' 'util-linux')
source=(https://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2
        0001-Set-DEFAULT_PGSOCKET_DIR-to-run-postgresql.patch
        0002-Force-RPATH-to-be-used-for-the-PL-Perl-plugin.patch
        postgresql.pam
        postgresql.logrotate
        postgresql.service
        postgresql-check-db-dir.in
        postgresql.sysusers
        postgresql.tmpfiles
        libxml2-2.12.patch
        openssl3.2.patch)
md5sums=('9cbfb9076ed06384471802b850698a6d'
         '6ce1dab3da98a10f9190e6b3037f93aa'
         '632e22e96d6ace85b76a380487cfbf8c'
         '96f82c38f3f540b53f3e5144900acf17'
         'd28e443f9f65a5712c52018b84e27137'
         '0418d1b78f15b969c1575657d47ab130'
         '10123caec61006cbe316de8ab7a497bc'
         'da6a0dda84638d0c3463abf4bc01e621'
         '02d017978f0bba21f455feceb3f0a45a'
         '9840030511edcd07a363299293ff3aca'
         'e5b0bb9f9cda8781756873131597de75')
sha256sums=('ce3c4d85d19b0121fe0d3f8ef1fa601f71989e86f8a66f7dc3ad546dd5564fec'
            '4d5a1020626d6cdd8eabbcb54e71d719a8d4cf0228f20173d16a86b374d32acd'
            'e37e54caead0568b3605b14314faa62732b6ef188ee18edf2e73459795125717'
            '57dfd072fd7ef0018c6b0a798367aac1abb5979060ff3f9df22d1048bb71c0d5'
            '6abb842764bbed74ea4a269d24f1e73d1c0b1d8ecd6e2e6fb5fb10590298605e'
            '25fb140b90345828dc01a4f286345757e700a47178bab03d217a7a5a79105b57'
            '94af93b53bf7772e6664c239523ef952ffc905a0de3c2c4b2dfc2fe8f3a2efed'
            '3cfe36dd202af56b3ef8e6d6a746b24e6f46f0d9e0d3fa125dbfb5e598170afb'
            '4a4c0bb9ceb156cc47e9446d8393d1f72b4fe9ea1d39ba17213359df9211da57'
            'd63e66b46fbe845998c1f4bdf0f74dd5298d2384fceaae055ef28bcf655e2728'
            'efb5473a368f3b228acbc8776282ecb00f7a0f00ca632aaec10848bcbb177628')
b2sums=('f59859af644134cf0fc9289c0e0d93fe0f877794a1cc8881280d0439605a6e312866a0114d453af8e269e26173fa3742073fe5485901b7cb0af925a5c3506aad'
        '283b5a025a3a5ed500317b7a0b8fa9af66816bc7c6a59a90d826e4e8420f9631d41b7219617d63e2c20e58e553bfe715d3b6d31dd3ed3ec07233a7f178dba368'
        '920d9601b0573e086045ecf699122afa81f02f1e88490268ccf5ab880ba4e36990b9093628db8520cf23433ae813d2ad81e692b03600f58bbc0b73a3d3e1a70b'
        '3eab84d332d96678fe6e435ee243c8f1a82b838f601d61d3604d11e918aed7a62202edca5e476c4b9031ed284570e6fcd6c659cfdbd9624aa0019d3233755f81'
        '2209b7550acad7955102ec6922754b4046b2a2ad2a7e1cfb2cc4053c0705abac7aa7d7968eab617f50894797d06345f51c9a669926bd2a77dcf688206a2027e0'
        'a4255df47b7ac1418d20aa73aa0f6e70c7952a10d706e5523043c48b2c3b6d8e39838049dfcc826913cd0f2c06502561d1abe8b19cce7071db66139ae93a37bf'
        '682bfab1189221d82f6cce9bcb8c40c6d37ecacbcaf0568bbaa2706503b1f102e5476d0d110dcc6240ab62c5e7c2c20a5d9af8edd4038d212327f8866b1cf15a'
        'e025e78797879323bb78752dff8ea4eae984015240d273de8e83e1b7f7f7a8b9a549c33df6b72c40721f05a3df149fe065f9bb8c3fdf4f3b9f759963bb3da288'
        '8a8e5ec24ea338b2b51b8d2be5a336ac8d4cc6b25200ed0f0d564df9942997478df0c54da2fac7b27d677774a34398f69047eecd0f97bdc0df8fe50a1b5ed74d'
        'acfe2d4381065715cbcd7de8393015bb3341fabd7301ca2a8b9cde4e33e524842f4ea0d97c4164ebbcbb980b1dc0cbe5f0fa031b33d78f7932fea97825a6dea3'
        'b5c2c1f662ff5b531f24d15388195a95b25fd09e8c3467c5c9f2ed0858693c34332c9c87d69da0312348669c4b993353e2cdeefebc826dfd4d0a3ec83d11f940')

# Upstream provides md5 and sha256

prepare() {
  cd postgresql-${pkgver}
  patch -p1 < ../0001-Set-DEFAULT_PGSOCKET_DIR-to-run-postgresql.patch
  patch -p1 < ../0002-Force-RPATH-to-be-used-for-the-PL-Perl-plugin.patch
  patch -p1 < ../libxml2-2.12.patch
  patch -p1 < ../openssl3.2.patch
}

build() {
  cd postgresql-${pkgver}
  local configure_options=(
    --prefix=/usr
    --mandir=/usr/share/man
    --datadir=/usr/share/postgresql
    --sysconfdir=/etc
    --with-gssapi
    --with-libxml
    --with-openssl
    --with-perl
    --with-python
    --with-tcl
    --with-pam
    --with-readline
    --with-system-tzdata=/usr/share/zoneinfo
    --with-uuid=e2fs
    --with-icu
    --with-systemd
    --with-ldap
    --with-llvm
    --with-libxslt
    --with-lz4
    --with-zstd
    --enable-nls
    --enable-thread-safety
    --disable-rpath
  )

  # Fix static libs
  CFLAGS+=" -ffat-lto-objects"

  ./configure "${configure_options[@]}"
  make world
}

_postgres_check() {
  make "${1}" || (find . -name regression.diffs | \
    while read -r line; do
      echo "make ${1} failure: ${line}"
      cat "${line}"
    done; exit 1)
}

check() {
  export LANG=C
  cd postgresql-${pkgver}
  _postgres_check check
  _postgres_check check-world
}

package_postgresql-libs() {
  pkgdesc="Libraries for use with PostgreSQL"
  depends=('krb5' 'openssl' 'readline' 'zlib' 'libldap')
  provides=('postgresql-client' 'libpq.so' 'libecpg.so' 'libecpg_compat.so' 'libpgtypes.so')
  conflicts=('postgresql-client')

  cd postgresql-${pkgver}

  # install license
  install -Dm 644 COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"

  # install libs and non-server binaries
  for dir in src/interfaces src/bin/pg_config src/bin/pg_dump src/bin/psql src/bin/scripts; do
    make -C ${dir} DESTDIR="${pkgdir}" install
  done

  for util in pg_config pg_dump pg_dumpall pg_restore psql \
      clusterdb createdb createuser dropdb dropuser pg_isready reindexdb vacuumdb; do
    install -Dm 644 doc/src/sgml/man1/${util}.1 "${pkgdir}"/usr/share/man/man1/${util}.1
  done

  cd src/include

  install -d "${pkgdir}"/usr/include/{libpq,postgresql/internal/libpq}

  # these headers are needed by the public headers of the interfaces
  install -m 644 pg_config.h "${pkgdir}/usr/include"
  install -m 644 pg_config_os.h "${pkgdir}/usr/include"
  install -m 644 pg_config_ext.h "${pkgdir}/usr/include"
  install -m 644 postgres_ext.h "${pkgdir}/usr/include"
  install -m 644 libpq/libpq-fs.h "${pkgdir}/usr/include/libpq"
  install -m 644 pg_config_manual.h "${pkgdir}/usr/include"

  # these he aders are needed by the not-so-public headers of the interfaces
  install -m 644 c.h "${pkgdir}/usr/include/postgresql/internal"
  install -m 644 port.h "${pkgdir}/usr/include/postgresql/internal"
  install -m 644 postgres_fe.h "${pkgdir}/usr/include/postgresql/internal"
  install -m 644 libpq/pqcomm.h "${pkgdir}/usr/include/postgresql/internal/libpq"
}

package_postgresql-docs() {
  pkgdesc="HTML documentation for PostgreSQL"
  options+=('docs')

  cd postgresql-${pkgver}

  install -Dm 644 COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"

  make -C doc/src/sgml DESTDIR="${pkgdir}" install-html
  chown -R root:root "${pkgdir}/usr/share/doc/postgresql/html"

  # clean up
  rmdir "${pkgdir}"/usr/share/man/man{1,3,7}
  rmdir "${pkgdir}"/usr/share/man
}

package_postgresql() {
  pkgdesc='Sophisticated object-relational DBMS'
  backup=('etc/pam.d/postgresql' 'etc/logrotate.d/postgresql')
  depends=("postgresql-libs>=${pkgver}" 'krb5' 'libxml2' 'readline' 'openssl'
           'pam' 'icu' 'systemd-libs' 'libldap' 'llvm-libs' 'libxslt' 'lz4'
           'zstd')
  optdepends=('python: for PL/Python 3 support'
              'perl: for PL/Perl support'
              'tcl: for PL/Tcl support'
              'postgresql-old-upgrade: upgrade from previous major version using pg_upgrade'
              'logrotate: rotates system logs automatically')
  options+=('staticlibs')
  install=postgresql.install

  cd postgresql-${pkgver}

  # install
  make DESTDIR="${pkgdir}" install
  make -C contrib DESTDIR="${pkgdir}" install
  make -C doc/src/sgml DESTDIR="${pkgdir}" install-man

  # we don't want these, they are in the -libs package
  for dir in src/interfaces src/bin/pg_config src/bin/pg_dump src/bin/psql src/bin/scripts; do
    make -C ${dir} DESTDIR="${pkgdir}" uninstall
  done
  for util in pg_config pg_dump pg_dumpall pg_restore psql \
      clusterdb createdb createuser dropdb dropuser pg_isready reindexdb vacuumdb; do
    rm "${pkgdir}"/usr/share/man/man1/${util}.1
  done

  install -Dm 644 COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"

  cd "${srcdir}"
  sed -e "s/%PGMAJORVERSION%/$_majorver/g" \
      -e "s/%PREVMAJORVERSION%/$((_majorver - 1))/g" \
      postgresql-check-db-dir.in |
    install -Dm 755 /dev/stdin "${pkgdir}/usr/bin/postgresql-check-db-dir"

  install -Dm 644 ${pkgname}.pam "${pkgdir}/etc/pam.d/${pkgname}"
  install -Dm 644 ${pkgname}.logrotate "${pkgdir}/etc/logrotate.d/${pkgname}"

  install -Dm 644 ${pkgname}.service -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 ${pkgname}.sysusers "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
  install -Dm 644 ${pkgname}.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

  # clean up unneeded installed items
  rm -rf "${pkgdir}/usr/include/postgresql/internal"
  rm -rf "${pkgdir}/usr/include/libpq"
  find "${pkgdir}/usr/include" -maxdepth 1 -type f -execdir rm {} +
  rmdir "${pkgdir}/usr/share/doc/postgresql/html"
}

# vim:set sw=2 sts=-1 et:
