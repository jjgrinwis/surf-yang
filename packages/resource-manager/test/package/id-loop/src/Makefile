ncsvsn=$(shell ncs --version)
ncsvsn_clean=$(shell echo ${ncsvsn} | sed 's/_[0-9]*//')

JAVA_PACKAGE = com.tailf.pkg.loop
JDIR = $(shell echo $(JAVA_PACKAGE) | sed 's/\./\//g')

NS  = namespaces

JFLAGS = --java-disable-prefix \
         --exclude-enums \
         --fail-on-warnings \
         --java-package $(JAVA_PACKAGE).$(NS) \
         --emit-java java/src/$(JDIR)/namespaces

all:	create-package-metadata fxs javac

# Include standard NCS examples build definitions and rules
include $(NCS_DIR)/src/ncs/build/include.ncs.mk

SRC = $(wildcard yang/*.yang)
FXS = $(SRC:yang/%.yang=../load-dir/%.fxs)
YANGPATH = --yangpath ./yang --yangpath ../../../../src/yang

.PHONY: create-package-metadata
create-package-metadata:
ifeq ($(shell ../../../greater-or-equal $(ncsvsn_clean) 4.2), yes)
	cp ../package-meta-data-4.2.xml ../package-meta-data.xml
else
	cp ../package-meta-data-3.0.xml ../package-meta-data.xml
endif

fxs:	$(FXS)

../load-dir/%.fxs:	yang/%.yang
	$(NCSC)  `ls $*-ann.yang  > /dev/null 2>&1 && echo "-a $*-ann.yang"` \
             $(YANGPATH) -c -o $@ $<
	$(NCSC) $(JFLAGS)/$*.java $@

javac:
	cd java && ant -q all

clean:
	rm -rf  ../load-dir/*
	rm -f ../*jar/*.jar
	cd java && ant -q clean || true
	rm -f java/src/$(JDIR)/$(NS)/*.java
