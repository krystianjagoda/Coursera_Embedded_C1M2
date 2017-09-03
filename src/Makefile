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


# Platform Specific Flags
PLATFORM = HOST
LINKER_FILE = ../msp432p401r.lds

include sources.mk

GENERALFLAGS = -Wall -Werror -g -O0 -std=c99

# Compile Defines
TARGET = c1m2

ifeq ($(PLATFORM),HOST)
	CFLAGS = -DHOST
	CC = gcc $(GENERALFLAGS)
	LDFLAGS = -Wl,-Map=$(TARGET).map
	CPPFLAGS =
	# etc
endif

ifeq ($(PLATFORM),MSP432)
	CFLAGS = -DMSP432 -mcpu=cortex-m4 -mthumb -march=armv7e-m \
		-mfloat-abi=hard -mfpu=fpv4-sp-d16 --specs=nosys.specs
	CC = arm-none-eabi-gcc $(GENERALFLAGS)
	LDFLAGS = -T $(LINKER_FILE) -Wl,-Map=$(TARGET).map
	CPPFLAGS =
		# etc
endif


OBJS = $(SOURCES:.c=.o)
MAPS = $(SOURCES:.c=.map)
OUTS = $(SOURCES:.c=.out)
ASSEMBLYS = $(SOURCES:.c=.s)
DEPENDENTCIES = $(SOURCES:.c=.d)
CLEANS = $(TARGET).o $(TARGET).out $(TARGET).map

%.o : %.c
	$(CC) $(INCLUDES) -c $< $(CFLAGS) -o $@

%.i : %.c
	$(CC) -E $< $(INCLUDES) $(CFLAGS)

%.d: %.c
	@set -e; rm -f $@; \
	$(CC) $(INCLUDES) -MM $(CFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

%.asm : %.c
	$(CC) -S $< $(INCLUDES) $(CFLAGS)


.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out $(DEPENDENTCIES)

$(TARGET).out: $(OBJS)
	$(CC) $(INCLUDES) $(OBJS) $(CFLAGS)  $(LDFLAGS) -o $@

$(DEPENDENTCIES): $(SOURCES)
	@set -e; rm -f $@; \
	$(CC) $(INCLUDES) -MM $(CFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$


.PHONY: compile-all
compile-all:
	$(CC) $(INCLUDES) -c $(SOURCES) $(CFLAGS)


.PHONY: clean
clean:
	rm -f $(OBJS) $(MAPS) $(OUTS) $(ASSEMBLYS) $(DEPENDENTCIES) $(CLEANS)
