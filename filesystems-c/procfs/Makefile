# procfs as a OSXFUSE file system for Mac OS X
#
# Copyright 2007 Amit Singh (osxbook.com). All Rights Reserved.

OSXFUSE_PROCFS_ARCH=i386

CXXFLAGS=-D_FILE_OFFSET_BITS=64 -D__FreeBSD__=10 -O -g -I/usr/local/include/osxfuse -arch $(OSXFUSE_PROCFS_ARCH) -isysroot /Developer/SDKs/MacOSX10.6.sdk
LDFLAGS=-L/usr/local/lib -losxfuse -framework Carbon -framework IOKit -framework ApplicationServices -framework Accelerate -framework OpenGL -weak-lproc
SEQUENCEGRAB_LDFLAGS=-framework AudioUnit -framework Cocoa -framework CoreAudioKit -framework Foundation -framework QuartzCore -framework QuickTime -framework QuartzCore

OSXFUSE_PROCFS_ENABLE_CAMERA=1

# Configure this depending on where you installed pcrecpp
# http://www.pcre.org
#
PCRECPP_PREFIX=$(shell pcre-config --prefix)

PCRECPP_CXXFLAGS=-I$(PCRECPP_PREFIX)/include
PCRECPP_LDFLAGS=-arch $(OSXFUSE_PROCFS_ARCH) $(PCRECPP_PREFIX)/lib/libpcrecpp.a $(PCRECPP_PREFIX)/lib/libpcre.a

all: procfs

procfs.o: procfs.cc
	g++ -c -Wall $(CXXFLAGS) $(PCRECPP_CXXFLAGS) -o $@ $<

procfs_displays.o: procfs_displays.cc procfs_displays.h
	g++ -c -Wall $(CXXFLAGS) -o $@ $<

procfs_proc_info.o: procfs_proc_info.cc procfs_proc_info.h
	g++ -c -Wall $(CXXFLAGS) -o $@ $<

procfs_tpm.o: procfs_tpm.cc procfs_tpm.h
	g++ -c -Wall $(CXXFLAGS) -o $@ $<

procfs_windows.o: procfs_windows.cc procfs_windows.h
	g++ -c -Wall $(CXXFLAGS) -o $@ $<

procfs: procfs.o procfs_displays.o procfs_proc_info.o procfs_tpm.o procfs_windows.o sequencegrab/libprocfs_sequencegrab.a
	g++ -Wall $(CXXFLAGS) $(PCRECPP_CXXFLAGS) -o $@ $^ $(LDFLAGS) $(PCRECPP_LDFLAGS) sequencegrab/libprocfs_sequencegrab.a $(SEQUENCEGRAB_LDFLAGS)

sequencegrab/libprocfs_sequencegrab.a:
	(cd sequencegrab && make)

install: procfs
	sudo chown root:wheel procfs
	sudo chmod u+s procfs
	sudo mv procfs /usr/local/bin/procfs
	
	sudo cp -i procfs.plist /Library/LaunchDaemons/com.osxbook.procfs.plist||:
	sudo chown root:wheel   /Library/LaunchDaemons/com.osxbook.procfs.plist
	sudo chmod go-w         /Library/LaunchDaemons/com.osxbook.procfs.plist
	
	@echo >&2 "\n\
		An inactive launchd(1) daemon has been installed.\n\
		It can be activated by running the following\n\
			\n\
			launchctl load -w /Library/LaunchDaemons/com.osxbook.procfs.plist\n"

clean:
	rm -f procfs procfs.o procfs_displays.o procfs_proc_info.o procfs_tpm.o procfs_windows.o
	(cd sequencegrab && make clean)
