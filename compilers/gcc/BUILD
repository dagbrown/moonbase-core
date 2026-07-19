# using -pipe causes spurious test-suite failures
bad_flags -pipe &&

export DESTDIR=$(pwd)/INSTALL_$VERSION &&
mkdir $DESTDIR &&

mkdir BUILD &&
cd BUILD &&

LANGUAGES="${LANGUAGES:=c++,fortran}" &&

OPTS+=" --build=$BUILD                \
        --host=$BUILD                 \
        --prefix=/usr                 \
        --libdir=/usr/lib             \
        --infodir=/usr/share/info     \
        --mandir=/usr/share/man       \
        --enable-languages=$LANGUAGES \
        --enable-gnu-unique-object    \
        --enable-ckecking=release     \
        --enable-target-optspace      \
        --enable-linker-build-id      \
        --enable-threads=posix        \
        --enable-__cxa_atexit         \
        --enable-clocale=gnu          \
        --enable-shared               \
        --enable-libmpx               \
        --enable-plugin               \
        --enable-lto                  \
        --with-gnu-ld                 \
        --with-system-zlib            \
        --disable-nls                 \
        --disable-werror              \
        --disable-libssp              \
        --disable-multilib            \
        --disable-libstdcxx-pch       \
        --disable-libunwind-exceptions"

../configure $OPTS &&

make CFLAGS='-O' LIBCFLAGS='-g -O2' LIBCXXFLAGS='-g -O2 -fno-implicit-templates' bootstrap-lean &&
make install DESTDIR=$DESTDIR &&
if [ -d $DESTDIR/usr/lib64 ]
then
    mv $DESTDIR/usr/lib64/* $DESTDIR/usr/lib
    rmdir $DESTDIR/usr/lib64
fi &&

( 

  cd $DESTDIR/usr/libexec/gcc/`arch`-pc-linux-gnu/$VERSION &&

  for file in *
  do
    if [ -f $file ]
    then
      strip $file || true
    fi
  done &&

  cd $DESTDIR/usr/bin &&
  strip * || true
) &&

install -d $DESTDIR/usr/share/gdb/auto-load &&
mv $DESTDIR/usr/lib/libstdc++.so.6.*-gdb.py $DESTDIR/usr/share/gdb/auto-load/ &&

ln -sf /usr/bin/cpp $DESTDIR/usr/lib/gcc/$BUILD/$VERSION/cpp &&
ln -sf gcc $DESTDIR/usr/bin/cc &&

# POSIX conformance launcher scripts for c89 and c99
install -m0755 {${SCRIPT_DIRECTORY},$DESTDIR/usr/bin}/c89 &&
install -m0755 {${SCRIPT_DIRECTORY},$DESTDIR/usr/bin}/c99 &&

# remove the offending broken freetype link
rm -f $DESTDIR/usr/lib/gcc/$BUILD/$VERSION/include-fixed/freetype

prepare_install &&
cp -rfv --remove-destination $DESTDIR/* /
