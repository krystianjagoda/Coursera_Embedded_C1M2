#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material.
#
#*****************************************************************************

#------------------------------------------------------------------------------
# Simple makefile for two target platforms and their own
# specific compilers. These two platforms are the HOST and
# the MSP432.
# The host embedded system will use the native compiler, gcc.
# The target embedded system will use the cross compiler,
# arm-none-eabi-gcc.
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.i - Generate <FILE>.i preprocessed output
#      <FILE>.asm - Generate <FILE>.asm assembly output
#      <FILE>.o - Builds <FILE>.o object file
#      compile-all - Compile all object files, but DO NOT link
#      build - Builds and links all source files
#      clean - Removes all generated files
#
# Platform Overrides:
#      LINKER_FILE -
#      CPU - ARM Cortex Architecture (cortex-m0plus, cortex-m4)
#      ARCH - ARM Architecture (arm, thumb)
#      SPECS - Specs file to give the linker (nosys.specs, nano.specs)
#
#------------------------------------------------------------------------------

# MOJE!!!!!!!!!!!!!!

include sources.mk

# Architecture Specific Flags
CPU =
ARCH =
SPECS =

# Platform Specific Flags
PLATFORM = HOST
LINKER_FILE = msp432p401r.lds


ifeq ($(PLATFORM),HOST)
	CFLAGS = -DHOST
	CC = gcc
	LDFLAGS = -Wl,-Map=$(TARGET).map
	# etc
endif

ifeq ($(PLATFORM),MSP432)
	CFLAGS = -MSP432
	CC = arm-none-eabi-gcc
	LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
		# etc
endif


# Compile Defines
LD = arm-none-eabi-ld
TARGET = c1m2


OBJS = $(SRCS:.c=.o)
MAPS = $(SRCS:.c=.map)
OUTS = $(SRCS:.c=.out)
ASSEMBLYS = $(SRCS:.c=.asm)
DEPENDENTCIES = $(SRCS:.c=.d)
CLEANS = $(TARGET).o $(TARGET).out $(TARGET).map

%.o : %.c
	$(CC) $(INCLUDES) -c $< $(CFLAGS) -o $@

%.d: %.c
	@set -e; rm -f $@; \
	$(CC) $(INCLUDES) -MM $(CFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$


.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out $(DEPENDENTCIES)

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) $(INCLUDES) -o $@

$(DEPENDENTCIES): $(SRCS)
	@set -e; rm -f $@; \
	$(CC) $(INCLUDES) -MM $(CFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$


.PHONY: compile-all
compile-all:
	$(CC) $(INCLUDES) -c $(SRCS) $(CFLAGS)


.PHONY: clean
clean:
	rm -f $(OBJS) $(MAPS) $(OUTS) $(ASSEMBLYS) $(DEPENDENTCIES) $(CLEANS)
