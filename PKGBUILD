# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Contributor: Filipe Laíns (FFY00) <lains@archlinux.org>
# Contributor: Daniel M. Capella <polyzen@archlinux.org>

_build="true"
_no_build="$( \
  "${_py}" \
    -m \
      build \
    --version | \
    grep \
      "No module named")"
[[ "${_no_build}" != "" ]] && \
  _build=false
_py="python"
_pkg=build
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
  "${_py}"
  "${_py}-packaging"
  "${_py}-pyproject-hooks"
)
makedepends=(
  'git'
  "${_py}-flit-core"
  "${_py}-installer"
)
[[ "${_build}" == true ]] && \
  makedepends+=(
    "${_py}-build"
  )
[[ "${_build}" == false ]] && \
  makedepends+=(
    "${_py}-wheel"
    "${_py}-setuptools"
  )
checkdepends=(
  "${_py}-pytest"
  "${_py}-pytest-mock"
  "${_py}-pytest-rerunfailures"
  "${_py}-filelock"
  "${_py}-setuptools"
  "${_py}-uv"
  "${_py}-virtualenv"
  "${_py}-wheel"
)
optdepends=(
  "${_py}-pip: to use as the Python package installer (default)"
  "${_py}-uv: to use as the Python package installer"
  "${_py}-virtualenv: to use virtualenv for build isolation"
)
source=(
  "${pkgname}::git+${url}#tag=${pkgver}?signed"
)
validpgpkeys=(
  # Filipe Laíns (FFY00) <lains@archlinux.org>
  # '3DCE51D60930EBA47858BA4146F633CBB0EB4BF2'
  # Henry Schreiner <henryschreineriii@gmail.com>
  '2FDEC9863E5E14C7BC429F27B9D0E45146A241E8'
)
b2sums=(
  '891acaf857efc18c210648a681c8499a47b6fe5ba58176d026bdfeffce665de26cd17580fcace2fb5970b2f21a37127ea73c196a5a2b8510dd84f6b873217c17'
)

build() {
  cd \
    "${pkgname}"
  if [[ "${_build}" == true ]]; then
    "${_py}" \
      -m build \
      --wheel \
      --skip-dependency-check \
      --no-isolation
  elif [[ "${_build}" == false ]]; then
    "${_py}" \
      setup.py \
        build
  fi
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
  if [[ "${_build}" == true ]]; then
    "${_py}" \
      -m installer \
      --destdir="${pkgdir}" \
      dist/*.whl
  elif [[ "${_build}" == false ]]; then
    "${_py}" \
      setup.py \
      install \
      --root="${pkgdir}" \
      --optimize=1
  fi
  # Symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${_site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

