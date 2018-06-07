PACKAGES=id-loop ip-loop ipaddress-allocator-test pool-service

all: cisco-ios packages

.PHONY:packages packages-clean
packages:
	(for i in $(PACKAGES); do \
	        $(MAKE) -C $${i}/src all || exit 1; \
	done)

packages-clean:
	(for i in $(PACKAGES); do \
	        $(MAKE) -C $${i}/src clean || exit 1; \
	done)

clean: packages-clean
	@rm -rf cisco-ios

cisco-ios:
	git clone ssh://git@stash.tail-f.com/PKG/minimal-cisco-ios.git cisco-ios;\
	cd $@; git checkout f4241ec61cd91e5e2ba775f449925b3b87390bd6; \
	cd src; make all
