# RAOP-Player

RAOP player and library (AirPlay)

This is an RAOP (AirPlay) player and library for the v2 protocol -- with synchronization -- now known as AirPlay 1. It works for Windows, OSX, Linux x86 and ARM.
The player can play raw PCM from any file or from `stdin` (useful for piping from a decoder like `lame`, `flac` or `ffmpeg`).

The player is intended as an example of how to use the library, but it has a few interesting options:

```text
usage: ./build/raop_play <options> <server_ip> <filename ('-' for stdin)>
	[-ntp <file>] write current NTP in <file> and exit
	[-p <port number>]
	[-v <volume> (0-100)]
	[-l <latency> (frames]
	[-w <wait>]  (start after <wait> milliseconds)
	[-n <start>] (start at NTP <start> + <wait>)
	[-nf <start>] (start at NTP in <file> + <wait>)
	[-e] (encrypt)
	[-s <secret>] (valid secret for AppleTV)
	[-d <debug level>] (0 = silent)
	[-i] (interactive commands: 'p'=pause, 'r'=(re)start, 's'=stop, 'q'=exit, ' '=block)
```

It is possible to send synchronous audio to multiple players by using the NTP options (optionally combined with the wait option).
Either get the NTP of the master machine from any application and then fork multiple instances of `raop_play` with that NTP and the same audio file, or use the `-ntp` option to get NTP to be written to a file and re-use that file when calling the instances of `raop_play`.

## Building using CMake

```sh
# Fetch all dependencies
git submodule update --init

# Create build directory
mkdir build
cd build

# Build project
cmake ..
make
```

## Building using Make

Makefiles are provided for OSX, Linux (x86 and ARM). Under Windows, I use Embarcadero C++, so I don't use makefile. You need some libraries:

- ALAC codec: https://github.com/macosforge/alac and

- Curve25519 crypto: https://github.com/msotoodeh/curve25519

You need pthread for Windows to recompile the player / use the library here: https://www.sourceware.org/pthreads-win32

It's largely inspired from https://github.com/chevil/raop2_play but limiting the playback to PCM as it focuses on creating a library and optimizing AirPlay synchronization.

Since iOS 10.2, pairing is required with AppleTV. A description of the protocol is available [here](https://htmlpreview.github.io/?https://github.com/philippe44/RAOP-Player/blob/master/doc/auth_protocol.html).
