###############################################################################
# Copyright (c) 2000-2014 Ericsson Telecom AB
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
###############################################################################
TOPDIR := ..
include $(TOPDIR)/Makefile.regression

.SUFFIXES: .ttcn .hh
.PHONY: all clean dep run

TTCN3_LIB := ttcn3$(RT2_SUFFIX)-parallel$(DYNAMIC_SUFFIX)

TTCN3_MODULES := TconfigOper.ttcn

GENERATED_SOURCES := $(TTCN3_MODULES:.ttcn=.cc)
GENERATED_HEADERS := $(GENERATED_SOURCES:.cc=.hh)
ifdef CODE_SPLIT
GENERATED_SOURCES := $(foreach file, $(GENERATED_SOURCES:.cc=), $(addprefix $(file), .cc _seq.cc _set.cc  _seqof.cc _setof.cc _union.cc))
endif
USER_SOURCES := configOper_port2.cc
USER_HEADERS := $(USER_SOURCES:.cc=.hh)
OBJECTS := $(GENERATED_SOURCES:.cc=.o) $(USER_SOURCES:.cc=.o)

TARGET := TconfigOper$(EXESUFFIX)

all: $(TARGET) ;

$(TARGET): $(GENERATED_SOURCES) $(USER_SOURCES)
	$(CXX) $(CPPFLAGS) $(CXXFLAGS) $(LDFLAGS) -o $@ $^ -L$(TTCN3_DIR)/lib -l$(TTCN3_LIB) -L$(OPENSSL_DIR)/lib -lcrypto $($(PLATFORM)_LIBS)

$(GENERATED_SOURCES) $(GENERATED_HEADERS) $(USER_SOURCES): $(TTCN3_MODULES)
	$(TTCN3_COMPILER) -t $<

clean distclean:
	-rm -f $(TARGET) $(OBJECTS) $(GENERATED_HEADERS) \
	$(GENERATED_SOURCES) $(USER_SOURCES) $(USER_HEADERS) *.log Makefile.bak

dep: $(GENERATED_SOURCES)
	makedepend $(CPPFLAGS) $(GENERATED_SOURCES)

run: $(TARGET)
	$(TTCN3_DIR)/bin/ttcn3_start -ip 127.0.0.1 $(TARGET)

