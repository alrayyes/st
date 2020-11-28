# Maintainer: Ryan Kes <alrayyes gmail com>

pkgname=higherlearning-st
pkgver=0.8.4
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'libxext' 'otf-nerd-fonts-fira-code')
makedepends=('ncurses')
conflicts=('st')
url="http://st.suckless.org"

_patches=("st-clipboard-0.8.3.diff"
          "st-scrollback-20200419-72e3f6c.diff"
          "st-scrollback-mouse-20191024-a2c479c.diff"
          "st-scrollback-mouse-altscreen-20200416-5703aa0.diff"
          "st-vertcenter-20180320-6ac8c8a.diff"
          "st-alpha-0.8.2.diff"
          )

source=("http://dl.suckless.org/st/st-$pkgver.tar.gz"
        "config.h"
        "${_patches[@]}")

sha256sums=('d42d3ceceb4d6a65e32e90a5336e3d446db612c3fbd9ebc1780bc6c9a03346a6'
            '2842d6e1030ff0afad7420c4a93062e6da4e498295222005eae2581a38c45913'
            '0f5ce33953abce74a9da3088ea35bf067a9a4cfeb9fe6ea9800268ce69e436c0'
            '1e41fe17a5ef5a8194eea07422b49d815e2c2bb4d58d84771f793be423005310'
            '319458d980195d18fa0f81a6898d58f8d046c5ff982ab872d741f54bb60e267d'
            'cb87eb654985da46ff63663407184402393ad3d3013c8795570552fe56a15b9d'
            '04e6a4696293f668260b2f54a7240e379dbfabbc209de07bd5d4d57e9f513360'
            '9c5b4b4f23de80de78ca5ec3739dc6ce5e7f72666186cf4a9c6b614ac90fb285')

prepare() {
  cd $srcdir/st-$pkgver
  # skip terminfo which conflicts with nsurses
  sed -i '/\ttic -sx st.info/d' Makefile

  for patch in "${_patches[@]}"; do
    echo "Applying patch $(basename $patch)..."
    patch -Np1 -i "$srcdir/$(basename $patch)"
  done

  cp $srcdir/config.h config.h
}

build() {
  cd $srcdir/st-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/st-$pkgver
  make PREFIX=/usr DESTDIR="$pkgdir" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
}
