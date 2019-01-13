# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux-hardened packages will only work with the default linux-hardened package! To have a single PKGBUILD target
# many kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="zfs-linux-hardened"
pkgname=("zfs-linux-hardened" "zfs-linux-hardened-headers")
_zfsver="0.7.12"
_kernelver="4.19.13.a-1"
_extramodules="4.19.13.a-1-hardened"

pkgver="${_zfsver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-hardened-headers=${_kernelver}" "spl-linux-hardened-headers")
arch=("x86_64")
url="http://zfsonlinux.org/"
source=("https://github.com/zfsonlinux/zfs/releases/download/zfs-${_zfsver}/zfs-${_zfsver}.tar.gz"
        "upstream-4f981f6-additional-fixes-for-current_kernel_time-in-4.20.patch")
sha256sums=("720e3b221c1ba5d4c18c990e48b86a2eb613575a0c3cc84c0aa784b17b7c2848"
            "6f27c3dae57c424e06aec31df6c1e1a821e547aa4e933f2f9b894b5e6762b52d")
license=("CDDL")
depends=("kmod" 'spl-linux-hardened' "zfs-utils=${_zfsver}" "linux-hardened=${_kernelver}")
prepare() {
    cd "${srcdir}/zfs-${_zfsver}"
    patch -Np1 -i ${srcdir}/upstream-4f981f6-additional-fixes-for-current_kernel_time-in-4.20.patch
}

build() {
    cd "${srcdir}/zfs-${_zfsver}"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/lib/udev \
                --libexecdir=/usr/lib/zfs-${zfsver} --with-config=kernel \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build
    make
}

package_zfs-linux-hardened() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs")
    groups=("archzfs-linux-hardened")
    conflicts=("zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" 'zfs-linux-hardened-git')
    cd "${srcdir}/zfs-${_zfsver}"
    make DESTDIR="${pkgdir}" install
    cp -r "${pkgdir}"/{lib,usr}
    rm -r "${pkgdir}"/lib
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-hardened-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    provides=("zfs-headers")
    conflicts=("zfs-headers" "zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc")
    cd "${srcdir}/zfs-${_zfsver}"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/${_extramodules}/Module.symvers
}
