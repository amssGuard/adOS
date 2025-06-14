GPPARAMS = -m32 -fno-use-cxa-atexit -nostdlib -fno-builtin -fno-rtti -fno-exceptions
ASPARAMS = --32
LDPARAMS = -melf_i386

objects = loader.o kernel.o

%.o: %.cpp
	g++ $(GPPARAMS) -o $@ -c $<

%.o: %.s
	as $(ASPARAMS) -o $@ $<

mykernel.bin: linker.ld $(objects)
	ld $(LDPARAMS) -T $< -o $@ $(objects)

iso:
	mkdir -p iso/boot/grub
	cp mykernel.bin iso/boot/
	cp grub/grub.cfg iso/boot/grub/

myos.iso: mykernel.bin iso
	grub-mkrescue -o myos.iso iso

run: myos.iso
	qemu-system-i386 -cdrom myos.iso

clean:
	rm -f *.o mykernel.bin myos.iso
	rm -rf iso
