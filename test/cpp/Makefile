# A sample Makefile for building Google Test and using it in user
# tests.  Please tweak it to suit your environment and project.  You
# may want to move it to your project's root directory.
#
# SYNOPSIS:
#
#   make [all]  - makes everything.
#   make TARGET - makes the given target.
#   make clean  - removes all files generated by make.

# Please tweak the following variable definitions as needed by your
# project, except GTEST_HEADERS, which you can use in your own targets
# but shouldn't modify.

# Points to the root of Google Test, relative to where this file is.
# Remember to tweak this if you move this file.
GTEST_DIR = ../../../../../ThirdParty/gtest-1.3.0

# Where to find user code.
USER_DIR = ../../src/cpp

# Flags passed to the preprocessor.
CPPFLAGS += -I$(GTEST_DIR) -I$(GTEST_DIR)/include \
			-I../../../../../ThirdParty/boost/boost_1_47_0 \
			-I../../../../../assembly/cpp/common \
            -I../../../../lib/cpp/alignment \
            -I../../../../../ThirdParty/log4cpp/log4cpp-1.0/include \
			-I$(USER_DIR)

BLASR_ARCH := ../../../../lib/cpp/alignment/libblasr.a
LOG4CPP_ARCH := ../../../../../ThirdParty/log4cpp/log4cpp-1.0/src/.libs/liblog4cpp.a

# Flags passed to the C++ compiler.
CXXFLAGS += -g -std=c++0x -fpermissive

# All tests produced by this Makefile.  Remember to add new tests you
# created to the list.
TESTS = test_alngraph test_alignment test_simple_aligner

# All Google Test headers.  Usually you shouldn't change this
# definition.
GTEST_HEADERS = $(GTEST_DIR)/include/gtest/*.h \
                $(GTEST_DIR)/include/gtest/internal/*.h

# House-keeping build targets.

all : $(TESTS)

clean :
	rm -f $(TESTS) gtest.a gtest_main.a *.o

# Builds gtest.a and gtest_main.a.

# Usually you shouldn't tweak such internal variables, indicated by a
# trailing _.
GTEST_SRCS_ = $(GTEST_DIR)/src/*.cc $(GTEST_DIR)/src/*.h $(GTEST_HEADERS)

# For simplicity and to avoid depending on Google Test's
# implementation details, the dependencies specified below are
# conservative and not optimized.  This is fine as Google Test
# compiles fast and for ordinary users its source rarely changes.
gtest-all.o : $(GTEST_SRCS_)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $(GTEST_DIR)/src/gtest-all.cc

gtest_main.o : $(GTEST_SRCS_)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $(GTEST_DIR)/src/gtest_main.cc

gtest.a : gtest-all.o
	$(AR) $(ARFLAGS) $@ $^

gtest_main.a : gtest-all.o gtest_main.o
	$(AR) $(ARFLAGS) $@ $^

# Builds a sample test.  A test should link with either gtest.a or
# gtest_main.a, depending on whether it defines its own main()
# function.

AlnGraphBoost.o : $(USER_DIR)/AlnGraphBoost.cpp $(USER_DIR)/AlnGraphBoost.hpp $(GTEST_HEADERS)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $(USER_DIR)/AlnGraphBoost.cpp

AlnGraphBoostTest.o : AlnGraphBoostTest.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c AlnGraphBoostTest.cpp

Alignment.o : $(USER_DIR)/Alignment.cpp $(USER_DIR)/Alignment.hpp $(GTEST_HEADERS)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $(USER_DIR)/Alignment.cpp

AlignmentTest.o : AlignmentTest.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c AlignmentTest.cpp

SimpleAligner.o: $(USER_DIR)/SimpleAligner.cpp $(GTEST_HEADERS)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c $(USER_DIR)/SimpleAligner.cpp

SimpleAlignerTest.o: SimpleAlignerTest.cpp
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c SimpleAlignerTest.cpp

test_alngraph: AlnGraphBoost.o AlnGraphBoostTest.o Alignment.o $(LOG4CPP_ARCH) gtest_main.a
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) $^ -o $@ -lpthread

test_alignment: Alignment.o AlignmentTest.o gtest_main.a
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) $^ -o $@

test_simple_aligner: Alignment.o SimpleAlignerTest.o SimpleAligner.o $(BLASR_ARCH) gtest_main.a
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) $^ -o $@