# Maintainer: James Humphries <james@james-humphries.co.uk>

pkgname=monodevelop-git
pkgver=6.1r41620.5f38813
pkgrel=1
pkgdesc="An IDE primarily designed for C# and other .NET languages"
arch=('any')
url="http://www.monodevelop.com"
license=('GPL')
depends=('mono>=4.0.1' 'mono-addins>=0.6.2' 'gnome-sharp' 'desktop-file-utils' 'hicolor-icon-theme')
makedepends=('rsync' 'cmake' 'git' 'nuget')
options=(!makeflags)
optdepends=('xsp: To run ASP.NET pages directly from monodevelop')
install=monodevelop-git.install
source=(git://github.com/mono/monodevelop.git)
md5sums=('SKIP')
conflicts=('monodevelop')

build() {
  export MONO_SHARED_DIR=$srcdir/src/.wabi
  mkdir -p $MONO_SHARED_DIR

  cd $srcdir/monodevelop
  git submodule update --init --recursive || return 1
  git clean -dfx

  ./configure --prefix=/usr --profile=stable
  LD_PRELOAD="" make
}

pkgver() {
  cd $srcdir/monodevelop
  printf "%sr%s.%s" "$(cat $srcdir/monodevelop/version.config | head -n1 | sed 's/Version=//g')" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

package() {
  cd $srcdir/monodevelop

  LD_PRELOAD="" make DESTDIR=$pkgdir install
  # delete conflicting files
  find $pkgdir/usr/share/mime/ -type f -exec rm {} \;
  rm -r $MONO_SHARED_DIR

  # NuGet.exe is missing somehow, fixed FS#43423
  install -Dm755 "${srcdir}"/monodevelop/main/external/nuget-binary/NuGet.exe "${pkgdir}"/usr/lib/monodevelop/AddIns/MonoDevelop.PackageManagement/NuGet.exe
}

