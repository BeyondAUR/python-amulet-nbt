# Maintainer: 0x9fff00 <0x9fff00+git@protonmail.ch>

_name=Amulet-NBT
_lowername=${_name,,}
_pyname=${_lowername/-/_}
pkgname=python-$_lowername
pkgver=2.1.2
pkgrel=1
pkgdesc='A Python and Cython library for reading and writing binary NBT and stringified NBT'
arch=('x86_64')
url="https://github.com/Amulet-Team/$_name"
license=('custom')
depends=('python' 'python-mutf8' 'python-numpy')
makedepends=('cython' 'python-build' 'python-installer' 'python-setuptools' 'python-versioneer' 'python-wheel')
# tests directory isn't in pypi sdist
source=("$pkgname-$pkgver.tar.gz::$url/archive/$pkgver.tar.gz")
sha256sums=('efefff4b3d29e44c9fee6b30c37d7f68c22c397f88bcd8a6bb671a19add038fd')

prepare() {
  cd "$_name-$pkgver"

  # use current versioneer
  sed -Ei 's/(versioneer)-518/\1/' pyproject.toml
}

build() {
  cd "$_name-$pkgver"

  python -m build --wheel --no-isolation
}

check() {
  cd "$_name-$pkgver"

  # based on https://wiki.archlinux.org/title/Python_package_guidelines#Check
  # build directory needs to be before script directory
  CARCH="$CARCH" python -c 'import os, sys; sys.path.insert(0, f"{os.getcwd()}/build/lib.linux-{os.environ['\''CARCH'\'']}-cpython-" + "".join(map(str, sys.version_info[:2]))); from unittest import __main__' discover -s tests
}

package() {
  cd "$_name-$pkgver"

  python -m installer --destdir="$pkgdir" dist/*.whl

  # https://wiki.archlinux.org/title/Python_package_guidelines#Using_site-packages
  local _site_packages="$(python -c 'import site; print(site.getsitepackages()[0])')"

  install -d "$pkgdir/usr/share/licenses/$pkgname"
  local _license_path="$_site_packages/$_pyname-$pkgver.dist-info/LICENSE"
  [ -f "$pkgdir/$_license_path" ] || { echo "License file not found"; exit 1; }
  ln -s "$_license_path" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
