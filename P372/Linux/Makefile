UNAME_S := $(shell uname -s)

# Set CC based on the operating system
ifeq ($(UNAME_S),Darwin)
    CC = clang
else
    CC = gcc
endif

source_dir = ../Src/P372/
bin_dir = ../Bin/
# Use the following for testing
#CFLAGS = -fPIC -Wall -Wextra -O0 -g -I$(source_dir)
# Use the following for production
OPTIMIZE ?= -O2
CFLAGS = -fPIC -Wall -Wextra $(OPTIMIZE) -I$(source_dir)

# Set linker flags based on the operating system
UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Darwin)
    LDFLAGS = -shared -lm -ldl
else
    LDFLAGS = -shared -lm -ldl -z muldefs
endif

RM = rm -f
TARGET_LIB = libp372.so

prefix ?= /usr/local
exec_prefix = $(prefix)
libdir = $(exec_prefix)/lib

SRCS = $(source_dir)InitializeNoise.c \
       $(source_dir)Noise.c \
       $(source_dir)NoiseMemory.c

OBJS = $(SRCS:.c=.o)

.PHONY: all
all: ${TARGET_LIB}

$(TARGET_LIB): $(OBJS)
	$(CC) -o $@ $^ $(LDFLAGS)

$(SRCS:.c=.d):%.d:%.c
	$(CC) $(CFLAGS) -MM $< >$@
-include $(SRCS:.c=.d)

.PHONY: clean

clean:
	${RM} ${OBJS} $(SRCS:.c=.d)
	${RM} ${TARGET_LIB}

install: all
	install -d $(DESTDIR)$(libdir)/
	install -m 755 $(TARGET_LIB) $(DESTDIR)$(libdir)/$(TARGET_LIB)
