# Rust out-of-tree module

This is a basic template for an out-of-tree Linux kernel module written in Rust.

Please note that:

  - The Rust support is experimental.

  - The kernel that the module is built against needs to be Rust-enabled (`CONFIG_RUST=y`).

  - The kernel tree (`KDIR`) requires the Rust metadata to be available. These are generated during the kernel build, but may not be available for installed/distributed kernels (the scripts that install/distribute kernel headers etc. for the different package systems and Linux distributions are not updated to take into account Rust support yet).

  - All Rust symbols are `EXPORT_SYMBOL_GPL`.

Example:

```sh
$ make
make -C /lib/modules/`uname -r`/build M=$PWD
make[1]: Entering directory '/home/arch/rust-linux'
  RUSTC [M] /home/arch/rust-netfilter-module/rust_out_of_tree.o
  MODPOST /home/arch/rust-netfilter-module/Module.symvers
  CC [M]  /home/arch/rust-netfilter-module/rust_out_of_tree.mod.o
  LD [M]  /home/arch/rust-netfilter-module/rust_out_of_tree.ko
make[1]: Leaving directory '/home/arch/rust-linux'
```

```sh
$ sudo insmod rust_out_of_tree.ko

$ sudo dmesg -w
```

```txt
[ 3004.972269] rust_netfilter: packet headlen=52, len=52, first bytes=[45, 48, 00, 34, 00, 00, 40, 00, 2d, 06]
[ 3004.973194] rust_netfilter: packet headlen=52, len=460, first bytes=[45, 48, 01, cc, 1a, 4a, 40, 00, 40, 06]
[ 3004.974895] rust_netfilter: packet headlen=52, len=52, first bytes=[45, 48, 00, 34, 00, 00, 40, 00, 2d, 06]
[ 3004.980094] rust_netfilter: packet headlen=52, len=348, first bytes=[45, 48, 01, 5c, 1a, 4b, 40, 00, 40, 06]
```

For details about the Rust support, see https://github.com/Rust-for-Linux/linux.

For details about out-of-tree modules, see https://www.kernel.org/doc/html/latest/kbuild/modules.html.
