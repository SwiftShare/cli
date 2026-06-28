# SwiftShare CLI

Official CLI for SwiftShare — fast and secure file sharing from your terminal.

[![asciicast](https://asciinema.org/a/SHlOgtddLW4bRShl.svg)](https://asciinema.org/a/SHlOgtddLW4bRShl)

## Installation

Download the latest binary for your platform:

| Platform | Download |
| --- | --- |
| Linux x64 | [sws-linux-amd64](https://github.com/SwiftShare/cli/releases/latest/download/sws-linux-amd64) |
| macOS Intel | [sws-darwin-amd64](https://github.com/SwiftShare/cli/releases/latest/download/sws-darwin-amd64) |
| macOS Apple Silicon | [sws-darwin-arm64](https://github.com/SwiftShare/cli/releases/latest/download/sws-darwin-arm64) |
| Windows x64 | [sws-windows-amd64.exe](https://github.com/SwiftShare/cli/releases/latest/download/sws-windows-amd64.exe) |

On Linux or macOS, make the downloaded binary executable and install it as
`sws`. For example, on Linux x64:

```sh
chmod +x sws-linux-amd64
sudo mv sws-linux-amd64 /usr/local/bin/sws
sws --version
```

On Windows, download `sws-windows-amd64.exe`, rename it to `sws.exe`, and add
it to a folder listed in your `PATH`.

## Quick Start

Upload a file:

```sh
sws up ./video.mp4
```

Upload several files or folders:

```sh
sws up ./video.mp4 ./photos ./notes.txt
```

Download from a transfer URL or identifier:

```sh
sws dl https://swiftshare.io/t/abc123
sws dl abc123
```

## Upload

```sh
sws upload <file-or-folder-path>... [flags]
```

Alias: `sws up`

Common examples:

```sh
sws up ./archive.zip
sws up ./project ./README.md
sws up ./private.zip --password
sws up ./report.pdf --expiration 7
sws up ./large-file.bin --chunk-concurrency 8
```

Options:

| Option | Description |
| --- | --- |
| `-p, --password` | Prompt for a password before creating the transfer. |
| `--password-stdin` | Read the password from standard input. |
| `-e, --expiration <days>` | Set the expiration in days. If omitted, the server default is used. |
| `--chunk-concurrency <count>` | Number of chunks uploaded in parallel for the current file. Default: `4`. |

What to expect:

- Files and folders are supported.
- Folder structure is preserved.
- A transfer can contain up to `1000` files.
- Temporary upload failures are retried automatically.
- Progress output includes per-file status, size, duration, and average speed.
- When the upload finishes, the transfer URL is printed.
- Pressing `Ctrl+C` starts cleanup and shows a loader while the incomplete transfer is removed.

## Download

```sh
sws download <identifier-or-url> [flags]
```

Alias: `sws dl`

Common examples:

```sh
sws dl abc123
sws dl https://swiftshare.io/t/abc123
sws dl abc123 --destination ./downloads
sws dl abc123 --archive
sws dl abc123 --archive --archive-name photos.zip
sws dl abc123 --password
sws dl abc123 --chunk-concurrency 8
```

Options:

| Option | Description |
| --- | --- |
| `-p, --password` | Prompt for the transfer password. |
| `--password-stdin` | Read the password from standard input. |
| `--archive` | Download the transfer as a ZIP archive. |
| `--archive-name <filename>` | Name of the ZIP file. `.zip` is added when needed. |
| `-d, --destination <dir>` | Directory where files are written. Default: current directory. |
| `--chunk-concurrency <count>` | Number of chunks downloaded in parallel for the current file. Default: `4`. |

What to expect:

- A transfer URL or a bare identifier can be used.
- If the transfer is still being prepared, `sws` shows `Preparing transfer...` with the API progress percentage when available, then downloads when ready.
- Folder structure is preserved when downloading files directly.
- Archive downloads preserve the same folder structure inside the ZIP.
- Existing output paths are confirmed before replacement.
- Files are downloaded to temporary paths first, then moved into place when complete.
- Completed files are kept if a later file fails.
- Archive downloads use chunk concurrency too; each file is downloaded first, then added to the ZIP.
- Pressing `Ctrl+C` starts cleanup and shows a loader while temporary files are removed.

## Updates

`sws` checks for new releases automatically. To update manually:

```sh
sws self-update
```

To skip the automatic update check for one command:

```sh
sws --skip-update-check up ./file.bin
```

## Shell Completion

Generate completion scripts with:

```sh
sws completion bash
sws completion zsh
sws completion fish
sws completion powershell
```

Use `sws <command> --help` for the latest command-specific help.
