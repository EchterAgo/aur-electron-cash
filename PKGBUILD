# Maintainer: Dan Beste <dan.ray.beste@gmail.com>
# Maintainer: Marcel O'Neil <marcel@marceloneil.com>
# Contributor: Andy Weidenbaum <archbaum@gmail.com>
# Contributor: RunningDroid <runningdroid AT zoho.com>
# Contributor: Sebastian Lindqvist <dunpin@gmail.com>

#
# ThomasV PGP key: gpg --recv-key 6694D8DE7BE8EE5631BED9502BD5824B7F9470E6
# http://bitcoin-otc.com/viewgpg.php?nick=ThomasV
#

pkgname='electron-cash'
pkgdesc='Lightweight Bitcoin Cash wallet'
pkgver=3.1.5
pkgrel=2
url='http://www.electroncash.org/'
install="${pkgname}.install"
arch=('any')
license=('MIT')
makedepends=(
  'git'
  'protobuf'
  'python-requests'
  'python-setuptools'
  'python-tox'
)
depends=(
  'hicolor-icon-theme'
  'python'
  'python-dnspython'
  'python-ecdsa'
  'python-jsonrpclib-pelix'
  'python-pbkdf2'
  'python-protobuf'
  'python-pyaes'
  'python-pyqt5'
  'python-pysocks'
  'python-qrcode'
  'python-requests'
  'python-six'
  'qt5-base'
)
optdepends=(
  'desktop-file-utils: update desktop icon'
  'gtk-update-icon-cache: update desktop icon'
  'python-btchip: Ledger hardware wallet support'
  'python-pycryptodomex: use PyCryptodome AES implementation instead of pyaes'
  'python-matplotlib: plot transaction history in graphical mode'
  'python-rpyc: send commands to Electrum Python console from an external script'
  'xdg-utils: update desktop icon'
  'zbar: QR code reading support'
)
provides=("${pkgname}")
conflicts=("${pkgname}")
install="${pkgname}.install"
source=(
  "${pkgname}-${pkgver}.tar.gz::https://github.com/fyookball/electrum/archive/${pkgver/.0}.tar.gz"
  "${pkgname}.install"
  "0001-prevent-requirements.txt-from-being-installed-to-usr.patch"
)
sha256sums=('abc7806fdea08c42579614ad15c3521b13fdee4724170c373a260bc08b35f019'
            'd682766321f9981ee38aee26dc050209882d8c1c6006e3e509649b47fa1bb073'
            'c16d0cbb44f6446bfdeb1d5d539a799aa84ced067b269ab4bf95e6a1ab0d4f9f')

prepare() {
  cd "${pkgname/on-cash/um}-${pkgver/.0}"
  patch -Np1 -i "${srcdir}/0001-prevent-requirements.txt-from-being-installed-to-usr.patch"
}

build() {
  cd "${pkgname/on-cash/um}-${pkgver/.0}"

  # python2-pyqt5 and qt5-base are needed for _only_ the icons...

  # Compile the icons file for Qt:
  pyrcc5 icons.qrc -o gui/qt/icons_rc.py
  # Compile the protobuf description file:
  protoc --proto_path=lib/ --python_out=lib/ lib/paymentrequest.proto
  # Create translations (optional):
  python contrib/make_locale
  # Build
  python setup.py build
}

check() {
  cd "${pkgname/on-cash/um}-${pkgver/.0}"

  tox -e py36
}

package() {
  cd "${pkgname/on-cash/um}-${pkgver/.0}"

  python setup.py install --root="${pkgdir}" --optimize=1
}


# vim: ts=2 sw=2 et:
