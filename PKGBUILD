# Maintainer: Bruce Hsieh <bruce_,mail_kh_edu_tw>

pkgname=mpd-sacd
pkgver=0.23.9
pkgrel=1
pkgdesc='MPD with patches for SACD and DVDA ISO playback.'
arch=('i686' 'x86_64' 'aarch64' 'armv7h')
url="https://sourceforge.net/projects/mpd.sacddecoder.p/"
license=(GPL2)
depends=(
  gcc-libs
  glibc
  libcdio
  libcdio-paranoia
  libgcrypt
  libgme
  libmad
  libmms
  libmodplug
  libmpcdec
  libnfs
  libshout
  libsidplayfp
  libsoxr
  # smbclient  # disabled because of https://bugzilla.samba.org/show_bug.cgi?id=11413
  wavpack
  wildmidi
  zlib
  zziplib
)
makedepends=(
  alsa-lib
  audiofile
  avahi
  boost
  bzip2
  chromaprint
  curl
  dbus
  expat
  faad2
  ffmpeg
  flac
  fluidsynth
  fmt
  icu
  jack
  lame
  libao
  libid3tag
  libmikmod
  libmpdclient
  libogg
  libopenmpt
  libpulse
  libsamplerate
  libsndfile
  libupnp
  liburing
  libvorbis
  meson
  mpg123
  openal
  opus
  pipewire
  python-sphinx
  sqlite
  systemd
  twolame
  yajl
)
backup=(etc/mpd.conf)
#options=(debug)
conflicts=('mpd')
provides=("mpd=${pkgver}")
source=(
  "${pkgname}::git+https://git.code.sf.net/p/sacddecoder/mpd/MPD.git#commit=cf54bc62c66b637da47422e5595fdfe572615bf2"
  mpd.conf
  mpd.sysusers
  mpd.tmpfiles
  mpd.service.override
  libfmt.patch
  pipewire.patch
)
sha512sums=('SKIP'
            '25a823740d92da8e186916701413114142eb6ad91a172c592e68b569c8e4f50fa99580e555ccf6cd31fc4f55a09bfe0278efa46e4e76ee0fe02846292fadf3c1'
            '6e467481406279767b709ec6d5c06dbd825c0de09045c52ffa2d21d0604dcfe19b7a92bf42bed25163d66a3a0d1dbde6185a648b433eaf5eac56be90491e2e18'
            'db473db27cd68994c3ee26e78e0fb34d13126301d8861563dcc12a22d62ecb14c4ffb1e0798c6aaccdff34e73bae3fbeeff7b42606c901a2d35e278865cdf35d'
            'c1782b82f9db1d30aece43a07230c5d57370f2494a16e108af03815d83968805472f10f53ea5495cf0e08ff8f245430c3c3bc44025af43aaf9ecd12fcd6afc6c'
            '7cf9d988ae9b5224bd9ef9eef4c2bf292c01cfa3c3864bef0d9e583b3a14dbf53442c47dcc2dcf86c78ba7c69c58b35531b9c9b151956542ebff7156c1d4cab2'
            '6bd9dc97ad50d06d6ec8dfea7fb7b94beda2570bddacd7f65a443ed31c0a9a05fa504daeed65698303934bea6cde2658c9468d210e19880111b813a9ae1eebdd')
b2sums=('SKIP'
        '0969a3c477b6a3f34b44e067e515d7f306414dd14e0163584417b9d071e3cc825898219f7ff66ead7905b15429b8411304052d3b2b14a72e560bfabf9bf0adcf'
        '4ab6e415284c77802a39d0913d701fe55e56f3c22b19557661fbef77e456b5e1d151da4202695282b956602e716a7afdb994aa2fc17368b9a0d0d051d47a3afb'
        'd7b587c25dd5830c27af475a8fdd8102139d7c8fdd6f04fe23b36be030e4411582e289f575c299255ff8183096f7d47247327276f9a24641cbd032d9675b837a'
        '753664445d7d5cc0b36f51ac66549beea403b9731cbcb81b0a782974a0a73d90559ba93e6afcaa470b6f2f5a844c09ef695bdf3b1e6dfee97aa080f41b7fe513'
        'd100dc5867d0f2fbe54c180a0184d3b0106f976f00962b9ac24fe60ea71c33801b823fe7d02202e4be27323fa68de141269b93621d8a08f92d8f7c7b82d75913'
        '91b209d68a11a6ed693b367d790525f09f6ea2723314f98688c773f9772ff34657ee66c7d2912db72ddc496be3c021570558d324b356f21276e83c48811e403d')

prepare() {
	cd "${srcdir}/${pkgname}"
  patch --forward --strip=1 --input="${srcdir}/../libfmt.patch"
  patch --forward --strip=1 --input="${srcdir}/../pipewire.patch"
}

build() {
  local _meson_options=(
    -D documentation=enabled
    -D adplug=disabled
    -D sndio=disabled
    -D shine=disabled
    -D tremor=disabled
    -D b_ndebug=true
    -D sacdiso=true
    -D dvdaiso=true
  )

  # NOTE: sndio conflicts with alsa
  # TODO: package adplug
  # TODO: package shine
  arch-meson "${_meson_options[@]}" build $pkgname
  ninja -C build
}

check() {
  ninja -C build test
}

package() {
  depends+=(
    alsa-lib libasound.so
    audiofile libaudiofile.so
    avahi libavahi-{client,common}.so
    bzip2 libbz2.so
    chromaprint libchromaprint.so
    curl libcurl.so
    dbus libdbus-1.so
    expat libexpat.so
    faad2 libfaad.so
    ffmpeg libav{codec,filter,format,util}.so
    flac libFLAC.so
    fluidsynth libfluidsynth.so
    fmt libfmt.so 
    icu libicui18n.so libicuuc.so
    jack libjack.so
    lame libmp3lame.so
    libao libao.so
    libid3tag libid3tag.so
    libmikmod libmikmod.so
    libmpdclient libmpdclient.so
    libogg libogg.so
    libopenmpt libopenmpt.so
    libpulse libpulse.so
    libsamplerate libsamplerate.so
    libsndfile libsndfile.so
    libupnp libixml.so libupnp.so
    liburing liburing.so
    libvorbis libvorbis{,enc}.so
    mpg123 libmpg123.so
    openal libopenal.so
    opus libopus.so
    pipewire libpipewire-0.3.so
    sqlite libsqlite3.so
    systemd-libs libsystemd.so
    twolame libtwolame.so
    yajl libyajl.so
  )

  DESTDIR="$pkgdir" ninja -C build install
  install -vDm 644 $pkgname/doc/mpdconf.example -t "$pkgdir/usr/share/doc/mpd/"
  install -vDm 644 mpd.service.override "$pkgdir/usr/lib/systemd/system/mpd.service.d/00-arch.conf"
  install -vDm 644 mpd.conf -t "$pkgdir/etc/"
  install -vDm 644 mpd.sysusers "$pkgdir/usr/lib/sysusers.d/mpd.conf"
  install -vDm 644 mpd.tmpfiles "$pkgdir/usr/lib/tmpfiles.d/mpd.conf"
}

# vim: ts=2 sw=2 et:
