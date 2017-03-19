# This is an example PKGBUILD file. Use this as a start to creating your own,
# and remove these comments. For more information, see 'man PKGBUILD'.
# NOTE: Please fill out the license field for your package! If it is unknown,
# then please put 'unknown'.

# The following guidelines are specific to BZR, GIT, HG and SVN packages.
# Other VCS sources are not natively supported by makepkg yet.

# Maintainer: Your Name <youremail@domain.com>
pkgname=swift-git # '-bzr', '-git', '-hg' or '-svn'
pkgver=swift.DEVELOPMENT.SNAPSHOT.2017.03.18.a.r16.39473258a4
pkgrel=1
pkgdesc='Swift Programming Language (git master)'
arch=('x86_64')
url="https://swift.org"
license=('Apache')

depends=(
  'libxml2'
  'python'
  'python2'
  'libutil-linux'
  'libedit'
  'icu'
  'libbsd'
  'libblocksruntime'
)

makedepends=(
  'git'
  'cmake'
  'ninja'
  'clang'
  'sqlite'
  'swig'
  'ncurses'
  'rsync'
  'python-pexpect'
  'python2-pexpect'
  'libblocksruntime'
)

provides=(
  'swift-language'
  'lldb'
  'swift-lldb'
)

conflicts=(
  'lldb'
  'swift-lldb'
  'swift-language-git'
  'swift-bin'
  'swift'
  'libkqueue'
)

options=(!strip)

LDFLAGS=""

source=(
  "swift::git+https://github.com/apple/swift.git"
  "llvm::git+https://github.com/apple/swift-llvm.git"
  "clang::git+https://github.com/apple/swift-clang.git"
  "lldb::git+https://github.com/apple/swift-lldb.git"
  "cmark::git+https://github.com/apple/swift-cmark.git"
  "llbuild::git+https://github.com/apple/swift-llbuild.git"
  "swiftpm::git+https://github.com/apple/swift-package-manager.git"
  "swift-corelibs-xctest::git+https://github.com/apple/swift-corelibs-xctest.git"
  "swift-corelibs-foundation::git+https://github.com/apple/swift-corelibs-foundation.git"
  "swift-corelibs-libdispatch::git+https://github.com/apple/swift-corelibs-libdispatch.git"
  "swift-integration-tests::git+https://github.com/apple/swift-integration-tests.git"
  "PathSanitizingFileCheck-python2.patch"
  "foundation-blocks-runtime.patch"
  "build-presets.ini"
)

sha256sums=(
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
)

pkgver() {
	cd "$srcdir/swift"

# Git, tags available
	printf "%s" "$(git describe --long | sed 's/\([^-]*-\)g/r\1/;s/-/./g')"

# Git, no tags available
	# printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
	cd "$srcdir/swift-corelibs-libdispatch"
	git submodule update --init --recursive

	cd "$srcdir/swift"
	git apply "$srcdir/PathSanitizingFileCheck-python2.patch"

	cd "$srcdir/swift-corelibs-foundation"
	git apply "$srcdir/foundation-blocks-runtime.patch"
}

# build() {
# }

package() {
	export LC_CTYPE=en_US.UTF-8
	export LANG=en_US.UTF-8
	installable_package="$(readlink -f ${srcdir}/../swift-${pkgver}.tar.gz)"
	cd "$srcdir/swift"
	utils/build-script \
		--preset-file="${srcdir}/build-presets.ini" \
		--preset=buildbot_arch_linux \
		installable_package="${installable_package}" \
		install_destdir="$pkgdir/"
	tar xf "${installable_package}" -C "$pkgdir/"

    cp "$srcdir/build/buildbot_linux/swift-linux-x86_64/lib/libsourcekitdInProc.so" "$pkgdir/usr/lib/"

    # why is this in here <_<
    rm "$pkgdir/usr/lib/python2.7/site-packages/six.py"
    rm "$pkgdir/usr/lib/python2.7/site-packages/readline.so"
}

# check() {
# }
