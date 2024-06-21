# Maintainer: Brian Thompson <brianrobt@pm.me>
#
# This PKGBUILD is based off of the PKGBUILD in python-cx-freeze[1].  The only difference
# is that this package is built from the latest commit in the main development branch.
#
# References:
# [1]: https://gitlab.archlinux.org/archlinux/packaging/packages/python-cx-freeze

pkgname=python-cx-freeze-git
pkgver=r2017.1225084
pkgrel=1
pkgdesc="Create standalone executables from Python scripts (built from latest commit)"
arch=('x86_64')
url="https://marcelotduarte.github.io/cx_Freeze"
license=('PSF-2.0')
groups=()
depends=('glibc' 'python' 'python-packaging' 'python-setuptools' 'python-filelock' 'python-pyqt5')
makedepends=('python-wheel' 'python-build' 'python-installer' 'git')
checkdepends=('python-pytest-mock' 'python-bcrypt' 'python-cryptography' 'python-openpyxl'
              'python-pandas' 'python-pillow' 'python-pydantic' 'python-pytz' 'rpm-tools' 'python-pytest-xdist' 'python-pytest-datafiles')
provides=("${pkgname%}")
conflicts=("${pkgname%}")
replaces=("${pkgname%}")
source=('python-cx-freeze-git::git+https://github.com/marcelotduarte/cx_Freeze#branch=main')
sha256sums=('SKIP')

pkgver() {
  cd "$srcdir/$pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short=7 HEAD)"
}

build() {
	cd "$srcdir/$pkgname"
  python -m build --wheel --no-isolation
}

check() {
  echo "Removing the following directory for a clean install: $PWD/cx_Freeze-$pkgver/test_dir"
  rm -rf "$pkgname/test_dir"
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  cd "$srcdir/$pkgname"
  python -m installer --destdir=test_dir dist/*.whl
  export PYTHONPATH="$PWD/test_dir/$site_packages:$PYTHONPATH"
  unset SOURCE_DATE_EPOCH # Workaround for 'FATAL ERROR:SOURCE_DATE_EPOCH' (see https://github.com/AppImage/AppImageKit/issues/1202)
  pytest -vv
}

package() {
	cd "$srcdir/$pkgname"
  python -m installer --destdir="$pkgdir" dist/*.whl
}
