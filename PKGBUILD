# Maintainer: Thijs Vermeir <thijsvermeir@gmail.com>

_upver=0.10.36
pkgbase=('gstreamer0.10-base')
pkgname=gstreamer0.10-base-git
true && pkgname=('gstreamer0.10-base-git' 'gstreamer0.10-base-plugins-git')
pkgver=20130218
pkgrel=1
pkgdesc="GStreamer Multimedia Framework Base plugin libraries"
arch=(i686 x86_64)
license=('LGPL')
makedepends=('pkgconfig' 'gstreamer0.10-git' 'orc' 'libxv' 'alsa-lib' 'cdparanoia' 'libvisual' 'libvorbis' 'libtheora' 'pango' 'gobject-introspection')
options=(!libtool)
url="http://gstreamer.freedesktop.org/"
groups=('gstreamer')

_gitname=gst-plugins-base
_gitroot=git://anongit.freedesktop.org/gstreamer/${_gitname}

build() {
  cd "${srcdir}"
  if [ -d "${_gitname}" ] ; then
    cd "${_gitname}" && git pull && cd ..
  else
    git clone "${_gitroot}" 
    cd "${_gitname}" && git checkout 0.10 && cd ..
  fi

  rm -rf build
  cp -r ${_gitname} build
  cd build

  sed 's|AM_CONFIG_HEADER|AC_CONFIG_HEADERS|' -i configure.ac
  ./autogen.sh --noconfigure
  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
    --disable-static --enable-experimental --disable-gnome_vfs \
    --with-package-name="GStreamer Base Plugins (Archlinux)" \
    --with-package-origin="http://www.archlinux.org/"

  make
}


package_gstreamer0.10-base-git() {
  pkgdesc="GStreamer Multimedia Framework Base plugin libraries"
  depends=('gstreamer0.10-git' 'orc' 'libxv')
  conflicts=('gstreamer0.10-base')
  provides=("gstreamer0.10-base=${_upver}")

  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}" install
  make -C ext DESTDIR="${pkgdir}" uninstall
}

package_gstreamer0.10-base-plugins-git() {
  pkgdesc="GStreamer Multimedia Framework Base Plugins (gst-plugins-base)"
  depends=("gstreamer0.10-base-git=${pkgver}" 'alsa-lib' 'cdparanoia' 'libvisual' 'libvorbis' 'libtheora' 'pango')
  conflicts=('gstreamer0.10-base-plugins')
  provides=("gstreamer0.10-base-plugins=${_upver}")
  groups=('gstreamer0.10-plugins')

  cd "${srcdir}/build"
  make -C gst-libs DESTDIR="${pkgdir}" install
  make -C ext DESTDIR="${pkgdir}" install
  make -C gst-libs DESTDIR="${pkgdir}" uninstall
}
