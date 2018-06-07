
JAVA_PACKAGE = com.tailf.pkg.resourcemanager
JDIR = $(shell echo $(JAVA_PACKAGE) | sed 's/\./\//g')

NS  = namespaces

JFLAGS = --java-disable-prefix \
         --exclude-enums \
         --fail-on-warnings \
         --java-package $(JAVA_PACKAGE).$(NS) \
         --emit-java java/src/$(JDIR)/namespaces

ifeq ($(USE_DEPS),)
DEPS=../..
all:	fxs xmlload javac doc
else
DEPS=../deps
all:	deps fxs xmlload javac doc
endif

ifeq ($(BUILD_DOCS),)
doc:
	@echo "Doc generation skipped, set BUILD_DOCS=true to create"
else
doc:
	$(MAKE) -C ../doc
endif

# Include standard NCS examples build definitions and rules
include $(NCS_DIR)/src/ncs/build/include.ncs.mk

SRC = $(wildcard yang/*.yang)
FXS = $(SRC:yang/%.yang=../load-dir/%.fxs)
XMLINIT = $(wildcard ../initial_data/*.xml)
XMLLOAD = $(XMLINIT:../initial_data/%.xml=../load-dir/%.xml)
YANGPATH = --yangpath ./yang

deps:
	cd ../ && pkg-get-deps --build

fxs:	$(FXS)

xmlload: $(XMLLOAD)

../load-dir/id-allocator-alarms.fxs: 	yang/id-allocator-alarms.yang
	@echo "Skipping submodules"

../load-dir/ipaddress-allocator-alarms.fxs: 	yang/ipaddress-allocator-alarms.yang
	@echo "Skipping submodules"


../load-dir/resource-allocator.fxs:	yang/resource-allocator.yang
	$(NCSC)  `ls resource-allocator-ann.yang  > /dev/null 2>&1 && echo "-a resource-allocator-ann.yang"` \
             $(YANGPATH) -c -o $@ $<
	$(NCSC) $(JFLAGS)/resource-allocator.java $@

../load-dir/%.fxs:	APP_NAME = $(shell echo $* | sed 's/\-oper//g' | sed 's/\-//g')
../load-dir/%.fxs:	JAVA_PKG = com.tailf.pkg.$(APP_NAME)
../load-dir/%.fxs:	JAVA_DIR = $(shell echo $(JAVA_PKG) | sed 's/\./\//g')
../load-dir/%.fxs:	JAVA_FLAGS = --java-disable-prefix --exclude-enums --fail-on-warnings --java-package $(JAVA_PKG).$(NS) --emit-java java/src/$(JAVA_DIR)/namespaces
../load-dir/%.fxs:	yang/%.yang
	$(NCSC)  `ls $*-ann.yang  > /dev/null 2>&1 && echo "-a $*-ann.yang"` \
             $(YANGPATH) -c -o $@ $<
	$(NCSC) $(JAVA_FLAGS)/$*.java $@

%.xml: %.xml.in
ifeq "$(NCS_VERSION)" "3.3"
	sed -e 's|<local-user>admin</local-user>||g' $< > $@
else
	cp $< $@
endif

../load-dir/%.xml: ../initial_data/%.xml
	cp $< $@

javac:
	cd java && DEPS=../$(DEPS) ant -q all

clean:
	$(MAKE) -C ../doc clean
	rm -rf  ../load-dir/*
	rm -f ../*jar/*.jar
	cd java && ant -q clean || true
	rm -f java/src/$(JDIR)/$(NS)/*.java
