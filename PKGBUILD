# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>

_fdroid="true"
_github="false"
_offline="false"
_git="false"
_pkgname="termux"
_pkg="com.${_pkgname}"
_Pkg="Termux"
pkgname="${_pkgname}-bin"
pkgver=0.118.1
# _commit="e117ccae32d5a7d75479b61f034000122fe9fa24"
_fdroid_pkgrel=1000
pkgrel=1
if [[ "${_fdroid}" == "true" ]]; then
  pkgrel="${_fdroid_pkgrel}"
fi
_pkgdesc=(
  "Terminal emulator application"
  "for Android OS extendible by"
  "variety of packages."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  arm
  aarch64
  x86_64
  i686
)
url="${_pkgname}.dev"
license=(
  GPL3
)
depends=(
)
_os="$( \
  uname \
    -o)"
[[ "${_os}" != "GNU/Linux" ]] && \
[[ "${_os}" == "Android" ]] && \
  depends+=(
    inteppacman
  )
optdepends=(
)
[[ "${_os}" != "GNU/Linux" ]] && \
[[ "${_os}" == "Android" ]] && \
  optdepends+=(
  )
makedepends=(
  coreutils
)
checkdepends=(
)
provides=(
  "${_pkgname}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
)
source=()
sha256sums=()
_url="https://f-droid.org/repo"
_tag="${pkgrel}"
_tag_name="pkgrel"
_tarname="${_pkgname}-${pkgver}-${pkgrel}"
[[ "${_offline}" == "true" ]] && \
  _url="file://${HOME}/${pkgname}"
[[ "${_git}" == true ]] && \
  makedepends+=(
    "git"
  ) && \
  source+=(
    "${_tarname}::git+${_url}#${_tag_name}=${_tag}"
  ) && \
  sha256sums+=(
    SKIP
  )
if [[ "${_git}" == false ]]; then
  if [[ "${_fdroid}" == "true" ]]; then
    if [[ "${_tag_name}" == 'pkgrel' ]]; then
      _src="${_tarname}.apk::${_url}/${_pkg}_${pkgrel}.apk"
      _sig="${_tarname}.apk.sig::${_url}/${_pkg}_${pkgrel}.apk.asc"
      _sum="f137958392a800fca583bfc00f191b8edb29b77c705fddf27dffb6c26ca5d413"
    fi
  elif [[ "${_github}" == "true" ]]; then
    if [[ "${_tag_name}" == "commit" ]]; then
      _src="${_tarname}.zip::${_url}/archive/${_commit}.zip"
      _sum="dacf4a05e8dab38c49034e5d58deb477c36d005fe81324cf7973ba5487d87eb7"
    fi
  fi
  source+=(
    "${_src}"
    "${_sig}"
  ) && \
  sha256sums+=(
    "${_sum}"
    "SKIP"
  )
fi
validgpgkeys=(
  # F-droid binary releases
  "37D2C98789D8311948394E3E41E7044E1DBA2E89"
)

package() {
  local \
    _dest_dir \
    _dest
  _dest_dir="/usr/bin"
  _dest="${_pkgname}.apk"
  if [[ "${_os}" == "Android" ]]; then
    _dest_dir="/system/app/${_Pkg}"
    _dest="base.apk"
  fi
  install \
    -dm755 \
    "${pkgdir}${_dest_dir}"
  install \
    -Dm644 \
    "${srcdir}/${_tarname}.apk" \
    "${pkgdir}/${_dest_dir}/${_dest}"
}

# vim: ft=sh syn=sh et
