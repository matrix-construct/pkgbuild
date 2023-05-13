# Maintainer: 


_rocksdb_version=8.1.1    # Should match the version of rocksdb shipped in the offical arch repositories

pkgname=('matrix-construct')
pkgver=0.9.578
pkgrel=1
pkgdesc="A Matrix homeserver written in C++ powered by RocksDB"
arch=('x86_64')
url="https://github.com/matrix-construct/construct"
license=('BSD')
depends=('boost-libs' 'file' 'icu' 'jemalloc' 'libatomic_ops' 'libpng' 'libsodium' 'lz4' 'openssl' 'rocksdb' 'xz' 'zstd')
makedepends=('autoconf2.13' 'autoconf-archive' 'boost' 'cmake' 'imagemagick' 'opencl-headers')
optdepends=('imagemagick: Thumbnail generation'
	    "ocl-icd: Enables Construct's AI/ML features")
source=("$pkgname-$pkgver::git+https://github.com/matrix-construct/construct.git#tag=$pkgver"
	"rocksdb-$_rocksdb_version.tar.gz::https://github.com/facebook/rocksdb/archive/refs/tags/v$_rocksdb_version.tar.gz"
	"matrix-construct@.service"
	"matrix-construct.sysusers"
	"matrix-construct.tmpfiles")

sha512sums=('SKIP'
	    'SKIP'
	    'SKIP'
	    'SKIP'
	    'SKIP'
	   )



prepare() {
	cd "$pkgname-$pkgver"

	rm -rf deps/rocksdb
	ln -sf "${srcdir}/rocksdb-$_rocksdb_version/." deps/rocksdb
	
	./autogen.sh
}

build() {
	cd "$pkgname-$pkgver"

	./configure \
	--prefix=/ --enable-debug \
	bindir=/usr/bin \
	sysconfdir=/etc \
	includedir=/usr/include \
	libdir=/usr/lib \
	datarootdir=/usr/share \
	datadir=/usr/share/construct \
	localstatedir=/var/lib/construct \
	--with-dbdir=/var/lib/construct/db \
	--with-logdir=/var/log/construct \
	runstatedir=/run \
	--with-moduledir=/usr/lib/construct/modules \
	--with-webappdir=/usr/share/webapps

	make
}

#check() {
#	
#}

package() {
	cd "$pkgname-$pkgver"

	make DESTDIR="$pkgdir" install

	# Deleting the webapp
	rm -rf "${pkgdir}/usr/share/construct"
	install -vDm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"

	install -vDm644 "${srcdir}/${pkgname}@.service" -t "${pkgdir}/usr/lib/systemd/system"
	install -vDm644 "${srcdir}/${pkgname}.sysusers" "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
	install -vDm644 "${srcdir}/${pkgname}.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

}
