
if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

EXTRA_DIST	= example-cfg.json nomacro.pl

SUBDIRS		= compat

bin_PROGRAMS	= minerd

minerd_SOURCES	= elist.h miner.h compat.h \
		  cpu-miner.c util.c \
		  sha2.c \
		  randomx-miner.c \
		  pthread_barrier.c \
		  RandomX/src/aes_hash.cpp \
		  RandomX/src/allocator.cpp \
		  RandomX/src/argon2_avx2.c \
		  RandomX/src/argon2_core.c \
		  RandomX/src/argon2_ref.c \
		  RandomX/src/argon2_ssse3.c \
		  RandomX/src/blake2/blake2b.c \
		  RandomX/src/blake2_generator.cpp \
		  RandomX/src/bytecode_machine.cpp \
		  RandomX/src/cpu.cpp \
		  RandomX/src/dataset.cpp \
		  RandomX/src/instruction.cpp \
		  RandomX/src/instructions_portable.cpp \
		  RandomX/src/randomx.cpp \
		  RandomX/src/reciprocal.c \
		  RandomX/src/soft_aes.cpp \
		  RandomX/src/superscalar.cpp \
		  RandomX/src/virtual_machine.cpp \
		  RandomX/src/virtual_memory.c \
		  RandomX/src/vm_compiled.cpp \
		  RandomX/src/vm_compiled_light.cpp \
		  RandomX/src/vm_interpreted.cpp \
		  RandomX/src/vm_interpreted_light.cpp
		  
# Compiler flags from:
# https://github.com/tevador/RandomX/blob/master/CMakeLists.txt
# https://github.com/xmrig/xmrig/blob/master/cmake/flags.cmake
RANDOMX_FLAGS = -Ofast

# Enabled for gcc, explicitly enable for clang
RANDOMX_FLAGS += -funroll-loops -fmerge-all-constants

if ARCH_x86_64
minerd_SOURCES += RandomX/src/assembly_generator_x86.cpp RandomX/src/jit_compiler_x86.cpp RandomX/src/jit_compiler_x86_static.S
RANDOMX_FLAGS += -maes -mavx2 -mssse3
endif

if ARCH_AARCH64
minerd_SOURCES += RandomX/src/jit_compiler_a64.cpp RandomX/src/jit_compiler_a64_static.S
minerd_SOURCES += sha2-arm.S
RANDOMX_FLAGS += -march=armv8-a+crypto -flax-vector-conversions
if HAVE_APPLE
RANDOMX_FLAGS += -D__APPLE__
else
RANDOMX_FLAGS += -D__aarch64__
endif
endif

if USE_ASM
if ARCH_x86
minerd_SOURCES += sha2-x86.S
endif
if ARCH_x86_64
minerd_SOURCES += sha2-x64.S
endif
if ARCH_ARM
minerd_SOURCES += sha2-arm.S
endif
if ARCH_PPC
minerd_SOURCES += sha2-ppc.S
endif
endif

minerd_LDFLAGS	= $(PTHREAD_FLAGS)
minerd_LDADD	= @LIBCURL_LIBS@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@
minerd_CFLAGS	= -fno-strict-aliasing $(RANDOMX_FLAGS)
minerd_CPPFLAGS	= @LIBCURL_CFLAGS@ $(JANSSON_INCLUDES) $(PTHREAD_FLAGS) $(RANDOMX_FLAGS) -s
