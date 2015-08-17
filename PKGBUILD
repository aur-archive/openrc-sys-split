# Maintainer: udeved <udeved@openrc4arch.site40.net>

# file vars for easy update
_Icron1=cronie-1.3-initd
_Icron2=anacron-1.0-initd
_Ccrypt=1.0.6-dmcrypt.confd
_Icrypt=1.5.1-dmcrypt.rc
_Idbus=dbus.initd
_Inscd=nscd
_Cdm=device-mapper.conf-1.02.22-r3
_Idm1=device-mapper.rc-2.02.105-r2
_Idm2=dmeventd.initd-2.02.67-r1
_Clvm=lvm.confd-2.02.28-r2
_Ilvm1=lvm.rc-2.02.105-r2
_Ilvm2=lvm-monitoring.initd-2.02.105-r2
_Ilvm3=lvmetad.initd-2.02.105-r2
_Cmdadm=mdadm.confd
_Imdadm=mdadm.rc
_Idhcpcd=dhcpcd.initd

_gentoo_uri="http://sources.gentoo.org/cgi-bin/viewvc.cgi/gentoo-x86"

pkgbase=openrc-sys-split
pkgname=openrc-sys-split
true && pkgname=('cronie-openrc'
				'cryptsetup-openrc'
				'dbus-openrc'
				'dhcpcd-openrc'
				'device-mapper-openrc'
				'glibc-openrc'
				'lvm2-openrc'
				'mdadm-openrc'
				'openrc-sys-split')
pkgver=20140414
pkgrel=1
pkgdesc="OpenRC init scripts"
arch=('any')
url="https://github.com/udeved/pkgbuilds"
license=('GPL2')
groups=('openrc' 'openrc-sys')
makedepends=('cronie' 'cryptsetup' 'dhcpcd' 'dbus' 'device-mapper' 'glibc' 'lvm2' 'mdadm')
conflicts=('openrc-arch-services-git' 'initscripts' 'systemd-sysvcompat' 'openrc' 'openrc-git')

source=("${_gentoo_uri}/sys-process/cronie/files/${_Icron1}"
	"${_gentoo_uri}/sys-process/cronie/files/${_Icron2}"
	"${_gentoo_uri}/sys-fs/cryptsetup/files/${_Ccrypt}"
	"${_gentoo_uri}/sys-fs/cryptsetup/files/${_Icrypt}"
	"${_gentoo_uri}/sys-apps/dbus/files/${_Idbus}"
	"${_gentoo_uri}/sys-libs/glibc/files/${_Inscd}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Cdm}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Idm1}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Idm2}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Clvm}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Ilvm1}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Ilvm2}"
	"${_gentoo_uri}/sys-fs/lvm2/files/${_Ilvm3}"
	"${_gentoo_uri}/sys-fs/mdadm/files/${_Cmdadm}"
	"${_gentoo_uri}/sys-fs/mdadm/files/${_Imdadm}"
	"${_gentoo_uri}/net-misc/dhcpcd/files/${_Idhcpcd}")

pkgver() {
  date +%Y%m%d
}

_shebang='s|#!/sbin/runscript|#!/usr/bin/runscript|'
_runpath='s|/var/run|/run|g'
_binpath=('s|/usr/sbin|/usr/bin|g' 's|/sbin|/usr/bin|g')

