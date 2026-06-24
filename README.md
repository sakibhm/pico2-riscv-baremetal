# pico2-riscv-baremetal

Bare-metal firmware for RP2350 (Pico 2) running on Hazard3 RISC-V cores.
No SDK, no HAL, no abstractions — direct register access only.

## Target Hardware

__________________________________________________
| Property      | Value                          |
|---------------|--------------------------------|
| Board         | Raspberry Pi Pico 2            |
| Chip          | RP2350                         |
| RISC-V Core   | Hazard3 (RV32IMAC)             |
| Flash         | 2MB external (W25Q16) via QSPI |
| SRAM          | 520KB                          |
| Flash Address | `0x10000000`                   |
| SRAM Address  | `0x20000000`                   |
|---------------|--------------------------------|

## IMAGE_DEF

Minimum 20-byte block placed at the very start of flash (`0x10000000`).
BootROM scans for this on every boot to validate the image and select the RISC-V core.

____________________________________________________________
| Word | Value        |    Description                     |
|------|--------------|------------------------------------|
| 0    | `0xffffded3` | Block start magic number           |
| 1    | `0x11010142` | Image type: EXE, RISC-V, RP2350    |
| 2    | `0x000001ff` | End marker for item list           |
| 3    | `0x00000000` | Points to self — single block loop |
| 4    | `0xab123579` | Block end magic number             |
|------|--------------|------------------------------------|

## Toolchain Setup

Install the RISC-V GCC toolchain on Ubuntu/WSL:

```bash
sudo apt update
sudo apt install gcc-riscv64-unknown-elf
```

Verify installation:

```bash
riscv64-unknown-elf-gcc --version
```
## Toolchain Utilities :
```bash
riscv64-unknown-elf-  +  2 X TAB
```
__________________________________________________________________________
| Utility                       | Purpose                                |
|-------------------------------|----------------------------------------|
| `riscv64-unknown-elf-gcc`     | Compile C and assembly files           |
| `riscv64-unknown-elf-objcopy` | Convert ELF to raw `.bin` for flashing |
| `riscv64-unknown-elf-objdump` | Disassemble and inspect binary         |
| `riscv64-unknown-elf-size`    | Show flash and RAM usage per section   |
| `riscv64-unknown-elf-readelf` | Inspect ELF sections and symbols       |
|-------------------------------|----------------------------------------|

## Compile Flags

```bash
-march=rv32imac -mabi=ilp32 -nostdlib
```

`-march=rv32imac` — RV32 with Integer, Multiply, Atomic, Compressed extensions  
`-mabi=ilp32` — 32-bit ABI, no floating point  
`-nostdlib` — no standard library, pure bare metal

