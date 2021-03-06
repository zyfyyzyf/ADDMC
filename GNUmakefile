OPTIMIZATION = -Ofast# on for MCC
STATIC = -static# on for MCC

################################################################################

LIB_DIR = lib
LIB_TAR = $(LIB_DIR).tar

CUDD_DIR = $(LIB_DIR)/cudd-3.0.0
CUDD_INCLUSIONS = -I$(CUDD_DIR) -I$(CUDD_DIR)/cudd -I$(CUDD_DIR)/epd -I$(CUDD_DIR)/mtr -I$(CUDD_DIR)/st
CUDD_LINK = -L$(CUDD_DIR)/cudd/.libs -lcudd

HPP_DIR = src/interface
CPP_DIR = src/implementation

OBJ_DIR = obj
_OBJ = util.o graph.o formula.o join.o visual.o counter.o
_OBJ_ADDMC = $(_OBJ) main_addmc.o
OBJ_ADDMC = $(patsubst %, $(OBJ_DIR)/%, $(_OBJ_ADDMC))

ADDMC_BIN = addmc

################################################################################

GCC = g++ -g# debugging information
GCC_FLAGS = $(OPTIMIZATION) $(STATIC) -I$(HPP_DIR) -I$(LIB_DIR) $(CUDD_INCLUSIONS) -std=c++14

################################################################################

$(ADDMC_BIN): $(OBJ_ADDMC)
	$(GCC) -o $(ADDMC_BIN) $^ $(GCC_FLAGS) $(CUDD_LINK)

$(OBJ_DIR)/%.o: $(CPP_DIR)/%.cpp $(HPP_DIR)/%.hpp $(LIB_DIR)
	mkdir -p $(OBJ_DIR) && $(GCC) -c -o $@ $< $(GCC_FLAGS) && echo

$(LIB_DIR): $(LIB_TAR)
	tar -xf $(LIB_TAR) && touch $(LIB_DIR) && cd $(CUDD_DIR) && ./configure --enable-silent-rules --enable-obj && make && echo

addmc.tgz:
	tar -czf addmc.tgz examples src CMakeLists.txt INSTALL.md INSTALL.sh lib.tar LICENSE.md README.md

################################################################################

.PHONY: clean

clean:
	rm -rf addmc.tgz build CMakeCache.txt CMakeFiles Makefile cmake_install.cmake $(LIB_DIR) $(OBJ_DIR) $(ADDMC_BIN)
