PWD := $(shell pwd)

SRCS := log.capnp car.capnp arne182.capnp

GENS := gen/cpp/car.capnp.c++ gen/cpp/log.capnp.c++ gen/cpp/arne182.capnp.c++
JS := gen/js/car.capnp.js gen/js/log.capnp.js gen/js/arne182.capnp.js

UNAME_M ?= $(shell uname -m)

GENS += gen/c/car.capnp.c gen/c/log.capnp.c gen/c/include/c++.capnp.h gen/c/include/java.capnp.h gen/c/arne182.capnp.c

ifeq ($(UNAME_M),x86_64)

ifneq (, $(shell which capnpc-java))
GENS += gen/java/Car.java gen/java/Log.java gen/java/Arne182.java
else
$(warning capnpc-java not found, skipping java build)
endif

endif


ifeq ($(UNAME_M),aarch64)
CAPNPC=PATH=$(PWD)/../phonelibs/capnp-cpp/aarch64/bin/:$$PATH capnpc
else
CAPNPC=capnpc
endif

.PHONY: all
all: $(GENS)
js: $(JS)

.PHONY: clean
clean:
	rm -rf gen
	rm -rf node_modules
	rm -rf package-lock.json

gen/c/%.capnp.c: %.capnp
	@echo "[ CAPNPC C ] $@"
	mkdir -p gen/c/
	$(CAPNPC) '$<' -o c:gen/c/

gen/js/%.capnp.js: %.capnp
	@echo "[ CAPNPC JavaScript ] $@"
	mkdir -p gen/js/
	sh ./generate_javascript.sh

gen/cpp/%.capnp.c++: %.capnp
	@echo "[ CAPNPC C++ ] $@"
	mkdir -p gen/cpp/
	$(CAPNPC) '$<' -o c++:gen/cpp/

gen/java/Car.java gen/java/Log.java: $(SRCS)
	@echo "[ CAPNPC java ] $@"
	mkdir -p gen/java/
	$(CAPNPC) $^ -o java:gen/java

# c-capnproto needs some empty headers
gen/c/include/c++.capnp.h gen/c/include/java.capnp.h:
	mkdir -p gen/c/include
	touch '$@'
