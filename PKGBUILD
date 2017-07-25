# Maintainer : Charles Gueunet <Azrael>

# This package can take a long time to build.
# you should consider adding MAKEFLAGS='-j<nb threads>'
# in your /etc/makepkg.conf to allow parallel build

pkgname=('ttk')
_ttk_ver=0.9.2
pkgver=${_ttk_ver}
pkgrel=1
pkgdesc='the Topology ToolKit: plugins suite for Paraview and VTK.'
arch=('i686' 'x86_64')
url='https://topology-tool-kit.github.io/'
license=('bsd')

depends=('vtk>=7' 'qt5-webkit' 'paraview-ttk')
makedepends=('cmake' 'git')

install=ttk.install

source=("https://codeload.github.com/topology-tool-kit/ttk/tar.gz/v${_ttk_ver}")
sha1sums=('ba24f0234b69f18e483cee8a5d6ddff9ef129a04')

package_ttk() {

    mkdir -p "${srcdir}/build-ttk"
    cd "${srcdir}/build-ttk"

    # plugins
    ttk_prefix="/usr/lib/ttk"
    ttk_plugins="plugins"
    plugins_dir="${ttk_prefix}/${ttk_plugins}"
    install_dir="${pkgdir}${plugins_dir}"

    # flags to enable system libs
    # add PROTOBUF when https://gitlab.kitware.com/paraview/paraview/issues/13656 gets fixed
    local VTK_USE_SYSTEM_LIB=""
    for lib in EXPAT FREETYPE GLEW HDF5 JPEG LIBXML2 OGGTHEORA PNG TIFF ZLIB; do
        VTK_USE_SYSTEM_LIB+="-DVTK_USE_SYSTEM_${lib}:BOOL=ON "
    done

    msg "Build TTK..."
    msg2 "Configuration"

    cmake \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_C_COMPILER=mpicc \
        -DCMAKE_CXX_COMPILER=mpicxx \
        -DCMAKE_VERBOSE_MAKEFILE:BOOL=OFF \
        -DCMAKE_INSTALL_PREFIX:PATH="" \
        -DwithMPI=ON \
        -DwithKamikaze=ON \
        ${VTK_USE_SYSTEM_LIB} \
        ../ttk-${_ttk_ver}

    msg2 "Compilation"

    make DESTDIR="${install_dir}" install

    msg2 "Finishing..."
    msg2 "Add plugins to Paraview"

    env_path="${pkgdir}/etc/profile.d"
    mkdir -p ${env_path}
    echo "export PV_PLUGIN_PATH='${plugins_dir}/usr/lib/paraview-5.4/plugins'" > "${env_path}/ttk.sh"
}
