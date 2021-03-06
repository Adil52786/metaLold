
CPU     = i386
TARGET  = $(CPU)-elf
CFG_CPU = --with-cpu=$(CPU)
CROSS   = $(CURDIR)/$(TARGET)/bin

QEMU	= qemu-system-i386
GDB		= $(CROSS)/$(TARGET)-gdb
CFLAGS += -g2 -O0

DDD		= "$(GDB) -x $<.gdb $< ; killall $(QEMU)"

go: game.elf
	# exit from QEMU: [Ctrl-A] [X]
	$(QEMU) -kernel $< -nographic -s -S &
	ddd --debugger $(DDD)

cross: ../mcu/Makefile
	make -f $< TARGET=$(TARGET) CFG_CPU="$(CFG_CPU)" cross0
clean: ../mcu/Makefile
	make -f $< TARGET=$(TARGET) clean

C = multiboot.S
H = multiboot.h

GCC = $(CROSS)/$(TARGET)-gcc
CFLAGS += -march=$(CPU) -mtune=$(CPU) -m32 -I$(CURDIR)

LD = $(CROSS)/$(TARGET)-ld
LDFLAGS = -m elf_i386 -T multiboot.ld

OBJDUMP = LANG=C $(CROSS)/$(TARGET)-objdump

game.elf: multiboot.o
	$(LD) $(LDFLAGS) -o $@ $^
	$(OBJDUMP) -x -d $@ > $@.objdump

multiboot.o: multiboot.S multiboot.h Makefile
	$(GCC) $(CFLAGS) -c -o $@ $<
	$(OBJDUMP) -x -d $@ > $@.objdump
	
