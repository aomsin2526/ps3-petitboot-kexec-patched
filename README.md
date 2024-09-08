Original kexec for ps3 can't handle elf that have loadable segment more than one (such as seperate code and data segment) Causing "Overlapping memory segments" or "sort_segments failed" error.

    readelf -l kernel-bad

    Elf file type is DYN (Shared object file)
    Entry point 0x148a220
    There are 4 program headers, starting at offset 64

    Program Headers:
      Type           Offset             VirtAddr           PhysAddr
                     FileSiz            MemSiz              Flags  Align
      LOAD           0x0000000000010000 0x0000000000100000 0x0000000000100000
                     0x0000000000fefb1d 0x0000000000fefb1d  R E    0x10000
      LOAD           0x0000000001000000 0x00000000010f0000 0x00000000010f0000
                     0x0000000000451748 0x000000000082a4c0  RW     0x10000
      DYNAMIC        0x0000000001451648 0x0000000001541648 0x0000000001541648
                     0x0000000000000100 0x0000000000000100  RW     0x8
      GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                     0x0000000000000000 0x0000000000000000  RW     0x8

    readelf -l kernel-good

    Elf file type is DYN (Shared object file)
    Entry point 0x147c498
    There are 3 program headers, starting at offset 64

    Program Headers:
      Type           Offset             VirtAddr           PhysAddr
                     FileSiz            MemSiz              Flags  Align
      LOAD           0x0000000000010000 0x0000000000100000 0x0000000000100000
                     0x00000000014335e0 0x000000000180edc0  RWE    0x10000
      DYNAMIC        0x00000000014434e0 0x00000000015334e0 0x00000000015334e0
                     0x0000000000000100 0x0000000000000100  RW     0x8
      GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                     0x0000000000000000 0x0000000000000000  RW     0x8

By simply merge those segment into one at load time it is now possible to load such elf.

One of iso that affected is FreeBSD-12.2-RELEASE and FreeBSD-12.4-RELEASE.
