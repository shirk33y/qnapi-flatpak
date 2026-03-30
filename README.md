# QNapi Flatpak

Flatpak manifest for [QNapi](https://github.com/QNapi/qnapi) — a subtitle downloader for Linux supporting NapiProjekt, Napisy24, and OpenSubtitles.

## Install

Download the latest `qnapi.flatpak` from [Releases](https://github.com/shirk33y/qnapi-flatpak/releases/latest) and install:

```bash
flatpak install qnapi.flatpak
flatpak run io.github.qnapi.QNapi
```

## Build locally

Requires `flatpak-builder` and the KDE Platform runtime:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak-builder --install --user --force-clean build-dir io.github.qnapi.QNapi.yml
```

Or with Podman (no `flatpak-builder` needed on the host):

```bash
podman run --rm --privileged \
  -v "$PWD":/build:z -w /build \
  docker.io/bilelmoussaoui/flatpak-github-actions:kde-5.15-23.08 \
  bash -c "flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo --user \
    && flatpak-builder --repo=repo --disable-rofiles-fuse --install-deps-from=flathub --force-clean --user build-dir io.github.qnapi.QNapi.yml"
```

## Notes

- **7-zip** is bundled at `/app/bin/7z` (7-zip 26.00 static binary) — no host installation needed.
- Subtitles are downloaded to the same directory as the video file (`--filesystem=home`).
- Uses KDE Platform 5.15-23.08 runtime.

## Patches applied

- `0001-fix-missing-QIcon-include-for-Qt-5.15.patch` — adds missing `#include <QIcon>` for Qt 5.15 compatibility.
