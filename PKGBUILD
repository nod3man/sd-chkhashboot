pkgname=mkinitcpio-sd-chkcryptoboot
pkgver=c00922d
pkgrel=1
arch=(any)
license=(WTFPL)
conflicts=(mkinitcpio-chkcryptoboot)
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

	install -D {${srcdir},${pkgdir}}/etc/default/chkcryptoboot.conf
	install -D {${srcdir},${pkgdir}}/etc/default/chkcryptoboot.motd
	install -D {${srcdir},${pkgdir}}/etc/default/chkcryptoboot-check
	install -D {${srcdir},${pkgdir}}/usr/lib/initcpio/install/sd-chkcryptoboot
	install -D {${srcdir},${pkgdir}}/usr/lib/systemd/system/chkcryptoboot.service
}
