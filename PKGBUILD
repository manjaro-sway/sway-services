# Maintainer: Antoine Damhet <antoine.damhet@lse.epita.fr>

_pkgname=sway-services
pkgname=${_pkgname}
pkgdesc="Collection of sway and friends systemd unit files"
pkgver=r33.e3d9b8b
pkgrel=4
arch=(any)
depends=('sway' 'python3' 'python-yaml' 'swayidle')
makedepends=('meson')
optdepends=('python3: for swayidle.service' 'python-yaml: for swayidle.service' 'mako' 'swayidle' 'kanshi')
url="https://github.com/xdbob/sway-services"
_commit="e3d9b8b390dc12224bdca23a0eb78265c824a360"
source=(
	"git+${url}#commit=$_commit"
)
license=('MIT')
md5sums=('SKIP')
provides=('sway-services')
conflicts=('sway-services-git')

prepare() {
	cd "$srcdir/${_pkgname}" || exit
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
	# sway >=1.12 ships its own /usr/lib/systemd/user/sway-session.target,
	# which collides with the copy from upstream sway-services. Drop ours and
	# let the sway package own it; sway-session-pre.target (which sway does not
	# provide) and the wayland-session targets are still installed from here.
	rm -f "${pkgdir}/usr/lib/systemd/user/sway-session.target"
	install -D -m 0644 "${srcdir}/${_pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
