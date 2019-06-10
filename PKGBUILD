# Maintainer: TBK <aur at jjtc dot eu>
# Contributor: TBK <aur at jjtc dot eu>

pkgname=sdfat-dkms-git
_gitname=kernel-sdfat
pkgver=26.4f65bdd
pkgrel=1
pkgdesc='FAT12/16/32(VFAT)/64(exFAT) filesytems kernel module - use with DKMS'
arch=('any')
url='https://github.com/cryptomilk/kernel-sdfat'
license=('GPL2')
depends=('dkms')
optdepends=('exfat-utils-nofuse: Tools for managening Exfat')
makedepends=('git' 'linux-headers')
conflicts=('vfat' 'exfat' 'exfat-dkms-git' 'sdfat-dkms-git')
options=('!strip')
source=("${_gitname}::git+${url}.git"
        dkms.conf
        mount.exfat
        fix-blkdev.patch
        fix-kernel_5.patch
        fix-makefile.patch
        fix-module_alias.patch)
sha512sums=('SKIP'
            'd61b198adef98554767c596e9708b64a6ed8cd2ffd382c8cb38938fc467d27e8849969ea94d803683d44397b4f5f0c512c01ed05d0f35fb03611f56401e19a31'
            '85c54950e69e342221343b8b542fda47f80b9a7104e1097a7f1c9a09f69758ef8881c238d2740f3a21aa2b5cdcb3b9b8224c731ec2e39fa4632f56c2df85bc84'
            'a84005de882a14a29252956f009d9785435a1dff7e578537eb5d0f222e2d89925aa95f883cd7817becac384ebaa4cc44ef636a41ea8e063b306cfa33add54a69'
            'c0e4d47b070373da4a3a201289fc56b734f6b31659d27b9772b48a2ea440247b0c619cf71a99dd02041d67ab9fbf9865ff443c70a456389dcc3b3ac985b229d1'
            'a732110e0b697c89f405b1e5086b30a01182e23f40d5c36cc519386519704e914edae474c8199d2d6d3f5825c2006d93ca4911435e286718ca5a18ad76edcf26'
            '5859133537c76b333eaeaa12f5144528891479d21062bbd90560e7f100f074c4f4b5f846b7ad4d720f2ea4595ace88c3bd3f9b67e5c76ff4b9e4d52989dacf96')

pkgver() {
  cd ${_gitname}
  echo $(git rev-list --count master).$(git rev-parse --short master)
}

prepare() {
  # update PACKAGE_VERSION to pkgver
  sed -i "s/PACKAGE_VERSION=\"[-._ 0-9a-zA-Z]*\"/PACKAGE_VERSION=\"${pkgver}\"/g" "${srcdir}/dkms.conf"
  # apply patchs
  cd ${_gitname}
  patch -p1 < "${srcdir}/fix-blkdev.patch"
  patch -p1 < "${srcdir}/fix-makefile.patch"
  patch -p1 < "${srcdir}/fix-module_alias.patch"
  patch -p1 < "${srcdir}/fix-kernel_5.patch"
}

package() {
  rm -fr ${_gitname}/{.git,.gitignore}

  mkdir -p "${pkgdir}/usr/src"
  cp -r ${_gitname} "${pkgdir}/usr/src/sdfat-${pkgver}"

  install -Dm644 "${srcdir}/dkms.conf" "${pkgdir}/usr/src/sdfat-${pkgver}/dkms.conf"
  install -Dm755 "${srcdir}/mount.exfat" "${pkgdir}/usr/bin/mount.exfat"
}
