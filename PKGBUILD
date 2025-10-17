pkgname=(
    qt6-qtmultimedia
    qt6-qtmultimedia-ffmpeg
    qt6-qtmultimedia-gstreamer
)
pkgbase=qt6-qtmultimedia
pkgver=6.10.0
pkgrel=2
pkgdesc="Classes for audio, video, radio and camera functionality"
arch=('x86_64')
url="https://www.qt.io"
license=(
    'GPL-3.0-only'
    'LGPL-3.0-only'
    'LicenseRef-Qt-Commercial'
    'Qt-GPL-exception-1.0'
)
depends=(
    'gcc-libs'
    'glibc'
    'libpulse'
    'qt6-qtbase'
)
makedepends=(
    'cmake'
    'ffmpeg'
    'git'
    'gst-plugins-bad-libs'
    'gst-plugins-base'
    'libxrandr'
    'ninja'
    'pipewire'
    'qt6-qtdeclarative'
    'qt6-qtquick3d'
    'qt6-qtshadertools'
    'vulkan-headers'
)
source=(git+https://code.qt.io/qt/${pkgbase#*-}#tag=v${pkgver})
sha256sums=(a48376d0219435d27a0bbe22d3e8451b12f89bed2c88a404282f9293fa85cd2a)

build() {
    cd ${pkgbase#*-}

    local cmake_args=(
        -B flarebird-build
        -G Ninja
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D INSTALL_LIBDIR=lib64
        -D CMAKE_MESSAGE_LOG_LEVEL=STATUS
        -D CMAKE_CXX_FLAGS="${CXXFLAGS} -ffat-lto-objects"
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package_qt6-qtmultimedia() {
    depends+=(
        'qt6-qtmultimedia-gstreamer'
        'qt6-qtmultimedia-ffmpeg'
    )

    cd ${pkgbase#*-}

    DESTDIR=${pkgdir} cmake --install flarebird-build

    # Split plugins
    rm -r ${pkgdir}/usr/lib64/qt6/plugins/

    rm    ${pkgdir}/usr/lib64/cmake/Qt6Multimedia/Qt6Q{FFmpeg,Gstreamer}*

    rm -r ${pkgdir}/usr/include/qt6/Qt{FFmpeg,Gstreamer}MediaPluginImpl \
          ${pkgdir}/usr/lib64/cmake/Qt6{FFmpeg,Gstreamer}MediaPluginImplPrivate \
          ${pkgdir}/usr/lib64/libQt6{FFmpeg,Gstreamer}MediaPluginImpl.a \
          ${pkgdir}/usr/lib64/qt6/metatypes/qt6{ffmpeg,gstreamer}mediapluginimplprivate_metatypes.json \
          ${pkgdir}/usr/lib64/qt6/mkspecs/modules/qt_lib_{ffmpeg,gstreamer}mediapluginimpl_private.pri \
          ${pkgdir}/usr/lib64/qt6/modules/{FFmpeg,Gstreamer}MediaPluginImplPrivate.json
}

package_qt6-qtmultimedia-gstreamer() {
    pkgdesc="Gstreamer backend for qt6-multimedia"
    depends=(
        'gcc-libs'
        'glib2'
        'glibc'
        'gst-libav'
        'gst-plugins-bad-libs'
        'gst-plugins-base'
        'gst-plugins-good'
        'gstreamer'
        'libglvnd'
        'libpulse'
        'qt6-qtbase'
        'qt6-qtmultimedia'
    )

    cd ${pkgbase#*-}

    DESTDIR=${pkgdir} cmake --install flarebird-build/src/plugins/multimedia/gstreamer
}

package_qt6-qtmultimedia-ffmpeg() {
    pkgdesc="FFMpeg backend for qt6-multimedia"
    depends=(
        'ffmpeg'
        'gcc-libs'
        'glibc'
        'libglvnd'
        'libx11'
        'libxext'
        'libxrandr'
        'qt6-qtbase'
        'qt6-qtdeclarative'
        'qt6-qtmultimedia'
    )

    cd ${pkgbase#*-}

    DESTDIR=${pkgdir} cmake --install flarebird-build/src/plugins/multimedia/ffmpeg
}
