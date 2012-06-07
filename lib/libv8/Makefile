PYTHON_LIBSUBDIR=lib
PYTHON_BINSUBDIR=bin
PYTHON_BINEXT=
SCONSVER=$(shell ruby -e 'puts STDIN.lines.grep(/^Version: /).first.split(/ /,2).last' <scons/PKG-INFO)
SCONS=build/scons/install/$(PYTHON_BINSUBDIR)/scons$(PYTHON_BINEXT)
SCONSLIB=build/scons/install/$(PYTHON_LIBSUBDIR)/site-packages/scons-$(SCONSVER)
SCONSSRC=build/scons
V8SRC=build/v8
LIBV8=build/v8/libv8.a
LIBV8_G=build/v8/libv8_g.a
GCC_VERSION=$(shell ruby -e 'puts %x{gcc --version} =~ /(\d)\.(\d)\.\d/ ? $$1 + $$2 : "UNKNOWN"')
BUILDBASE=$(shell pwd)
ARCH=$(shell ruby detect_cpu.rb)
TOOLCHAIN=

all: $(LIBV8)

debug: $(LIBV8_G)
	cp $(LIBV8_G) $(LIBV8)

$(LIBV8): $(SCONS) $(V8SRC)
	cd build/v8 && env GCC_VERSION=$(GCC_VERSION) SCONS_LIB_DIR=$(BUILDBASE)/$(SCONSLIB) ../../$(SCONS) arch=$(ARCH) toolchain=$(TOOLCHAIN)

$(LIBV8_G): $(SCONS) $(V8SRC)
	cd build/v8 && env GCC_VERSION=$(GCC_VERSION) SCONS_LIB_DIR=$(BUILDBASE)/$(SCONSLIB) ../../$(SCONS) arch=$(ARCH) toolchain=$(TOOLCHAIN) mode=debug

$(SCONS): $(SCONSSRC)
	mkdir -p $(SCONSSRC)/install
	python build/scons/setup.py install --prefix=install

$(V8SRC): build
	cp -r v8 build
	patch -td build/v8 -i ../../fpic-on-linux-amd64.patch

$(SCONSSRC): build
	cp -r scons build

build:
	mkdir -p build

scons: $(SCONS)

clean:
	rm -rf build
