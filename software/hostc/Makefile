UNAME := $(shell uname)

ifeq ($(UNAME), Darwin)
        # Default install location for libusb-1.0 on Mac OS
	CFLAGS += -I/usr/local/include/libusb-1.0
	LDFLAGS += -L/usr/local/lib -lusb-1.0 -lsdl2

	# Large file support
	CFLAGS += -DFILE_OFFSET_BITS=64
else
        # Load libusb via pkg-config
	PACKAGES := libusb-1.0
	CFLAGS += $(shell pkg-config --cflags $(PACKAGES))
	LDFLAGS += $(shell pkg-config --libs $(PACKAGES)) -lSDL2

	# Large file support
	CFLAGS += $(shell getconf LFS_CFLAGS)

	CFLAGS += -fPIC
endif

MAIN := hostc

# Local headers
CFLAGS += -I../include

MAIN_OBJS := main.o  fastftdi.o fpgaconfig.o bit_file.o hw_common.o ftdieep.o 

CFLAGS += -O3 -g --std=c99

all: $(MAIN)

$(MAIN): $(MAIN_OBJS)
	cc -o $@ $^ $(LDFLAGS) -lpthread
  
*.o: *.h Makefile

clean:
	rm -f $(MAIN) $(OBJS)
