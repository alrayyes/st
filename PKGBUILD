# Maintainer: Ryan Kes <alrayyes gmail com>

pkgname=higherlearning-st
pkgver=0.8.4
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft' 'ttf-jetbrains-mono')
makedepends=('ncurses')
conflicts=('st')
url="http://st.suckless.org"

_patches=("st-clipboard-0.8.3.diff"
          "st-scrollback-20200419-72e3f6c.diff"
          "st-scrollback-mouse-20191024-a2c479c.diff"
          "st-scrollback-mouse-altscreen-20200416-5703aa0.diff"
          "st-vertcenter-20180320-6ac8c8a.diff"
          "st-alpha-0.8.2.diff"
          "st-ligatures-alpha-scrollback-20200430-0.8.3.diff"
          "st-font2-20190416-ba72400.diff"
          "st-xresources-20200604-9ba7ecf.diff"
          )

source=("http://dl.suckless.org/st/st-$pkgver.tar.gz"
        "config.h"
        "${_patches[@]}")

sha256sums=('d42d3ceceb4d6a65e32e90a5336e3d446db612c3fbd9ebc1780bc6c9a03346a6'
            'fc89e358946dd837b1c4edafdb1887d51b99c81ca3eb66e7432985f2defab174'
            '0f5ce33953abce74a9da3088ea35bf067a9a4cfeb9fe6ea9800268ce69e436c0'
            '1e41fe17a5ef5a8194eea07422b49d815e2c2bb4d58d84771f793be423005310'
            '319458d980195d18fa0f81a6898d58f8d046c5ff982ab872d741f54bb60e267d'
            'cb87eb654985da46ff63663407184402393ad3d3013c8795570552fe56a15b9d'
            '04e6a4696293f668260b2f54a7240e379dbfabbc209de07bd5d4d57e9f513360'
            '9c5b4b4f23de80de78ca5ec3739dc6ce5e7f72666186cf4a9c6b614ac90fb285'
            'a7b3681d849fd8cca75cd38022a14fef2e26dc085bed1ebd8dcf0a65baa14898'
            '7279c787dba23d72a8a0a4613c0917e03d0087f0254f56a530cd9c521857d20b'
            '5be9b40d2b51761685f6503e92028a7858cc6571a8867b88612fce8a70514d5b')

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
