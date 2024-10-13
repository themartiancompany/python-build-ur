# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Contributor: Filipe Laíns (FFY00) <lains@archlinux.org>
# Contributor: Daniel M. Capella <polyzen@archlinux.org>

_git=false
_py="python"
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
_pkg=build
pkgname="${_py}-${_pkg}"
pkgver=1.2.2
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
  "${_py}-flit-core"
  "${_py}-installer"
)
[[ "${_git}" == true ]] && \
  makedepends+=(
    'git'
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
if [[ "${_git}" == "true" ]]; then
  _source="${_pkg}-${pkgver}::git+${url}#tag=${pkgver}?signed"
  _sum='891acaf857efc18c210648a681c8499a47b6fe5ba58176d026bdfeffce665de26cd17580fcace2fb5970b2f21a37127ea73c196a5a2b8510dd84f6b873217c17'
else
  _source="${_pkg}-${pkgver}.tar.gz::${url}/archive/refs/tags/${pkgver}.tar.gz"
  _sum='308faba9fca554fc2ea347d20ee2f2a460060922c028d7ae37648290f4caa374616105d740ed285729204028d40bfb838b4de59ae20eaa8db1c0924f0d1cd8a8'
fi
source=(
  "${_source}"
)
validpgpkeys=(
  # Filipe Laíns (FFY00) <lains@archlinux.org>
  # '3DCE51D60930EBA47858BA4146F633CBB0EB4BF2'
  # Henry Schreiner <henryschreineriii@gmail.com>
  '2FDEC9863E5E14C7BC429F27B9D0E45146A241E8'
)
b2sums=(
  "${_sum}"
)

build() {
  cd \
    "${_pkg}-${pkgver}"
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
    "${_pkg}-${pkgver}"
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
    "${_pkg}-${pkgver}"
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

