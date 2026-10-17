# Maintainer: hiruocha <hiruocha[at]outlook[dot]com>

pkgname=linux-firmware-xiaomi-marble
pkgver=20261008
pkgrel=1
pkgdesc="Firmware for Xiaomi POCO F5 / Redmi Note 12 Turbo"
url="https://github.com/iLtanaka/firmware-xiaomi-marble"
arch=('aarch64')
license=('LicenseRef-qcom' 'LicenseRef-Xiaomi')
options=('!debug' '!strip')

_fwdir=usr/lib/firmware/qcom/sm8475/xiaomi/marble

source=("$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
        "https://gitlab.com/kernel-firmware/linux-firmware/-/raw/20260916/LICENSES/LICENSE.qcom"
        "https://gitlab.com/kernel-firmware/linux-firmware/-/raw/20260916/LICENSES/NOTICE.qcom")
sha256sums=('2555c7696919efa5de17901f46737a77867e834fc1d2be066ca6b89a07852c0e'
            'be904cd28cb292b80cdb6cf412ab0d9159d431671e987ad433c1f62e0988a9bc'
            'fa43e1b9a13b341a07adca9dbe73d0f9072d7966fdfe811c01f0dd2872d7309a')

package() {
  cd "$srcdir/firmware-xiaomi-marble-$pkgver"

  # adsp, modem (incl. mcfg), slpi, adreno zap, ipa, amplifier profile
  install -d "$pkgdir/$_fwdir"
  find marble -maxdepth 1 -type f -exec install -m644 {} "$pkgdir/$_fwdir/" \;
  cp -a marble/modem_pr "$pkgdir/$_fwdir/"

  # adreno sqe/gmu and iris (venus) video firmware
  install -Dm644 generic/qcom/a730_sqe.fw \
    "$pkgdir/usr/lib/firmware/qcom/a730_sqe.fw"
  install -Dm644 generic/qcom/gmu_gen70000.bin \
    "$pkgdir/usr/lib/firmware/qcom/gmu_gen70000.bin"
  install -d "$pkgdir/usr/lib/firmware/qcom/vpu"
  install -m644 generic/qcom/vpu/* "$pkgdir/usr/lib/firmware/qcom/vpu/"

  # audio topology
  install -Dm644 generic/SM8475-Xiaomi-Marble-tplg.bin \
    "$pkgdir/usr/lib/firmware/qcom/sm8450/SM8475-Xiaomi-Marble-tplg.bin"

  # wcn6855 bluetooth
  install -d "$pkgdir/usr/lib/firmware/qca"
  install -m644 generic/qca/hp* "$pkgdir/usr/lib/firmware/qca/"
  local f
  for f in "$pkgdir"/usr/lib/firmware/qca/hp*; do
    f="${f##*/}"
    ln -s "$f" "$pkgdir/usr/lib/firmware/qca/wcn${f#hp}"
  done

  # wcn6855 wi-fi
  cp -a generic/ath11k "$pkgdir/usr/lib/firmware/ath11k"

  # <chipset>/<vendor>/<device> layout
  install -d "$pkgdir/usr/share/qcom/sm8475/Xiaomi/marble"
  cp -a hexagonfs/sensors hexagonfs/socinfo \
    "$pkgdir/usr/share/qcom/sm8475/Xiaomi/marble/"

  # qcom firmware licence
  install -Dm644 "$srcdir/LICENSE.qcom" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.qcom"
  install -Dm644 "$srcdir/NOTICE.qcom" \
    "$pkgdir/usr/share/licenses/$pkgname/NOTICE.qcom"
}
