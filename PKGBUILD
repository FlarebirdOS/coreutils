pkgname=coreutils
pkgver=9.9
pkgrel=4
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
    https://www.linuxfromscratch.org/patches/downloads/${pkgname}/${pkgname}-${pkgver}-i18n-1.patch)
sha256sums=(19bcb6ca867183c57d77155eae946c5eced88183143b45ca51ad7d26c628ca75
    1c0a923feb2d84c58562f192333916c2914214ce9d96c54571cdd90aceb2ad3c)

prepare() {
    cd ${pkgname}-${pkgver}

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
