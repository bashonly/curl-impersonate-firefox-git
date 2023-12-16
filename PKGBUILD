# Maintainer: bashonly <bashonly@protonmail.com>

pkgname=curl-impersonate-firefox-git
pkgver=r252.845db62
pkgrel=1
pkgdesc='CLI tool & library for transferring data with URLs, compiled with NSS'
arch=('x86_64' 'i686' 'aarch64' 'armv7h')
url='https://github.com/lwthiker/curl-impersonate'
license=('MIT')
depends=(
  'bash'
  'ca-certificates'
  'glibc'
  'libidn2'
  'libldap'
  'libpsl'
  'zlib'
  'zstd'
)
makedepends=(
  'cmake'
  'curl'
  'git'
  'gyp'
  'ninja'
)
provides=(
  'curl-impersonate-firefox'
  'libcurl-impersonate-firefox'
  'libcurl-impersonate-ff.so'
)
conflicts=(
  'curl-impersonate-firefox'
  'curl-impersonate-bin'
  'libcurl-impersonate-bin'
)
_curl_version=8.1.1
source=(
  "curl-impersonate::git+${url}.git"
  "curl-${_curl_version}.tar.xz::https://curl.se/download/curl-${_curl_version}.tar.xz"
  "curl-${_curl_version}.tar.xz.asc::https://curl.se/download/curl-${_curl_version}.tar.xz.asc"
  "brotli-1.0.9.tar.gz::https://github.com/google/brotli/archive/refs/tags/v1.0.9.tar.gz"
  "nghttp2-1.56.0.tar.bz2::https://github.com/nghttp2/nghttp2/releases/download/v1.56.0/nghttp2-1.56.0.tar.bz2"
  "nss-3.92.tar.gz::https://ftp.mozilla.org/pub/security/nss/releases/NSS_3_92_RTM/src/nss-3.92-with-nspr-4.35.tar.gz"
)
noextract=(
  "curl-${_curl_version}.tar.xz"
  "brotli-1.0.9.tar.gz"
  "nghttp2-1.56.0.tar.bz2"
  "nss-3.92.tar.gz"
)
# Import key with: curl https://daniel.haxx.se/mykey.asc | gpg --import
validpgpkeys=('27EDEAF22F3ABCEB50DB9A125CC908FDB71E12C2')  # Daniel Stenberg <daniel@haxx.se>
sha256sums=(
  'SKIP'
  '08a948e061929645597c1ef7194e07b308b22084ff03fa7400b465e6c05149e5'  # curl
  '783be14d644d54e19be7a703c1781511f310024cce6bba4f363069cc5b6fa10e'  # curl signature
  'f9e8d81d0405ba66d181529af42a3354f838c939095ff99930da6aa9cdf6fe46'  # brotli
  # https://github.com/nghttp2/nghttp2/releases/download/v1.56.0/checksums.txt
  '2f5dcdbf577a2df513a2464645484cc35d2e4d0733653d63193ae51db41def41'  # nghttp2
  # https://ftp.mozilla.org/pub/security/nss/releases/NSS_3_92_RTM/src/SHA256SUMS
  '21c176bfffb6ec840b5f985c7f8f014682f4a2fb55b06924734172d5c0486dc5'  # nss
)

prepare() {
  git -C curl-impersonate clean -dfx
  mkdir -p curl-impersonate/build
  cp -t curl-impersonate/build "${noextract[@]}"
}

pkgver() {
  cd curl-impersonate
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build () {
  cd curl-impersonate/build
  ../configure --prefix="/usr"
  make firefox-build
}

package () {
  depends+=('nss')
  cd curl-impersonate/build
  ../configure --prefix="${pkgdir}/usr"
  sed -i "s|prefix = /usr|prefix = ${pkgdir//|/\\|}/usr|" "curl-${_curl_version}"/{,lib/,src/}Makefile
  mkdir -p "${pkgdir}/usr"
  make firefox-install
  install -Dm644 ../LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
