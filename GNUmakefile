.SUFFIXES:

.DELETE_ON_ERROR:

override OUTPUT := StratOS

TOOLCHAIN :=
TOOLCHAIN_PREFIX :=
ifneq ($(TOOLCHAIN),)
	ifeq ($(TOOLCHAIN_PREFIX),)
		TOOLCHAIN_PREFIX := $(TOOLCHAIN)-
	endif
endif

ifneq ($(TOOLCHAIN_PREFIX),)
	CC := $(TOOLCHAIN_PREFIX)gcc
else
	CC := cc
endif

LD := $(TOOLCHAIN_PREFIX)ld

ifeq ($(TOOLCHAIN),llvm)
	CC := clang
	LD := ld.lld
endif

CFLAGS := -g -O2 -pipe

CPPFLAGS :=

NASMFLAGS := -g

LDFLAGS :=
override CC_IS_CLANG := $(shell ! $(CC) --version 2>/dev/null | grep -q '^Target: '; echo $$?)

ifeq ($(CC_IS_CLANG), 1)
	override CC += \
		-target x86_64-unknown-none-elf
endif

override CFLAGS += \
	-Wall \
	-Wextra \
	-std=gnu11 \
	-ffreestanding \
	-fno-stack-protector \
	-fno-stack-check \
	-fno-lto \
	-fno-PIC \
	-ffunction-sections \
	-fdata-sections \
	-m64 \
	-march=x86-64 \
	-mabi=sysv \
	-mno-80387 \
	-mno-mmx \
	-mno-sse \
	-mno-sse2 \
	-mno-red-zone \
	-mcmodel=kernel

override CPPFLAGS := \
	-I src \
	$(CPPFLAGS) \
	-MMD \
	-MP

override NASMFLAGS := \
	-f elf64 \
	$(patsubst -g,-g -F dwarf,$(NASMFLAGS)) \
	-i src/ \
	-Wall

override LDFLAGS += \
	-m elf_x86_64 \
	-nostdlib \
	-static \
	-z max-page-size=0x1000 \
	-z noexecstack \
	--gc-sections \
	-T linker.lds

override LD_HAS_NO_PIE := $(shell ! $(LD) --help 2>/dev/null | grep -qE '(^|[[:space:]])--?no-pie([[:space:]]|$$)'; echo $$?)

ifeq ($(LD_HAS_NO_PIE),1)
	override LDFLAGS += \
		-no-pie
endif

override SRCFILES := $(shell find -L src -type f 2>/dev/null | LC_ALL=C sort)
override CFILES := $(filter %.c,$(SRCFILES))
override ASFILES := $(filter %.S,$(SRCFILES))
override NASMFILES := $(filter %.asm,$(SRCFILES))
override OBJ := $(addprefix obj/,$(CFILES:.c=.c.o) $(ASFILES:.S=.S.o) $(NASMFILES:.asm=.asm.o))
override HEADER_DEPS := $(addprefix obj/,$(CFILES:.c=.c.d) $(ASFILES:.S=.S.d) $(NASMFILES:.asm=.asm.d))

.PHONY: all
all: bin/$(OUTPUT)

-include $(HEADER_DEPS)

bin/$(OUTPUT): GNUmakefile linker.lds $(OBJ)
	mkdir -p "$(dir $@)"
	$(LD) $(LDFLAGS) $(OBJ) -o $@

obj/%.c.o: %.c GNUmakefile
	mkdir -p "$(dir $@)"
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

obj/%.S.o: %.S GNUmakefile
	mkdir -p "$(dir $@)"
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

obj/%.asm.o: %.asm GNUmakefile
	mkdir -p "$(dir $@)"
	nasm $(NASMFLAGS) -MD $(@:.o=.d) -MP $< -o $@

ISO_NAME := StratOS.iso
ISO_DIR := iso

iso: $(ISO_NAME)

$(ISO_NAME): $(OUTPUT)
	curl -fL -o limine-binary.tar.gz https://github.com/Limine-Bootloader/Limine/releases/latest/download/limine-binary.tar.gz
# 	tar -xzf -limine-binary.tar.gz
	gunzip < limine-binary.tar.gz | tar -xf -

	make -C limine-binary

	mkdir -p $(ISO_DIR)/boot
	cp -v bin/$(OUTPUT) $(ISO_DIR)/boot/
	mkdir -p $(ISO_DIR)/boot/limine
	cp -v src/boot/limine.conf limine-binary/limine-bios.sys limine-binary/limine-bios-cd.bin \
		limine-binary/limine-uefi-cd.bin $(ISO_DIR)/boot/limine/

	mkdir -p $(ISO_DIR)/EFI/BOOT
	cp -v limine-binary/BOOTX64.EFI $(ISO_DIR)/EFI/BOOT/
	cp -v limine-binary/BOOTIA32.EFI $(ISO_DIR)/EFI/BOOT/

	xorriso -as mkisofs -R -r -J -b boot/limine/limine-bios-cd.bin \
		-no-emul-boot -boot-load-size 4 -boot-info-table -hfsplus \
		-apm-block-size 2048 --efi-boot boot/limine/limine-uefi-cd.bin \
		-efi-boot-part --efi-boot-image --protective-msdos-label \
		$(ISO_DIR) -o $(OUTPUT).iso

.PHONY: iso clean
clean:
	rm -rf bin obj