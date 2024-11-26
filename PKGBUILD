# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>

_fdroid="false"
_github="true"
_strip="false"
if [[ "${_fdroid}" == "true" ]]; then
  _strip="true"
fi
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
_arch="$( \
  uname \
    -m)"
_aarch="${_arch}"
if [[ "${_arch}" == "armv7l" ]]; then
  _aarch="armeabi-v7a"
elif [[ "${_arch}" == "aarch64" ]]; then
  _aarch="arm64-v8a"
fi
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
  'coreutils'
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
_fdroid_url="https://f-droid.org/repo"
_http="https://github.com"
_ns="${_pkgname}"
_github_url="${_http}/${_ns}/${_pkgname}-app"
# _tag="${pkgrel}"
_tag="${pkgver}"
# _tag_name="pkgrel"
_tag_name="pkgver"
_tarname="${_pkgname}-${pkgver}-${pkgrel}"
[[ "${_offline}" == "true" ]] && \
  _url="file://${HOME}/${pkgname}"
source=()
sha256sums=()
if [[ "${_git}" == true ]]; then
  makedepends+=(
    "git"
  )
  _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}"
  _sum="SKIP"
elif [[ "${_git}" == false ]]; then
  if [[ "${_fdroid}" == "true" ]]; then
    _url="${_fdroid_url}"
    if [[ "${_tag_name}" == 'pkgrel' ]]; then
      _src="${_tarname}.apk::${_url}/${_pkg}_${pkgrel}.apk"
      _sig="${_tarname}.apk.sig::${_url}/${_pkg}_${pkgrel}.apk.asc"
      source+=(
        "${_sig}"
      )
      sha256sums+=(
        "SKIP"
      )
      _sum="f137958392a800fca583bfc00f191b8edb29b77c705fddf27dffb6c26ca5d413"
    fi
  elif [[ "${_github}" == "true" ]]; then
    _url="${_github_url}"
    if [[ "${_tag_name}" == "pkgver" ]]; then
      _dl_name="${_pkgname}-app_v${pkgver}+github-debug_${_aarch}.apk"
      _src="${_url}/releases/download/v${pkgver}/${_dl_name}"
      _sum="ciao"
      if [[ "${_aarch}" == "x86_64" ]]; then
        _sum="cc63b1cda554adf84a152f99703847871790943fb0ce492ec6d200bd0bec2dc6"
      fi
    elif [[ "${_tag_name}" == "commit" ]]; then
      _src="${_tarname}.zip::${_url}/archive/${_commit}.zip"
      _sum="dacf4a05e8dab38c49034e5d58deb477c36d005fe81324cf7973ba5487d87eb7"
    fi
  fi
fi
source+=(
  "${_src}"
)
sha256sums+=(
  "${_sum}"
)
noextract=(
  "${_src}"
)
validgpgkeys=(
  # F-droid binary releases
  "37D2C98789D8311948394E3E41E7044E1DBA2E89"
)

_strip_extra_libs() {
  mkdir \
    -p \
    "${srcdir}/build"
  cd \
    "build"
  7z \
    -y \
    x \
    "${srcdir}/${_tarname}.apk" 1>&/dev/null
  _extra_libs=(
    $(find \
        "lib" \
        -type \
          "f" \
        ! -wholename \
          "lib/${_aarch}/*")
  )
  _manifests=(
    $(find \
      "META-INF" \
      -type \
        "f" \
      -wholename \
        "*.MF") 
    $(find \
      "META-INF" \
      -type \
        "f" \
      -wholename \
        "*.SF") 
  )
  for _lib in "${_extra_libs[@]}"; do
    for _manifest in "${_manifests[@]}"; do
      echo \
        "removing mentions of '${_lib}' from '${_manifest}'"
      sed \
        "\|${_lib}|,+1d" \
        -i \
        "${_manifest}"
      cat \
        "${_manifest}" | \
        grep \
          "${_lib}" || \
        true
      rm \
        -rf \
        "${_lib}" || \
        true
    done
  done
  # TODO: resign apk as simply rezipping it
  # won't work unless signature verification
  # is disabled
  _tarname="${_tarname}-clean"
  7z \
    "a" \
    "${srcdir}/${_tarname}.zip" \
    . 1>/dev/null
  cd ..
  mv \
   "${_tarname}.zip" \
   "${_tarname}.apk"
}

package() {
  local \
    _dest_dir \
    _dest \
    _extra_libs=() \
    _manifest \
    _manifests=() \
    _lib
  _dest_dir="/usr/bin"
  _dest="${_pkgname}.apk"
  if [[ "${_os}" == "Android" ]]; then
    _dest_dir="/system/app/${_Pkg}"
    _dest="base.apk"
  fi
  if [[ "${_fdroid}" == "true" ]]; then
    _strip_extra_lib
  fi
  install \
    -dm755 \
    "${pkgdir}${_dest_dir}"
  install \
    -Dm644 \
    "${srcdir}/${_tarname}.apk" \
    "${pkgdir}${_dest_dir}/${_dest}"
}

# vim: ft=sh syn=sh et
