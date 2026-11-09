# AutoFirma Flatpak

[Leer en español](README.md)

---

## Required actions for most browsers

Autofirma requires you to install its self-signed certificate on the browser you use it with.
We recommend reading the [official FAQ] for instructions to manually install it.

You can find the certificate file on the following path:
```
~/.afirma/Autofirma/Autofirma_ROOT.cer
```

You can check if your setup is working on the following websites: 
- https://valide.redsara.es/valide/?lang=en
- https://www.sededgsfp.gob.es/es/Paginas/TestAutofirma.aspx (Spanish Only)

## FAQ

 > Doesn't Autofirma works out-of-the-box on Windows?
 > Why do I need to manually install a certificate?

Autofirma's certificate auto-installation code is not robust. 
Only detects the two main browsers (Firefox and Chrome/Chromium) even on Windows,
and its only able to detect them on the following setups without additional 
permissions:
- Chrome (package manager)
- Other browsers that check NSS's shared database (`~/.pki/nssdb`)

And the following ones with additional permissions:
- Firefox Snap
- Firefox (package manager)
- Chromium Snap

Also, giving permission to these last ones would open a big attack surface to your
browser, so we don't enable them by default and don't document how to do it here.

## Smart cards

The package bundles the PC/SC client library (pcsc-lite) and the OpenSC PKCS#11 module.
With them, the application reaches smart cards through the system `pcscd` daemon, with no need for extra USB or filesystem permissions.
The DNIe is handled by AutoFirma's own built-in driver; it does not need to be registered as a PKCS#11 module.

> ⚠️ This has not been tested with real cards yet.
> Feedback is welcome.

To use an OpenSC-compatible cryptographic card (for example, a USB key with a PIV applet):

1. Open AutoFirma: *Herramientas* → *Preferencias* → *Almacenes de claves* tab (the interface is in Spanish).
2. Click *Agregar tarjeta*.
3. Enter any name in *Nombre de la tarjeta* and set *Controlador PKCS#11* to:
   ```
   /app/lib/opensc-pkcs11.so
   ```
4. Click *Conectar* to check that the card is detected.

From then on, the store will appear in the certificate selection dialog when signing.

> Why do I have to enter the driver path manually?
> AutoFirma does not auto-detect PKCS#11 drivers on Linux, they must be registered.
> The path must be the sandbox-internal one (`/app/lib/...`), not the host one (`/usr/lib64/...`), because host libraries are not available inside the Flatpak.

## Contributing

Any contributions are welcomed.

For solving questions, we are available in [Matrix]: at [#autofirma-flatpak:matrix.org][matrix-chat]

Please be aware that most conversation takes place in spanish.

[Matrix]: https://matrix.org
[matrix-chat]: https://matrix.to/#/#autofirma-flatpak:matrix.org
[official FAQ]: https://github.com/ctt-gob-es/clienteafirma/wiki/Faq-autofirma-execution-en-US
