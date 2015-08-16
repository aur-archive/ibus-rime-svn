# Maintainer: Limao Luo <luolimao+AUR@gmail.com>
# Contributor: GONG Chen <chen.sst@gmail.com>
# Contributor: 網軍總司令

_pkgname=ibus-rime
pkgname=$_pkgname-svn
pkgver=1026
pkgrel=1
pkgdesc="Rime input method engine for ibus"
arch=(i686 x86_64)
url=http://code.google.com/p/rimeime/
license=(GPL3)
depends=(kyotocabinet ibus boost opencc yaml-cpp)
makedepends=(cmake gtest subversion)
install=ibus-rime.install
changelog=ChangeLog

_svntrunk=http://rimeime.googlecode.com/svn/trunk
_svnmod=trunk

build() {
    install -d $_pkgname/
    cd $_pkgname/
    msg "Starting SVN checkout..."
    if [[ -d $_svnmod/.svn ]]; then
        (cd $_svnmod; svn up -r $pkgver)
        msg2 "The local files have been updated."
    else
        svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
    fi
    msg2 "SVN checkout done or server timeout"

    rm -rf $_svnmod-build
    cp -r $_svnmod $_svnmod-build
    cd $_svnmod-build

    msg2 "Building librime..."
    cd librime/
    make

    msg2 "Building ibus-rime engine..."
    cd ../$_pkgname/
    [[ -e cmake ]] || ln -s ../librime/cmake
    install -d build
    pushd build
    cmake ..
    make clean
    make
    popd
}

package() {
    cd $_pkgname/$_svnmod-build/$_pkgname/

    msg2 "Installing binaries..."
    install -Dm644 rime.xml "$pkgdir"/usr/share/ibus/component/rime.xml
    install -Dm755 build/ibus-engine-rime $pkgdir/usr/lib/$_pkgname/ibus-engine-rime

    msg2 "Installing data..."
    for i in ../brise/{default.yaml,essay.kct,{preset,supplement}/*.yaml}; do
        install -Dm644 $i "$pkgdir"/usr/share/$_pkgname/$(basename $i)
    done
    install -Dm644 zhung.png "$pkgdir"/usr/share/$_pkgname/icons/zhung.png
}
