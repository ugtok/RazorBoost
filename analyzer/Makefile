#------------------------------------------------------------------------------
# Description: Makefile to build executable rzrBTanalyzer
# Created:     Tue Jul  2 16:42:38 2013 by mkanalyzer.py
#
#               available switches:
#
#                 debugflag  (e.g., debugflag=-ggdb [default])
#                 cppflags
#                 cxxflags
#                 optflag
#                 verbose    (e.g., verbose=1)
#                 withcern   (e.g., withcern=1  expects to find CERN_LIB)
# Author:      Sezen Sekmen
#------------------------------------------------------------------------------
ifndef ROOTSYS
$(error *** Please set up Root)
endif
withroot:=1
#------------------------------------------------------------------------------
ifndef program
# default program name
program := rzrBTanalyzer
endif

cppsrcs	:= $(wildcard *.cpp)
ccsrcs  := $(wildcard *.cc)

# filter out all main programs except the one to be built
# 1. search files for main(...) and write list of files to .main
$(shell grep -H "main[(].*[)]" $(cppsrcs) $(ccsrcs)|cut -f1 -d: > .main)
# 2. send list back to Makefile
main	:= $(shell cat .main)
# 3. remove subset of files (including the file to be built) from main
main	:= $(filter-out $(program).cc $(program).cpp treestream.cc,$(main))
# 4. remove the set main from the set of all files in the directory
cppsrcs	:= $(filter-out $(main),$(cppsrcs))
ccsrcs	:= $(filter-out $(main),$(ccsrcs))

cppobjs	:= $(patsubst %.cpp,tmp/%.o,$(cppsrcs))
ccobjs	:= $(patsubst %.cc,tmp/%.o,$(ccsrcs))
objects	:= $(ccobjs) $(cppobjs)
say     := $(shell echo "Program: $(program)" >& 2)
#------------------------------------------------------------------------------
ifdef GCC_DIR
GCC_BIN_PREFIX	:= $(GCC_DIR)/bin/
else
GCC_BIN_PREFIX	:=
endif
C++	    := $(GCC_BIN_PREFIX)g++
LDSHARED:= $(GCC_BIN_PREFIX)g++
C++VER	:= $(shell $(C++) --version)
COMP	:= $(word 1, $(C++VER))
CTYPE	:= $(word 2, $(C++VER))
CVER	:= $(word 3, $(C++VER))
say 	:= $(shell echo "$(COMP) $(CTYPE) $(CVER)" >& 2)
#------------------------------------------------------------------------------
ifdef verbose
	AT =
else
	AT = @
endif
#------------------------------------------------------------------------------
# Products to compile/link against
#------------------------------------------------------------------------------
ifdef withcern
	ifndef CERN_LIB
		ifdef CERN_DIR
			CERN_LIB:= $(CERN_DIR)/lib
		else
			say:=$(error CERN_LIB must point to CERN lib directory)
		endif
	endif
	cernlib	:= -L$(CERN_LIB) -lpacklib -lmathlib -lkernlib
endif

ifdef withroot
	rootcpp	:= $(shell root-config --cflags)
	rootlib	:= $(shell root-config --glibs) -lTreePlayer
endif
#------------------------------------------------------------------------------
# Switches/includes
# debug flag is on by default
#------------------------------------------------------------------------------
debugflag:=-ggdb

ifndef optflag
	optflag:=-O2
endif

CPPFLAGS:= -I. $(rootcpp) $(cppflags)
CXXFLAGS:= -c -pipe $(optflag) -fPIC -Wall $(cxxflags) $(debugflag)
LDFLAGS	:= $(ldflags) $(debugflag)
LIBS	:= $(libs) $(rootlib) $(cernlib)
#------------------------------------------------------------------------------
# Rules
#------------------------------------------------------------------------------
bin:	$(program)

$(program)	: $(objects)
	@echo "---> Linking $@"
	$(AT)$(LDSHARED) $(LDFLAGS) $(objects) $(LIBS) -o $@
	@echo ""

$(cppobjs)	: tmp/%.o : %.cpp
	@echo "---> Compiling $<" 
	$(AT)$(CXX)	$(CXXFLAGS) $(CPPFLAGS) $< -o $@

$(ccobjs)	: tmp/%.o : %.cc
	@echo "---> Compiling $<" 

	$(AT)$(CXX)	$(CXXFLAGS) $(CPPFLAGS) $< -o $@

