# modreveal

`modreveal` is a small utility that prints the names of hidden LKMs (Linux Kernel Modules) if any exist. It's useful for detecting rootkits that hide themselves from standard tools like `lsmod`.

![Demo](https://github.com/h2337/file-hosting/blob/59be44dca2845a68c210e61ec2733d20ddfad63f/modreveal.gif?raw=true)

## Requirements

- Linux kernel 5.2 or newer (updated for modern kernel API)
- Kernel headers matching your running kernel
- GCC compiler
- libnl-3 and libnl-genl-3 development libraries

### Installing Dependencies

#### Arch Linux
```bash
sudo pacman -S linux-headers gcc libnl
```

#### Ubuntu/Debian
```bash
sudo apt-get install linux-headers-$(uname -r) gcc libnl-3-dev libnl-genl-3-dev
```

#### Fedora/RHEL
```bash
sudo dnf install kernel-devel gcc libnl3-devel
```

## Usage

```bash
make
sudo ./modreveal
```

## How It Works

1. Loads a kernel module that uses kprobes to access `kallsyms_lookup_name`
2. Iterates through all kernel modules using the internal `module_kset` structure
3. Communicates the complete module list to userspace via generic netlink
4. Compares the kernel's internal module list with the output of `lsmod`
5. Reports any modules that exist in the kernel but are hidden from `lsmod`

## Testing

To test the utility, you can use a rootkit that hides itself, such as:
- Diamorphine rootkit (https://github.com/m0nad/Diamorphine)

## Compatibility

- Updated for Linux kernel 5.2+ (uses modern generic netlink API)
- Tested on kernel 6.x series
- Should work on any modern Linux distribution with proper dependencies installed
