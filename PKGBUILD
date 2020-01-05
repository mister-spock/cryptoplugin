# Maintainer: Oleg Kozak <olegus1203(at)gmail(dot)com>

pkgname=privatbank-cryptoplugin
pkgver=1.2.2
pkgrel=1
pkgdesc="PrivatBank (UA) CryptoPlugin for Privat24 internet banking system (You'll have to install BankID CryptoPlugin yourself)"
url="https://privatbank.ua/"
arch=('x86_64')
license=('custom')
depends=('pcsclite' 'gtk2')
source=("https://cb.privatbank.ua/p24-cryptoplugin/${pkgver}/cryptoplugin-${pkgver}-all.deb")
sha256sums=('dde37f2141ee90b0c9e315ad6c3b612994340ab690c2e3149082e35625bc7885')

package() {
    # Unpack data tar
    bsdtar -O -xf "cryptoplugin-${pkgver}"*.deb data.tar.gz | bsdtar -C "${pkgdir}" -xJf -

    # Permission fix
    find "${pkgdir}" -type d -exec chmod 755 {} +

    # Remove all unnecessary stuff
    rm -rf "${pkgdir}/debian-binary"

    ### Install ###

    src_dir="${pkgdir}/usr/lib/cryptoplugin"
    install_dir="${pkgdir}/usr/lib/privatbank-cryptoplugin"
    license_dir="${pkgdir}/usr/share/licenses/privatbank-cryptoplugin"

    lib_name="libnpcryptoplugin.so"
    bin_name="nmcryptoplugin"

    manifest_file="com.privatbank.cryptoplugin.json"
    ff_extension_file="cryptoplugin_ext_id@privatbank.ua.xpi"
    size_lib=$(wc -c ${src_dir}/linux_64/${lib_name} | sed 's/ .*//g')
    size_bin=$(wc -c ${src_dir}/linux_64/${bin_name} | sed 's/ .*//g')

    chromium_dir="${pkgdir}/etc/chromium/native-messaging-hosts"

    # Install library files
    mkdir -p "${install_dir}"
    dd if="${src_dir}/linux_64/${lib_name}" bs=1 count="${size_lib}" skip=2 of="${install_dir}/${lib_name}" > /dev/null 2>&1
    dd if="${src_dir}/linux_64/${bin_name}" bs=1 count="${size_bin}" skip=2 of="${install_dir}/${bin_name}" > /dev/null 2>&1
    chmod 0644 "${install_dir}/${lib_name}"
    chmod 0755 "${install_dir}/${bin_name}"

    # Edit manifest files to point to new library location
    sed -i "s/\"\/usr\/lib\/mozilla\/plugins\/${bin_name}\"/\"\/usr\/lib\/privatbank-cryptoplugin\/${bin_name}\"/g" "${src_dir}/${manifest_file}"
    sed -i "s/\"\/usr\/lib\/mozilla\/plugins\/${bin_name}\"/\"\/usr\/lib\/privatbank-cryptoplugin\/${bin_name}\"/g" "${src_dir}/mozilla/${manifest_file}"

    # Create directories for NMAPI
    mkdir -p "${chromium_dir}"

    # Install NMAPI extensions
    install -m 755 "${src_dir}/${manifest_file}" "${chromium_dir}"

    # Install licenses
    mkdir -p "${license_dir}"
    install "${pkgdir}/usr/share/doc/cryptoplugin/copyright" "${license_dir}/LICENSE"
    rm -rf "${pkgdir}/usr/share/doc"

    ### END Install ###

    # Cleanup
    rm -rf "${src_dir}"
}
