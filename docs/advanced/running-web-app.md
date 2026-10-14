# Running the Web App

Short answer: advanced users and developers may run FeedBack in a browser-hosted or server-backed mode instead of the desktop app.

## When To Use This

- Development.
- Remote library hosting.
- Testing web-only behavior.
- Running FeedBack on a machine that is not your main desktop.

## Important Differences

| Area | Desktop | Web/Server |
|---|---|---|
| Local folders | Desktop can expose local workflows. | Browser may not see server folders directly. |
| Audio engine | Native desktop audio features may be available. | Browser audio limitations apply. |
| Import | May use local folder workflows. | Often needs upload/import. |
| Plugins | Some plugins may behave differently. | Depends on browser support. |

## Run It With Docker

The server is published as a container image at `ghcr.io/got-feedback/feedback`.

| Tag | What it is |
|---|---|
| `0.3.0-alpha.1` | The current release. Pin a version tag like this one. |
| `nightly` | Built every night from `main`. Least stable. |
| `latest` | Only published for stable releases, so it may not exist yet. |

The container expects two folders:

| Container path | What goes there |
|---|---|
| `/dlc` | Your song library (feedpak files). Read by the library scan. |
| `/config` | Settings, library database, caches, favorites, and practice data. Keep it on a persistent folder. |

### Docker Compose

Create a folder for FeedBack, with a `library` folder for your songs inside it. Save this as `docker-compose.yml` in that folder:

```yaml
services:
  feedback:
    image: ghcr.io/got-feedback/feedback:0.3.0-alpha.1
    container_name: feedback
    restart: unless-stopped
    ports:
      - "8000:8000"
    volumes:
      - ./library:/dlc
      - ./config:/config
    environment:
      - DLC_DIR=/dlc
      - CONFIG_DIR=/config
      # Optional logging:
      # - LOG_LEVEL=INFO          # DEBUG | INFO | WARNING | ERROR
      # - LOG_FILE=/config/feedBack.log
```

Then run `docker compose up -d` and open `http://<server-ip>:8000` in a browser.

To use a library that lives somewhere else (a NAS share, an external drive), change `./library` to that path, for example `/volume1/music/feedBack:/dlc`.

### Docker Run

The same setup without Compose:

```bash
docker run -d --name feedback --restart unless-stopped \
  -p 8000:8000 \
  -v "$PWD/library:/dlc" \
  -v "$PWD/config:/config" \
  -e DLC_DIR=/dlc -e CONFIG_DIR=/config \
  ghcr.io/got-feedback/feedback:0.3.0-alpha.1
```

### Updating

1. Change the image tag to the new version.
2. Run `docker compose pull`, then `docker compose up -d`.

Your songs and settings stay in the `library` and `config` folders.

### Notes

- `DLC_DIR` sets the library folder. While it is set, changing the library folder in Settings doesn't move the scan. Change the `/dlc` mount instead.
- The microphone and audio interface are captured by the device running the browser, not by the server.
- Browsers only allow audio input on a secure page: HTTPS, or `localhost`. Opening the server by its LAN IP over plain HTTP blocks the input. Put the server behind an HTTPS reverse proxy, or forward the port over SSH (below) and open `http://localhost:8000`.
- The `docker-compose.yml` files in the main repository build the image from source. They are meant for development, not for running a published image.

To reach the server through an SSH tunnel from the computer running the browser:

```bash
ssh -L 8000:localhost:8000 <user>@<server-ip>
```

## Common Problems

| Problem | Try This |
|---|---|
| Cannot import from local disk | Use the upload/import path supported by the hosted instance. |
| Audio input behaves differently | Check browser permissions and selected device. |
| Audio input is blocked or no device is listed | Open the app over HTTPS or `localhost` (see Notes above). |
| Library is empty | Check that your songs are in the folder mounted at `/dlc`, then run a Rescan. |
| Desktop-only feature missing | Use the desktop app for native audio features. |

## Related Pages

- [Import Songs](../songs/import.md)
- [Audio Devices](../tuning/audio-devices.md)
