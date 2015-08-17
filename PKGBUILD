# Contributor: Adam Dariusz Szkoda <adaszko at gmail dot com>
# Contributor: Egon Geerardyn <egon.geerardyn@gmail.com>
# Contributor: Marcos Heredia <chelqo@gmail.com>
# Maintainer : He Yeshuang <yeshuanghe[at]gmail[dot]com> 

pkgname=texmacs-svn
pkgver=6742
pkgrel=1
pkgdesc="WYSIWYG Qt4 editor and graphical frontend to various CASes,SVN edition"
arch=('i686' 'x86_64')
url='http://texmacs.org/'
license=('GPL3')
depends=('qt4' 'texlive-core' 'guile' 'cairo' 'freetype2' 'imlib2'
         'perl' 'python2' 'libxext'
         'desktop-file-utils' 'shared-mime-info' 'gtk-update-icon-cache')
install=${pkgname}.install
provides=('texmacs')
conflicts=('texmacs' 'texmacs-qt')
optdepends=('gawk: conversion of some files'
            'transfig: convert images using fig2ps')
source=('http://www.texmacs.org/Images/tm_gnu1b.png'
        'texmacs.desktop')
md5sums=('48c15c09000cc38728d847c3a8ffabc0'
         'a1856736b4defd6f3a46cf608b108ef1')
_svntrunk="svn://svn.savannah.gnu.org/texmacs/trunk/src"
_svnmod="texmacs"

build() {
  cd "${srcdir}"
  if [ -d ${_svnmod}/.svn ]; then
    (cd ${_svnmod} && svn up -r ${pkgver})
  else
    svn co ${_svntrunk} --config-dir ./ -r ${pkgver} ${_svnmod}
  fi
  msg "SVN checkout done or server timeout"

    cd $srcdir/texmacs
    QMAKE=/usr/bin/qmake-qt4 \
    MOC=/usr/bin/moc-qt4 \
    UIC=/usr/bin/uic-qt4 \
    RCC=/usr/lib/qt4/bin/rcc \
    ./configure \
	--prefix=/usr \
	--enable-pdf-renderer \
	--enable-optimize \
	--with-freetype \
	--with-imlib2 \
	--with-qt
    make || return 1
}

package() {
    cd $srcdir/texmacs
    make prefix=$pkgdir/usr install

    _appdir=$pkgdir/usr/share/applications
    _pngdir=$pkgdir/usr/share/pixmaps
    _docdir=$pkgdir/usr/share/doc/$pkgname-$pkgver
    _licdir=$pkgdir/usr/share/licenses/$pkgname
    install -dm 755 ${_appdir} ${_pngdir} ${_docdir} ${_licdir}
    install -Dpm 0644 $srcdir/../texmacs.desktop ${_appdir}/
    install -Dpm 0644 $srcdir/../tm_gnu1b.png ${_pngdir}/texmacs.png
    install -Dpm 0644 COMPILE COPYING LICENSE TODO TeXmacs/INSTALL TeXmacs/README TeXmacs/TEX_FONTS ${_docdir}/
    cd ${_licdir} ; ln -s ../../TeXmacs/LICENSE .

    # file corrections
    (cd $pkgdir/usr/share/icons/gnome ; [ -f icon-theme.cache ] && rm *.cache)
    (cd $pkgdir/usr/share/mime ; for f in *; do [ -f $f ] && rm $f; done)
}
