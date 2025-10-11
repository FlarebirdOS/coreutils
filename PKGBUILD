pkgname=coreutils
pkgver=9.8
pkgrel=3
pkgdesc="The basic file, shell and text manipulation utilities of the GNU operating system"
arch=('x86_64')
url="https://www.gnu.org/software/coreutils/"
license=(
    'GPL-3.0-or-later'
    'GFDL-1.3-or-later'
)
groups=('base')
depends=(
    'acl'
    'attr'
    'glibc'
    'gmp'
    'libcap'
    'openssl'
)
makedepends=(
    'gperf'
    'python'
)
source=(https://ftp.gnu.org/gnu/${pkgname}/${pkgname}-${pkgver}.tar.xz
    https://www.linuxfromscratch.org/patches/downloads/${pkgname}/${pkgname}-${pkgver}-i18n-2.patch)
sha256sums=(e6d4fd2d852c9141a1c2a18a13d146a0cd7e45195f72293a4e4c044ec6ccca15
    07f2e1d6d4085feed1788b6d7a1bf4ef47af4b6f8f6702cf6868f01fc718c099)

prepare() {
    cd ${pkgname}-${pkgver}

    patch -Np1 -i ${srcdir}/${pkgname}-${pkgver}-i18n-2.patch

    autoreconf -fv
    automake -af
}

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --enable-no-install-program=kill,uptime
        --with-openssl
        ${configure_options}
    )

    FORCE_UNSAFE_CONFIGURE=1 ./configure "${configure_args[@]}"

    make
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} install

    install -vdm755 ${pkgdir}/usr/sbin ${pkgdir}/usr/share/man/man8
    mv -v ${pkgdir}/usr/bin/chroot ${pkgdir}/usr/sbin
    mv -v ${pkgdir}/usr/share/man/man1/chroot.1 ${pkgdir}/usr/share/man/man8/chroot.8
    sed -i 's/"1"/"8"/' ${pkgdir}/usr/share/man/man8/chroot.8
}
