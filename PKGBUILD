#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [FixMe]: Namcap warnings and errors

# Maintainer: Ross Clark <https://github.com/orgs/Archiv8/add-gitignore/discussions>
# Contributor: Ross Clark <https://github.com/orgs/Archiv8/add-gitignore/discussions>

# pkgbase=
pkgname="add-gitignore"
pkgver=1.1.1
pkgrel=4
# epoch=
pkgdesc="Interactive CLI tool to generate .gitignore files."
arch=("any")
url="https://github.com/TejasQ/add-gitignore"
license=("MIT")
# groups=()
depends=(
  # Official Arch Linux repositories
  "nodejs"
)
# optdepends=()
makedepends=(
  # Official Arch Linux repositories
  "jq"
  "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
# changelog="CHANGELOG.md"
source=(
  "https://registry.npmjs.org/$pkgname/-/$pkgname-$pkgver.tgz"
  # "CC-by-SA-v4.md"
  # "CHANGELOG.md"
  # "ISSUES.md"
  # "LICENSE.md"
  # "MIT.md"
  # "README.md"
)
noextract=("$pkgname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=(
  "5d3e94dbed4e1a6eda647dce735e28915c57e113e3a187f54fb2e49bd1a1fffdb2738f394b698d27a48380bcffaf1fcf88c26a36b3bb113325eb4cec0e9d22b0"
)

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$pkgname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done < <(find "$pkgdir" -not -group root -not -user root)

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
  local pkgjson="$pkgdir/usr/lib/node_modules/$pkgname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"
  rm -rf "$pkgdir/usr/lib/node_modules/root"
  # Install license
  # install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
  # ln -s ../../../lib/node_modules/eslint/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Create Archiv8 documentation folder
  # install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  # install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  # install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  # install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  # install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  # install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
  # install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
}
