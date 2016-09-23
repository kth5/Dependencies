host=i686-w64-mingw32
cc=$(host)-gcc
cxx=$(host)-g++
ranlib=$(host)-ranlib

# Local compiler
build := i686-linux-gnu
qtplatform := linux-g++-32

# How many source files to build in parallel within each package
jobs=-j2

# Source and binary directories
src=/src
prefix=/target
work=$(src)/windows
stow=$(prefix)/stow

# Enable ccache if installed
export PATH := /usr/lib/ccache:$(PATH)

include ../makefile.common

# ovveride some versions for windows
gettext=gettext-0.18.3.2

# List of dlls that get included in the final package for Clementine
clementine_dlls=glew32.dll \
		libcdio-16.dll \
		libchromaprint.dll \
		libeay32.dll \
		libechonest.dll \
		libfaac.dll \
		libfaad.dll \
		libffi-6.dll \
		libfftw3-3.dll \
		libFLAC-8.dll \
		libgcrypt-20.dll \
		libgio-2.0-0.dll \
		libglib-2.0-0.dll \
		libgmodule-2.0-0.dll \
		libgmp-10.dll \
		libgnutls-30.dll \
		libgobject-2.0-0.dll \
		libgpg-error-0.dll \
		libgstapp-1.0-0.dll \
		libgstaudio-1.0-0.dll \
		libgstbase-1.0-0.dll \
		libgstcontroller-1.0-0.dll \
		libgstfft-1.0-0.dll \
		libgstnet-1.0-0.dll \
		libgstpbutils-1.0-0.dll \
		libgstreamer-1.0-0.dll \
		libgstriff-1.0-0.dll \
		libgstrtp-1.0-0.dll \
		libgstrtsp-1.0-0.dll \
		libgstsdp-1.0-0.dll \
		libgsttag-1.0-0.dll \
		libgstvideo-1.0-0.dll \
		libgthread-2.0-0.dll \
		libhogweed-4-2.dll \
		libiconv-2.dll \
		libid3tag.dll \
		libintl-8.dll \
		liblastfm.dll \
		libmad-0.dll \
		libmms-0.dll \
		libmp3lame-0.dll \
		libnettle-6-2.dll \
		libogg-0.dll \
		liboil-0.3-0.dll \
		libopus-0.dll \
		liborc-0.4-0.dll \
		liborc-test-0.4-0.dll \
		libplist.dll \
		libpcre-1.dll \
		libprotobuf-9.dll \
		libqjson.dll \
		libspeex-1.dll \
		libspotify.dll \
		libsqlite3-0.dll \
		libtag.dll \
		libtasn1-6.dll \
		libvorbis-0.dll \
		libvorbisenc-2.dll \
		libwinpthread-1.dll \
		libxml2-2.dll \
		pthreadGC2.dll \
		QtCore4.dll \
		QtGui4.dll \
		QtNetwork4.dll \
		QtOpenGL4.dll \
		QtSql4.dll \
		QtSvg4.dll \
		QtXml4.dll \
		ssleay32.dll \
		zlib1.dll
		#libgpod.dll
clementine_gst_plugins=libgstapetag.dll \
		       libgstapp.dll \
		       libgstasf.dll \
		       libgstaudioconvert.dll \
		       libgstaudiofx.dll \
		       libgstaudioparsers.dll \
		       libgstaudioresample.dll \
		       libgstaudiotestsrc.dll \
		       libgstautodetect.dll \
		       libgstcdio.dll \
		       libgstcoreelements.dll \
		       libgstdirectsoundsink.dll \
		       libgstequalizer.dll \
		       libgstfaac.dll \
		       libgstfaad.dll \
		       libgstflac.dll \
		       libgstgdp.dll \
		       libgstgio.dll \
		       libgsticydemux.dll \
		       libgstid3demux.dll \
		       libgstisomp4.dll \
		       libgstlame.dll \
		       libgstlibav.dll \
		       libgstmad.dll \
		       libgstmms.dll \
		       libgstogg.dll \
		       libgstopusparse.dll \
		       libgstplayback.dll \
		       libgstreplaygain.dll \
		       libgstsouphttpsrc.dll \
		       libgstspectrum.dll \
		       libgstspeex.dll \
		       libgsttaglib.dll \
		       libgsttcp.dll \
		       libgsttypefindfunctions.dll \
		       libgstudp.dll \
		       libgstvolume.dll \
		       libgstvorbis.dll \
		       libgstwavpack.dll \
		       libgstwavparse.dll
clementine_imageformats=qgif4.dll \
			qjpeg4.dll
dont_strip=-not -name glew32.dll -not -name zlib1.dll -not -name libspotify.dll \
	   -not -name ssleay32.dll -not -name libeay32.dll \
	   -not -name ShellExecAsUser.dll \
	   -not -name KillProc.dll \
	   -not -name AccessControl.dll

