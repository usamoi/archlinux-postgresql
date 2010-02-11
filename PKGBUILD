# Maintainer: Douglas Soares de Andrade <douglas@archlinux.org>

pkgname=postgresql
pkgver=8.4.2
pkgrel=3
pkgdesc="A sophisticated object-relational DBMS"
arch=('i686' 'x86_64')
url="http://www.postgresql.org"
license=('BSD')
backup=('etc/conf.d/postgresql' 'etc/pam.d/postgresql')
depends=("postgresql-libs>=${pkgver}" "libxml2" 'readline>=6.0')
optdepends=('python: for PL/Python support')
options=('!makeflags')
source=(ftp://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2 \
        postgresql postgresql.confd build.patch postgresql.pam)

build() {
  cd ${srcdir}/${pkgname}-${pkgver} || return 1

  # patch to remove regress/test make target (won't build with it present)
  patch -Np1 -i ../build.patch || return 1

  # configure
  ./configure --prefix=/usr --mandir=/usr/share/man \
  --with-docdir=/usr/share/doc --with-openssl --with-python \
  --datadir=/usr/share/postgresql --with-pam --with-libxml || return 1

  sed -i -e '/interfaces/d' src/Makefile || return 1

  # build
  make || return 1

  # install
  make DESTDIR=${pkgdir} install || return 1

  (
  cd contrib/adminpack;
  make || return 1;
  make DESTDIR=${pkgdir} install || return 1;
  );


  # clean up unneeded installed items
  rm -rf ${pkgdir}/usr/include/postgresql/internal || return 1
  rm -rf ${pkgdir}/usr/include/libpq || return 1
  # the below line is expected to produce an error; fix this one day
  #rm -f ${pkgdir}/usr/include/*
  find ${pkgdir}/usr/include -maxdepth 1 -type f -execdir rm {} + || return 1
  rm -f ${pkgdir}/usr/bin/pg_config || return 1
  
  # Maintaining the lib below because of qt
  #rm -f ${pkgdir}/usr/lib/libpgport.a || return 1

  # install launch script
  install -D -m755 ../postgresql ${pkgdir}/etc/rc.d/postgresql \
  	|| return 1

  # install license
  install -D -m644 COPYRIGHT ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE || return 1

  # install conf file
  install -D -m644 ${srcdir}/postgresql.confd \
    ${pkgdir}/etc/conf.d/postgresql || return 1

  install -D -m644 ${srcdir}/postgresql.pam \
    ${pkgdir}/etc/pam.d/postgresql

  chown root:root ${pkgdir}/usr/share/doc/postgresql/html/*
}
md5sums=('d738227e2f1f742d2f2d4ab56496c5c6'
         '2c9a8bf7942b627931e3242daa6c58d4'
         'd63a05943cf0a2726aa2070f3033c569'
         '4d74f4227dc5e12bf95b3490758d86c9'
         '96f82c38f3f540b53f3e5144900acf17')
