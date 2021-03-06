#===============================================================================
# User Options
#===============================================================================

COMPILER    = gnu
OPTIMIZE    = yes
OPENMP      = yes
DEBUG       = yes
PROFILE     = no
TEST        = no

#===============================================================================
# Program name & source code list
#===============================================================================

program = tallybench

source=$(wildcard *.c)

obj = $(source:.c=.o)

#===============================================================================
# Sets Flags
#===============================================================================

# Standard Flags
CFLAGS := -std=gnu99 -Wall

# Linker Flags
LDFLAGS = -lm

# Regular gcc Compiler
ifeq ($(COMPILER),gnu)
  CC = gcc
  CFLAGS += -flto
  ifeq ($(OPENMP),yes)
    CFLAGS += -fopenmp -DOPENMP
  endif
endif

# Intel Compiler
ifeq ($(COMPILER),intel)
  CC = icc
  CFLAGS += -ipo
  ifeq ($(OPENMP),yes)
    CFLAGS += -qopenmp -DOPENMP
  endif
endif


# Debug Flags
ifeq ($(DEBUG),yes)
  CFLAGS += -g
  LDFLAGS  += -g
endif

# Test Flags
ifeq ($(TEST),yes)
  CFLAGS += -DTEST
endif

# Profiling Flags
ifeq ($(PROFILE),yes)
  CFLAGS += -pg
  LDFLAGS  += -pg
endif

# Optimization Flags
ifeq ($(OPTIMIZE),yes)
  CFLAGS += -O3
endif

#===============================================================================
# Targets to Build
#===============================================================================

$(program): $(obj) tallybench_header.h Makefile
	$(CC) $(CFLAGS) $(obj) -o $@ $(LDFLAGS)

%.o: %.c Makefile tallybench_header.h
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -rf $(program) $(obj) tallybench.dSYM

edit:
	vim -p $(source) tallybench_header.h

run:
	./$(program)
