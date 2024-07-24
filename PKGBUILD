# Maintainer: Jiri Pospisil <jiri@jpospisil.com>

pkgname=ntpd-rs
pkgver=1.2.3
pkgrel=1
pkgdesc='A full-featured implementation of the Network Time Protocol, including NTS support.'
url='https://github.com/pendulum-project/ntpd-rs'
arch=('x86_64')
depends=('gcc-libs' 'glibc')
makedepends=('cargo' 'git')
license=('Apache')
changelog=CHANGELOG
source=(
  "https://github.com/pendulum-project/ntpd-rs/archive/refs/tags/v$pkgver.tar.gz"
  'ntpd-rs.service'
  'ntpd-rs-metrics.service')
backup=('etc/ntpd-rs/ntp.toml')
b2sums=('52edbbba000f78afb01e4b8a8d1ce9f03d8ceaa3df34eeaeae6e2fe2cf04c5b12d1d6be6126ea95359966529293834ccf627f3486baf03004548d1359f0205a1'
        '3bddde4990de7c1fb2b792cb2847d51ca00d00283bc337ab4b8786c2459cf0f0e62bc80cc09d4d76267e723e8c17a032c9d000d20e9ca8ba0a5eb2a6a1d980cd'
        '80355c29433138805efd4acbdb6c684a206afae43f75466d3996c100dea534d099049131279ad8d1e5c80ebaa6792b7101cccad91d085e5630c5356c295a3c22')

build() {
  cd "$pkgname-$pkgver"

  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  export CFLAGS+=" -ffat-lto-objects"

  cargo build --release --locked --target x86_64-unknown-linux-gnu
}

package() {
  install -Dm644 -t "$pkgdir/usr/lib/systemd/system" ntpd-rs.service ntpd-rs-metrics.service
  echo "ntpd-rs.service" | install -Dm 644 /dev/stdin "${pkgdir}/usr/lib/systemd/ntp-units.d/50-ntpd-rs.list"

  cd "$pkgname-$pkgver"

  install -Dm644 docs/examples/conf/ntp.toml.default "$pkgdir/etc/ntpd-rs/ntp.toml"
  install -Dm755 -t "$pkgdir/usr/bin" target/x86_64-unknown-linux-gnu/release/{ntp-daemon,ntp-ctl,ntp-metrics-exporter}

  install -Dm755 -t "$pkgdir/usr/share/man/man8" docs/precompiled/man/{ntp-daemon.8,ntp-ctl.8,ntp-metrics-exporter.8}
  install -Dm755 -t "$pkgdir/usr/share/man/man5" docs/precompiled/man/ntp.toml.5
}
