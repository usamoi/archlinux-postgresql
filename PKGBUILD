# $Id: PKGBUILD,v 1.44 2008/02/05 18:31:20 paul Exp $
# Maintainer: Paul Mattal <paul@archlinux.org>
pkgname=postgresql
pkgver=8.3.0
pkgrel=1
pkgdesc="A sophisticated object-relational DBMS"
arch=(i686 x86_64)
license=('BSD')
backup=('etc/conf.d/postgresql')
url="http://www.postgresql.org/"
depends=('postgresql-libs>=8.3.0')
options=('!makeflags')
source=(ftp://ftp.postgresql.org/pub/source/v${pkgver}/postgresql-${pkgver}.tar.bz2 \
        postgresql postgresql.confd build.patch)
md5sums=('53d6816eac7442f9bc8103439ebee22e' '7d8ea2abb6a8cdacf35604bda659a34a'\
         'df6ddf9e2ab4700a3415f17c0f4f4172' '4d74f4227dc5e12bf95b3490758d86c9')

build() {
  cd ${startdir}/src/$pkgname-$pkgver || return 1

  # patch to remove regress/test make target (won't build with it present)
  patch -Np1 -i ../build.patch || return 1

  # configure
  ./configure --prefix=/usr --with-openssl --datadir=/usr/share/postgresql \
  	|| return 1
  sed -i -e '/interfaces/d' src/Makefile || return 1

  # build
  make || return 1

  # install
  make DESTDIR=${startdir}/pkg install || return 1

  # clean up unneeded installed items
  rm -rf $startdir/pkg/usr/include/postgresql/internal || return 1
  rm -rf $startdir/pkg/usr/include/libpq || return 1
  # the below line is expected to produce an error; fix this one day
  rm -f $startdir/pkg/usr/include/*
  rm -f $startdir/pkg/usr/bin/pg_config || return 1
  rm -f $startdir/pkg/usr/lib/libpgport.a || return 1

  # install launch script
  install -D -m755 ../postgresql $startdir/pkg/etc/rc.d/postgresql \
  	|| return 1

  # install license
  install -D -m644 COPYRIGHT $startdir/pkg/usr/share/licenses/$pkgname/LICENSE || return 1

  # install conf file
  install -D -m644 $startdir/src/postgresql.confd \
    $startdir/pkg/etc/conf.d/postgresql || return 1
}
