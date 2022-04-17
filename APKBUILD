# Maintainer: Ultracoolguy <myownpersonalaccount@protonmail.com>
# Kernel config based on: arch/arm64/configs/msm8953_defconfig

_flavor="postmarketos-qcom-msm8953"
pkgname=linux-$_flavor
pkgver=5.16
pkgrel=5
pkgdesc="Mainline kernel fork for Qualcomm MSM8953 devices"
arch="aarch64"
url="https://github.com/msm8953-mainline/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native pmb:kconfigcheck-anbox"
makedepends="bison findutils flex installkernel openssl-dev perl"

_carch="arm64"
# Source
_commit="9537839cf55b2a269081428fe9a7a7e25b325d18"
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
158d14e4f57d41c3910f66abf2e48d7d4bfa99d1800645ea12fd9cdf9fb64f1553cf9eec4ace74bc0d46fb69da050582fd4e66696019070d65b9e8b623ff28ad  linux-postmarketos-qcom-msm8953-9537839cf55b2a269081428fe9a7a7e25b325d18.tar.gz
e7d44ae98d28e799113d9601d54d67056c562628a0d0dca3c75843a1603647c0b12a69ec8ad789840c7b8999fa84faeb9b4c8927830943d9152f9b7a689c1898  config-postmarketos-qcom-msm8953.aarch64
"
