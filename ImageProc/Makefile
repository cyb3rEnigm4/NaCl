# Project Build flags
WARNINGS := -Wno-long-long -Wall -Wswitch-enum -pedantic -Werror
CXXFLAGS := -pthread -std=c++11 $(WARNINGS)

#
# Compute tool paths
#
GETOS := python $(NACL_SDK_ROOT)/tools/getos.py
OSHELPERS = python $(NACL_SDK_ROOT)/tools/oshelpers.py
OSNAME := $(shell $(GETOS))
RM := $(OSHELPERS) rm

PNACL_TC_PATH := $(abspath $(NACL_SDK_ROOT)/toolchain/$(OSNAME)_pnacl)
PNACL_CXX := $(PNACL_TC_PATH)/bin/pnacl-clang++
PNACL_FINALIZE := $(PNACL_TC_PATH)/bin/pnacl-finalize
CXXFLAGS := -I$(NACL_SDK_ROOT)/include $(CXXFLAGS) 
LDFLAGS := -L$(NACL_SDK_ROOT)/lib/pnacl/Release -lppapi_cpp -lppapi

# Declare the ALL target first, to make the 'all' target the default build
all: image_proc.pexe

test_mc: image_proc.hpp test_mc.cpp
	$(CXX) -o test_mc test_mc.cpp -std=c++11

clean:
	$(RM) image_proc.pexe image_proc.bc

image_proc.bc: image_proc.cpp image_proc.hpp instance_factory.hpp improc_instance.hpp singleton_factory.hpp 
	$(PNACL_CXX) -o $@ $< -O2 $(CXXFLAGS) $(LDFLAGS)

image_proc.pexe: image_proc.bc
	$(PNACL_FINALIZE) -o $@ $<

serve:
	python -m SimpleHTTPServer 8000