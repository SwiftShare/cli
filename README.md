# SwiftShare CLI
Our official command-line tool to upload and download file transfers from SwiftShare.  
Development is active, and more features will be added over time. The project will also be open-sourced soon.

## Installation
[Download the latest version](https://github.com/SwiftShare/cli/releases/latest)

## Commands

### Upload
```sh
sws upload <file|folder>... [flags]
```
or using its alias:
```sh
sws up <file|folder>... [flags]
```
| Flag                      | Description                                                          |
| ------------------------- | -------------------------------------------------------------------- |
| `-p, --password [value]`  | Protect transfer with password.                                      |
| `-e, --expiration <days>` | Optionally set transfer expiration.                                  |

### Download
```sh
sws download <identifier> [flags]
```
or using its alias:
```sh
sws dl <identifier> [flags]
```
| Flag                     | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| `-p, --password [value]` | Unlock password-protected transfer. Prompts if no value. |
