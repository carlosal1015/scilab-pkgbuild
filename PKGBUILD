# Maintainer: Carlos Aznarán <caznaranl@uni.pe>
# Contributor: bartus <scilab-aur@bartus.33mail.com>
# Contributor: Felix Golatofski <contact@xdfr.de>
# Contributor: eolianoe <eolianoe [at] gmail [DoT] com>
# Contributor: Kurnevsky Evgeny <kurnevsky@gmail.com>
# Contributor: Victor Dmitriyev <mrvvitek@gmail.com>
pkgname=scilab
pkgver=2024.0.0
pkgrel=1
pkgdesc="A scientific software package for numerical computations"
arch=(i686 x86_64)
url="https://www.${pkgname}.org"
license=(GPL-2.0-or-later BSD-3-Clause CECILL-2.1)
depends=(bwidget arpack eigen suitesparse fftw libmatio
  'java-runtime>=17' 'java-flexdock>=1.2.4' jaf-api jaxb-api
  jgoodies-looks jgoodies-common 'jogl>=2.3.2' 'jrosetta>=1.0.4'
  'apache-lucene>=8.4.0' java-skinlf inetutils beanshell eclipse-ecj
  fop-hyph jeuclid-core 'jgraphx>=1.4.0' javahelp2 saxon-he hdf5-openmpi
  'jlatexmath-fop>=1.0.3' java-qdox xalan-java docbook-xsl 'java-batik>=1.8'
  'java-xmlgraphics-commons>=2.0.1') # checkstyle java-commons-beanutils junit java-hamcrest cobertura
makedepends=(gcc-fortran 'java-environment=17' curl pcre libxml2 pkgconf tk ocaml-num time ant)
source=(https://gitlab.com/${pkgname}/${pkgname}/-/archive/${pkgver}/${pkgname}-${pkgver}.tar.gz
  # https://salsa.debian.org/science-team/scilab/-/raw/debian/${pkgver}+dfsg-5/debian/patches/jar_names_in_configure.patch
  ${pkgname}-strict-jar.patch
  ${pkgname}-LD_LIBRARY_PATH.patch
  ${pkgname}-num.patch)
sha512sums=('cae64d34612ab6924c405f418c5566163931b33523702e5a3d4c300ad71c42aea2f29677d47369a70a56b670510ee5a2d450b5725db1cb25c3604a5c8acfdb07'
  # 'c8763234973f3012725fe5acc98ebd558622a6ccc7be0e5dd8e0f1527a2bde266738e0f36d8d901bf41cd6d2c9e2f4283aaf58e91b85909c60b5d0d4dd7a9338'
  '07989944681251de1f9389e4f079241fc3ff7efc8ed0dbebd4a1af6e193ba10693f9de23ece0e1a4be4c402c58ccccb06f1f982de2ae6fb43c6fdb01d29770ef'
  '7468b50c60427f8fdea871561565743ee8bc7bfc8b31de5cb84d4729441453043bc624399a28c0869d80e972a078014adc48919145a7fc91a300ddca74f2b066'
  '7d69779fa41e8c37bd49ceebcd91c15d6ac46c89624461867ee734596833c80177e1800a8fe94ad329f690d6dec0530feae2e55dce957adf48234908afe74538')

prepare() {
  cd ${pkgname}-${pkgver}/${pkgname}
  # Linked to: https://codereview.scilab.org/#/c/18089
  patch <"${srcdir}"/${pkgname}-strict-jar.patch
  # Fix path, to avoid the following error:
  # An error has been detected while loading /usr/share/scilab//modules/functions/.libs/libscifunctions.so: /usr/share/scilab//modules/functions/.libs/libscifunctions.so: cannot open shared object file: No such file or directory
  patch -p0 <"${srcdir}"/${pkgname}-LD_LIBRARY_PATH.patch
  # OCaml
  patch -p0 <"${srcdir}"/${pkgname}-num.patch
  # # Jakarta
  # patch -p0 <"${srcdir}"/jar_names_in_configure.patch
}

build() {
  cd ${pkgname}-${pkgver}/${pkgname}
  ./configure \
    --prefix=/usr \
    --with-gcc \
    --with-gfortran \
    --with-tk \
    --with-javasci \
    --with-gui \
    --with-xcos \
    --without-modelica \
    --with-jdk=/usr/lib/jvm/java-17-openjdk \
    --without-emf \
    --with-fftw \
    --with-mpi \
    --with-openmp \
    --with-umfpack \
    --with-x \
    --with-matio \
    --disable-build-help \
    --enable-build-localization \
    --disable-static-system-lib \
    --without-install-help-xml \
    --with-hdf5-include=/usr/include \
    --with-hdf5-library=/usr/lib \
    --with-arpack-ng \
    --with-arpack-library=/usr/lib \
    --with-umfpack-library=/usr/include/suitesparse \
    --with-umfpack-include=/usr/lib \
    --disable-build-doxygen \
    FFLAGS="-fallow-argument-mismatch" \
    CFLAGS="$CFLAGS -fcommon" \
    CXXFLAGS="$CXXFLAGS -fcommon"
  make
  make doc
}

package() {
  cd ${pkgname}-${pkgver}/${pkgname}
  make DESTDIR="${pkgdir}" install
  make DESTDIR="${pkgdir}" install-data install-html
  install -Dm 644 COPYING* -t "${pkgdir}/usr/share/licenses/${pkgname}"
}
