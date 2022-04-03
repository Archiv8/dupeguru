#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [FixMe]: Namcap warnings and errors

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>


pkgname=dupeguru
pkgver=4.2.1
pkgrel=1
pkgdesc="Find duplicate files with various contents, using perceptual diff for pictures"
arch=("any")
url="https://dupeguru.voltaicideas.net/"
license=("GPL3")
depends=(
    # Official Arch Linux repositories
    "libxkbcommon-x11"
    "python"
    "python-mutagen"
    "python-pyqt5"
    "python-send2trash"

    # Archiv8 on github
    "python-hsaudiotag3k"
    "python-polib"
)
makedepends=(
    # Official Arch Linux repositories
    "python-distro"
    "python-sphinx"
)
source=( ${pkgname}-${pkgver}.tar.gz::https://github.com/arsenetar/${pkgname}/archive/refs/tags/${pkgver}.tar.gz )
sha512sums=("7f2a5975a61832828af788022981654b501416dfaf90b9c4294f5f809c75d07fb9503b7086170d44901aefd36591f0d7ed5d75d7f96ebe35107c383970b27408")
provides=("dupeguru")
conflicts=("dupeguru-git" "dupeguru-se" "dupeguru-pe" "dupeguru-me")

build() {
  cd "${pkgname}-${pkgver}"
  # Instead of doing the full ./bootstrap.sh
  python3 -m venv env --system-site-packages
  source env/bin/activate
  python3 -m pip install --no-index --find-links=deps -r requirements.txt
  msg "Starting build..."
  python build.py --clean
}

package() {
  cd "${pkgname}-${pkgver}"

  cp -R "help" "build"
  cp -R "locale" "build"
  python package.py --arch-pkg
  cd "build/${pkgname}-arch"

  mkdir -p "${pkgdir}/usr/share/applications"
  mv ${pkgname}.desktop "${pkgdir}/usr/share/applications"

  mkdir -p "${pkgdir}/usr/share/${pkgname}"
  cp -a -- * "${pkgdir}/usr/share/${pkgname}/"
  chmod a+x "${pkgdir}/usr/share/${pkgname}/run.py"

  mkdir -p "${pkgdir}/usr/share/pixmaps"
  ln -s "/usr/share/${pkgname}/dgse_logo_128.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
  mkdir -p "${pkgdir}/usr/bin"
  ln -s ../share/${pkgname}/run.py "${pkgdir}/usr/bin/${pkgname}"
}
