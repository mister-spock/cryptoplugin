# Maintainer: Oleg Kozak <olegus1203(at)gmail(dot)com>

pkgname=privatbank-cryptoplugin
pkgver=1.2.2
pkgrel=3
pkgdesc="PrivatBank (UA) CryptoPlugin for Privat24 internet banking system (You'll have to install BankID CryptoPlugin yourself)"
url="https://github.com/mister-spock/cryptoplugin"
arch=('x86_64')
license=('custom')
depends=('pcsclite' 'gtk2')
source=("https://cb.privatbank.ua/p24-cryptoplugin/${pkgver}/cryptoplugin-${pkgver}-all.deb")
sha256sums=('dde37f2141ee90b0c9e315ad6c3b612994340ab690c2e3149082e35625bc7885')
install=privatbank-cryptoplugin.install

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
    moz_extension_file="cryptoplugin_ext_id@privatbank.ua.xpi"

    # Get size of the library and the executable file
    size_lib=$(wc -c ${src_dir}/linux_64/${lib_name} | sed 's/ .*//g')
    size_bin=$(wc -c ${src_dir}/linux_64/${bin_name} | sed 's/ .*//g')

    chromium_dir="${pkgdir}/etc/chromium/native-messaging-hosts"
    chrome_dir="${pkgdir}/etc/opt/chrome/native-messaging-hosts"

    moz_plugins_dir="${pkgdir}/usr/lib/mozilla/plugins"
    moz_nmhosts_dir="${pkgdir}/usr/lib/mozilla/native-messaging-hosts"
    moz_extensions_dir="${pkgdir}/usr/lib/firefox-addons/extensions"

    # Install library files
    mkdir -p "${install_dir}"
    dd if="${src_dir}/linux_64/${lib_name}" bs=1 count="${size_lib}" skip=2 of="${install_dir}/${lib_name}" > /dev/null 2>&1
    dd if="${src_dir}/linux_64/${bin_name}" bs=1 count="${size_bin}" skip=2 of="${install_dir}/${bin_name}" > /dev/null 2>&1
    chmod 0644 "${install_dir}/${lib_name}"
    chmod 0755 "${install_dir}/${bin_name}"

    # Edit manifest files to point to new library location (except Mozilla manifest)
    sed -i "s/\"\/usr\/lib\/mozilla\/plugins\/${bin_name}\"/\"\/usr\/lib\/privatbank-cryptoplugin\/${bin_name}\"/g" "${src_dir}/${manifest_file}"

    # Create directories for NMAPI
    mkdir -p "${chromium_dir}"
    mkdir -p "${chrome_dir}"

    # Create Mozilla related directories
    mkdir -p "${moz_plugins_dir}"
    mkdir -p "${moz_nmhosts_dir}"
    mkdir -p "${moz_extensions_dir}"

    # Install NMAPI extensions
    install -m 755 "${src_dir}/${manifest_file}" "${chromium_dir}"
    install -m 755 "${src_dir}/${manifest_file}" "${chrome_dir}"
    install -m 755 "${src_dir}/mozilla/${manifest_file}" "${moz_nmhosts_dir}"

    # Mozilla related installation
    install -m 755 "${src_dir}/mozilla/${moz_extension_file}" "${moz_extensions_dir}"

    # Install licenses
    mkdir -p "${license_dir}"
    install "${pkgdir}/usr/share/doc/cryptoplugin/copyright" "${license_dir}/LICENSE"
    rm -rf "${pkgdir}/usr/share/doc"

    ### END Install ###

    # Cleanup
    rm -rf "${src_dir}"
}
