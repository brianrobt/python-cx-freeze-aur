# Maintainer: Brian Thompson <brianrobt@pm.me>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Alexander Rødseth <rodseth@gmail.com>
# Contributor: Ray Rashif <schiv@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: Eric Belanger <eric@archlinux.org>
# Contributor: Roberto Alsina <ralsina@kde.org>
# Contributor: Julien Duponchelle <julien@gns3.net>

pkgname=python-cx-freeze
pkgver=7.2.0
pkgrel=1
pkgdesc='Create standalone executables from Python scripts'
arch=('x86_64')
url='https://marcelotduarte.github.io/cx_Freeze'
license=('PSF-2.0')
depends=('glibc' 'patchelf' 'python' 'python-packaging' 'python-setuptools' 'python-filelock' 'python-pyqt5')
makedepends=('python-wheel' 'python-build' 'python-installer')
checkdepends=('python-pytest-mock' 'python-pytest-xdist' 'python-pytest-datafiles' 'python-pytest')
replaces=('python-cx_freeze')
provides=('python-cx_freeze')
conflicts=('python-cx_freeze')
source=("https://github.com/marcelotduarte/cx_Freeze/archive/$pkgver/$pkgname-$pkgver.tar.gz")
sha512sums=('e7ff9c3ed578e20d9c85b8b500ba405fff924325d61d0f92a2a42898d46ba2d32216ada31d255252f8962731f2c0a59dae94a803fca31c6acea0dfdea16db2eb')

prepare() {
  sed -e 's|69|70|g' -i cx_Freeze-$pkgver/pyproject.toml # Support setuptools 69
  sed -e '/patchelf/d' -i cx_Freeze-$pkgver/pyproject.toml # don't require patchelf pip module
}

build() {
  cd cx_Freeze-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  echo "Removing the following directory for a clean install: $PWD/cx_Freeze-$pkgver/test_dir"
  rm -rf cx_Freeze-$pkgver/test_dir
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  cd cx_Freeze-$pkgver
  python -m installer --destdir=test_dir dist/*.whl
  export PYTHONPATH="$PWD/test_dir/$site_packages:$PYTHONPATH"
  unset SOURCE_DATE_EPOCH # Workaround for 'FATAL ERROR:SOURCE_DATE_EPOCH' (see https://github.com/AppImage/AppImageKit/issues/1202)
  pytest -vv -k 'not pandas'
}

package() {
  cd cx_Freeze-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
}
