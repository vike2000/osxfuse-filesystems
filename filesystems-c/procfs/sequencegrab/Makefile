# Sequence Grabbing Library
#
# Dave MacLachlan

OSXFUSE_PROCFS_ARCH=i386

CC = gcc
CFLAGS = -arch $(OSXFUSE_PROCFS_ARCH) -isysroot /Developer/SDKs/MacOSX10.6.sdk
CPPFLAGS = -arch $(OSXFUSE_PROCFS_ARCH) -isysroot /Developer/SDKs/MacOSX10.6.sdk
LDFLAGS = -framework Cocoa -framework CoreAudioKit -framework Foundation -framework QuartzCore -framework QuickTime -framework QuartzCore

OBJECTS = CSGCamera.o CSGImage.o procfs_sequencegrab.o

all: libprocfs_sequencegrab.dylib libprocfs_sequencegrab.a

%o.%m:
	gcc -c $(CFLAGS) -o $@ $<

CSGCamera.o: CSGCamera.m
CSGImage.o: CSGImage.m

procfs_sequencegrab.o: procfs_sequencegrab.mm
	g++ -c $(CPPFLAGS) -o $@ $<

libprocfs_sequencegrab.dylib: $(OBJECTS)
	g++ -dynamiclib -install_name /tmp/libprocfs_sequencegrab.dylib $(CFLAGS) -o $@ $(OBJECTS) $(LDFLAGS)

libprocfs_sequencegrab.a: $(OBJECTS)
	libtool -static -o libprocfs_sequencegrab.a $(OBJECTS)
	ranlib libprocfs_sequencegrab.a

clean:
	rm -f *.o libprocfs_sequencegrab.dylib libprocfs_sequencegrab.a
