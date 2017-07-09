TARGET=nm-dso

OBJS = \
	main.o 


ifndef EXT_INSTALL
USAGE=1
endif

ifndef HPCT_SRCROOT
USAGE=1
endif

ifndef HPCT_BUILD
USAGE=1
endif

ifndef HPCT_INSTALL
USAGE=1
endif

ifdef USAGE
usage:
	@echo "usage: make EXT_INSTALL=<hpctoolkit-externals install dir> \\"
	@echo "            HPCT_SRCROOT=<hpctoolkit source dir> \\"
	@echo "            HPCT_INSTALL=<hpctoolkit install dir>"
	@echo "            HPCT_BUILD=<hpctoolkit build dir>"
endif

EU_ROOT=$(EXT_INSTALL)/libelf

all: $(TARGET) test

BU_INCLUDE=-I$(HPCT_SRCROOT)/src/lib/binutils
BU_LIB=$(HPCT_BUILD)/src/lib/binutils/libHPCbinutils.la
PL_LIB=$(HPCT_INSTALL)/lib/hpctoolkit/libhpcrun.a 

EU_INCLUDE=-I$(EU_ROOT)
EU_LIB=-L$(EU_ROOT)/lib -Wl,-rpath,$(EU_ROOT)/lib -lelf 


INCLUDES= \
	$(BU_INCLUDE) \
	$(EU_INCLUDE) 

LIBS= \
	$(BU_LIB) \
	$(PL_LIB) \
	$(EU_LIB) \
	-ldl

$(TARGET): main.c $(OBJS) 
	libtool --mode=link gcc -g $(INCLUDES) -o $@ $(OBJS) $(LIBS)

%.o: %.c
	gcc -c -g $(INCLUDES) -o $@ $<


test: $(TARGET)
	@for f in load-modules/*; do echo ./$(TARGET) $$f; ./$(TARGET) $$f; echo; done

clean:
	/bin/rm -f $(TARGET) *.o

