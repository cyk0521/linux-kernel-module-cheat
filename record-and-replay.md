# Record and replay

QEMU supports deterministic record and replay by saving external inputs, which would be awesome to understand the kernel, as you would be able to examine a single run as many times as you would like.

Unfortunately it is not working in the current QEMU: <https://stackoverflow.com/questions/46970215/how-to-use-qemus-deterministic-record-and-replay-feature-for-a-linux-kernel-boo>

Alternatively, [`mozilla/rr`](https://github.com/mozilla/rr) claims it is able to run QEMU: but using it would require you to step through QEMU code itself. Likely doable, but do you really want to?
