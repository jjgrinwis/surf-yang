include $(TEST_DIR)/support/lux_testarea.mk

export TARGET_DIR=$(shell cd ../../../ncs/; pwd)
export RM_TEST_DIR=$(shell cd ../../..; pwd)
export SIMULATED_DIR=$(shell pwd)

ncsvsn=$(shell ncs --version)
ncsvsn_clean=$(shell echo ${ncsvsn} | sed 's/_[0-9]*//')
export NCS_VSN_CLEAN=$(ncsvsn_clean)

ifeq ($(shell $(RM_TEST_DIR)/greater-or-equal $(ncsvsn_clean) 4.2), yes)
	export TAILF_ID_ALLOC_4_2=1
endif

build:
	$(MAKE) build_testarea
.PHONY: build

clean:
	$(MAKE) clean_testarea
	rm -rf lux_logs
.PHONY: clean

test:
	$(MAKE) build_testarea
	$(MAKE) test_testarea
.PHONY: test
