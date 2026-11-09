# Autofirma Flatpak

[Read in English](README.en.md)

---

## Acciones necesarias para la mayoria de navegadores

Autofirma requiere instalar un certificado en el navegador que vayas a utilizar. Recomendamos leer el [FAQ oficial] para instalar los certificados manualmente.

Puedes encontrar el archivo del certificado a instalar en la siguiente ubicacion:

```
~/.afirma/Autofirma/Autofirma_ROOT.cer
```

Puedes comprobar tu configuracion en las siguientes paginas: 
- https://valide.redsara.es/valide/
- https://www.sededgsfp.gob.es/es/Paginas/TestAutofirma.aspx

## FAQ

> ¿Por qué necesito instalar manualmente el certificado? 
> ¿Autofirma no hace esto automatico en Windows?

No realmente, el código de auto-instalación de certificados de Autofirma no es robusto.
Solo es capaz de detectar los dos principales navegadores (Chrome/Chromium y Firefox) incluso en Windows, y solo es capaz de detectarlos en
las siguientes configuraciones automaticamente sin permisos adicionales:
- Chrome Nativo (gestor de paquetes)
- Otros que comprueben el almacen NSS compartido (`~/.pki/nssdb`)
Y los siguientes con permisos adicionales:
- Firefox Snap
- Firefox (gestor de paquetes)
- Chromium Snap

También, dar permiso a estos ultimos abriría una brecha de ataque grande a tu navegador, por ello no lo habilitamos por defecto ni documentamos como hacerlo aquí.

## Tarjetas inteligentes

El paquete incluye la biblioteca de PC/SC (pcsc-lite) y el módulo PKCS#11 de OpenSC.
Con ellos, la aplicación accede a las tarjetas a través del demonio `pcscd` del sistema, sin necesidad de permisos USB ni de sistema de archivos adicionales.
El DNIe se gestiona con el controlador que incorpora la propia aplicación; no hace falta registrarlo como módulo PKCS#11.

> ⚠️ Esta función aún no ha sido probada con tarjetas reales.
> Los comentarios son bienvenidos.

Para usar una tarjeta criptográfica compatible con OpenSC (por ejemplo, una llave USB con applet PIV):

1. Abre AutoFirma: *Herramientas* → *Preferencias* → pestaña *Almacenes de claves*.
2. Pulsa *Agregar tarjeta*.
3. En *Nombre de la tarjeta* escribe un nombre cualquiera, y en *Controlador PKCS#11* introduce:
   ```
   /app/lib/opensc-pkcs11.so
   ```
4. Pulsa *Conectar* para comprobar que se detecta la tarjeta.

A partir de ese momento, el almacén aparecerá en el diálogo de selección de certificados al firmar.

> ¿Por qué tengo que indicar la ruta del controlador a mano?
> AutoFirma no detecta automáticamente controladores PKCS#11 en Linux, hay que registrarlos.
> La ruta debe ser la interna del sandbox (`/app/lib/...`), no la del sistema (`/usr/lib64/...`), porque dentro del Flatpak las bibliotecas del sistema no están disponibles.

## Contribuir

Cualquier contribución es bien recibida.
Para resolver dudas o cuestiones al respecto estamos disponibles en [Matrix]: [#autofirma-flatpak:matrix.org][matrix-chat]

[Matrix]: https://matrix.org
[matrix-chat]: https://matrix.to/#/#autofirma-flatpak:matrix.org
[FAQ oficial]:https://github.com/ctt-gob-es/clienteafirma/wiki/Faq-autofirma-execution#no-se-abre-autofirma-al-ejecutar-firmas-desde-el-navegador
