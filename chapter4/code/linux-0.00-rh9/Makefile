# Makefile for the simple example kernel.
AS86	=as86 -0 -a
LD86	=ld86 -0
AS	=as
LD	=ld
LDFLAGS =-m elf_i386 -Ttext 0 -e startup_32 -M  

all:	Image

Image: boot system
	dd bs=32 if=boot of=Image skip=1
	objcopy -O binary system head
	cat head >> Image

disk: Image
	bximage -q -fd=1.44M -func=create -sectsize=512 -imgmode=flat floppy.img
	dd bs=512 if=Image of=floppy.img seek=0 conv=notrunc
	sync;sync;sync

head.o: head.s
	$(AS) --32 -o head.o head.s

system:	head.o 
	$(LD) $(LDFLAGS) head.o  -o system > System.map

boot:	boot.s
	$(AS86) -o boot.o boot.s
	$(LD86) -s -o boot boot.o
bochs:
	bochs -q -f bochsrc

clean:
	rm -f Image System.map core boot head *.o system floppy.img
