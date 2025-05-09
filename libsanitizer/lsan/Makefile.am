AM_CPPFLAGS = -I $(top_srcdir)/include -I $(top_srcdir)
 
# May be used by toolexeclibdir.
gcc_version := $(shell @get_gcc_base_ver@ $(top_srcdir)/../gcc/BASE-VER)

DEFS = -D_GNU_SOURCE -D_DEBUG -D__STDC_CONSTANT_MACROS -D__STDC_FORMAT_MACROS -D__STDC_LIMIT_MACROS 
AM_CXXFLAGS = -Wall -W -Wno-unused-parameter -Wwrite-strings -Wno-long-long -fPIC -fno-builtin -fno-exceptions -fno-rtti -fomit-frame-pointer -funwind-tables -fvisibility=hidden -Wno-variadic-macros
AM_CXXFLAGS += $(LIBSTDCXX_RAW_CXX_CXXFLAGS)
AM_CXXFLAGS += -std=gnu++17
AM_CXXFLAGS += $(EXTRA_CXXFLAGS)
AM_CCASFLAGS = $(EXTRA_ASFLAGS)
ACLOCAL_AMFLAGS = -I m4

noinst_LTLIBRARIES = libsanitizer_lsan.la
if LSAN_SUPPORTED
toolexeclib_LTLIBRARIES = liblsan.la
nodist_toolexeclib_HEADERS = liblsan_preinit.o
endif

sanitizer_lsan_files = \
	lsan_common.cpp \
	lsan_common_linux.cpp \
	lsan_common_mac.cpp

lsan_files = \
	$(sanitizer_lsan_files) \
	lsan.cpp \
	lsan_linux.cpp \
	lsan_mac.cpp \
	lsan_malloc_mac.cpp \
	lsan_allocator.cpp \
	lsan_interceptors.cpp \
	lsan_thread.cpp \
	lsan_posix.cpp

libsanitizer_lsan_la_SOURCES = $(sanitizer_lsan_files)

liblsan_la_SOURCES = $(lsan_files)
liblsan_la_LIBADD = $(top_builddir)/sanitizer_common/libsanitizer_common.la $(top_builddir)/interception/libinterception.la
if LIBBACKTRACE_SUPPORTED
liblsan_la_LIBADD += $(top_builddir)/libbacktrace/libsanitizer_libbacktrace.la
endif
liblsan_la_LIBADD += $(LIBSTDCXX_RAW_CXX_LDFLAGS)
if ENABLE_DARWIN_AT_RPATH
liblsan_darwin_rpath = -Wc,-nodefaultrpaths
liblsan_darwin_rpath += -Wl,-rpath,@loader_path
endif
liblsan_la_LDFLAGS = -version-info `grep -v '^\#' $(srcdir)/libtool-version` \
	$(link_liblsan) $(liblsan_darwin_rpath)
liblsan_preinit.o: lsan_preinit.o
	cp $< $@

# Work around what appears to be a GNU make bug handling MAKEFLAGS
# values defined in terms of make variables, as is the case for CC and
# friends when we are called from the top level Makefile.
AM_MAKEFLAGS = \
	"AR_FLAGS=$(AR_FLAGS)" \
	"CC_FOR_BUILD=$(CC_FOR_BUILD)" \
	"CFLAGS=$(CFLAGS)" \
	"CXXFLAGS=$(CXXFLAGS)" \
	"CFLAGS_FOR_BUILD=$(CFLAGS_FOR_BUILD)" \
	"CFLAGS_FOR_TARGET=$(CFLAGS_FOR_TARGET)" \
	"INSTALL=$(INSTALL)" \
	"INSTALL_DATA=$(INSTALL_DATA)" \
	"INSTALL_PROGRAM=$(INSTALL_PROGRAM)" \
	"INSTALL_SCRIPT=$(INSTALL_SCRIPT)" \
	"JC1FLAGS=$(JC1FLAGS)" \
	"LDFLAGS=$(LDFLAGS)" \
	"LIBCFLAGS=$(LIBCFLAGS)" \
	"LIBCFLAGS_FOR_TARGET=$(LIBCFLAGS_FOR_TARGET)" \
	"MAKE=$(MAKE)" \
	"MAKEINFO=$(MAKEINFO) $(MAKEINFOFLAGS)" \
	"PICFLAG=$(PICFLAG)" \
	"PICFLAG_FOR_TARGET=$(PICFLAG_FOR_TARGET)" \
	"SHELL=$(SHELL)" \
	"RUNTESTFLAGS=$(RUNTESTFLAGS)" \
	"exec_prefix=$(exec_prefix)" \
	"infodir=$(infodir)" \
	"libdir=$(libdir)" \
	"prefix=$(prefix)" \
	"includedir=$(includedir)" \
	"AR=$(AR)" \
	"AS=$(AS)" \
	"LD=$(LD)" \
	"LIBCFLAGS=$(LIBCFLAGS)" \
	"NM=$(NM)" \
	"PICFLAG=$(PICFLAG)" \
	"RANLIB=$(RANLIB)" \
	"DESTDIR=$(DESTDIR)"

MAKEOVERRIDES=

## ################################################################

