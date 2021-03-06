# KGDB

KGDB is kernel dark magic that allows you to GDB the kernel on real hardware without any extra hardware support.

It is useless with QEMU since we already have full system visibility with `-gdb`, but this is a good way to learn it.

Cheaper than JTAG (free) and easier to setup (all you need is serial), but with less visibility as it depends on the kernel working, so e.g.: dies on panic, does not see boot sequence.

Usage:

    ./run -k
    ./rungdb -k

In GDB:

    c

In QEMU:

    /count.sh &
    /kgdb.sh

In GDB:

    b sys_write
    c
    c
    c
    c

And now you can count from GDB!

If you do: `b sys_write` immediately after `./rungdb -k`, it fails with `KGDB: BP remove failed: <address>`. I think this is because it would break too early on the boot sequence, and KGDB is not yet ready.

See also:

- <https://github.com/torvalds/linux/blob/v4.9/Documentation/DocBook/kgdb.tmpl>
- <https://stackoverflow.com/questions/22004616/qemu-kernel-debugging-with-kgdb/44197715#44197715>

## KGDB kernel modules

In QEMU:

    /kgdb-mod.sh

In GDB:

    lx-symbols ../kernel_module-1.0/
    b fop_write
    c
    c
    c

and you now control the count.

TODO: if I `-ex lx-symbols` to the `gdb` command, just like done for QEMU `-gdb`, the kernel oops. How to automate this step?

## KDB

If you modify `runqemu` to use:

    -append kgdboc=kbd

instead of `kgdboc=ttyS0,115200`, you enter a different debugging mode called KDB.

Usage: in QEMU:

    [0]kdb> go

Boot finishes, then:

    /kgdb.sh

And you are back in KDB. Now you can:

    [0]kdb> help
    [0]kdb> bp sys_write
    [0]kdb> go

And you will break whenever `sys_write` is hit.

The other KDB commands allow you to instruction steps, view memory, registers and some higher level kernel runtime data.

But TODO I don't think you can see where you are in the kernel source code and line step as from GDB, since the kernel source is not available on guest (ah, if only debugging information supported full source).

