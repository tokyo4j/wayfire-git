# Maintainer: Solomon Choina <shlomochoina@gmail.com>
# Co-Maintainer: Frank Tao <frank.tao@uwaterloo.ca>
# Co-Maintainer: Christopher Snowhill <kode54@gmail.com>
pkgname=wayfire-git
pkgver=0.8.1.r314.g05831570
pkgrel=1
pkgdesc="3D wayland compositor"
arch=('x86_64')
url="https://github.com/WayfireWM/wayfire"
license=(MIT)
depends=(
         # Wayfire
         'cairo' 'pango' 'libdrm' 'libevdev'
         'libglvnd' 'libjpeg' 'libpng'
         'libxkbcommon' 'pixman' 'polkit'
         'seatd' 'xorg-xwayland' 'wayland'
         'wf-config-git'

         # wlroots
         'glslang' 'libinput' 'libdisplay-info'
         'libxcb' 'opengl-driver'
         'xcb-util-errors' 'xcb-util-renderutil'
         'xcb-util-wm' 'libpixman-1.so' 'libseat.so'
         'libudev.so' 'libvulkan.so' 'libwayland-client.so'
         'libwayland-server.so' 'libxkbcommon.so'
)
makedepends=('git' 'meson' 'ninja' 'cmake' 'vulkan-headers' 'doctest'
             'pkgconf' 'wayland-protocols' 'nlohmann-json' 'libxml2' 'glm'
)
optdepends=('xorg-xeyes')
provides=("${pkgname%-git}" 'wlroots' 'wlroots-git' 'libwlroots.so')
conflicts=("${pkgname%-git}" 'wlroots-git')
replaces=()
options=()

source=('git+https://github.com/WayfireWM/wayfire')
sha256sums=('SKIP')

pkgver() {
    cd "$srcdir/wayfire"
    tag=$(git tag -l | awk '/^[0-9.]+$/ {print $0} /^v{1}[0-9.]+$/ {print substr($0,2)}'|sort -n|tail -n1)
    printf "$tag.r%s.g%s" "$(git rev-list --count v${tag}..HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
    cd "$srcdir/wayfire/"
    arch-meson \
        --buildtype=release \
        -Dxwayland=auto \
        -Duse_system_wlroots=disabled \
        -Duse_system_wfconfig=enabled \
        -Db_lto=true \
        -Db_pie=true \
        build
    sed "/WF_SRC_DIR/d" -i build/config.h
    ninja -C build
}


package() {
    cd "$srcdir/wayfire"
    DESTDIR="$pkgdir/" ninja -C build install
    install -Dm644 wayfire.desktop $pkgdir/usr/share/wayland-sessions/wayfire.desktop
    cp wayfire.ini $pkgdir/usr/share
    install -Dm644 "LICENSE" \
        "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
