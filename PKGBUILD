# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>

_pkgname=build
pkgname=python-$_pkgname
pkgver=1.1.1
pkgrel=2
pkgdesc='A simple, correct Python packaging build frontend'
arch=('any')
url='https://github.com/pypa/build'
license=('MIT')
depends=('python-packaging' 'python-pyproject-hooks')
makedepends=('git' 'python-build' 'python-flit-core' 'python-installer'
             'python-sphinx' 'python-sphinx-argparse-cli' 'python-sphinx-autodoc-typehints' 'python-sphinx-furo' 'python-sphinx-issues')
checkdepends=('python-pytest' 'python-pytest-mock' 'python-pytest-rerunfailures' 'python-filelock' 'python-setuptools' 'python-wheel')
optdepends=('python-virtualenv: Use virtualenv for build isolation')
source=("$pkgname::git+$url#tag=$pkgver?signed")
validpgpkeys=(
#  '3DCE51D60930EBA47858BA4146F633CBB0EB4BF2' # Filipe Laíns (FFY00) <lains@archlinux.org>
  '2FDEC9863E5E14C7BC429F27B9D0E45146A241E8' # Henry Schreiner <henryschreineriii@gmail.com>
)
b2sums=('fbbf71287a3e716a8997eb92f03c57d949a3b520b336bd737a443f1db8c368c3a1e48341fa1ea5eadf49eb4f3b9bc2a1b96f1279bde9e1eb20fdd06f561f0c77')

build() {
  cd $pkgname

  python -m build --wheel --skip-dependency-check --no-isolation

  PYTHONPATH=src sphinx-build -b dirhtml -v docs docs-output
}

check() {
  cd $pkgname

  PYTHONPATH=src pytest
}

package() {
  cd $pkgname

  python -m installer --destdir="$pkgdir" dist/*.whl

  install -d "$pkgdir"/usr/share/doc/$pkgname
  cp -r docs-output/* "$pkgdir"/usr/share/doc/$pkgname

  # Symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s "$site_packages"/$_pkgname-$pkgver.dist-info/LICENSE \
    "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}
