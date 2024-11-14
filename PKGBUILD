# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Tobias Roettger <toroettg@gmail.com>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=platformdirs
pkgname="${_py}-${_pkg}"
pkgver=4.1.0
pkgrel=1
_pkgdesc=(
  'A library to determine'
  'platform-specific system directories'
)
arch=(
  'any'
)
_http="https://github.com"
_ns="${_pkg}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-typing-extensions"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-hatchling"
  "${_py}-hatch-vcs"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-pytest-mock"
  "${_py}-appdirs"
)
source=(
  "${pkgname}::git+${url}#tag=${pkgver}"
)
b2sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${pkgname}"
  git \
    describe \
    --tags | \
    sed \
      's/^v//'
}

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  PYTHONPATH="$(pwd)/src" \
  pytest \
    -v
}

package() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/${pkgname#python-}-${pkgver}.dist-info/licenses/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set ts=2 sw=2 et:
