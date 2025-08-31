pkgname=coreutils
pkgver=9.7
pkgrel=2
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
    https://www.linuxfromscratch.org/patches/downloads/${pkgname}/${pkgname}-${pkgver}-upstream_fix-1.patch
    https://www.linuxfromscratch.org/patches/downloads/${pkgname}/${pkgname}-${pkgver}-i18n-1.patch)
sha256sums=(e8bb26ad0293f9b5a1fc43fb42ba970e312c66ce92c1b0b16713d7500db251bf
    7fc7d0925452808b6eeb13a059b495fd7b7290bc7adc9fec213126f4862da649
    4af1cddb73b98350f64ba56cc6db3cb27ab3b3ca92af0089aad0965182f56ce0)

prepare() {
    cd ${pkgname}-${pkgver}

    patch -Np1 -i ${srcdir}/${pkgname}-${pkgver}-upstream_fix-1.patch
    patch -Np1 -i ${srcdir}/${pkgname}-${pkgver}-i18n-1.patch

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
