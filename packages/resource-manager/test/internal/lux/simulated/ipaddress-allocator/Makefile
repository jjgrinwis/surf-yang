include $(TEST_DIR)/support/lux_testarea.mk

#packages/resource-manager/test/internal/lux/simulated/ipaddress-allocator
export TARGET_DIR=$(shell cd ../../../../../../../; pwd)
export RM_TEST_DIR=$(shell cd ../../../..; pwd)
export SIMULATED_DIR=$(shell cd ../; pwd)

ncsvsn=$(shell ncs --version)
ncsvsn_clean=$(shell echo ${ncsvsn} | sed 's/_[0-9]*//')

ifeq ($(shell $(RM_TEST_DIR)/greater-or-equal $(ncsvsn_clean) 4.2), yes)
	export TAILF_ID_ALLOC_4_2=1
endif


build: build_testarea
.PHONY: build

clean: clean_testarea
	rm -rf lux_logs
.PHONY: clean

test: build_testarea test_testarea
.PHONY: test