# Useful macros for building packages
pkgconfig=PKG_CONFIG_LIBDIR=$(prefix)/lib/pkgconfig
autoconf=ACLOCAL_PATH=$(prefix)/share/aclocal:/usr/share/aclocal
ldflags=-L$(prefix)/lib
# -O2 is required to work around link errors in gettext
cflags=-I$(prefix)/include -O2 -march=i686 -mtune=i686
cxxflags=$(cflags)
configure=$(pkgconfig) LDFLAGS=$(ldflags) CFLAGS="$(cflags)" CXXFLAGS="$(cxxflags)" ./configure --host=$(host) --build=$(build) --prefix=$(prefix)
cmake=$(pkgconfig) cmake .. -DCMAKE_TOOLCHAIN_FILE=$(src)/Toolchain-mingw32.cmake -DCMAKE_INSTALL_PREFIX=$(prefix) -DCMAKE_C_FLAGS="$(cflags)" -DCMAKE_CXX_FLAGS="$(cxxflags)"
make=make $(jobs)
extractpkg=tar -xvf $(1)
buildpkg=(cd $(1) && $(configure) && $(make))
buildpkgcmake=(cd $(1) && mkdir build ; cd build && $(cmake) && $(make))
installpkg=rm -rf $(stow)/$(1) && \
	cd $(1) && \
	make install DESTDIR=$(stow)/$(1) && \
	cp -rfv $(stow)/$(1)$(prefix)/* $(stow)/$(1)/ && \
	rm -rfv $(stow)/$(1)$(prefix) && \
	rm -rfv $(stow)/$(1)/share/info && \
	cd $(stow) && \
	stow -R $(1) && \
	touch $(work)/$@
installpkgcmake=rm -rf $(stow)/$(1) && \
	cd $(1)/build && \
	make install DESTDIR=$(stow)/$(1) && \
	rm -rfv $(stow)/$(1)$(prefix) && \
	rm -rfv $(stow)/$(1)/share/info && \
	cp -rfv $(stow)/$(1)$(prefix)/* $(stow)/$(1)/ && \
	cd $(stow) && \
	stow -R $(1) && \
	touch $(work)/$@

extractandbuild=$(call extractpkg,$(1)) && $(call buildpkg,$(2)) && $(call installpkg,$(2))
extractandbuildcmake=$(call extractpkg,$(1)) && $(call buildpkgcmake,$(2)) && $(call installpkgcmake,$(2))


# Top-level targets
clementine-build-deps: pthreads boost liblastfm qt taglib \
		       libgpod glew gstreamer win7sdk qtsparkle chromaprint \
		       protobuf spotify qjson fftw sparsehash echonest
clementine-gst-plugins: gst-plugins-base gst-plugins-good gst-plugins-bad \
			gst-plugins-ugly gst-libav

clementine-deps: clementine-build-deps clementine-gst-plugins openssl glibnetworking
	-rm -rf clementine-deps
	mkdir -p clementine-deps/gstreamer-plugins
	mkdir -p clementine-deps/imageformats
	mkdir -p clementine-deps/nsisplugins
	mkdir -p clementine-deps/gio-modules
	for f in $(clementine_dlls); do \
		cp -v $(prefix)/bin/$$f clementine-deps/ || exit 1; \
	done
	for f in $(clementine_gst_plugins); do \
		cp -v $(prefix)/lib/gstreamer-1.0/$$f clementine-deps/gstreamer-plugins || exit 1; \
	done
	for f in $(clementine_imageformats); do \
		cp -v $(prefix)/plugins/imageformats/$$f clementine-deps/imageformats || exit 1; \
	done
	cp -v $(prefix)/lib/gio/modules/libgiognutls.dll clementine-deps/gio-modules/
	cp -vr $(src)/mingwm10.dll $(src)/nsisplugins clementine-deps/
	-cp -vr /usr/lib/gcc/$(host)/*/libgcc_s_sjlj-1.dll clementine-deps/
	-cp -vr /usr/lib/gcc/$(host)/*/libstdc++-6.dll clementine-deps/
	for f in `find clementine-deps/ -name '*.dll' $(dont_strip)`; do \
		$(host)-strip -v $$f; \
	done


# The actual commands for compiling each package follow

.done-system:
	if [ -d /usr/lib/gcc/i686-w64-mingw32/$(gccver)/ ]; then cp /usr/lib/gcc/i686-w64-mingw32/$(gccver)/*.dll $(prefix)/bin; fi
	if [ -f /usr/i686-w64-mingw32/lib/libwinpthread-1.dll ]; then cp /usr/i686-w64-mingw32/lib/libwinpthread-1.dll $(prefix)/bin; fi 
	if [ -d /usr/i686-w64-mingw32/sys-root/mingw/bin ]; then cp /usr/i686-w64-mingw32/sys-root/mingw/bin/libwinpthread-1.dll $(prefix)/bin; fi
	touch $@

.done-qjson: .done-qt
	$(call extractpkg,$(qjsontgz))
	cd $(qjson) && patch -p0 < ../patches/qjson-01-pkgconfig.patch
	mkdir -p $(qjson)/build
	cd $(qjson)/build && $(cmake) \
		-DQT_HEADERS_DIR=$(prefix)/include/ \
		-DQT_LIBRARY_DIR=$(prefix)/bin
	cd $(qjson)/build && $(make)
	sed -e 's/QtCore//' -i $(qjson)/build/QJson.pc
	rm -rf $(stow)/$(qjson)
	cd $(qjson)/build && make install DESTDIR=$(stow)/$(qjson)
	cd $(qjson)/build && mv -fv $(stow)/$(qjson)$(prefix)/* $(stow)/$(qjson)
	cd $(stow) && stow -R $(qjson)
	touch $@

.done-zlib:
	$(call extractpkg,$(zlibtgz))
	cd $(zlib) && PREFIX="$(host)-" CC=$(cc) CXX=$(cxx) RANLIB=$(ranlib) ./configure --prefix=$(prefix)
	cd $(zlib) && $(make) -j1 -f win32/Makefile.gcc PREFIX="$(host)-" CC=$(cc) CXX=$(cxx) LDSHAREDLIBC="" SHARED_MODE=1
	cd $(zlib) && $(make) -j1 -f win32/Makefile.gcc \
		PREFIX="$(host)-" CC=$(cc) CXX=$(cxx) \
		BINARY_PATH=$(stow)/$(zlib)/bin \
		INCLUDE_PATH=$(stow)/$(zlib)/include \
		LIBRARY_PATH=$(stow)/$(zlib)/lib \
		LDSHAREDLIBC="" SHARED_MODE=1 install
	cd $(stow) && stow -R $(zlib)
	touch $@

.done-iconv:
	$(call extractpkg,$(libiconvtgz))
	$(call buildpkg,$(libiconv))
	rm -rf $(stow)/$(libiconv)
	cd $(libiconv) && make install DESTDIR=$(stow)/$(libiconv)
	mv -fv $(stow)/$(libiconv)$(prefix)/* $(stow)/$(libiconv)
	rm -f $(stow)/$(libiconv)/lib/charset.alias
	cd $(stow) && stow -R $(libiconv)
	touch $@

.done-gettext: .done-iconv
	$(call extractpkg,$(gettexttgz))
	patch -d $(gettext) -p1 < patches/gettext-01-woe32.patch
	patch -d $(gettext) -p1 < patches/gettext-02-reloc.patch
	cd $(gettext) && $(configure) --disable-threads --without-git --disable-java
	$(call buildpkg,$(gettext))
	$(call installpkg,$(gettext))

.done-pcre:
	$(call extractandbuild,$(pcretgz),$(pcre))

.done-glib: .done-zlib .done-pcre .done-gettext .done-ffi
	$(call extractpkg,$(glibtgz))
	cd $(glib) && $(configure) --with-threads=win32 --with-libiconv=native --disable-compile-warnings
	cd $(glib) && $(make)
	$(call installpkg,$(glib))

.done-gmp:
	$(call extractpkg,$(gmptgz))
	cd $(gmp) && $(configure) --enable-shared --disable-static
	cd $(gmp) && $(make)
	$(call installpkg,$(gmp))

.done-nettle: .done-gmp
	$(call extractpkg,$(nettletgz))
	cd $(nettle) && $(configure)
	cd $(nettle) && $(make)
	$(call installpkg,$(nettle))

.done-libgpgerror:
	$(call extractandbuild,$(gpgerrortgz),$(gpgerror))

.done-gcrypt: .done-libgpgerror .done-gettext
	$(call extractpkg,$(gcrypttgz))
	cd $(gcrypt) && $(configure) --with-gpg-error-prefix=$(prefix)
	cd $(gcrypt) && $(make)
	$(call installpkg,$(gcrypt))

.done-libtasn1:
	$(call extractpkg,$(tasn1tgz))
	cd $(tasn1) && $(configure) --disable-gcc-warnings
	cd $(tasn1) && $(make)
	$(call installpkg,$(tasn1))

.done-gnutls: .done-gcrypt .done-libtasn1 .done-nettle
	$(call extractpkg,$(gnutlstgz))
	cd $(gnutls) && $(configure) --with-libgcrypt --disable-cxx --without-p11-kit
	cd $(gnutls) && $(make)
	$(call installpkg,$(gnutls))

.done-win7sdk:
	cp -r $(src)/$(win7sdk) $(stow)/
	cd $(stow) && stow $(win7sdk)
	touch $@

.done-sqlite:
	$(call extractpkg,$(sqlitetgz),$(sqlite))
	cd $(sqlite) && $(configure) --disable-static --enable-shared 
	cd $(sqlite) && $(make) install DESTDIR=$(stow)/$(sqlite) 
	cp -rv $(stow)/$(sqlite)$(prefix)/* $(stow)/$(sqlite)/ && rm -rf $(stow)/$(sqlite)/target
	cd $(sqlite) && $(cc) -shared sqlite3.c -o sqlite3.dll 
	cd $(sqlite) && install -m755 -D sqlite3.dll $(stow)/$(sqlite)/bin/libsqlite3-0.dll
	cd $(stow) && stow -R $(sqlite)
	touch $@

.done-libxml: .done-iconv
	$(call extractpkg,$(xmltgz))
	cd $(xml) && $(configure) --without-python
	cd $(xml) && $(make)
	$(call installpkg,$(xml))

.done-glibnetworking: .done-glib .done-gnutls
	$(call extractandbuild,$(glibnetworkingtgz),$(glibnetworking))

.done-libsoup: .done-glib .done-libxml
	$(call extractpkg,$(souptgz))
	cd $(soup) && patch -p0 < ../patches/libsoup-01-winsock.patch
	(cd $(soup) && $(configure) && $(make))
	$(call installpkg,$(soup))
	touch $@

.done-libplist: .done-libxml .done-glib
	$(call extractpkg,$(plisttgz))
	cd $(plist) && $(configure) --without-cython
	cd $(plist) && $(make)
	rm -rf $(stow)/$(plist)
	cd $(plist) && make install DESTDIR=$(stow)/$(plist)
	mv -fv $(stow)/$(plist)$(prefix)/* $(stow)/$(plist)/
	cd $(stow) && stow $(plist)
	touch $@

.done-libgpod: .done-sqlite .done-libplist .done-glib
	$(call extractpkg,$(gpodtgz))
	cd $(gpod) && $(configure)
	cd $(gpod)/src && $(make) install DESTDIR=$(stow)/$(gpod)
	cp -rv $(stow)/$(gpod)$(prefix)/* $(stow)/$(gpod)/ && rm -rf $(stow)/$(gpod)/target
	cd $(stow) && stow -R $(gpod)
	touch $@

.done-gstreamer: .done-glib .done-gcrypt .done-libxml .done-orc
	$(call extractpkg,$(gsttgz))
	cd $(gst) && patch -Np0 -i ../patches/gst-01-clockid_t.patch
	cd $(gst) && $(configure) --disable-examples --disable-tests
	cd $(gst) && $(make)
	$(call installpkg,$(gst))

.done-gst-plugins-base: .done-gstreamer .done-ogg .done-vorbis
	$(call extractpkg,$(gstbasetgz))
	cd $(gstbase) && autoreconf
	cd $(gstbase) && $(configure) --disable-examples --disable-x --disable-xvideo --disable-xshm
	cd $(gstbase) && $(make)
	$(call installpkg,$(gstbase))

.done-orc:
	$(call extractandbuild,$(orctgz),$(orc))

.done-ogg:
	$(call extractandbuild,$(oggtgz),$(ogg))

.done-vorbis: .done-ogg
	$(call extractandbuild,$(vorbistgz),$(vorbis))

.done-gst-plugins-good: .done-gstreamer .done-gst-plugins-base .done-flac .done-speex .done-libsoup .done-wavpack
	$(call extractpkg,$(gstgoodtgz))
	cd $(gstgood) && patch -p1 < ../patches/gst-plugins-good-02-equalizer-writeable-buffers.patch

	cd $(gstgood) && $(configure) --disable-shout2 --disable-examples --disable-x --disable-deinterlace
	sed -i.bak 's/$$ECHO "$$win32_libid_type"/$$ECHO "x86 DLL"/' $(gstgood)/libtool
	cd $(gstgood) && $(make)
	$(call installpkg,$(gstgood))

.done-libmpcdec:
	$(call extractpkg,$(mpcdectgz))
	cd $(mpcdec) && \
		CFLAGS="-fno-common" \
		CXXFLAGS="-fno-common" \
		LDFLAGS="-Wl,-no-undefined" \
			$(configure) ac_cv_func_memcmp_working=yes
	cd $(mpcdec) && $(make)
	rm -rf $(stow)/$(mpcdec)
	cd $(mpcdec) && make install DESTDIR=$(stow)/$(mpcdec)
	mv -fv $(stow)/$(mpcdec)$(prefix)/* $(stow)/$(mpcdec)/
	cp $(mpcdec)/include/mpcdec/config_win32.h $(stow)/$(mpcdec)/include/mpcdec/
	cd $(stow) && stow -R $(mpcdec)
	touch $(work)/$@

.done-wavpack:
	$(call extractpkg,$(wavpacktgz))
	cd $(wavpack) && \
		CPPFLAGS="-D__MSVCRT_VERSION__=0x0601" \
		CFLAGS="-fno-common" \
		CXXFLAGS="-fno-common" \
		LDFLAGS="-Wl,-no-undefined" \
			$(configure)
	cd $(wavpack) && $(make)
	$(call installpkg,$(wavpack))

.done-flac: .done-ogg
	$(call extractpkg,$(flactgz))
	cd $(flac) && $(configure) --disable-xmms-plugin --disable-asm-optimizations
	cd $(flac) && $(make)
	cd $(flac)/src/libFLAC && $(make) DESTDIR=$(stow)/$(flac) install
	$(call installpkg,$(flac))

.done-speex: .done-ogg
	$(call extractpkg,$(speextgz))
	cd $(speex) && $(configure)
	cd $(speex) && $(make)

	rm -rf $(stow)/$(speex)
	cd $(speex) && make install DESTDIR=$(stow)/$(speex)
	mv -fv $(stow)/$(speex)$(prefix)/* $(stow)/$(speex)/
	rm -f $(stow)/$(speex)/lib/libspeex.*
	sed -i.bak -e's/\/lib/\/bin/' $(stow)/$(speex)/lib/pkgconfig/*.pc
	sed -i.bak2 -e's/-lspeex/-lspeex-1/' $(stow)/$(speex)/lib/pkgconfig/speex.pc
	cd $(stow) && stow -R $(speex)
	touch $@

.done-gst-plugins-ugly: .done-gstreamer .done-gst-plugins-base .done-oil .done-lame .done-cdio
	$(call extractpkg,$(gstuglytgz))
	cd $(gstugly) && \
		$(pkgconfig) LDFLAGS="-L/target/bin $(ldflags)" CFLAGS="$(cflags)" \
		./configure --host=$(host) --build=$(build) --prefix=$(prefix) \
		--disable-examples --disable-static --enable-experimental
	sed -i.bak 's/$$ECHO "$$win32_libid_type"/$$ECHO "x86 DLL"/' $(gstugly)/libtool
	sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' $(gstugly)/libtool
	cd $(gstugly) && $(make)
	$(call installpkg,$(gstugly))

.done-oil:
	$(call extractandbuild,$(oiltgz),$(oil))

.done-id3tag:
	$(call extractpkg,$(id3tagtgz))
	cd $(id3tag) && patch -Np0 -i ../patches/libid3tag-01-remove-cygwin-bits.patch
	cd $(id3tag) && rm -v aclocal.m4 Makefile.in
	cd $(id3tag) && touch NEWS AUTHORS ChangeLog
	cd $(id3tag) && autoreconf -fiv
	cd $(id3tag) && $(configure)
	cd $(id3tag) && $(make)

	# libtool is retarded and doesn't link this dll properly
	cd $(id3tag) && /bin/bash ./libtool --mode=link $(host)-gcc \
		-Wall -g -O2 --shared  -o libid3tag.dll -rpath /target/lib \
		-version-info 3:0:3 *.lo -L/target/lib -lz

	rm -rf $(stow)/$(id3tag)
	cd $(id3tag) && make install DESTDIR=$(stow)/$(id3tag)
	mv -fv $(stow)/$(id3tag)$(prefix)/* $(stow)/$(id3tag)/
	-mkdir $(stow)/$(id3tag)/bin/
	cp $(id3tag)/libid3tag.dll.exe $(stow)/$(id3tag)/bin/libid3tag.dll
	rm -f $(stow)/$(id3tag)/lib/libid3tag.*
	cd $(stow) && stow -R $(id3tag)
	touch $@

.done-mad: .done-id3tag
	$(call extractpkg,$(madtgz))
	#cd $(mad) && patch -Np0 -i ../patches/libmad-01-force-mem.patch
	cd $(mad) && patch -Np0 -i ../patches/libmad-02-remove-cygwin-bits.patch
	cd $(mad) && patch -Np1 -i ../patches/libmad-03-config.patch
	cd $(mad) && patch -Np1 -i ../patches/libmad-04-no-undefined.mingw.patch
	cd $(mad) && patch -Np1 -i ../patches/libmad-05-frame-length.patch
	cd $(mad) && patch -Np1 -i ../patches/libmad-06-optimize.patch
	cd $(mad) && rm -v aclocal.m4 Makefile.in
	cd $(mad) && touch NEWS AUTHORS ChangeLog
	cd $(mad) && autoreconf -iv
	cd $(mad) && $(configure) --enable-shared --disable-static --enable-experimental
	cd $(mad) && $(make)

	cd $(mad) && $(host)-gcc $(cflags) -shared .libs/*.o -o .libs/libmad.dll \
		-Xlinker --out-implib -Xlinker .libs/libmad.dll.a

	rm -rf $(stow)/$(mad)
	cd $(mad) && make install DESTDIR=$(stow)/$(mad)
	cp -rv $(stow)/$(mad)$(prefix)/* $(stow)/$(mad)/
	#cp $(mad)/libmad.dll.exe $(stow)/$(mad)/bin/libmad.dll
	cd $(stow) && stow -R $(mad)
	touch $@

.done-lame:
	$(call extractpkg,$(lametgz),$(lame))
	cd $(lame) && patch -Np0 -i ../patches/lame-01-disable-sse.patch
	cd $(lame) && autoconf
	cd $(lame) && $(configure) --disable-frontend --disable-cpml
	#cd $(lame) && $(make) -f Makefile.unix UNAME='MSDOS' NOUNIXCMD=NO HAVE_NASM=NO \
	#	CC_OPTS="-I$(prefix)/include -Wall -pipe -O3 -march=pentium -march=pentium -mfancy-math-387" \
	#	CC=i686-w64-mingw32-gcc DESTDIR=$(prefix) install
	#touch $@
	$(call installpkg,$(lame))

.done-gst-plugins-bad: .done-gstreamer .done-gst-plugins-base .done-faad .done-faac .done-libmms .done-oil .done-libmpcdec .done-opus
	$(call extractpkg,$(gstbadtgz))
	cd $(gstbad) && \
		$(pkgconfig) LDFLAGS="-L/target/bin $(ldflags)" CFLAGS="$(cflags)" \
		./configure --host=$(host) --build=$(build) --prefix=$(prefix) \
		--disable-acm \
		--disable-apexsink \
		--disable-examples \
		--disable-dccp \
		--disable-dtls
	sed -i.bak 's/$$ECHO "$$win32_libid_type"/$$ECHO "x86 DLL"/' $(gstbad)/libtool
	cd $(gstbad) && $(make)
	$(call installpkg,$(gstbad))

.done-gst-libav: .done-gstreamer .done-gst-plugins-base
	$(call extractpkg,$(gstlibavtgz))
	cd $(gstlibav) && autoreconf
	cd $(gstlibav) && $(configure) --with-ffmpeg-extra-configure="--enable-shared --disable-debug"
	cd $(gstlibav) && $(make)
	rm -rf $(stow)/$(gstlibav)
	cd $(gstlibav) && make install DESTDIR=$(stow)/$(gstlibav)
	mv -fv $(stow)/$(gstlibav)$(prefix)/* $(stow)/$(gstlibav)/
	-mkdir $(stow)/$(gstlibav)/bin/
	cp $(gstlibav)/ext/libav/.libs/*.dll $(stow)/$(gstlibav)/bin/
	cd $(stow) && stow -R $(gstlibav)
	touch $@

.done-faad:
	$(call extractpkg,$(faadtgz))
	cd $(faad) && patch -p0 < ../patches/faad-01-off_t.patch
	$(call buildpkg,$(faad))

	# libtool is retarded and doesn't link this dll properly
	cd $(faad)/libfaad && /bin/bash ../libtool --mode=link $(host)-gcc \
		-iquote ../include -g -O2 --shared -o libfaad.dll -rpath $(prefix)/bin \
		-version-info 2:0:0 *.lo -lm

	rm -rf $(stow)/$(faad)
	cd $(faad) && make install DESTDIR=$(stow)/$(faad)
	mv -fv $(stow)/$(faad)$(prefix)/* $(stow)/$(faad)/
	-mkdir $(stow)/$(faad)/bin/
	cp $(faad)/libfaad/libfaad.dll $(stow)/$(faad)/bin/
	rm -f $(stow)/$(faad)/lib/libfaad.*
	cd $(stow) && stow -R $(faad)
	touch $@

.done-faac:
	$(call extractpkg,$(faactgz))
	cd $(faac) && patch -p0 < ../patches/faac-01-buildfixes.patch
	$(call buildpkg,$(faac))

	# libtool is retarded and doesn't link this dll properly
	cd $(faac)/libfaac && /bin/bash ../libtool --mode=link $(host)-gcc \
		-iquote ../include -g -O2 --shared -o libfaac.dll -rpath $(prefix)/bin \
		-version-info 2:0:0 *.lo -lm

	rm -rf $(stow)/$(faac)
	cd $(faac) && make install DESTDIR=$(stow)/$(faac)
	mv -fv $(stow)/$(faac)$(prefix)/* $(stow)/$(faac)/
	-mkdir $(stow)/$(faac)/bin/
	cp $(faac)/libfaac/libfaac.dll $(stow)/$(faac)/bin/
	rm -f $(stow)/$(faac)/lib/libfaac.*
	cd $(stow) && stow -R $(faac)
	touch $@

.done-libmms: .done-glib
	$(call extractpkg,$(mmswin32tgz))
	cd $(mmswin32) && $(autoconf) ./autogen.sh
	cd $(mmswin32) && patch -p0 < ../patches/mms-01-buildfixes.patch
	cd $(mmswin32) && $(configure)
	cd $(mmswin32) && patch -p0 < ../patches/mms-02-config.patch
	cd $(mmswin32) && patch -p0 < ../patches/mms-03-off_t.patch
	cd $(mmswin32) && $(make)
	$(call installpkg,$(mmswin32))

# Not working - requires iconv.h
#.done-libquicktime: .done-dlfcn
#	$(call extractpkg,$(quicktimetgz))
#	cd $(quicktime) && patch -p0 < ../patches/libquicktime-01-configure.patch
#	cd $(quicktime) && autoconf
#	cd $(quicktime) && $(configure) --without-doxygen --without-ffmpeg --without-libiconv-prefix
#	cd $(quicktime) && $(make)
#	$(call installpkg,$(quicktime))

#.done-dlfcn:
#	-mkdir $(stow)/$(dlfcn)/
#	tar -xzf $(dlfcntgz) -C $(stow)/$(dlfcn)/
#	cd $(stow) && stow -R $(dlfcn)
#	touch $@

.done-usb:
	unzip -o -d $(stow) $(usbwin32zip)
	mv $(stow)/$(usbwin32)/bin/x86/* $(stow)/$(usbwin32)/bin
	mv $(stow)/$(usbwin32)/lib/gcc/* $(stow)/$(usbwin32)/lib
	-rm -f $(stow)/$(usbwin32)/lib/pkgconfig/libusb-1.0.pc
	patch -d $(stow)/$(usbwin32) -p0 < patches/libusb-01-pkgconfig.patch
	cp $(stow)/$(usbwin32)/include/usb.h $(stow)/$(usbwin32)/include/libusb.h
	cd $(stow) && stow $(usbwin32)
	touch $@

.done-pthreads:
	tar -xzf $(pthreadstgz) -C $(stow)
	patch -d $(stow)/$(pthreads) -p0 < patches/pthreads-01-timespec.patch
	cd $(stow) && stow $(pthreads)
	touch $@

.done-taglib: .done-glib .done-opus
	$(call extractpkg,$(taglibtgz))
	-mkdir $(taglib)/build

	cd $(taglib)/build && $(cmake) -DWITH_ASF=ON -DWITH_MP4=ON -DBUILD_SHARED_LIBS=ON
	cd $(taglib)/build && $(make)
	rm -rf $(stow)/$(taglib)
	cd $(taglib)/build && make install DESTDIR=$(stow)/$(taglib)
	cd $(taglib)/build && mv -fv $(stow)/$(taglib)$(prefix)/* $(stow)/$(taglib)
	cd $(stow) && stow -R $(taglib)
	touch $@

.done-qt: .done-openssl .done-sqlite .done-glib
	$(call extractpkg,$(qttgz))
	patch -d $(qt) -p0 < patches/qt-01-codecs.patch
	patch -d $(qt) -p0 < patches/qt-02-dir.patch
	patch -d $(qt) -p1 < patches/qt-03-disable-sslv3.patch
	patch -d $(qt) -p1 < patches/qt-04-moc-boost.patch
	patch -d $(qt) -p0 < patches/qt-05-qclipboard.patch
	patch -d $(qt) -p1 < patches/qt-06-glib-honor-ExcludeSocketNotifiers-flag.patch 
	patch -d $(qt) -p1 < patches/qt-07-webkit-widgets.patch
	patch -d $(qt) -p2 < patches/qt-09-cygwin-mkspecs.patch
	patch -d $(qt) -p1 < patches/qt-4.8.0-build-qtuitools-dynamically.patch 
	#patch -d $(qt) -p1 < patches/qt4-use-correct-pkg-config-static-flags.patch 
	#patch -d $(qt) -p0 < patches/qt-dont-perform-ipc-checks-for-win32.patch 
	#patch -d $(qt) -p1 < patches/qt-dont-set-qt-dll-define-for-static-builds.patch 
	patch -d $(qt) -p2 < patches/4.8.5-qmake-implib.patch
	patch -d $(qt) -p1 < patches/qt-fix-javascript-jit-on-mingw-x86_64.patch 
	patch -d $(qt) -p2 < patches/4.8.7-cygwin-posix.patch
	cd $(qt) &&  PKG_CONFIG_PATH=/target/lib/pkgconfig OPENSSL_LIBS="-llibeay32 -lssleay32" ./configure \
		-opensource -confirm-license \
		-platform $(qtplatform) \
		-xplatform win32-g++ -device-option CROSS_COMPILE=i686-w64-mingw32- \
		-prefix $(stow)/$(qt) \
		-force-pkg-config \
		-no-phonon -no-xmlpatterns -no-qt3support \
		-no-declarative -no-audio-backend \
		-no-script -no-scripttools \
		-nomake demos -nomake examples \
		-nomake docs -no-exceptions \
		-plugin-sql-sqlite -system-sqlite -system-zlib -host-little-endian \
		-openssl-linked \
		-webkit \
		-release -no-debug -shared -fast
	cd $(qt) && $(make)
	cd $(qt) && make install
	mv -fv $(stow)/$(qt)/lib/*.dll $(stow)/$(qt)/bin/
	cd $(stow) && stow -R $(qt)
	touch $@

.done-liblastfm: .done-qt
	$(call extractpkg,$(lastfmtgz))
	-mkdir $(lastfm)/build
	cd $(lastfm)/build && $(cmake) \
		-DQT_HEADERS_DIR=$(prefix)/include/ \
		-DQT_LIBRARY_DIR=$(prefix)/bin
	cd $(lastfm)/build && $(make)
	rm -rf $(stow)/$(lastfm)
	cd $(lastfm)/build && make install DESTDIR=$(stow)/$(lastfm)
	cd $(lastfm)/build && mv -fv $(stow)/$(lastfm)$(prefix)/* $(stow)/$(lastfm)
	cd $(stow) && stow -R $(lastfm)
	touch $@

.done-glew:
	$(call extractpkg,$(glewtgz))
	mkdir -p $(glew)/bin $(glew)/lib
	cd $(glew) && $(host)-gcc -DGLEW_NO_GLU -O2 -Wall -W -Iinclude  -DGLEW_BUILD -o src/glew.o -c src/glew.c
	cd $(glew) && $(host)-gcc -nostartfiles -shared -Wl,-soname,libglew32.dll -Wl,--out-implib,lib/libglew32.dll.a    -o bin/glew32.dll src/glew.o -L/mingw/lib -lglu32 -lopengl32 -lgdi32 -luser32
	cd $(glew) && $(host)-ar cr lib/libglew32.a src/glew.o
	install -D -m644 $(glew)/bin/glew32.dll $(stow)/$(glew)/bin/glew32.dll
	install -D -m644 $(glew)/lib/libglew32.dll.a $(stow)/$(glew)/lib/libglew32.dll.a
	install -D -m644 $(glew)/include/GL/eglew.h $(stow)/$(glew)/include/GL/eglew.h
	install -D -m644 $(glew)/include/GL/glew.h $(stow)/$(glew)/include/GL/glew.h
	install -D -m644 $(glew)/include/GL/glxew.h $(stow)/$(glew)/include/GL/glxew.h
	install -D -m644 $(glew)/include/GL/wglew.h $(stow)/$(glew)/include/GL/wglew.h
	cd $(stow) && stow -R $(glew)
	touch $@

.done-boost:
	tar -xf $(boosttgz) -C $(stow)
	cd $(stow) && stow -R $(boost)
	touch $@

.done-qtsparkle:
	$(call extractpkg,$(qtsparkletgz))
	-mkdir $(qtsparkle)/build
	cd $(qtsparkle)/build && $(cmake) \
		-DQT_HEADERS_DIR=$(prefix)/include/ \
		-DQT_LIBRARY_DIR=$(prefix)/bin \
		-DCMAKE_BUILD_TYPE=Release
	cd $(qtsparkle)/build && $(make)
	rm -rf $(stow)/$(qtsparkle)
	cd $(qtsparkle)/build && make install DESTDIR=$(stow)/$(qtsparkle)
	cd $(qtsparkle)/build && mv -fv $(stow)/$(qtsparkle)$(prefix)/* $(stow)/$(qtsparkle)
	cd $(stow) && stow -R $(qtsparkle)
	touch $@

.done-curl:
	$(call extractandbuild,$(curltgz),$(curl))

.done-fftw:
	$(call extractpkg,$(fftwtgz))
	cd $(fftw) && $(configure) \
		--with-our-malloc16 --with-windows-f77-mangling --enable-shared \
		--disable-static --enable-threads --with-combined-threads
	cd $(fftw) && $(make) DESTDIR=$(stow)/$(fftw) install
	cd $(stow)/$(fftw) && cp -rv target/* . && rm -rf target
	cd $(stow) && stow -R $(fftw) 
	touch $@	


.done-libarchive: .done-libxml
	$(call extractpkg,$(archivetgz))
	-mkdir $(archive)/bin
	cd $(archive) && patch -p0 < ../patches/libarchive-01-no-iconv.patch
	cd $(archive) && patch -p0 < ../patches/libarchive-02-pkgconfig.patch
	cd $(archive)/bin && $(cmake) \
		-DENABLE_OPENSSL=OFF \
		-DENABLE_TAR=OFF \
		-DENABLE_CPIO=OFF \
		-DENABLE_XATTR=OFF \
		-DENABLE_ACL=OFF \
		-DENABLE_TEST=OFF
	cd $(archive)/bin && $(make)
	rm -rf $(stow)/$(archive)
	cd $(archive)/bin && make install DESTDIR=$(stow)/$(archive)
	mv -fv $(stow)/$(archive)$(prefix)/* $(stow)/$(archive)/
	cd $(stow) && stow -R $(archive)
	touch $@

.done-protobuf: .done-system
	# Compile a host version first for protoc
	$(call extractpkg,$(protobuftgz))
	#-cd $(protobuf) && make clean
	cd $(protobuf) && ./configure --prefix=$(prefix)
	cd $(protobuf) && $(make)
	cd $(protobuf) && $(make) install
	-rm -fv $(prefix)/lib/libproto*.la
	-rm -fv $(prefix)/lib/libproto*.a
	-rm -rfv $(prefix)/include/google
	-rm -rf $(prefix)/lib/pkgconfig/protobuf*.pc
	-rm -rfv $(prefix)/bin/libproto*dll
	cd $(protobuf) && $(make) clean
	# Now do the target build
	$(call extractpkg,$(protobuftgz))
	cd $(protobuf) && $(configure) --with-protoc=/target/bin/protoc.exe
	cd $(protobuf) && $(make) install
	touch $@

.done-spotify:
	-mkdir -p $(stow)/$(spotifywin32)
	unzip -o -d $(stow)/$(spotifywin32) $(spotifywin32zip)
	rm -rf $(stow)/$(spotifywin32)/README
	rm -rf $(stow)/$(spotifywin32)/LICENSE
	rm -rf $(stow)/$(spotifywin32)/ChangeLog
	rm -rf $(stow)/$(spotifywin32)/*.txt
	rm -rf $(stow)/$(spotifywin32)/*.xhtml
	rm -rf $(stow)/$(spotifywin32)/docs
	rm -rf $(stow)/$(spotifywin32)/examples
	rm -rf $(stow)/$(spotifywin32)/images
	-mkdir $(stow)/$(spotifywin32)/bin
	-mkdir -p $(stow)/$(spotifywin32)/lib/pkgconfig
	mv -v $(stow)/$(spotifywin32)/lib/libspotify.dll $(stow)/$(spotifywin32)/bin/
	cp $(src)/libspotify.pc $(stow)/$(spotifywin32)/lib/pkgconfig
	cd $(stow) && stow $(spotifywin32)
	touch $@

.done-cdio:
	$(call extractpkg,$(cdiotgz))
	cd $(cdio) && $(configure) \
		--without-cd-drive --without-cd-info \
		--without-cdda-player --without-cd-read --without-iso-info \
		--without-iso-read --disable-largefile
	cd $(cdio) && $(make)
	rm -rf $(stow)/$(cdio)
	cd $(cdio) && make install DESTDIR=$(stow)/$(cdio)
	mv -fv $(stow)/$(cdio)$(prefix)/* $(stow)/$(cdio)/
	rm -rf $(stow)/$(cdio)/share/info
	cd $(stow) && stow $(cdio)
	touch $@

.done-openssl:
	$(call extractpkg,$(openssltgz))
	cd $(openssl) && ./Configure \
		--cross-compile-prefix="$(host)-" \
		--prefix=$(stow)/$(openssl) \
		--shared \
		mingw
	cd $(openssl) && make
	cd $(openssl) && make install
	cd $(stow) && stow -R $(openssl)
	touch $@

.done-sparsehash:
	$(call extractandbuild,$(sparsehashtgz),$(sparsehash))

.done-ffi:
	$(call extractandbuild,$(ffitgz),$(ffi))

.done-opus:
	$(call extractandbuild,$(opustgz),$(opus))

.done-echonest: .done-qt .done-qjson
	$(call extractpkg,$(echonesttgz))
	-mkdir $(echonest)/build
	cd $(echonest)/build && $(cmake) \
		-DQT_HEADERS_DIR=$(prefix)/include/ \
		-DQT_LIBRARY_DIR=$(prefix)/bin \
		-DCMAKE_BUILD_TYPE=Release
	cd $(echonest)/build && $(make)
	rm -rf $(stow)/$(echonest)
	cd $(echonest)/build && make install DESTDIR=$(stow)/$(echonest)
	cd $(echonest)/build && mv -fv $(stow)/$(echonest)$(prefix)/* $(stow)/$(echonest)
	cd $(stow) && stow -R $(echonest)
	touch $@

.done-chromaprint: .done-fftw
	$(call extractpkg,$(chromaprinttgz))
	-mkdir $(chromaprint)/build
	cd $(chromaprint)/build && $(cmake) \
		-DQT_HEADERS_DIR=$(prefix)/include/ \
		-DQT_LIBRARY_DIR=$(prefix)/bin \
		-DCMAKE_BUILD_TYPE=Release
	cd $(chromaprint)/build && $(make)
	rm -rf $(stow)/$(chromaprint)
	cd $(chromaprint)/build && make install DESTDIR=$(stow)/$(chromaprint)
	cd $(chromaprint)/build && mv -fv $(stow)/$(chromaprint)$(prefix)/* $(stow)/$(chromaprint)
	cd $(stow) && stow -R $(chromaprint)
	touch $@
