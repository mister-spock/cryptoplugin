BankID CryptoPlugin for ArchLinux
=================================

This package installs BankID CryptoPlugin.

Allows to use keys issued by the Ukrainian key certification centers (IRS, Ministry of Justice, PrivatBank etc.) in Chromium, Google Chrome, and Mozilla Firefox browsers.

Works in conjunction with [BankID Extension](https://chrome.google.com/webstore/detail/bankid-cryptoplugin/pgfbdgicjmhenccemcijooffohcdanic), which you'll have to install yourself. For the Firefox extension, use the one that is proposed by the Privat24 app.

## Install

Clone repo. Navigate to repo directory, and run `makepkg`. This will generate a `privatbank-cryptoplugin-<version>-x86_64.pkg.tar.xz` package file. To proceed to install it you should run `sudo pacman -U <package-name>`.

## Uninstall

To uninstall CryptoPlugin simply run `sudo pacman -R privatbank-cryptoplugin`.
