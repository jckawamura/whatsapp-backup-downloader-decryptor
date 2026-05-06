# Running wabdd Locally

This guide covers how to run the project from source using [uv](https://docs.astral.sh/uv/).

## Prerequisites

- Python 3.9+
- [uv](https://docs.astral.sh/uv/getting-started/installation/) — install with:
  ```shell
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

## Setup

1. Clone the repository

   ```shell
   git clone https://github.com/giacomoferretti/whatsapp-backup-downloader-decryptor
   cd whatsapp-backup-downloader-decryptor
   ```

2. Install dependencies

   ```shell
   uv sync
   ```

   This creates a `.venv` virtualenv and installs all dependencies from `uv.lock`.

## Running

Prefix every `wabdd` command with `uv run`:

```shell
# Get an auth token
uv run wabdd token YOUR_GOOGLE@EMAIL.ADDRESS

# Download backup
uv run wabdd download --token-file tokens/YOUR_GOOGLE_EMAIL_ADDRESS_token.txt

# Download with filters
uv run wabdd download \
  --exclude "Media/WhatsApp Video/*" \
  --token-file tokens/YOUR_GOOGLE_EMAIL_ADDRESS_token.txt

# Decrypt backup
uv run wabdd decrypt \
  --key-file keys/PHONE_NUMBER_decryption.key \
  dump backups/PHONE_NUMBER_DATE
```

## Running tests

```shell
uv run pytest
```

HTML coverage report is written to `htmlcov/`.

## Using Docker

Build the image:

```shell
docker build . -t wabdd:local
```

Get token:

```shell
docker run -it --rm --user $(id -u):$(id -g) \
  -v $(pwd)/tokens:/tokens \
  wabdd:local token YOUR_GOOGLE@EMAIL.ADDRESS
```

Download backup:

```shell
docker run -it --rm --user $(id -u):$(id -g) \
  -v $(pwd)/backups:/backups \
  -v $(pwd)/tokens:/tokens \
  wabdd:local download --token-file /tokens/YOUR_GOOGLE_EMAIL_ADDRESS_token.txt
```

Decrypt backup:

```shell
docker run -it --rm --user $(id -u):$(id -g) \
  -v $(pwd)/backups:/backups \
  -v $(pwd)/keys:/keys \
  wabdd:local decrypt \
    --key-file /keys/PHONE_NUMBER_decryption.key \
    dump /backups/PHONE_NUMBER_DATE
```
