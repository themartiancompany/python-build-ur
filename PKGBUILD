# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

_py="python"
_pkg="build"
_pkgname=build
pkgname="${_py}-${_pkg}"
pkgver=1.2.1
pkgrel=3
pkgdesc='A simple, correct Python build frontend'
arch=(
  'any'
)
_http="https://github.com"
_ns="pypa"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  'python'
  'python-packaging'
  'python-pyproject-hooks'
)
makedepends=(
  'git'
  # One could tell arch devs have gone mad
  # by the fact they had python-build
  # depending on itself instead than
  # on python-wheel and python-setuptools
  # to build
  'python-build'
  'python-flit-core'
  'python-installer'
)
checkdepends=(
  'python-pytest'
  'python-pytest-mock'
  'python-pytest-rerunfailures'
  'python-filelock'
  'python-setuptools'
  'python-uv'
  'python-virtualenv'
  'python-wheel'
)
optdepends=(
  'python-pip: to use as the Python package installer (default)'
  'python-uv: to use as the Python package installer'
  'python-virtualenv: to use virtualenv for build isolation'
)
source=(
  "${pkgname}::git+${url}#tag=${pkgver}?signed"
)
validpgpkeys=(
#  '3DCE51D60930EBA47858BA4146F633CBB0EB4BF2' # Filipe Laíns (FFY00) <lains@archlinux.org>
  '2FDEC9863E5E14C7BC429F27B9D0E45146A241E8' # Henry Schreiner <henryschreineriii@gmail.com>
)
b2sums=(
  '891acaf857efc18c210648a681c8499a47b6fe5ba58176d026bdfeffce665de26cd17580fcace2fb5970b2f21a37127ea73c196a5a2b8510dd84f6b873217c17'
)

build() {
  cd \
    "${pkgname}"
  # This is foolishness
  "${_py}" \
    -m build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  PYTHONPATH=src \
  pytest \
    -k \
    'not test_verbose_output'
}

package() {
  local \
    _site_packages \
    _site_packages_cmd=()
  _site_packages_cmd=(
    "import site;"
    "print("
      "site.getsitepackages()[0]"
    ")"
  )
  _site_packages=$( \
    "${_py}" \
      -c \
        "${_site_packages_cmd[*]}")
  cd \
    "${pkgname}"
  python \
    -m installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # Symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_pkgname}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

