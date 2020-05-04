pkgname=sd-chkcryptoboot-uefi
pkgver=0.1
pkgrel=1
arch=(any)
license=(WTFPL)
conflicts=(mkinitcpio-chkcryptoboot mkinitcpio-sd-chkcryptoboot)
depends=('usbutils' 'pciutils')
source=()
md5sums=()

pkgver() {
	git describe --always | sed -e 's/\./-/g'
}

build() {
	echo "Nothing to build"
}

package() {
	# XXX
	cp -aR ../etc ../usr .
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkcryptoboot.conf
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkcryptoboot.motd
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkcryptoboot-check
	install -D {${srcdir},${pkgdir}}/usr/lib/initcpio/install/sd-chkcryptoboot
	install -D {${srcdir},${pkgdir}}/usr/lib/systemd/system/sd-chkcryptoboot.service
}
