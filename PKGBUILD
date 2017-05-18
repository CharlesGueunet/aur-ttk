# Maintainer : Charles Gueunet <Azrael>

# This package can take a long time to build.
# you should consider adding MAKEFLAGS='-j<nb threads>'
# in your /etc/makepkg.conf to allow parallel build

pkgname=('ttk')
_ttk_ver=0.9.1
pkgver=${_ttk_ver}
pkgrel=1
pkgdesc='the Topology ToolKit: plugins suite for Paraview and VTK.'
arch=('i686' 'x86_64')
url='https://topology-tool-kit.github.io/'
license=('bsd')

depends=('vtk' 'paraview-ttk')
makedepends=('cmake' 'git')

install=ttk.install

source=("ttk::https://topology-tool-kit.github.io/stuff/ttk-${_ttk_ver}.tar.gz")
sha1sums=('ea0b1eb37a3e326cee6de8e694a64168c1f91134')

package_ttk() {

    mkdir -p "${srcdir}/build-ttk"
    cd "${srcdir}/build-ttk"

    # plugins
    ttk_suffix="/usr/lib/ttk"
    plugins_suffix="${ttk_suffix}/plugins"
    plugins_path="${pkgdir}${plugins_suffix}"

    cmake \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_C_COMPILER=mpicc \
        -DCMAKE_CXX_COMPILER=mpicxx \
        -DCMAKE_VERBOSE_MAKEFILE:BOOL=OFF \
        -DCMAKE_INSTALL_PREFIX:PATH=${ttk_suffix} \
        -DParaView_DIR="${srcdir}/build-paraview" \
        -DVTK_DIR="${srcdir}/build-vtk" \
        -DwithVTK=1 \
        -DwithVTKGUI=1 \
        -DwithVTKCMD=1 \
        ../ttk-${_ttk_ver}

    make DESTDIR="${plugins_path}" install

    # add plugins location for paraview
    ttk_plugins="/lib/paraview-${_paraview_ver%\.[0-9]}/plugins"
    env_path="${pkgdir}/etc/profile.d"
    mkdir -p ${env_path}
    echo "export PV_PLUGIN_PATH='${plugins_suffix}${ttk_plugins}'" > "${env_path}/ttk.sh"
}
