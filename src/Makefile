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
CPU = cortex-m0plus
ARCH:= $(shell arch)
SPECS:= $(shell specs)

# Platform Specific Flags
PLATFORM:= HOST
LINKER_FILE = MKL25Z128xxx4_flash.ld



# Compile Defines
CC = gcc
LD = arm-none-eabi-ld
TARGET = demo
LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
CFLAGS =

OBJS = $(SRCS:.c=.o)
MAPS = $(SRCS:.c=.map)
OUTS = $(SRCS:.c=.out)
ASSEMBLYS = $(SRCS:.c=.asm)

%.o : %.c
	$(CC) -c $< $(CFLAGS) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: compile-all
compile-all:
	$(CC) $(INCLUDES) -c $(SRCS)

.PHONY: clean
clean:
	rm -f $(OBJS) $(MAPS) $(OUTS) $(ASSEMBLYS)
