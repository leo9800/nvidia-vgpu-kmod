# Maintainer: Leo <i@hardrain980.com>
pkgname=('nvidia-vgpu-18-kmod' 'nvidia-vgpu-18-kmod-open')
pkgbase=nvidia-vgpu-18-kmod
pkgver=570.148.06
pkgrel=1
arch=('x86_64')
url="https://www.nvidia.com/"
license=('custom:proprietary')
makedepends=('linux-headers')
source=("$pkgbase-$pkgver.tar.gz"
        "$pkgname-$pkgver.patch")
_pkg="NVIDIA-Linux-x86_64-${pkgver}-vgpu-kvm"
_pkg_unlock="NVIDIA-Linux-x86_64-${pkgver}-vgpu-kvm-unlock"
source=(
	"${_pkg}.run"
	'0001-CFLAGS-Set-std-gnu17-for-all-compilation-flags.patch'
)
sha256sums=(
	'fedcb2fca8bdb80ade416d113c0b31a8fbc33ebf4c8cfb1fcc5325023db0fd45'
	'8e746dc88b43e3e28787371cfb7bc5b3ed58ed27b71d29a191bb16a2f7ad0044'
)

prepare() {
	sh "${_pkg}.run" -x
	patch -Np1 -d "${srcdir}/${_pkg}/kernel" < "${srcdir}/0001-CFLAGS-Set-std-gnu17-for-all-compilation-flags.patch"
	patch -Np1 -d "${srcdir}/${_pkg}/kernel-open" < "${srcdir}/0001-CFLAGS-Set-std-gnu17-for-all-compilation-flags.patch"
}

build() {
	cd "${srcdir}/${_pkg}/kernel"
	CFLAGS= CXXFLAGS= LDFLAGS= make SYSSRC="/usr/src/linux"
	cd "${srcdir}/${_pkg}/kernel-open"
	CFLAGS= CXXFLAGS= LDFLAGS= make SYSSRC="/usr/src/linux"
}

package_nvidia-vgpu-18-kmod() {
	pkgdesc="NVIDIA vGPU kernel modules v18"
	depends=('linux' "nvidia-vgpu-18-utils=${pkgver}")
	provides=('NVIDIA-MODULE' 'nvidia')
	conflicts=('NVIDIA-MODULE' 'nvidia')

	install -Dm0644 "${srcdir}/${_pkg}/kernel"/*.ko -t "${pkgdir}/usr/lib/modules/$(</usr/src/linux/version)/extramodules"
	find "${pkgdir}" -name '*.ko' -exec strip --strip-debug {} +
	find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}

package_nvidia-vgpu-18-kmod-open() {
	pkgdesc="NVIDIA vGPU kernel modules v18, open source"
	depends=('linux' "nvidia-vgpu-18-utils=${pkgver}")
	provides=('NVIDIA-MODULE' 'nvidia-open')
	conflicts=('NVIDIA-MODULE' 'nvidia-open')

	install -Dm0644 "${srcdir}/${_pkg}/kernel-open"/*.ko -t "${pkgdir}/usr/lib/modules/$(</usr/src/linux/version)/extramodules"
	find "${pkgdir}" -name '*.ko' -exec strip --strip-debug {} +
	find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}
