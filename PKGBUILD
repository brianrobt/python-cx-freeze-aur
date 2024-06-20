# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Alexander Rødseth <rodseth@gmail.com>
# Contributor: Ray Rashif <schiv@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: Eric Belanger <eric@archlinux.org>
# Contributor: Roberto Alsina <ralsina@kde.org>
# Contributor: Julien Duponchelle <julien@gns3.net>

pkgname=python-cx-freeze
pkgver=7.1.1
pkgrel=1
pkgdesc='Create standalone executables from Python scripts'
arch=('x86_64')
url='https://marcelotduarte.github.io/cx_Freeze'
license=('PSF')
depends=('glibc' 'patchelf' 'python' 'python-packaging' 'python-setuptools')
makedepends=('python-wheel' 'python-build' 'python-installer')
checkdepends=('python-pytest-mock' 'python-bcrypt' 'python-cryptography' 'python-openpyxl'
              'python-pandas' 'python-pillow' 'python-pydantic' 'python-pytz' 'rpm-tools' 'python-pytest-xdist' 'python-pytest-datafiles')
replaces=('python-cx_freeze')
provides=('python-cx_freeze')
conflicts=('python-cx_freeze')
source=("https://github.com/marcelotduarte/cx_Freeze/archive/$pkgver/$pkgname-$pkgver.tar.gz")
sha512sums=('ad9e534f181db1577043149836dbffc95e0ae59a3b5bb4c87e349ab3b87f6a7f6425e123f3c8d38520e8b67e976e0e71ff6a113171b501f764708ba58d55728a')

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
  pytest -vv
}

package() {
  cd cx_Freeze-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
}
