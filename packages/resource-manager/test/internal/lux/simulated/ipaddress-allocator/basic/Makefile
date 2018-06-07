#Include youre build make include

include $(TEST_DIR)/support/lux_testcases.mk

LUX_LOGS=../../lux_logs
export TARGET_DIR=$(shell cd ../../../../../ncs/; pwd)
export RM_TEST_DIR=$(shell cd ../../../../..; pwd)
export SIMULATED_DIR=$(shell cd ../../; pwd)

ifeq ($(shell $(RM_TEST_DIR)/greater-or-equal $(ncsvsn_clean) 4.2), yes)
        export TAILF_ID_ALLOC_4_2=1
endif

clean:
	$(MAKE) -C $(TARGET_DIR) clean
	rm -rf $(LUX_LOGS)

clean_cdb:
	-rm $(TARGET_DIR)/ncs-cdb/*.cdb

clean_logs:
	-rm $(TARGET_DIR)/logs/*

build:
	$(MAKE) -C $(TARGET_DIR) all

netsim-clean:
	$(MAKE) -C $(TARGET_DIR) netsim-clean

stop:
	$(MAKE) -C $(TARGET_DIR) stop

ncs-stop:
	$(MAKE) -C $(TARGET_DIR) ncs-stop

start:
	cp pyvm.xml $(TARGET_DIR)/ncs-cdb
	$(MAKE) -C $(TARGET_DIR) start

netsim:
	$(MAKE) -C $(TARGET_DIR) netsim

netsim-start:
	$(MAKE) -C $(TARGET_DIR) netsim-start

netsim-stop:
	$(MAKE) -C $(TARGET_DIR) netsim-stop
