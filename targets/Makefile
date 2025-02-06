SRC=$(wildcard *.cc)
LDFLAGS += -lutil -lpthread -ldl -lm -lpthread

all:  check-env build

check-env:
ifndef LIB_FUZZING_ENGINE
	$(error LIB_FUZZING_ENGINE is not set)
endif

ifndef RADARE2_STATIC_BUILD
	$(error RADARE2_STATIC_BUILD is not set)
endif

build: $(SRC)
	${CXX} ${CXXFLAGS} $^ -o $(basename $^) ${LDFLAGS} -I ${RADARE2_STATIC_BUILD}/usr/include/libr ${RADARE2_STATIC_BUILD}/usr/lib/libr.a ${LIB_FUZZING_ENGINE} ; \

clean: $(SRC)
	rm -f $(basename $^)

