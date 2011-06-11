# Maintainer: Steven Merrill <steven.merrill@gmail.com>

pkgname=handlersocket-mysql
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

_mysqlpkgver=5.5.13
_gitroot="https://github.com/ahiguti/HandlerSocket-Plugin-for-MySQL"
_gitname=${pkgname}

source=("http://ftp.gwdg.de/pub/misc/mysql/Downloads/MySQL-5.5/mysql-${_mysqlpkgver}.tar.gz"
        "handlersocket.install")
md5sums=('f0e519e90ee7c00fceb0730edf859d7b'
         'b7a138323938a9a8028661aff7f34465')

build() {
  cd ${srcdir}
  msg "Connecting to git server...."

  if [[ -d ${_gitname} ]] ; then
    cd ${_gitname} && git pull origin
    msg "The local files are updated."
  else
    git clone ${_gitroot} ${_gitname}
  fi

  msg "git checkout done or server timeout"
  msg "Starting make..."

  rm -rf ${srcdir}/${_gitname}-build
  git clone ${srcdir}/${_gitname} ${srcdir}/${_gitname-build}
  cd ${srcdir}/${_gitname}-build

  ./autogen.sh
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --with-mysql-source=${srcdir}/mysql-${_mysqlpkgver} --with-mysql-bindir=/usr/bin
  make
}

package() {
  cd ${srcdir}/${_gitname}-build

  make DESTDIR=${pkgdir} install

  #license
  install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
}

