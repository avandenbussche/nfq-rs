nfq - NetFilter queue for Rust

`nfq` is Rust library for performing userspace handling of packets queued by the kernel packet
packet filter chains.

# License
In contrast to `libnetfilter_queue` which is licensed under GPL 2.0, which will require all
binaries using that library to be bound by GPL, `nfq` is dual-licensed under MIT/Apache-2.0.
To achieve this, `nfq` does not use `libnetfilter_queue`. Instead, `nfq` communicates using
libmnl directly, which is licensed under LGPL.

# Example

Here is an example which accepts all packets.
```rust
use libc;
use nfq::{Queue, Verdict};

fn main() -> std::io::Result<()> {
   let mut queue = Queue::open()?; 
   queue.bind(libc::AF_INET, 0)?;
   loop {
       let msg = queue.recv()?;
       queue.verdict(msg, Verdict::Accept)?;
   }
   Ok(())
}
```