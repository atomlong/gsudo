# Maintainer: Atom Long <atom.long@hotmail.com>

_realname=gsudo
pkgname="${_realname}"
pkgver=2.6.0
pkgrel=1
pkgdesc="Sudo for Windows"
arch=('x86_64' 'i686')
url="https://github.com/gerardog/gsudo"
license=('MIT')
options=('!strip')
install="${_realname}.install"
source=("${_realname}"-${pkgver}.tar.gz::"https://github.com/gerardog/gsudo/archive/v${pkgver}.tar.gz")
sha256sums=('c5d56e9d73a598719748adfc948d64d047afc5129265150d0fe686b748f148cc')

die () {
  echo "$*" >&2
  exit 1
}

[ -n "${CARCH}" ] && CARCH=$(sed -nr 's|^CARCH=\"(\w+).*|\1|p' /etc/makepkg.conf)
case "${CARCH}" in
i686) OUTDIR=artifacts/x86;;
x86_64) OUTDIR=artifacts/x64;;
*) die "Unsupported architecture: ${CARCH}"
esac

prepare() {
  cd "$srcdir/${_realname}-${pkgver}"
  # Disable arm64 build
  sed -i '/arm64/d' build/01-build.ps1
}

build() {
  cd "$srcdir/${_realname}-${pkgver}"
  export PATH=${PATH}:"/c/Program Files/PowerShell/7"
  export DOTNET_ROOT="C:\Program Files\dotnet"
  export CHOCO_ROOT="C:\ProgramData\chocolatey;C:\ProgramData\chocolatey\bin"
  pwsh -command "
  \$Env:PATH+=\";${DOTNET_ROOT};${CHOCO_ROOT}\"
  ./build/00-prerequisites.ps1
  ./build/01-build.ps1"
}

package () {
  cd "$srcdir"/${_realname}-${pkgver}

  install -d "${pkgdir}/usr/bin"
  install -m755 ${OUTDIR}/${_realname}.exe "${pkgdir}/usr/bin"
}
