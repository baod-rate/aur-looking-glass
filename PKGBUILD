# Maintainer: Omar Pakker <archlinux@opakker.nl>

pkgbase=looking-glass
pkgname=("${pkgbase}" "${pkgbase}-module-dkms")
pkgver=a12
pkgrel=2
pkgdesc="An extremely low latency KVMFR (KVM FrameRelay) implementation for guests with VGA PCI Passthrough"
url="https://looking-glass.hostfission.com"
arch=('x86_64')
license=('GPL2')
makedepends=('cmake' 'sdl2_ttf' 'glu' 'fontconfig' 'libconfig' 'spice-protocol')
source=("https://github.com/gnif/LookingGlass/archive/${pkgver}.tar.gz"
        "https://github.com/gnif/LookingGlass/commit/2567447b24b28458ba0f09c766a643ad8d753255.patch"
        "dkms.conf")
sha512sums=('72fa8bd1f8ced79bbd81784f9a8502cc39b9eea4d74caf7d27a98da29a2aa58abe71349661800f0b16cecd477ffb6b9a71e35abb68b942f3dad54fe339a70c47'
            '267729736dcb329ce5e5f6524cb9cce20e8b2222fa92644a56d83a937d73bd6efa0b10ab5cda75d1f51e83a6e9cce81afe0ca734005971c869fe61c107b84771'
            'e1f6cd6aabd336d2af97b44a2746e5a0b41d5d5942993379b1284d1cc8d4981fced0ae44d8105709f2bc45a939dfc7f229018c680b0742c3f0778fe28ba301f8')

prepare() {
	cd "${srcdir}/LookingGlass-${pkgver}"
	patch --verbose --forward -p1 --input="${srcdir}/2567447b24b28458ba0f09c766a643ad8d753255.patch"
}

build() {
	cd "LookingGlass-${pkgver}/client"
	cmake .
	make
}

package_looking-glass() {
	pkgdesc="A client application for accessing the LookingGlass IVSHMEM device of a VM"
	depends=('sdl2_ttf' 'glu' 'nettle' 'fontconfig' 'libconfig')

	install -Dm755 "LookingGlass-${pkgver}/client/looking-glass-client" "${pkgdir}/usr/bin/looking-glass-client"
}

package_looking-glass-module-dkms() {
	pkgdesc="A kernel module that implements a basic interface to the IVSHMEM device for when using LookingGlass in VM->VM mode"
	depends=('dkms')

	install -Dm644 "${srcdir}/dkms.conf" "${pkgdir}/usr/src/${pkgbase}-${pkgver}/dkms.conf"

	# Set module name and version
	sed -e "s/@PKGNAME@/${pkgbase}/" \
		-e "s/@PKGVER@/${pkgver}/" \
		-i "${pkgdir}/usr/src/${pkgbase}-${pkgver}/dkms.conf"

	cp -r "${srcdir}/LookingGlass-${pkgver}/module/"* "${pkgdir}/usr/src/${pkgbase}-${pkgver}/"
}
