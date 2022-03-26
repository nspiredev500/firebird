CC := emcc
CXX := em++
PREJS ?= firebird-utils.js
FLAGS := -O3 -Wall -I.. -s USE_ZLIB=1 -s DISABLE_EXCEPTION_CATCHING=1
CFLAGS := -std=c11 $(FLAGS)
CXXFLAGS := -std=c++11 $(FLAGS)
LFLAGS := --emrun -s TOTAL_MEMORY=536870912 -s INVOKE_RUN=0 -s NO_EXIT_RUNTIME=1 -s ASSERTIONS=0 --closure 0 -s EXPORTED_RUNTIME_METHODS='["callMain", "cwrap"]' --pre-js $(PREJS)
OUTPUT := firebird

CSOURCES :=    ../core/armsnippets_loader.c ../core/asmcode.c ../core/casplus.c ../core/des.c ../core/disasm.c \
	      ../core/gdbstub.c ../core/interrupt.c ../core/keypad.c ../core/lcd.c ../core/link.c ../core/mem.c \
	      ../core/misc.c ../core/mmu.c ../core/schedule.c ../core/serial.c ../core/sha256.c ../core/usb.c \
              ../core/usblink.c ../core/os/os-emscripten.c

CPPSOURCES := ../core/arm_interpreter.cpp ../core/coproc.cpp ../core/cpu.cpp ../core/debug.cpp ../core/emu.cpp \
	      ../core/flash.cpp ../core/gif.cpp ../core/thumb_interpreter.cpp ../core/usblink_queue.cpp main.cpp \
	      ../core/fieldparser.cpp ../core/usb_cx2.cpp ../core/usblink_cx2.cpp  ../core/cx2.cpp

OBJS = $(patsubst %.c, %.o, $(CSOURCES))
OBJS += $(patsubst %.cpp, %.o, $(CPPSOURCES))

all: $(OUTPUT).js

%.o: %.c Makefile
	$(CC) -c $(CFLAGS) $< -o $@

%.o: %.cpp Makefile
	$(CXX) -c $(CXXFLAGS) $< -o $@

test: $(OUTPUT).js
	emrun --safe_firefox_profile --browser firefox $(OUTPUT).html

$(OUTPUT).js: $(OBJS)
	$(CC) $(CFLAGS) $(LFLAGS) $^ -o $@

clean:
	rm -f $(OBJS) $(OUTPUT).js $(OUTPUT).data $(OUTPUT).js.mem
