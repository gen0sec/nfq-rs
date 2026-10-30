# nfq - NetFilter queue for Rust

> **Fork.** This is gen0sec's fork of [nbdd0121/nfq-rs](https://github.com/nbdd0121/nfq-rs) at
> upstream `master`. It adds no code; it only publishes `master` (which has the `OwnedFd`
> conversions) to the gen0sec registry as `0.2.6-gen0sec.N` until upstream releases it
> ([nbdd0121/nfq-rs#21](https://github.com/nbdd0121/nfq-rs/issues/21)).

`nfq` is Rust library for performing userspace handling of packets queued by the kernel packet
packet filter chains.

## License
In contrast to `libnetfilter_queue` which is licensed under GPL 2.0, which will require all
binaries using that library to be bound by GPL, `nfq` is dual-licensed under MIT/Apache-2.0.
`nfq` achieves this by communicates with kernel via NETLINK sockets directly.

## Example

Here is an example which accepts all packets.
```rust
use nfq::{Queue, Verdict};

fn main() -> std::io::Result<()> {
   let mut queue = Queue::open()?; 
   queue.bind(0)?;
   loop {
       let mut msg = queue.recv()?;
       msg.set_verdict(Verdict::Accept);
       queue.verdict(msg)?;
   }
   Ok(())
}
```
