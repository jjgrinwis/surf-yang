DIRS = internal
DIR = internal

all: test

test: package-build
	$(MAKE) -C $(DIR) test || exit 1

clean: package-clean
	$(MAKE) -C $(DIR) clean || exit 1

desc:
	@echo "==Test Cases=="
	@for d in $(DIRS); do \
		$(MAKE) -sC $$d desc || exit 1; \
	done

package-build:
	(cd package; make all)

package-clean:
	(cd package; make all)
