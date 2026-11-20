<p align="center"><img src="assets/logo.svg" alt="Kolo" width="96" height="96"></p>

<h1 align="center">Awesome Kolo</h1>

<p align="center">Libraries, examples and notes for building peer-to-peer software with Kolo.</p>

<p align="center"><a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0-blue" alt="license: MIT OR Apache-2.0"></a> <img src="https://img.shields.io/badge/status-beta-orange" alt="status: beta"> <img src="https://img.shields.io/badge/go-1.26%2B-00ADD8" alt="go 1.26+"></p>

Kolo is a peer-to-peer stack in Go. A person is an Ed25519 key, two peers talk over an encrypted QUIC channel, they find each other without a central server, and what they share is signed, encrypted and kept in sync. On top of that sit files, folders and live audio and video.

Every library is small, has its own repository, and does one thing. You can take one, or stack several.

> Kolo is beta. Every library is version 0.1.0 and the API can change before 1.0.0.

## Contents

- [Examples](#examples)
- [Libraries](#libraries)
- [How the libraries fit together](#how-the-libraries-fit-together)
- [Starters](#starters)
- [Community](#community)
- [Contributing](#contributing)
- [License](#license)

## Examples

Four desktop examples built only with the libraries below. They show what the stack does together; they are demonstrations, not products.

| Example | What it does | Built on |
|---|---|---|
| [Kolo Chat](https://github.com/ciranda-dev/kolo-chat-wails3-example) | Rooms with your own key per room, invites, optional pin that keeps messages while you are offline | `kolo-braid`, `kolo-invite`, `kolo-swarm`, `kolo-pin` |
| [Kolo Share](https://github.com/ciranda-dev/kolo-share-wails3-example) | Send a file or a folder to one person with a code that works once, large items (the default limit is 1 TiB), resume after a lost connection | `kolo-blobs`, `kolo-files`, `kolo-sync`, `kolo-swarm` |
| [Kolo Meet](https://github.com/ciranda-dev/kolo-meet-wails3-example) | Calls with audio and video, quality that moves between 360p, 720p and 1080p, reconnection after a lost connection | `kolo-moq`, `kolo-media`, `kolo-audio`, `kolo-video` |
| [Kolo Board](https://github.com/ciranda-dev/kolo-board-wails3-example) | A shared board of columns and cards that several people edit, offline included | `kolo-braid`, `kolo-invite`, `kolo-swarm` |

All four use Wails 3 for the window and React for the screens, and none of them needs an account. They find each other through public entry nodes (run by Ciranda, optional: you can run your own) and then connect directly, so your data does not pass through those nodes.

## Libraries

### Identity and encoding

| Library | What it is |
|---|---|
| [kolo-id](https://github.com/ciranda-dev/kolo-id) | Ed25519 node identity: create, store, sign, verify |
| [kolo-codec](https://github.com/ciranda-dev/kolo-codec) | Canonical binary encoding |

### Connections

| Library | What it is |
|---|---|
| [kolo-transport](https://github.com/ciranda-dev/kolo-transport) | One connection between two peers, with multiplexed streams over QUIC, and connection statistics |
| [kolo-secure](https://github.com/ciranda-dev/kolo-secure) | An encrypted channel bound to the peers' identities |

### Finding peers

| Library | What it is |
|---|---|
| [kolo-dht](https://github.com/ciranda-dev/kolo-dht) | Find peers by topic without a central server, and connect through NATs |
| [kolo-relay](https://github.com/ciranda-dev/kolo-relay) | An optional relay for peers that cannot connect directly, meant to be self-hosted |
| [kolo-swarm](https://github.com/ciranda-dev/kolo-swarm) | Join a topic and keep connections to the peers found on it |
| [kolo-node](https://github.com/ciranda-dev/kolo-node) | A bootstrap server for the discovery network |
| [kolo-invite](https://github.com/ciranda-dev/kolo-invite) | Pairing and joining: an invite code, a person who approves |

### Data

| Library | What it is |
|---|---|
| [kolo-log](https://github.com/ciranda-dev/kolo-log) | A signed append-only log with deletion |
| [kolo-store](https://github.com/ciranda-dev/kolo-store) | Persistent storage for logs and read-only replicas |
| [kolo-kv](https://github.com/ciranda-dev/kolo-kv) | An ordered key-value store with one writer, kept in a signed log |
| [kolo-sync](https://github.com/ciranda-dev/kolo-sync) | Replicate a signed log from one peer to another |
| [kolo-pin](https://github.com/ciranda-dev/kolo-pin) | A peer that stores encrypted data for others without being able to read it |
| [kolo-braid](https://github.com/ciranda-dev/kolo-braid) | The signed strands of several members merged into one deterministic order |

### Files

| Library | What it is |
|---|---|
| [kolo-blobs](https://github.com/ciranda-dev/kolo-blobs) | Encrypted, content-addressed file storage |
| [kolo-files](https://github.com/ciranda-dev/kolo-files) | Folders, names and versions: an encrypted tree of files with its history |

### Audio and video

| Library | What it is |
|---|---|
| [kolo-moq](https://github.com/ciranda-dev/kolo-moq) | MoQ Transport: publish, subscribe and relay over Kolo connections |
| [kolo-media](https://github.com/ciranda-dev/kolo-media) | Catalog, packaging, playout and audio and video sync for broadcasts |
| [kolo-audio](https://github.com/ciranda-dev/kolo-audio) | Audio capture, playback, PCM processing and Opus |
| [kolo-video](https://github.com/ciranda-dev/kolo-video) | Camera capture, raw frames, VP8 and VP9, and optional H.264 |

## How the libraries fit together

Each library depends only on the ones above it.

```text
identity     kolo-id   kolo-codec
connection   kolo-transport   kolo-secure
discovery    kolo-dht   kolo-relay   kolo-swarm   kolo-node   kolo-invite
data         kolo-log   kolo-store   kolo-kv   kolo-sync   kolo-pin   kolo-braid
files        kolo-blobs   kolo-files
media        kolo-moq   kolo-media   kolo-audio   kolo-video
```

You do not need all of it. A chat needs identity, a connection, discovery and a braid. A file transfer needs the data layer and the files layer. A call needs the connection and the media layer.

## Starters

Small projects to begin from, with a written guide for each step.

| Starter | What you build |
|---|---|
| [Chat starter](https://github.com/ciranda-dev/kolo-chat-wails3-starter) | A desktop chat, step by step |
| [Meet starter](https://github.com/ciranda-dev/kolo-meet-wails3-starter) | A video call, step by step |
| [Shared document](https://github.com/ciranda-dev/kolo-braid-starter) | Two terminals editing one document |
| [Share files](https://github.com/ciranda-dev/kolo-share-example) | Send a file or a folder from the command line |

## Community

Apps, tools and projects made by other people with Kolo. To add yours, see [CONTRIBUTING.md](CONTRIBUTING.md).

| Project | What it does | Built on |
|---|---|---|

## Contributing

Know a project built with Kolo, an article, a talk or a tool? Open a pull request that adds one line to this list. [CONTRIBUTING.md](CONTRIBUTING.md) has the format.

## License

Licensed under either of the [Apache License, Version 2.0](LICENSE) or the [MIT license](LICENSE-MIT), at your option.
