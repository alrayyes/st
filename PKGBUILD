# Maintainer: Ryan Kes <alrayyes gmail com>

pkgname=higherlearning-st
pkgver=0.8.2
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'libxext' 'otf-nerd-fonts-fira-code')
makedepends=('ncurses')
conflicts=('st')
url="http://st.suckless.org"

_patches=("st-clipboard-0.8.2.diff"
          "st-scrollback-20190122-3be4cf1.diff"
          "st-scrollback-mouse-0.8.diff"
          "st-vertcenter-20180320-6ac8c8a.diff"
          "local-st-alpha-0.8.2.diff"
          "local-disable-bold-italic-fonts.diff")

source=("http://dl.suckless.org/st/st-$pkgver.tar.gz"
        "config.h"
        "${_patches[@]}")

sha256sums=('aeb74e10aa11ed364e1bcc635a81a523119093e63befd2f231f8b0705b15bf35'
            '2842d6e1030ff0afad7420c4a93062e6da4e498295222005eae2581a38c45913'
            '7be1a09831f13361f5659aaad55110bde99b25c8ba826c11d1d7fcec21f32945'
            '30c9bcec5801614dd5cc8b96f470d7431e83d5d2af87bb2305df60082e5ab4ed'
            '3fb38940cc3bad3f9cd1e2a0796ebd0e48950a07860ecf8523a5afd0cd1b5a44'
            '04e6a4696293f668260b2f54a7240e379dbfabbc209de07bd5d4d57e9f513360'
            '67b1bdc717e3f7914d04f0c72bc8a3f6efe91790248611c0a2c2dc905bf206bd'
            '59d5719a68e2f0e25c44b6ad9fab0d62ee8a6c5bcbcffb38176e9950cda16b15')

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
