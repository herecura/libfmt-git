# Maintainer: Xentec <xentec at aix0 dot eu>
_name=fmt
pkgname="lib$_name-git"
pkgver=4.0.0.r26.g2a619d9
pkgrel=1
pkgdesc="Small, safe and fast formatting library for C++"
arch=('i686' 'x86_64')
url="http://cppformat.github.io"
license=('BSD')

depends=('gcc-libs')
makedepends=('cmake' 'git' 'doxygen' 'nodejs-less' 'python-virtualenv')
checkdepends=('gmock')
conflicts=('libfmt' 'cppformat')
replaces=('cppformat-git')

source=("$pkgname"::'git+https://github.com/fmtlib/fmt')
sha256sums=('SKIP')

pkgver() {
	cd "$pkgname"
	git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
	cd "$pkgname"

	# removes the need for nodejs-clean-css package
	sed -i  "s/'--clean-css',//" doc/build.py
}

build() {
	cd "$pkgname"
	mkdir -p build && cd build

	cmake \
		-DCMAKE_INSTALL_PREFIX=/usr \
		-DCMAKE_BUILD_TYPE=Release \
		-DBUILD_SHARED_LIBS=1 \
		-DFMT_TEST=0 \
		-DFMT_DOC=1 \
		-Wno-dev \
		..

	make
    make doc || true
}

check() {
	cd "$pkgname"/build

	cmake \
		-DFMT_TEST=1 \
		-Wno-dev \
		..

	make
	make test
}

package() {
	cd "$pkgname"/build
	
	make DESTDIR="$pkgdir" install

    cd "$srcdir/$pkgname"
	install -D -m644 LICENSE.rst "$pkgdir/usr/share/licenses/$_name/LICENSE"
	install -D -m644 ChangeLog.rst "$pkgdir/usr/share/doc/$_name/ChangeLog.rst"

	# clean up
	rm -rf "$pkgdir/usr/share/doc/$_name"/{.buildinfo,.doctrees,_sources}
}
