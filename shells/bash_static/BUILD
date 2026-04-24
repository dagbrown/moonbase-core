bad_flags -malign-double "-s "

OPTS+=" --bindir=/usr/bin \
        --with-curses \
	--disable-readline \
        --disable-profiling \
        --enable-static-link \
        --without-bash-mallocx"

sedit '/CAN_REDEFINE_GETENV/s,^,// ,' config.h.in &&

default_config &&
make version.h &&
make &&
prepare_install &&

cp -f bash /usr/bin/bash_static
