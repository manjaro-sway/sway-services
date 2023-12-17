# Maintainer: Antoine Damhet <antoine.damhet@lse.epita.fr>

_pkgname=sway-services
pkgname=${_pkgname}-git
pkgdesc="Collection of sway and friends systemd unit files"
pkgver=r32.e0d720e
pkgrel=2
arch=(any)
depends=('sway')
makedepends=('meson')
optdepends=('python3: for swayidle.service' 'python-yaml: for swayidle.service' 'mako' 'swayidle' 'kanshi')
url="https://github.com/xdbob/sway-services"
_commit="e0d720e9651a240e06d9215a569cac153b5e11f0"
source=(
	"git+${url}#commit=$_commit"
	"https://github.com/xdbob/sway-services/pull/28.patch"
)
license=('MIT')
md5sums=('SKIP' 'SKIP')

prepare() {
	cd "$srcdir/${_pkgname}" || exit
	patch -p1 -i "$srcdir/28.patch"
}

pkgver() {
	cd "$srcdir/${_pkgname}" || exit
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
	arch-meson "$srcdir/${_pkgname}" build
}

package() {
	DESTDIR="${pkgdir}" ninja -C "${srcdir}/build" install
	install -D -m 0644 "${srcdir}/${_pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
