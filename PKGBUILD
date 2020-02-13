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
depends=("nodejs")
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
            '3eb6d42c953879cc6581ad959e501525f4c36220e34769c66baaccb0f173e6e537ae6a26f9e4cd9321731c02ee9db16cf6ff4d41eb37df44c567045c75ba3b75'
            'e13e202ea514ea923967dc2a5b5611bda0275ee19669c196c08449d689fb5749ffe147f0c71141c1708258619404b2ed9870968a9ae22f544132f1ef4fd7ea89'
            'bde1bfa08641b6bebb9f67dc530770368ef46287ce1af9d4421a6e8fc19a0f8c910d086ba3799168789e71868e1f6d6ede0667f4ce68f9a766a6c2496b76a51a'
            '43234ee22a9d5311ca778d6b87c6dc33b20104829c54651b68625c41ace2f0928e3c7e702a639fac077b20978e6b6fa7611f903a04ef27b95c8b5b6616cb57e4'
            'b2b865c7f295d9c9a0e0bcfc6a9c841582a5df4732b663d81eae5adb87d3cd0a8917dc338e9e5c6b58c349e54afe72522c698e7172fcd34bca0c7b59ce1a9d41'
            '3f0b7e333b169871dc59c676974348652b43284dfac003a06762b0cae942f88d2ea3b3c8da96b0f8f739337c1ef55b08e03b8c8b380bfeaa01a96e994c577315')

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

  # Install license
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
