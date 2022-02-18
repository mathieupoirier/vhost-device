# vhost-device

## Design

This repository hosts various 'vhost-user' device backends in their own crates.
See their individual README.md files for specific information about those
crates.

Here is the list of device backends that we support:

- [I2C](https://github.com/rust-vmm/vhost-device/blob/main/i2c/README.md)
- [RNG](https://github.com/rust-vmm/vhost-device/blob/main/rng/README.md)

## Testing and Code Coverage

Like the wider rust-vmm project we expect new features to come with
comprehensive code coverage. However as a multi-binary repository
there are cases where avoiding a drop in coverage can be hard and an
exception to the approach is allowable. These are:

* adding a new binary target (aim at least 60% overall coverage)
* expanding the main function (a small drop is acceptable)

However any new feature added to an existing binary should not cause a
drop in coverage. The general aim should be to always improve
coverage.

## Separation of Concerns

The binaries built by this repository can be run with any VMM which
can act as a vhost-user master. Typically they have been tested with
[QEMU](https://www.qemu.org) although the rust-vmm project does
provide a [vhost-user
master](https://github.com/rust-vmm/vhost/tree/main/src/vhost_user)
crate for rust based VMMs.

While it's possible to implement all parts of the backend inside the
vhost-device workspace consideration should be given to separating the
VirtQueue handling and response logic to a crate in [vm-virtio
devices](https://github.com/rust-vmm/vm-virtio/tree/main/crates/devices).
This way a monolithic rust-vmm VMM implementation can reuse the core
logic to service the virtio requests directly in the application.

