# Maintainer: Weston Myers <Weston+Arch at IEEE dot org>
pkgname=distcc-svn
pkgver=r782
pkgrel=1
pkgdesc="A distributed C/C++ compiler"
arch=('i686' 'x86_64')
url="https://code.google.com/p/distcc/"
license=('GPL2')
makedepends=('gtk2'
             'python>=2.2'
             'subversion')
depends=('avahi'
         'dbus'
         'popt')
optdepends=('gtk2: distccmon-text/distccmon-gnome'
            'libglade>=2.0: distccmon-text/distccmon-gnome'
            'python>=2.4: pump-mode functionality'
            'linuxdoc-tools: rebuild documentation from its SGML source')
backup=('etc/conf.d/distccd'
	'etc/distcc/hosts')
conflicts=('distcc')
source=("$pkgname"::'svn+http://distcc.googlecode.com/svn/trunk'
        'distccd.conf.d'
	'distccd.service')
md5sums=('SKIP'
         '5f030cf67956a43fd750f2b2fbe01437'
         '6cdc082e9af8fd0c93b5669eef93b017')
pkgver() {
  cd "$srcdir/$pkgname"
  local ver="$(svnversion)"
  printf "r%s" "${ver//[[:alpha:]]}"
}
build() {
  cd "$srcdir/$pkgname"
  ./autogen.sh
  ./configure --prefix=/usr \
              --sysconfdir=/etc \
              --disable-Werror \
              --with-gtk \
              --mandir=/usr/share/man
  make
}
package() {
  cd "$srcdir/$pkgname"
  make DESTDIR="$pkgdir/" install
  install -D -m644 ${srcdir}/distccd.conf.d ${pkgdir}/etc/conf.d/distccd
  mkdir -p $pkgdir/usr/lib/distcc/bin
  ln -sv /usr/bin/distcc $pkgdir/usr/lib/distcc/bin/cc
  ln -sv /usr/bin/distcc $pkgdir/usr/lib/distcc/bin/cpp
  ln -sv /usr/bin/distcc $pkgdir/usr/lib/distcc/bin/g++
  ln -sv /usr/bin/distcc $pkgdir/usr/lib/distcc/bin/gcc
  install -Dm0644 $srcdir/distccd.service $pkgdir/usr/lib/systemd/system/distccd.service
}