# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Dan McGee <dan@archlinux.org>

pkgbase=postgresql
pkgname=(
  'postgresql'
  'postgresql-libs'
  'postgresql-docs'
)
pkgver=16.2
_majorver=${pkgver%.*}
pkgrel=2
pkgdesc='Sophisticated object-relational DBMS'
url='https://www.postgresql.org/'
arch=('x86_64')
license=('PostgreSQL')
depends=(
  'bash'
  'gcc-libs'
  'glibc'
  'icu'
  'krb5'
  'libldap'
  'libxml2'
  'libxslt'
  'llvm-libs'
  'lz4'
  'openssl'
  'pam'
  'readline'
  'systemd-libs'
  'util-linux-libs'
  'zlib'
  'zstd'
)
makedepends=(
  'clang'
  'docbook-xsl'
  'git'
  'llvm'
  'meson'
  'perl'
  'perl-ipc-run'
  'python'
  'systemd'
  'tcl'
  'util-linux'
)
source=(
  "git+https://git.postgresql.org/git/postgresql.git#tag=REL_${pkgver//./_}"
  0001-Set-DEFAULT_PGSOCKET_DIR-to-run-postgresql.patch
  postgresql-check-db-dir.in
  postgresql.logrotate
  postgresql.pam
  postgresql.service
  postgresql.sysusers
  postgresql.tmpfiles
)
md5sums=('6b2005d70080ab3abc2b4a7bd083f260'
         '6ce1dab3da98a10f9190e6b3037f93aa'
         '10123caec61006cbe316de8ab7a497bc'
         'd28e443f9f65a5712c52018b84e27137'
         '96f82c38f3f540b53f3e5144900acf17'
         '026a3dbde1a0b2909120f0012237a33d'
         'da6a0dda84638d0c3463abf4bc01e621'
         '9138244e0e6b98acc770931a4b150b86')
sha256sums=('484162ef6c0a6787660a7127ff1eec5efb3597272f8dd043050b6176d488f99e'
            '4d5a1020626d6cdd8eabbcb54e71d719a8d4cf0228f20173d16a86b374d32acd'
            '94af93b53bf7772e6664c239523ef952ffc905a0de3c2c4b2dfc2fe8f3a2efed'
            '6abb842764bbed74ea4a269d24f1e73d1c0b1d8ecd6e2e6fb5fb10590298605e'
            '57dfd072fd7ef0018c6b0a798367aac1abb5979060ff3f9df22d1048bb71c0d5'
            '5c23d7741bcd429d9552258decbcb1935febd671c237c1c99b9248dff4838eb8'
            '3cfe36dd202af56b3ef8e6d6a746b24e6f46f0d9e0d3fa125dbfb5e598170afb'
            'ea771830c15b24c8725ded92e6a9ba9848b13f722357c5f5857dfeb21985d54c')
b2sums=('4c9e850b345bc8eabf7ddc6bd7f5563cfbba438dd5e8c4e50693e0ea15b034170638df54f5bb5b184e8ca94484baf2ae16ce1b49cc866c619d5c15626032e77f'
        '283b5a025a3a5ed500317b7a0b8fa9af66816bc7c6a59a90d826e4e8420f9631d41b7219617d63e2c20e58e553bfe715d3b6d31dd3ed3ec07233a7f178dba368'
        '682bfab1189221d82f6cce9bcb8c40c6d37ecacbcaf0568bbaa2706503b1f102e5476d0d110dcc6240ab62c5e7c2c20a5d9af8edd4038d212327f8866b1cf15a'
        '2209b7550acad7955102ec6922754b4046b2a2ad2a7e1cfb2cc4053c0705abac7aa7d7968eab617f50894797d06345f51c9a669926bd2a77dcf688206a2027e0'
        '3eab84d332d96678fe6e435ee243c8f1a82b838f601d61d3604d11e918aed7a62202edca5e476c4b9031ed284570e6fcd6c659cfdbd9624aa0019d3233755f81'
        '312c5c877809be46d8823b3a16db6410fb061a3fdc3558710e4921b72886fabe619a49e20a583b9a717771c72dfc8c3575c6e103becf8bb8b9e2d3cdaf6aa3c1'
        'e025e78797879323bb78752dff8ea4eae984015240d273de8e83e1b7f7f7a8b9a549c33df6b72c40721f05a3df149fe065f9bb8c3fdf4f3b9f759963bb3da288'
        'd66fcad8c00ed29cb50cf6721c89023787b42973c3bf50535c4f822a8870883557396b3ec5cc79465e75d0786246a523db69368b2a7feff40c3430c736999cca')

# Upstream provides md5 and sha256

prepare() {
  cd postgresql
  patch -p1 < ../0001-Set-DEFAULT_PGSOCKET_DIR-to-run-postgresql.patch
}

