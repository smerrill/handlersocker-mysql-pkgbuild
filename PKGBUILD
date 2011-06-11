# Maintainer: Steven Merrill <steven.merrill@gmail.com>

pkgname=handlersocket-percona
pkgver=20110611
pkgrel=1
pkgdesc="A high-performance interface to MySQL's InnoDB read/write threads."
arch=('i686' 'x86_64')

depends=('mysql-clients mysql')
#optdepends=('perl-dbi' 'perl-dbd-mysql')
makedepends=('cmake' 'openssl' 'tcp_wrappers' 'zlib')

license=('GPL')
url="https://github.com/ahiguti/HandlerSocket-Plugin-for-MySQL"
options=('!libtool')
#backup=('etc/mysql/my.cnf')
install=handlersocket.install

_perconapkgver=5.5.12_rel20.3
_gitroot="https://github.com/ahiguti/HandlerSocket-Plugin-for-MySQL"
_gitname=${pkgname}

source=("http://www.percona.com/redir/downloads/Percona-Server-${_perconapkgver%.*_*}/Percona-Server-${_perconapkgver/_rel/-}/source/Percona-Server-${_perconapkgver/_/-}.tar.gz"
        "handlersocket.install")
md5sums=('6e4a48a5d2f291fbb72fac9a609d0e14'
         'b7a138323938a9a8028661aff7f34465')

build() {
  cd "$srcdir"
  msg "Connecting to git server...."

  if [[ -d $_gitname ]] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "git checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  ./autogen.sh
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --with-mysql-source=${srcdir}/Percona-Server-${_perconapkgver/_/-} --with-mysql-bindir=/usr/bin
  make
}

package() {
  cd ${srcdir}

  exit 1

  make DESTDIR=${pkgdir} install

  #license
  install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE


  install -d ${pkgdir}/var/${pkgname}/$(hostname)
  install -D -m755 ${srcdir}/${pkgname}.init ${pkgdir}/etc/rc.d/${pkgname}
  install -D -m755 ${srcdir}/${pkgname}.runit ${pkgdir}/etc/sv/${pkgname}/run
  install -D -m755 ${srcdir}/${pkgname}.log.runit ${pkgdir}/etc/sv/${pkgname}/log/run
  install -D -m755 ${srcdir}/${pkgname}.conf ${pkgdir}/etc/${pkgname}.conf
}

