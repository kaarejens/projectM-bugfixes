# $Id: PKGBUILD 185301 2016-08-05 12:21:25Z andyrtr $
# Maintainer: Alexander F Rødseth <xyproto@archlinux.org>
# Contributor: Lukas Fleischer <lfleischer@archlinux.org>
# Contributor: Alexander Baldeck <alexander@archlinux.org>
# Contributor: Denis A. Altoe Falqueto <denisfalqueto@gmail.com>

pkgbase=projectm
pkgname=('projectm' 'projectm-libvisual' 'projectm-pulseaudio' 'projectm-jack' 'projectm-qt' 'projectm-test')
pkgver=2.1.0
pkgrel=15
arch=('x86_64' 'i686')
url='http://projectm.sourceforge.net/'
license=('LGPL')
makedepends=('mesa-libgl' 'qt4' 'cmake' 'ftgl' 'glew' 'gtkglext' 'libvisual' 'sdl' 'libxext' 'pulseaudio' 'jack')
source=("http://downloads.sourceforge.net/$pkgname/projectM-complete-$pkgver-Source.tar.gz"
        'projectm-test-opengl.patch'
        'projectm-install-vera-ttf.patch'
        'projectm-gcc6.patch'
	'bugfixes-22.02.2015.patch')
sha256sums=('513204f033006bd3dcdf8aada196d816d6b7187266ddcbb1594d0285cc9406ee'
            'c577d8356be011a3b3ee9f9b389db55f47804d100f690d8ea12f2920cdd432d1'
            '7d67aad0b210edf25a527274504c9efdf3e9d5b737235b938fec361ac5a8b110'
            '5f7cd6baef1c90d2a22772c0352a40645c3554c6d75bde41a2b6ec3ebdaa6128'
	    'e5c5ca241ee6dace4990eee76c14f43e15da879729c35fd635c2e732fa7064f7')

prepare() {
  cd "projectM-complete-$pkgver-Source"

  patch -p1 -i ../projectm-test-opengl.patch
  patch -p1 -i ../projectm-install-vera-ttf.patch
  sed 's/projectM_isnan/std::isnan/g' -i src/libprojectM/Renderer/BeatDetect.cpp 
  patch -p1 -i ../projectm-gcc6.patch
  patch -p1 -i ../bugfixes-22.02.2015.patch
}

build() {
  mkdir -p build
  cd build
  cmake \
    -Wno-dev \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release \
    -DINCLUDE-PROJECTM-JACK=ON \
    "../projectM-complete-$pkgver-Source"
  make
}

package_projectm() {
  pkgdesc='Music visualizer which uses 3D accelerated iterative image based rendering'
  depends=('ftgl' 'glew' 'libgl')
  DESTDIR="$pkgdir" make -C "build/src/NativePresets" install
  DESTDIR="$pkgdir" make -C "build/src/libprojectM" install
}

package_projectm-libvisual() {
  pkgdesc='ProjectM plugin for XMMS'
  depends=('projectm' 'libvisual' 'gcc-libs')
  replaces=('libvisual-projectm')
  provides=('libvisual-projectm')
  DESTDIR="$pkgdir" make -C "build/src/projectM-libvisual" install
}

package_projectm-pulseaudio() {
  pkgdesc='ProjectM support for Pulseaudio'
  depends=('projectm-qt' 'pulseaudio')
  DESTDIR="$pkgdir" make -C "$srcdir/build/src/projectM-pulseaudio" install
}

package_projectm-jack() {
  pkgdesc='ProjectM support for Jack'
  depends=('projectm-qt' 'jack')
  DESTDIR="$pkgdir" make -C "$srcdir/build/src/projectM-jack" install
}

package_projectm-qt() {
  pkgdesc='Qt bindings for ProjectM'
  depends=('projectm' 'qt4' 'libgl')
  DESTDIR="$pkgdir" make -C "$srcdir/build/src/projectM-qt" install
}

package_projectm-test() {
  pkgdesc='ProjectM test applications'
  depends=('projectm' 'sdl' 'libgl')
  DESTDIR="$pkgdir" make -C "$srcdir/build/src/projectM-test" install
}
