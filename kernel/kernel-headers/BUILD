cd $SOURCE_DIRECTORY/include/ &&

# Don't conflict with libdrm
# As done in http://www.gentoo-portage.com/sys-kernel/linux-headers
rm -rf drm &&

# Delete useless files
find '(' -name '.install' -o -name '*.cmd' ')' -delete &&

prepare_install &&

for dir in ./* ; do
  mkdir -p ${DESTDIR}/usr/include &&
  cp -a $dir ${DESTDIR}/usr/include/ &&
  chown -R root:root ${DESTDIR}/usr/include/$dir &&

  for subdir in $(find ${DESTDIR}/usr/include/$dir -type d); do
    chmod 755 $subdir
  done &&

  for file in $(find ${DESTDIR}/usr/include/$dir -type f); do
    chmod 644 $file
  done
done
