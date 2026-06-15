
RISCV_PFX=riscv64-unknown-elf-
RISCV_CC=$(RISCV_PFX)gcc
RISCV_OBJDUMP=$(RISCV_PFX)objdump

BINARY_OPT=-static -specs=htif_nano.specs
OBJECT_DUMP_OPT=--disassemble-all

TARGET=mempress-rocc
# burst omitted: HW path in reqgen is still a stub. Build manually with -DPAT_BURST.
PATTERNS=stride rand

TARGET_RISCV=$(foreach p,$(PATTERNS),$(TARGET)-$(p).riscv)
TARGET_OBJDUMP=$(foreach p,$(PATTERNS),$(TARGET)-$(p).riscv.dump)
JUNK += $(TARGET_RISCV) $(TARGET_OBJDUMP)

all: $(TARGET_RISCV) $(TARGET_OBJDUMP)

# Map each binary's pattern suffix to its -D flag (see mempress-rocc.c).
$(TARGET)-stride.riscv: PAT_FLAG=-DPAT_STRIDE
$(TARGET)-burst.riscv: PAT_FLAG=-DPAT_BURST
$(TARGET)-rand.riscv: PAT_FLAG=-DPAT_RAND

$(TARGET)-%.riscv: mempress-rocc.c
	$(RISCV_CC) $(BINARY_OPT) $(PAT_FLAG) -o $@ $^

$(TARGET)-%.riscv.dump: $(TARGET)-%.riscv
	$(RISCV_OBJDUMP) $(OBJECT_DUMP_OPT) $< > $@

.PHONY: clean
clean:
	rm -f $(JUNK)
