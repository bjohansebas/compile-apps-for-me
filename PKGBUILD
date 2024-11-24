pkgname=ladybird-git
pkgver=r64004.5056bda0439
pkgrel=1
pkgdesc='Truly independent web browser'
arch=(x86_64)
url='https://github.com/LadybirdBrowser/ladybird'
license=(BSD-2-Clause)
conflicts=(ladybird)
provides=(ladybird)
depends=(ffmpeg libgl qt6-base qt6-tools qt6-wayland qt6-multimedia ttf-liberation)
makedepends=(git cmake ninja curl unzip zip tar autoconf-archive nasm)
options=('!lto' '!debug')
source=(
  "git+$url"
  "git+https://github.com/microsoft/vcpkg/commit/b2cb0da531c2f1f740045bfe7c4dac59f0b2b69c" # 2024-11-16 (Toolchain/BuildVcpkg.sh)
)
sha256sums=(
  'SKIP'
  'SKIP'
)

pkgver() {
  cd ladybird
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${srcdir}"

  export VCPKG_ROOT="${srcdir}/vcpkg"

  cmake \
    --preset default \
    -B build \
    -S ladybird \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX='/usr' \
    -DCMAKE_TOOLCHAIN_FILE="${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake" \
    -DENABLE_INSTALL_HEADERS=OFF \
    -GNinja \
    -Wno-dev
  ninja -C build
}

package() {
  cd "${srcdir}"

  DESTDIR="${pkgdir}" ninja -C build install

  install -Dm644 "ladybird/Base/res/icons/128x128/app-browser.png" "${pkgdir}/usr/share/pixmaps/ladybird.png"

  install -Dm644 ladybird/LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}