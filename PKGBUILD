# Maintainer: Oleg Kozak <olegus1203(at)gmail(dot)com>

pkgname=privatbank-cryptoplugin
pkgver=1.2.2
pkgrel=1
pkgdesc="PrivatBank (UA) CryptoPlugin for Privat24 internet banking system (You'll have to install BankID CryptoPlugin yourself)"
url="https://privatbank.ua/"
arch=('x86_64')
license=('custom')
depends=('pcsclite' 'atk' 'pango' 'gdk-pixbuf2')
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

    firefox_ext="${pkgdir}/$HOME/.mozilla/extensions"
    firefox_dir="${pkgdir}/$HOME/.mozilla/native-messaging-hosts"
    chromium_dir="${pkgdir}/$HOME/.config/chromium/NativeMessagingHosts"

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
    mkdir -p -m 700 "${firefox_dir}"
    mkdir -p -m 700 "${chromium_dir}"

    # Create directories for Firefox extension
    mkdir -p -m 700 "${firefox_ext}"

    # Install NMAPI extensions
    cp "${src_dir}/${manifest_file}" "${chromium_dir}"
    cp "${src_dir}/mozilla/${manifest_file}" "${firefox_dir}"

    # Install Firefox extension
    cp "${src_dir}/mozilla/${ff_extension_file}" "${firefox_ext}"

    # Install licenses
    mkdir -p "${license_dir}"
    mv "${pkgdir}/usr/share/doc/cryptoplugin/copyright" "${license_dir}/LICENSE"

    ### END Install ###

    # Fix perms
    chmod 700 "${pkgdir}/$HOME"
    chmod 700 "${pkgdir}/$HOME/.config"
    chmod 700 "${pkgdir}/$HOME/.config/chromium"
    chmod 700 "${pkgdir}/$HOME/.mozilla"

    # Cleanup
    rm -rf "${src_dir}"
}
