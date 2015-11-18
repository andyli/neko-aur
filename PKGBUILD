# Maintainer: Alexander Rødseth <rodseth@gmail.com>
# Maintainer: Andy Li <andy@onthewings.net>
# Contributor: Daichi Shinozaki <dsdseg@gmail.com>
# Contributor: Dwight Schauer <dschauer@gmail.com>
# Contributor: Stefan Husmann <stefan-husmann@t-online.de>
# Contributor: Christoph Zeiler <archNOSPAM_at_moonblade.dot.org>
# Contributor: Michael 'manveru' Fellinger <m.fellinger@gmail.com>
# Contributor: Caleb McCombs <erdrick016+aur@gmail.com>
# Contributor: Christian Hesse <arch@eworm.de>

pkgname=neko
pkgver=2.0.0
pkgrel=7
pkgdesc='High-level language which can also be used as an embedded scripting language'
url='http://nekovm.org/'
# The Neko virtual machine is available under the MIT License.
# The Neko runtime libraries (ndll) and other tools bundled are released under various open source licenses as described in https://github.com/HaxeFoundation/neko/blob/master/LICENSE
license=('MIT' 'GPL')
arch=('x86_64' 'i686')
depends=('gc' 'gtk2' 'libmariadbclient' 'sqlite' 'apache')
makedepends=('pkgconfig' 'apr')
options=('!strip')
source=("http://nekovm.org/media/neko-${pkgver}.tar.gz"
        'apache2.4.diff'
        'install.neko.patch')
sha256sums=('47a4714df7a603fc64995fc353f09205e52da4df954b6c6969d7d1b9a00a95d1'
            '25a2dc511bcadbdee7f54fd608fce1c7f69fac1af758e064be3fd3006c261af3'
            '999e85cb77941c908bd4650f9d7177519fb2b35e50cc24f9d8465506c03d8d4f')

# TODO: See if the hacky/unsual build procedures can be cleaned up a bit.
#       This should be done in cooperation with upstream, if possible.

prepare() {
  cd "$pkgname-$pkgver"
  sed 's|= -Wall -O3|+=|g;s|-mkdir|mkdir -p|g;/strip/d' -i Makefile
  sed -i 's/client_r.a/client_r.so/' src/tools/install.neko
  mkdir -p bin
  patch -p2 -i "$srcdir/install.neko.patch"
  patch -p1 -i "$srcdir/apache2.4.diff"
}

build() {
  cd "$pkgname-$pkgver"
  [ $CARCH = x86_64 ] && export CFLAGS+=" -D_64BITS"
  make clean
  # note that the Makefile doesn't support parallel build
  make -j1
}

check() {
  make -C "$pkgname-$pkgver" test
}

package() {
  install -d "$pkgdir/usr/"{bin,lib}
  make -C "$pkgname-$pkgver" INSTALL_PREFIX="$pkgdir/usr" install
  find "$pkgdir/usr/lib" -type f -exec chmod 644 {} \;
  #license
  install -D "$pkgname-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
