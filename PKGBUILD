pkgname=libfprint-elan3104-git
pkgver=1.94.5
pkgrel=1
pkgdesc="Fingerprint library with community patch for ELAN 3104 / 04f3:3104 sensors"
arch=('x86_64')
url="https://github.com/r4nd3l/elan-3104-fingerprint-linux"
license=('LGPL')
provides=('libfprint' 'libfprint-2.so=2-64')
conflicts=('libfprint')
depends=('libgusb' 'nss' 'pixman' 'opencv' 'glib2' 'systemd')
makedepends=('git' 'meson' 'ninja' 'gobject-introspection')

source=(
  "libfprint::git+https://github.com/goodix-fp-linux-dev/libfprint.git#commit=07306bbc9256942595e31fb0f407b364ffa24d07"
  "elan-3104::git+https://github.com/r4nd3l/elan-3104-fingerprint-linux.git"
)
sha256sums=('SKIP' 'SKIP')

prepare() {
  cd "$srcdir/libfprint"
  git apply "$srcdir/elan-3104/patches/elanspi-3104-sigfm.patch"
  sed -i "s/dependency('opencv4'/dependency('opencv5'/g" libfprint/sigfm/meson.build
}

build() {
  cd "$srcdir/libfprint"
  meson setup build \
    --prefix=/usr \
    --libdir=/usr/lib \
    -Ddrivers=elanspi \
    -Dudev_rules_dir=/usr/lib/udev/rules.d \
    -Ddoc=false
  ninja -C build
}

package() {
  cd "$srcdir/libfprint"
  DESTDIR="$pkgdir" ninja -C build install
}
