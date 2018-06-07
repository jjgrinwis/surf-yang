ifeq ($(JOB_DIR),)
JOB_DIR = simulated
endif

all: test

clean:
	$(MAKE) -C $(JOB_DIR) clean || exit 1

test:
	$(MAKE) -C $(JOB_DIR) test || exit 1

