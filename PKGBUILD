# Maintainer: Leo <i@hardrain980.com>
pkgname=('nvidia-vgpu-16-kmod-hardened' 'nvidia-vgpu-16-kmod-hardened-unlock')
pkgbase=nvidia-vgpu-16-kmod-hardened
pkgver=535.247.02
pkgrel=3
arch=('x86_64')
url="https://www.nvidia.com/"
license=('custom:proprietary')
makedepends=('linux-hardened-headers')
source=("$pkgbase-$pkgver.tar.gz"
        "$pkgname-$pkgver.patch")
_pkg="NVIDIA-Linux-x86_64-${pkgver}-vgpu-kvm"
_pkg_unlock="NVIDIA-Linux-x86_64-${pkgver}-vgpu-kvm-unlock"
source=(
	"${_pkg}.run"
	'0001-CFLAGS-Set-std-gnu17-for-all-compilation-flags.patch'
	"vgpu_unlock_${pkgver}.patch"
)
sha256sums=(
	'd6bffcc1c2bfc0613981541df573ef87fd49ed2cb1df99d66e7f0f65f051c994'
	'8e746dc88b43e3e28787371cfb7bc5b3ed58ed27b71d29a191bb16a2f7ad0044'
	'24c519d71d8c53cc1198dbebdc1e58470c7cdd1472f4d07342029e6600298907'
)

prepare() {
	sh "${_pkg}.run" -x
	patch -Np1 -d "${srcdir}/${_pkg}/kernel" < "${srcdir}/0001-CFLAGS-Set-std-gnu17-for-all-compilation-flags.patch"

	cp -rp "${srcdir}/${_pkg}" "${srcdir}/${_pkg_unlock}"
	patch -Np1 -d "${srcdir}/${_pkg_unlock}" < "${srcdir}/vgpu_unlock_${pkgver}.patch"
}

build() {
	cd "${srcdir}/${_pkg}/kernel"
	CFLAGS= CXXFLAGS= LDFLAGS= make SYSSRC="/usr/src/linux-hardened"
	cd "${srcdir}/${_pkg_unlock}/kernel"
	CFLAGS= CXXFLAGS= LDFLAGS= make SYSSRC="/usr/src/linux-hardened"
}

package_nvidia-vgpu-16-kmod-hardened() {
	pkgdesc="NVIDIA vGPU kernel modules v16 for linux-hardened"
	depends=('linux-hardened' "nvidia-vgpu-16-utils=${pkgver}")
	provides=('NVIDIA-MODULE' 'nvidia')
	conflicts=('NVIDIA-MODULE' 'nvidia')

	install -Dm0644 "${srcdir}/${_pkg}/kernel"/*.ko -t "${pkgdir}/usr/lib/modules/$(</usr/src/linux-hardened/version)/extramodules"
	find "${pkgdir}" -name '*.ko' -exec strip --strip-debug {} +
	find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}

package_nvidia-vgpu-16-kmod-hardened-unlock() {
	pkgdesc="NVIDIA vGPU kernel modules v16 for linux-hardened, with vGPU unlock patch"
	depends=('linux-hardened' "nvidia-vgpu-16-utils-unlock=${pkgver}")
	provides=('NVIDIA-MODULE' 'nvidia')
	conflicts=('NVIDIA-MODULE' 'nvidia')

	install -Dm0644 "${srcdir}/${_pkg_unlock}/kernel"/*.ko -t "${pkgdir}/usr/lib/modules/$(</usr/src/linux-hardened/version)/extramodules"
	find "${pkgdir}" -name '*.ko' -exec strip --strip-debug {} +
	find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}
