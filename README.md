# visible

Sí, puedes editarlo desde Thunar, pero fíjate en dos detalles de tu captura que lo complican:

- La carpeta es `/run/media/kali/6A39-5138/boot/grub`. Ese es el volumen de **4,3 MB** (`sda2`), el correcto.
- Dice **Free space: 2,0 KiB**. La partición está casi llena, y `grub.cfg` pesa 104 bytes. Mi bloque nuevo ocupa unos 700 bytes, así que puede **no caber**. Además, la copia `grub.cfg.bak` que te pedí hacer antes ya habrá consumido parte de ese espacio. En tu captura solo veo `grub.cfg`, así que esa copia no se creó (o está en otra carpeta).

**Si lo haces desde Thunar:**
1. Clic derecho sobre `grub.cfg` → **Open With** → editor de texto (Mousepad).
2. **Antes de borrar nada**, copia el contenido original (las 3 líneas) a un archivo en **DATOS** como respaldo. Es la copia segura, ya que ahí hay espacio de sobra.
3. Reemplaza el contenido por el bloque nuevo y guarda.

**Si da error de espacio o de permisos** (la partición se montó como root), usa la terminal, que sí tiene permisos:

```
sudo mousepad /mnt/efi/boot/grub/grub.cfg
```

Y si falta espacio, usa una versión más corta (menos de 2 KB) quitando `set timeout`, `set default` y los saltos extra:

```
menuentry "Kali Persistence fix" {
search --set=root --file /.disk/info
linux /live/vmlinuz-6.19.14+kali-amd64 boot=live components quiet splash noeject persistence modprobe.blacklist=snd_pci_acp6x,snd_pci_acp5x,snd_pci_acp3x
initrd /live/initrd.img-6.19.14+kali-amd64
}
menuentry "Menu original" {
search --set=root --file /.disk/info
configfile /boot/grub/grub.cfg
}
```

**Importante:** el respaldo original son exactamente estas 3 líneas, guárdalas en DATOS antes de editar:

```
search --set=root --file /.disk/info
set prefix=($root)/boot/grub
configfile ($root)/boot/grub/grub.cfg
```

Si algo sale mal, desde Windows (unidad F: de 4 MB) restauras ese contenido en `boot\grub\grub.cfg`.