PROJECT:=NACL_Module
LDFLAGS:=-lavdevice -lavformat -lavcodec -lavutil -lmp3lame -ltheoradec -ltheoraenc -ltheora -lvorbisfile -lvorbisenc -lvorbis -logg -lppapi_cpp -lppapi
CXX_SOURCES:=NACL_Module.cc transcoder.cc

#
# Get pepper directory for toolchain and includes.
#
THIS_MAKEFILE:=$(abspath $(lastword $(MAKEFILE_LIST)))
NACL_SDK_ROOT?=$(abspath $(dir $(THIS_MAKEFILE))../..)

# Project Build flags
#WARNINGS:=-Wno-long-long -Wall -Wswitch-enum -pedantic -Werror
WARNINGS:=-Wno-long-long -Wall -Wswitch-enum -pedantic
CXXFLAGS:=-pthread -std=gnu++98 $(WARNINGS)

#
# Compute tool paths
#
OSNAME:=$(shell python $(NACL_SDK_ROOT)/tools/getos.py)
TC_PATH:=$(abspath $(NACL_SDK_ROOT)/toolchain/$(OSNAME)_x86_newlib)
CXX:=$(TC_PATH)/bin/i686-nacl-g++

# Declare the ALL target first, to make the 'all' target the default build
all: $(PROJECT)_x86_32.nexe $(PROJECT)_x86_64.nexe

# Define 32 bit compile and link rules for C++ sources
x86_32_OBJS:=$(patsubst %.cc,%_32.o,$(CXX_SOURCES))
$(x86_32_OBJS) : %_32.o : %.cc $(THIS_MAKE)
	$(CXX) -o $@ -c $< -m32 -O0 -g $(CXXFLAGS)

$(PROJECT)_x86_32.nexe : $(x86_32_OBJS)
	$(CXX) -o $@ $^ -m32 -O0 -g $(CXXFLAGS) $(LDFLAGS)

# Define 64 bit compile and link rules for C++ sources
x86_64_OBJS:=$(patsubst %.cc,%_64.o,$(CXX_SOURCES))
$(x86_64_OBJS) : %_64.o : %.cc $(THIS_MAKE)
	$(CXX) -o $@ -c $< -m64 -O0 -g $(CXXFLAGS)

$(PROJECT)_x86_64.nexe : $(x86_64_OBJS)
	$(CXX) -o $@ $^ -m64 -O0 -g $(CXXFLAGS) $(LDFLAGS)


# Define a phony rule so it always runs, to build nexe and start up server.
.PHONY: RUN 
RUN: all
	python ../httpd.py

