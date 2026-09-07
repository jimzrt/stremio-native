pkgname=stremio-native-downmix-git
pkgver=1.0.6.r1.g936ee48
pkgrel=1
pkgdesc="Community-native Stremio desktop client with multichannel dialogue downmixing"
arch=('x86_64')
url="https://github.com/jimzrt/stremio-native"
license=('GPL-3.0-only')
depends=('mpv' 'libtorrent-rasterbar' 'boost' 'gtk3' 'libayatana-appindicator' 'fontconfig' 'freetype2' 'libxkbcommon' 'libxkbcommon-x11' 'libx11' 'libxcb' 'openssl')
makedepends=('git' 'rust' 'cargo' 'clang' 'cmake' 'ninja' 'pkgconf')
provides=('stremio-native')
conflicts=('stremio-native' 'stremio-native-git')
source=("stremio-native::git+https://github.com/jimzrt/stremio-native.git#branch=feature/downmix")
sha256sums=('SKIP')

pkgver() {
    cd "${srcdir}/stremio-native"
    git describe --long --tags --match 'v[0-9]*' | sed -E 's/^v//;s/([^-]+)-([0-9]+)-g/\1.r\2.g/;s/-/./g'
}

prepare() {
    cd "${srcdir}/stremio-native"
    git submodule update --init --recursive
}

build() {
    cd "${srcdir}/stremio-native"
    mv .cargo/config.toml .cargo/config.toml.disabled
    trap 'mv .cargo/config.toml.disabled .cargo/config.toml' EXIT
    RUSTUP_TOOLCHAIN=stable cargo build --locked --release --package stremio-native
}

package() {
    cd "${srcdir}/stremio-native"
    install -Dm755 target/release/stremio-native "${pkgdir}/usr/bin/stremio-native"
    sed -e 's/Stremio Native (Devel)/Stremio Native (Downmix)/' \
        -e 's/com.stremio.StremioNative.Devel/com.stremio.StremioNative.Downmix/g' \
        flatpak/com.stremio.StremioNative.Devel.desktop \
        > "${pkgdir}/usr/share/applications/com.stremio.StremioNative.Downmix.desktop"
    install -Dm644 app/ui/assets/images/stremio-logo.svg \
        "${pkgdir}/usr/share/icons/hicolor/scalable/apps/com.stremio.StremioNative.Downmix.svg"
}
