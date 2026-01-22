# shellcheck shell=bash disable=SC2034 disable=SC2154
# Maintainer: Lucas Melo <luluco250 at gmail dot com>

pkgname=soniccd-git
pkgver=r712.6ecf959
pkgrel=1
pkgdesc='A full decompilation of Sonic CD 2011, based on the PC remake with
improvements & tweaks from the mobile remakes.'
arch=('x86_64' 'aarch64')
url='https://github.com/RSDKModding/RSDKv3-Decompilation'
license=('custom:RSDKv3/4 Decompilation Source Code License v1')
makedepends=('git' 'cmake' 'pkg-config')
depends=('glew' 'sdl2' 'libogg' 'libtheora' 'libvorbis')
options=('!debug' 'strip')
provides=(soniccd)
source=(
	"git+${url}.git"
	'soniccd.desktop'
)
sha256sums=(
	'SKIP'
	'SKIP'
)

_safe_cd() {
	if ! cd "$1"; then
		>&2 echo "Directory '$1' not found"
		exit 1
	fi
}

pkgver() {
	_safe_cd "$srcdir/RSDKv3-Decompilation"
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
	_safe_cd "$srcdir/RSDKv3-Decompilation"
	git submodule init
	git -c protocol.file.allow=always submodule update
}

build() {
	_safe_cd "$srcdir/RSDKv3-Decompilation"
	cmake -B build
	cmake --build build --config release
}

package() {
	install -Dm644 soniccd.desktop "$pkgdir/usr/share/applications/soniccd.desktop"
	install -dm755 "$pkgdir/usr/share/icons"
	cp -r "$srcdir/RSDKv3-Decompilation/RSDKv3/RSDKv3 Decomp Icon.ico" "$pkgdir/usr/share/icons/RSDKv3.ico"
	_safe_cd "$srcdir/RSDKv3-Decompilation"
	install -Dm755 ./build/RSDKv3 "$pkgdir/usr/bin/RSDKv3"
	install -Dm644 ./LICENSE.md "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