build() {
  local meson_options=(
    -D bonjour=disabled
    -D bsd_auth=disabled
    -D docs_pdf=disabled
    -D llvm=enabled
    -D rpath=false
    -D selinux=disabled
    -D system_tzdata=/usr/share/zoneinfo
    -D uuid=e2fs
  )

  # Fix static libs
  CFLAGS+=" -ffat-lto-objects"

  arch-meson postgresql build "${meson_options[@]}"
  meson compile -C build
  ninja -C build alldocs
}

check() (
  export LANG=C LC_ALL=C
  meson test -C build --print-errorlogs || (
    find . -name regression.diffs | while read -r line; do
      echo "test failure diff: ${line}"
      cat "${line}"
    done
    exit 1
  )
)

_pick() {
  local p="$1" f d; shift
  for f; do
    d="$srcdir/$p/${f#$pkgdir/}"
    mkdir -p "$(dirname "$d")"
    mv "$f" "$d"
    rmdir -p --ignore-fail-on-non-empty "$(dirname "$f")"
  done
}

package_postgresql() {
  pkgdesc='Sophisticated object-relational DBMS'
  backup=(
    'etc/logrotate.d/postgresql'
    'etc/pam.d/postgresql'
  )
  depends+=("postgresql-libs>=${pkgver}")
  optdepends=(
    'logrotate: rotates system logs automatically'
    'perl: for PL/Perl support'
    'postgresql-old-upgrade: upgrade from previous major version using pg_upgrade'
    'python: for PL/Python 3 support'
    'tcl: for PL/Tcl support'
  )
  options+=('staticlibs')
  install=postgresql.install

  meson install -C build --destdir "${pkgdir}"
  DESTDIR="$pkgdir" ninja -C build install-docs

  # Shared libraries
  _pick libs "${pkgdir}"/usr/include/{ecpg,libpq,pg_config,pgtypes,postgres_ext,sql}*
  _pick libs "${pkgdir}"/usr/include/postgresql/{informix,internal}
  _pick libs "${pkgdir}"/usr/lib/lib{ecpg,ecpg_compat,pgtypes,pq}.{a,so*}
  _pick libs "${pkgdir}"/usr/lib/pkgconfig/lib{ecpg,ecpg_compat,pgtypes,pq}.pc
  _pick libs "${pkgdir}"/usr/share/locale/*/LC_MESSAGES/{ecpglib6,libpq5}-*

  # Non-server binaries
  _pick libs "${pkgdir}"/usr/bin/{clusterdb,ecpg,psql,reindexdb,vacuumdb}
  _pick libs "${pkgdir}"/usr/bin/{create,drop}{db,user}
  _pick libs "${pkgdir}"/usr/bin/pg_{config,dump,dumpall,isready,restore}
  _pick libs "${pkgdir}"/usr/share/locale/*/LC_MESSAGES/{ecpg,pg_config,pg_dump,pgscripts,psql}-*
  _pick libs "${pkgdir}"/usr/share/man/man1/{clusterdb,psql,reindexdb,vacuumdb}.1
  _pick libs "${pkgdir}"/usr/share/man/man1/{create,drop}{db,user}.1
  _pick libs "${pkgdir}"/usr/share/man/man1/pg_{config,dump,dumpall,isready,restore}.1
  _pick libs "${pkgdir}"/usr/share/postgresql/{pg_service.conf,psqlrc}.sample

  # Documentation
  _pick docs "${pkgdir}"/usr/share/doc

  sed -e "s/%PGMAJORVERSION%/${_majorver}/g" \
      -e "s/%PREVMAJORVERSION%/$((_majorver - 1))/g" \
      postgresql-check-db-dir.in |
    install -Dm 755 /dev/stdin "${pkgdir}/usr/bin/postgresql-check-db-dir"

  install -Dm 644 ${pkgname}.pam "${pkgdir}/etc/pam.d/${pkgname}"
  install -Dm 644 ${pkgname}.logrotate "${pkgdir}/etc/logrotate.d/${pkgname}"

  install -Dm 644 ${pkgname}.service -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 ${pkgname}.sysusers "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
  install -Dm 644 ${pkgname}.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

  install -Dm 644 postgresql/COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"
}

package_postgresql-libs() {
  pkgdesc="Libraries for use with PostgreSQL"
  depends=(
    'glibc'
    'krb5'
    'libldap'
    'lz4'
    'openssl'
    'readline'
    'zlib'
    'zstd'
  )
  provides=(
    'libecpg.so'
    'libecpg_compat.so'
    'libpgtypes.so'
    'libpq.so'
    'postgresql-client'
  )
  conflicts=('postgresql-client')

  mv libs/* "${pkgdir}"

  install -Dm 644 postgresql/COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"
}

package_postgresql-docs() {
  pkgdesc="HTML documentation for PostgreSQL"
  options+=('docs')

  mv docs/* "${pkgdir}"

  install -Dm 644 postgresql/COPYRIGHT -t "${pkgdir}/usr/share/licenses/${pkgname}"
}

# vim:set sw=2 sts=-1 et:
