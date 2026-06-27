# SwiftShare CLI

Transfer files directly from your terminal.

SwiftShare CLI (`sws`) lets you upload files or folders with a single command
and get a transfer link you can send anywhere. It is built for quick command
line sharing from your laptop, a server, an SSH session, a script, or an
automated workflow.

## Installation

Download the binary for your platform from the latest release:

[Download SwiftShare CLI](https://github.com/SwiftShare/cli/releases/latest)

After downloading, make the binary executable if your system requires it, then
run `sws` from your terminal.

## Create Transfers

Upload one file:

```sh
sws upload ./document.pdf
```

Upload a folder:

```sh
sws upload ./photos
```

Upload multiple items at once:

```sh
sws upload ./document.pdf ./photos
```

When the upload completes, SwiftShare CLI prints a transfer link:

```text
Transfer completed. Transfer URL: https://swiftshare.io/t/...
```

You can open this link in a browser or use it with the `download` command.

## Protect A Transfer

Prompt for a password during upload:

```sh
sws upload ./document.pdf --password
```

Read the password from standard input, useful for scripts:

```sh
printf '%s\n' "$SWIFTSHARE_PASSWORD" | sws upload ./document.pdf --password-stdin
```

Set an expiration in days:

```sh
sws upload ./document.pdf --expiration 3
```

## Download A Transfer

Download with a transfer identifier:

```sh
sws download <identifier>
```

Download with a full SwiftShare URL:

```sh
sws download https://swiftshare.io/t/<identifier>
```

Choose the destination folder:

```sh
sws download <identifier> --destination ./downloads
```

Download the transfer as a ZIP archive:

```sh
sws download <identifier> --archive
```

Choose the archive name:

```sh
sws download <identifier> --archive --archive-name photos.zip
```

If the transfer is password-protected, SwiftShare CLI asks for the password when
needed.

## Commands

### Upload

```sh
sws upload <file-or-folder-path>... [flags]
```

Alias:

```sh
sws up <file-or-folder-path>... [flags]
```

Useful options:

| Option | Description |
| --- | --- |
| `-p, --password` | Prompt for a password to protect the transfer. |
| `--password-stdin` | Read the password from standard input. |
| `-e, --expiration <days>` | Set the transfer expiration in days. |
| `--chunk-concurrency <count>` | Number of chunks uploaded in parallel for the current file. Default: 4. |

Notes:

- Files and folders are supported.
- Folder structure is preserved.
- A transfer can contain up to 1000 files.
- Temporary upload issues are retried automatically.
- If the upload is interrupted, the transfer is removed.

### Download

```sh
sws download <identifier-or-url> [flags]
```

Alias:

```sh
sws dl <identifier-or-url> [flags]
```

Useful options:

| Option | Description |
| --- | --- |
| `-p, --password` | Prompt for the transfer password. |
| `--password-stdin` | Read the password from standard input. |
| `--archive` | Download the transfer as a ZIP archive. |
| `--archive-name <filename>` | Name of the ZIP file to create. `.zip` is added when needed. |
| `-d, --destination <dir>` | Folder where downloaded files are written. Default: current folder. |
| `--chunk-concurrency <count>` | Number of chunks downloaded in parallel for the current file. Default: 4. |

Notes:

- The argument can be a transfer identifier or a full SwiftShare URL.
- Before downloading, SwiftShare CLI shows the files included in the transfer.
- By default, files are written to the current folder.
- Folder structure is preserved.
- If a file already exists, SwiftShare CLI asks before replacing it.
- If the download is interrupted, files created by that download are removed.

## Updates

SwiftShare CLI automatically checks whether a new version is available. When an
update is found, it asks whether to install it.

Force an update check:

```sh
sws self-update
```

Skip the automatic update check for one command:

```sh
sws upload ./document.pdf --skip-update-check
```
