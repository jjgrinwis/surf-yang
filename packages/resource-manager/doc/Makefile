BOOK=resource_manager

PRODUCT = NSO

# Uncertain if this variable should be set explicitly,
# or defined by the tailf-doc environment.
TAILF_DOC_COMMON = --product "$(PRODUCT)"

ifneq ($(MAKECMDGOALS),clean)
  ifeq ($(shell ! which tailf-doc && echo yes),yes)
    $(error "You need to have tailf-doc in PATH, see doc/README")
  endif
endif

ifeq ($(shell ! which ncs && echo yes),yes)
  $(error "You need to have env.sh sourced from NSO installation.")
endif

all: global.ent pyang_check
	$(MAKE) $(BOOK).pdf

pdf: global.ent $(BOOK).pdf

html: output/doc/html/$(BOOK)/index.html

.PHONY: clean pdf depend generate all html pyang_check

clean::
	rm -f *.fo lint.log *.tree *.tmp*
	rm -f depend.mk schemas.xml global.ent
	rm -f *.h.xml *.xi.xml *.db
	rm -rf html
	rm -rf gen
	rm -rf output

target.%.db: generate
	tailf-doc $(TAILF_DOC_COMMON) $* --generate-target-db $(BOOK).xml

%.pdf: %.fo
	tailf-doc $(TAILF_DOC_COMMON) --output $@ pdf $<

%.fo: %.xml
	tailf-doc $(TAILF_DOC_COMMON) --output $@ fo $<

lint:
	tailf-doc $(TAILF_DOC_COMMON) lint resource_manager.xml

output/doc/html/$(BOOK)/index.html: $(BOOK).xml
	@mkdir -p $(dir $@)
	tailf-doc $(TAILF_DOC_COMMON) \
		--output output/doc/html/$(BOOK) html $< \
		--target-db "`pwd`/../olink.html.db" $<

.PHONY: clean pdf depend generate all html

pyang_check: tailf-resource-manager.tree tailf-resource-manager-idallocator.tree tailf-resource-manager-ipaddressallocator.tree

tailf-resource-manager.tree: ../src/yang/resource-allocator.yang
	pyang -f tree -p ../src/yang \
          ../src/yang/resource-allocator.yang > tailf-resource-manager.tree || true

tailf-resource-manager-idallocator.tree: ../src/yang/id-allocator.yang
	pyang -f tree -p ../src/yang \
          ../src/yang/id-allocator.yang > tailf-resource-manager-idallocator.tree || true

tailf-resource-manager-ipaddressallocator.tree: ../src/yang/ipaddress-allocator.yang
	pyang -f tree -p ../src/yang \
          ../src/yang/ipaddress-allocator.yang > tailf-resource-manager-ipaddressallocator.tree || true


generated/vnfr2esc.png: vnfr2esc.txt
	java -jar plantuml.8041.jar $< -o generated

global.ent: NSO_VSN = $(shell ncs --version)
global.ent: RM_VSN = $(shell grep -A 10 "<package-version" ../package-meta-data.xml | \
                             grep -B 10 "<\/package-version" | \
                             grep "[0-9]" | \
                             sed 's/[-<\/package version>]//g')
global.ent: global.ent.in
	@echo "NSO_VSN = $(NSO_VSN)"
	@echo "RM_VSN = $(RM_VSN)"
	@sed -e "s|@NSO_VERSION@|$(NSO_VSN)|g" < $< > $@.tmp1
	@sed -e "s|@RM_VERSION@|$(RM_VSN)|g" < $@.tmp1 > $@

depend:
	@rm -f depend.mk
	@$(MAKE) depend.mk

depend.mk:
	@echo "creating $@"
	@tailf-doc depend -t generate $(BOOK).xml > $@
	@tailf-doc depend -s fo $(BOOK).xml >> $@
	@: # tailf-doc depend -s xi.xml $(BOOK).xml >> $@
	@tailf-doc depend -t output/doc/html/$(BOOK)/index.html \
    $(BOOK).xml >> $@

ifneq ($(filter-out clean depend,$(MAKECMDGOALS)),)
include depend.mk
endif
