# Maintainer: graysky <therealgraysky AT protonmail DOT com>
pkgname=rpi-eeprom

pkgver=20260116
pkgrel=1
arch=(any)
url='https://github.com/ikarustechnologies/rpi-eeprom'
license=(custom)
source=("rpi-eeprom.tar")
md5sums=('SKIP')

package() {
  pkgdesc="Bootloader and VLI USB controller EEPROM update for bcm2711/RPi4 and bcm2712/RPi5 SoC"
  depends=(binutils coreutils nano pciutils python raspberrypi-utils)
  conflicts=(rpi4-eeprom rpi5-eeprom)
  backup=("etc/default/$pkgbase-update"
          "etc/default/$pkgbase-eeprom-update")

  cd "$pkgbase-$_commit"
  install -pd "$pkgdir/usr/bin"
  install -pm755 rpi-eeprom-config "$pkgdir/usr/bin/rpi-eeprom-config"
  install -pm755 rpi-eeprom-digest "$pkgdir/usr/bin/rpi-eeprom-digest"
  install -pm755 rpi-eeprom-update "$pkgdir/usr/bin/rpi-eeprom-update"
  # Arch ARM does not ship raspi-config
  sed -i '/to change the release/d' "$pkgdir/usr/bin/rpi-eeprom-update"
  install -pDm644 rpi-eeprom-update-default "$pkgdir/etc/default/rpi-eeprom-update"
  install -pDm644 LICENSE "$pkgdir/usr/share/doc/$pkgname"

  install -pd "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2711/backup"
  install -pd "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2712/backup"
  for target in latest default; do
    cp -a "firmware-2711/$target" "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2711"
    cp -a "firmware-2712/$target" "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2712"
    # remove old images
    rm -f "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2711/$target/"pieeprom-202[0,1,2]*.bin
    rm -f "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2712/$target/"pieeprom-202[0,1,2]*.bin
  done

  cd $pkgdir/usr/lib/firmware/raspberrypi/bootloader
  ln -sf latest beta
  ln -sf latest stable
  ln -sf default critical

  # firmware files should not be executable
  find "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2711" -name '*.bin' -exec chmod 644 '{}' +
  find "$pkgdir/usr/lib/firmware/raspberrypi/bootloader-2712" -name '*.bin' -exec chmod 644 '{}' +

  install -pd "$pkgdir/usr/lib/firmware/raspberrypi/bootloader/backup"
  for target in latest default; do
    cp -a "firmware-2712/$target" "$pkgdir/usr/lib/firmware/raspberrypi/bootloader"
  done
}