package_cronie-openrc() {
	true
	pkgdesc="OpenRC cronie init script"
	depends=('openrc-base' 'cronie')
	provides=('openrc-cron')
	conflicts=('fcron' 'fcron-openrc' 'openrc-arch-services-git'
		  'initscripts' 'systemd-sysvcompat' 'openrc' 'openrc-git')
	backup=('etc/init.d/cronie')
	install=cronie.install

	install -Dm755 "${srcdir}/${_Icron1}" "${pkgdir}/etc/init.d/cronie"
	install -Dm755 "${srcdir}/${_Icron2}" "${pkgdir}/etc/init.d/anacron"

	for f in ${pkgdir}/etc/init.d/*;
	do
	  sed -e "${_shebang}" -e "${_binpath[0]}" -e "${_runpath}" -i $f
	done
}

package_dhcpcd-openrc() {
	true
	pkgdesc="OpenRC dhcpcd init script"
	depends=('openrc-base' 'dhcpcd')
	install=dhcpcd.install

	install -Dm755 "${srcdir}/${_Idhcpcd}" "${pkgdir}/etc/init.d/dhcpcd"

	sed -e "${_shebang}" -e "${_binpath[1]}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/dhcpcd"
}

package_dbus-openrc() {
	true
	pkgdesc="OpenRC dbus init script"
	depends=('openrc-base' 'dbus')
	install=dbus.install

	install -Dm755 "${srcdir}/${_Idbus}" "${pkgdir}/etc/init.d/dbus"

	local _p1='s|dbus.pid|dbus/pid|g'
	sed -e "${_shebang}" -e "${_runpath}" -e "${_p1}" -i "${pkgdir}/etc/init.d/dbus"
}

package_device-mapper-openrc() {
	true
	pkgdesc="OpenRC device-mapper init script"
	depends=('openrc-base' 'device-mapper')
	backup=('etc/conf.d/device-mapper')
	install=device-mapper.install

	install -Dm755 "${srcdir}/${_Cdm}" "${pkgdir}/etc/conf.d/device-mapper"
	install -Dm755 "${srcdir}/${_Idm1}" "${pkgdir}/etc/init.d/device-mapper"
	install -Dm755 "${srcdir}/${_Idm2}" "${pkgdir}/etc/init.d/dmeventd"

	for f in ${pkgdir}/etc/init.d/*;
	do
	  sed -e "${_shebang}" -e "${_binpath[1]}" -e "${_runpath}" -i $f
	done
}

package_cryptsetup-openrc() {
	true
	pkgdesc="OpenRC cryptsetup init script"
	depends=('cryptsetup' 'device-mapper-openrc')
	backup=('etc/conf.d/dmcrypt')
	install=cryptsetup.install

	install -Dm755 "${srcdir}/${_Ccrypt}" "${pkgdir}/etc/conf.d/dmcrypt"
	install -Dm755 "${srcdir}/${_Icrypt}" "${pkgdir}/etc/init.d/dmcrypt"

	sed -e "${_shebang}" -e "${_binpath[0]}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/dmcrypt"
}

package_glibc-openrc() {
	true
	pkgdesc="OpenRC nscd init script"
	depends=('openrc-base' 'glibc')
	optdepends=('openldap-openrc' 'bind-openrc')
	install=glibc.install

	install -Dm755 "${srcdir}/${_Inscd}" "${pkgdir}/etc/init.d/nscd"

	sed -e "${_shebang}" -e "${_binpath[0]}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/nscd"
}

package_lvm2-openrc() {
	true
	pkgdesc="OpenRC lvm2 init script"
	depends=('lvm2' 'device-mapper-openrc')
	backup=('etc/conf.d/lvm')
	install=lvm2.install

	install -Dm755 "${srcdir}/${_Clvm}" "${pkgdir}/etc/conf.d/lvm"
	install -Dm755 "${srcdir}/${_Ilvm1}" "${pkgdir}/etc/init.d/lvm"
	install -Dm755 "${srcdir}/${_Ilvm2}" "${pkgdir}/etc/init.d/lvm-monitoring"
	install -Dm755 "${srcdir}/${_Ilvm3}" "${pkgdir}/etc/init.d/lvmetad"
	
	for f in ${pkgdir}/etc/init.d/*;
	do
	  sed -e "${_shebang}" -e "${_binpath[1]}" -e "${_runpath}" -i $f
	done
}

package_mdadm-openrc() {
	true
	pkgdesc="OpenRC mdadm init script"
	depends=('openrc-base' 'mdadm')
	optdepends=('bind-openrc')
	backup=('etc/conf.d/mdadm')
	install=mdadm.install

	install -Dm755 "${srcdir}/${_Cmdadm}" "${pkgdir}/etc/conf.d/mdadm"
	install -Dm755 "${srcdir}/${_Imdadm}" "${pkgdir}/etc/init.d/mdadm"

	sed -e "${_shebang}" -e "${_runpath}" -i "${pkgdir}/etc/init.d/mdadm"
}

# Comment out if you build for your personal repo
# remove 'openrc-sys-split' from pkgname array
# Dummy package to make AUR display correct info
# If installed, it should make upgrade from AUR possible
package_openrc-sys-split() {
	true
	pkgdesc="OpenRC sys init scripts, AUR upgrade and info split-pkg helper"
	depends=('openrc-base')
	provides=("openrc-sys-split-helper=${pkgver}")
}

sha256sums=('292a7b20fe33bd027357475fea6aa1194afa7e5c1c47a85299db945b9d1c847e'
            '7ff283ee8b492929d33831461b72e872fe9d3a98344cf39af442f575875b0132'
            '0c30e081c0b8f879964ae49735f10b05a1d92f4f481042851958860945e13271'
            '02faf27470ea0e50b764c923786724847e77dfdf9680a50cc202546cad2bf02f'
            '98e37b8b6ed25004e48c5855d74c9361eea06d3fee13cefcc0ed10ccf452aa01'
            '6165db3a2fcb251d4f3655c0461e018ce9c92a37f7f22a8fd2b75178b5435bc8'
            '57777904f12a35617e5a4193c964ebb32396452487fd02353e71e16e7b46bc22'
            '036b6de05e6cbd921a667d6fc6b01d30c8f9b720e1a0d0e2453ecd62d32573fb'
            '0c051388991ba69afbf2f6baf36ba227d7c26fc8f0d7588d8de76d9a74886d79'
            '28370c089c39c248d7ded0960b8d8a9256bada44d44c22ce3cec87d512ef6844'
            'a5754ffa0a05a0c29a9f6b5acf1b21dd313581fd6156c1ef722dc620e0114676'
            'd7655cadd3a3a9d3683a540413365310ca9503c38fd21a9bfccec40630ca72f1'
            '60accb4b6114753232f2db0adf3fc3f46d4459bfedf79b888801a13c55d79fa9'
            'ec55674955af7a31da51b8b72b599e8519809287dad796a9b16155bcba471b79'
            '3073b14619cb7b2c99c33f2d6cfd1e59ce5557899bffebaa65fa52f3caffadc7'
            '72b42c9939fda3fb56666813513029ed36194c1708bddce06bcb3e131e547492')
