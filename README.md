# SwiftShare CLI

Official CLI for SwiftShare - fast and secure file sharing from your terminal.

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
mkdir -p ~/.local/bin
mv sws-linux-amd64 ~/.local/bin/sws
sws --version
```

If `~/.local/bin` is not in your `PATH`, add it before running `sws`.

On Windows, download `sws-windows-amd64.exe`, rename it to `sws.exe`, and add
it to a folder listed in your `PATH`.

Windows may show a Microsoft Defender SmartScreen warning because the binary is
not signed yet and may not have enough download reputation. This does not mean
the file is unsafe; it means Windows cannot verify the publisher. Binaries are
distributed only through the official GitHub Releases page for this repository.
If you downloaded `sws-windows-amd64.exe` from that release, choose
**More info** and then **Run anyway**.

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
| `-p, --password` | Prompt for a password before creating and uploading the transfer. |
| `--password-stdin` | Read the password from standard input. |
| `-e, --expiration <days>` | Set the expiration in days. If omitted, the server default is used. |
| `--chunk-concurrency <count>` | Number of chunks uploaded in parallel for the current file. Default: `4`. |

What to expect:

- Files and folders are supported.
- Folder structure is preserved.
- Current SwiftShare transfer limits are checked before the upload starts.
- Before upload starts, `sws` prints whether the transfer is password-protected, plus the selected expiration and chunk concurrency settings.
- If uploads are temporarily disabled, `sws` stops before creating a transfer.
- If the platform transfer limit has been reached, `sws` stops before uploading files and shows a highlighted message asking you to try again later.
- Uploads and incomplete-transfer cleanup keep working when switching networks.
- In an interactive terminal, press `p` during upload to pause or resume chunk uploads.
- Temporary upload chunk failures are retried automatically. If retries keep failing in an interactive terminal, `sws` asks whether to retry again or stop.
- Progress output includes per-file status, size, duration, and average speed.
- When the upload finishes, `sws` waits for the server to finalize the transfer, then prints the file count, total size, password-protection status, expiration date, and transfer URL.
- If the upload stops before completion, including after `Ctrl+C` or choosing to stop retrying, `sws` removes the incomplete transfer. `Ctrl+C` shows a cleanup loader while it runs.

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
```

Options:

| Option | Description |
| --- | --- |
| `-p, --password` | Prompt for the transfer password. |
| `--password-stdin` | Read the password from standard input. |
| `--archive` | Download the transfer as a ZIP archive. |
| `--archive-name <filename>` | Name of the ZIP file. `.zip` is added when needed. |
| `-d, --destination <path>` | Directory where files are written. Default: current directory. |
| `--chunk-concurrency <count>` | Number of file chunks downloaded in parallel. Default: `4`. |

What to expect:

- A transfer URL or a bare identifier can be used.
- If downloads are temporarily disabled, `sws` stops before fetching the transfer.
- If the transfer is still being prepared, `sws` shows `Preparing transfer...` with the API progress percentage when available, then downloads when ready.
- Before download starts, `sws` prints the destination, archive name when `--archive` is used, and chunk concurrency settings.
- Folder structure is preserved when downloading files directly.
- Archive downloads preserve the same folder structure inside the ZIP.
- For direct downloads, or with `--archive`, `--destination` may be an existing writable special file such as `/dev/null`. Omit `--archive-name` when writing an archive to a special file.
- Existing output paths are confirmed before replacement.
- Files are downloaded to temporary paths first, then moved into place when complete.
- In an interactive terminal, press `p` during download to pause or resume file, chunk, and archive writes.
- Temporary download chunk failures are retried automatically. If retries keep failing in an interactive terminal, `sws` asks whether to retry again or stop.
- Completed files are kept if a later file fails.
- Archive downloads show when each finished file is being added to the ZIP.
- Pressing `Ctrl+C` starts cleanup and shows a loader while temporary files are removed.

## Updates

`sws` checks for new releases automatically. To update manually:

```sh
sws self-update
```

If `sws` was installed in a protected system directory, run
`sudo sws self-update` or reinstall it in `~/.local/bin` to update without
`sudo`.

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
