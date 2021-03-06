PROJ = kuview.bin
CXX = /usr/bin/g++
BUILD = debug

WXCFG = ../../wxGTK-2.8.10/build_u$(BUILD)/wx-config
FIINC = ../../FreeImage/Dist
FILIB = ../../FreeImage/Dist

ifeq ($(BUILD), release)
CXXFLAGS = -I$(FIINC) -DFREEIMAGE_LIB $(shell $(WXCFG) --cxxflags) -O2
else
CXXFLAGS = -I$(FIINC) -DFREEIMAGE_LIB $(shell $(WXCFG) --cxxflags) -g
endif

LDFLAGS = -L$(FILIB) -lfreeimage $(shell $(WXCFG) --libs)

SRCDIR = ../src
SRCS = $(shell cd $(SRCDIR) && ls *.cpp)
OBJS = $(SRCS:%.cpp=%.o)

all: info $(OBJS) $(PROJ)

info:
	@echo "$(PROJ) BUILD = $(BUILD)"
ifneq ($(wildcard build.*), build.$(BUILD))
	@rm -f build.*
	@rm -f *.o
	@touch build.$(BUILD)
endif

$(PROJ): $(OBJS)
	@echo "Linking $@..."
	@$(CXX) $^ -o $@ $(LDFLAGS)
ifeq ($(BUILD), release)
	@echo "Strip all symbols in $@..."
	@strip -s $(PROJ)
	@echo "Compress $@..."
	@upx $@
endif

%.o: $(SRCDIR)/%.cpp $(SRCDIR)/ku.h
	@echo "Compiling $<..."
	@$(CXX) -c $(CXXFLAGS) -o $@ $<

clean:
	rm -f *.o

