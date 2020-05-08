pkgname=sd-chkhashboot
pkgver=0.1
pkgrel=1
arch=(any)
license=(WTFPL)
conflicts=(mkinitcpio-chkcryptoboot mkinitcpio-sd-chkhashboot)
depends=('usbutils' 'pciutils')
source=()
md5sums=()

pkgver() {
  git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
	echo "Nothing to build"
}

package() {
	# XXX
	cp -aR ../etc ../usr .
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkhashboot.conf
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkhashboot.motd
	install -D {${srcdir},${pkgdir}}/etc/default/sd-chkhashboot-check
	install -D {${srcdir},${pkgdir}}/usr/lib/initcpio/install/sd-chkhashboot
	install -D {${srcdir},${pkgdir}}/usr/lib/systemd/system/sd-chkhashboot.service
}
