PACKAGE_NAMES = cisco-ios id-loop ip-loop ipaddress-allocator-test pool-service \
				resource-manager
PACKAGES = $(PACKAGE_NAMES:%=packages/%)
NETWORK = create-network packages/cisco-ios 1 cisco-ios

.PHONY: all ncs start stop clean cli setup-packages build-packages \
	    netsim-start netsim-stop netsim-clean

all: ncs setup-packages netsim

ncs:
	@echo "Setting up NCS"
	@ncs-setup --dest .

start: netsim-start
	@echo "Starting NCS"
	@ncs

stop: ncs-stop netsim-stop

ncs-stop:
	@echo "Stopping NCS"
	@-ncs --stop

clean: netsim-clean
	@-rm -rf README.ncs logs ncs-cdb ncs.conf packages scripts state

cli:
	@ncs_cli -u admin -C --interactive

setup-packages: build-packages $(PACKAGES)

build-packages:
	$(MAKE) -C ../package all

packages/resource-manager: | packages
	@echo "Setting up $@"
	@(cd packages; ln -s ../../../../resource-manager/ .)

packages/%: | packages
	@echo "Setting up $@"
	@(cd packages; ln -s ../../$(@:packages/%=package/%) .)

packages:
	@echo "Setting up packages"
	@mkdir packages

netsim:
	@echo "Setting up netsim network"
	@mkdir netsim
	@ncs-netsim --dir netsim $(NETWORK)
	@ncs-netsim ncs-xml-init > ncs-cdb/netsim_devices_init.xml

netsim-start:
	@echo "Starting netsim"
	@ncs-netsim start

netsim-stop:
	@echo "Stopping netsim"
	@-ncs-netsim stop

netsim-clean:
	@-rm -rf README.netsim netsim ncs-cdb/netsim_devices_init.xml
