# Maintainer: Ultracoolguy <myownpersonalaccount@protonmail.com>
# Kernel config based on: arch/arm64/configs/msm8953_defconfig

_flavor="postmarketos-qcom-msm8953"
pkgname=linux-$_flavor
pkgver=5.16
pkgrel=4
pkgdesc="Mainline kernel fork for Qualcomm MSM8953 devices"
arch="aarch64"
url="https://github.com/msm8953-mainline/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native pmb:kconfigcheck-anbox"
makedepends="bison findutils flex installkernel openssl-dev perl"

_carch="arm64"
# Source
_commit="9aa7ebd007d167cf7807997a3ed6c28de18fde69"
source="
	$pkgname-$_commit.tar.gz::https://github.com/troplo/linux/archive/$_commit.tar.gz
	config-$_flavor.$arch
"
builddir="$srcdir/linux-$_commit"

prepare() {
	default_prepare
	cp "$srcdir/config-$_flavor.$arch" .config
}

build() {
	unset LDFLAGS
	make -j 8 ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION=$((pkgrel + 1 ))
}

package() {
	mkdir -p "$pkgdir"/boot
	make zinstall modules_install dtbs_install \
		ARCH="$_carch" \
		INSTALL_PATH="$pkgdir"/boot \
		INSTALL_MOD_PATH="$pkgdir" \
		INSTALL_DTBS_PATH="$pkgdir"/usr/share/dtb
	rm -f "$pkgdir"/lib/modules/*/build "$pkgdir"/lib/modules/*/source

	install -D "$builddir"/include/config/kernel.release \
		"$pkgdir"/usr/share/kernel/$_flavor/kernel.release
}
sha512sums="
65119785d552e2d158bffcc38c9982bb40b4e25dab4a4118854f02d1157967aa0fe6b98da59a15b6e16e9cba95eedae364956563c2103d63a3b833d718b99126  linux-postmarketos-qcom-msm8953-9aa7ebd007d167cf7807997a3ed6c28de18fde69.tar.gz
e7d44ae98d28e799113d9601d54d67056c562628a0d0dca3c75843a1603647c0b12a69ec8ad789840c7b8999fa84faeb9b4c8927830943d9152f9b7a689c1898  config-postmarketos-qcom-msm8953.aarch64
"
