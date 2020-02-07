# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

_relname="add-gitignore"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=1.1.1
pkgrel=1
# epoch=
pkgdesc="Interactive CLI tool to generate .gitignore files."
arch=("any")
url="https://github.com/TejasQ/add-gitignore"
license=("MIT")
# groups=()
depends=("bash" "coffeescript" "nodejs")
# optdepends=()
makedepends=("jq" "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE.md"
"MIT.md"
"README.md"
)
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('5d3e94dbed4e1a6eda647dce735e28915c57e113e3a187f54fb2e49bd1a1fffdb2738f394b698d27a48380bcffaf1fcf88c26a36b3bb113325eb4cec0e9d22b0'
            '97bf4be8ac17b712e1e649c1ffef53c9b4693318bdda76d1ecd6f66100ec7ea63b8487af1b9bb421ffb2bbfa7b2cc75ebb432c3a0cde6caccef4dd24599ddf3a'
            '3fe60d4dd9b368f60b3803699d56d7f0f37b1753b17dd293eb3e45812edc05f6a29b13e6f026dfb7f9cfe5fc6e996beccb446b4a7e307c0b1628509cb5f96c63'
            '7ba956cd2fe97fca9e5622dccc57642de5db3964ce47f939f163c5123fe7445e3edf0cab34517232e1813050fb0499f623bcdc84f6aa6a4606fc80678fd87780'
            'a15949a660a8dd769a09509930cbe931dbd0e4832956744ed78d7df796fc6eaf7d1add2a59059aed5a01283e9711e27ac87ae93fb9799bcd3869952209232771'
            'abbd0edda28a8b524411d936dbf3136772eb5d33a30bacd26f208ce150d9e2a0ac49511faccc27d1df6c5eead78d15f32bc6cfb4cec51455fbd25f11b833ca3b'
            '9d471b340b7d5adb44f118bc4967ee54e969307ba8adf901c527c759f9c20bfe042fe5be9ac86f8e30e9f531e05dc0bb8c1a5d68bd72b47bd508640b4bc37d30')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done <   <(find "$pkgdir" -not -group root -not -user root)

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # Remove references to $pkgdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

  # echo $srcdir

  # Install license
  # install -Dm 644 "$srcdir/package/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -s ../../../lib/node_modules/eslint/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Create Archiv8 documentation folder
  install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
}