# Define clean up rule
clean   	:
	rm -rf tmp/*.o $(program)
#------------------------------------------------------------------------------
# Description: Makefile to build executable rzrBTanalyzer
# Created:     Tue Jul  2 16:42:38 2013 by mkanalyzer.py
#
#               available switches:
#
#                 debugflag  (e.g., debugflag=-ggdb [default])
#                 cppflags
#                 cxxflags
#                 optflag
#                 verbose    (e.g., verbose=1)
#                 withcern   (e.g., withcern=1  expects to find CERN_LIB)
# Author:      Sezen Sekmen
#------------------------------------------------------------------------------
ifndef ROOTSYS
$(error *** Please set up Root)
endif
withroot:=1

# ADDED:  28-May-2014, HBP
withlhapdf:=0
#------------------------------------------------------------------------------
ifndef program
# default program name
program := rzrBTanalyzer
endif

cppsrcs	:= $(wildcard *.cpp)
ccsrcs  := $(wildcard *.cc)

# filter out all main programs except the one to be built
# 1. search files for main(...) and write list of files to .main
$(shell grep -H "main[(].*[)]" $(cppsrcs) $(ccsrcs)|cut -f1 -d: > .main)
# 2. send list back to Makefile
main	:= $(shell cat .main)
# 3. remove subset of files (including the file to be built) from main
main	:= $(filter-out $(program).cc $(program).cpp treestream.cc,$(main))
# 4. remove the set main from the set of all files in the directory
cppsrcs	:= $(filter-out $(main),$(cppsrcs))
ccsrcs	:= $(filter-out $(main),$(ccsrcs))

cppobjs	:= $(patsubst %.cpp,tmp/%.o,$(cppsrcs))
ccobjs	:= $(patsubst %.cc,tmp/%.o,$(ccsrcs))
objects	:= $(ccobjs) $(cppobjs)
say     := $(shell echo "Program: $(program)" >& 2)
#------------------------------------------------------------------------------
ifdef GCC_DIR
GCC_BIN_PREFIX	:= $(GCC_DIR)/bin/
else
GCC_BIN_PREFIX	:=
endif
C++	    := $(GCC_BIN_PREFIX)g++
LDSHARED:= $(GCC_BIN_PREFIX)g++
C++VER	:= $(shell $(C++) --version)
COMP	:= $(word 1, $(C++VER))
CTYPE	:= $(word 2, $(C++VER))
CVER	:= $(word 3, $(C++VER))
say 	:= $(shell echo "$(COMP) $(CTYPE) $(CVER)" >& 2)
#------------------------------------------------------------------------------
ifdef verbose
	AT =
else
	AT = @
endif
#------------------------------------------------------------------------------
# Products to compile/link against
#------------------------------------------------------------------------------
# ADDED:  28-May-2014, HBP
ifdef withlhapdf
	lhapdfcpp:= $(shell lhapdf-config --cflags) 
	lhapdflib:= $(shell lhapdf-config --libs)
endif

ifdef withcern
	ifndef CERN_LIB
		ifdef CERN_DIR
			CERN_LIB:= $(CERN_DIR)/lib
		else
			say:=$(error CERN_LIB must point to CERN lib directory)
		endif
	endif
	cernlib	:= -L$(CERN_LIB) -lpacklib -lmathlib -lkernlib
endif

ifdef withroot
	rootcpp	:= $(shell root-config --cflags)
	rootlib	:= $(shell root-config --glibs) -lTreePlayer
endif
#------------------------------------------------------------------------------
# Switches/includes
# debug flag is on by default
#------------------------------------------------------------------------------
debugflag:=-ggdb

ifndef optflag
	optflag:=-O2
endif

# ADDED:  28-May-2014, $(lhapdfcpp) and $(lhapdflib) HBP
CPPFLAGS:= -I. $(rootcpp) $(cppflags) $(lhapdfcpp)
CXXFLAGS:= -c -pipe $(optflag) -fPIC -Wall $(cxxflags) $(debugflag)
LDFLAGS	:= $(ldflags) $(debugflag)
LIBS	:= $(libs) $(rootlib) $(cernlib) $(lhapdflib)
#------------------------------------------------------------------------------
# Rules
#------------------------------------------------------------------------------
bin:	$(program)

$(program)	: $(objects)
	@echo "---> Linking $@"
	$(AT)$(LDSHARED) $(LDFLAGS) $(objects) $(LIBS) -o $@
	@echo ""

$(cppobjs)	: tmp/%.o : %.cpp
	@echo "---> Compiling $<" 
	$(AT)$(CXX)	$(CXXFLAGS) $(CPPFLAGS) $< -o $@

$(ccobjs)	: tmp/%.o : %.cc
	@echo "---> Compiling $<" 

	$(AT)$(CXX)	$(CXXFLAGS) $(CPPFLAGS) $< -o $@

# Define clean up rule
clean   	:
	rm -rf tmp/*.o $(program)
