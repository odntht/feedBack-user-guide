# Export Diagnostics

Short answer: export diagnostics when you report a bug so maintainers can see logs, environment details, and recent app state.

## When To Use This

- FeedBack hears the wrong input.
- Calibration hangs.
- A plugin fails to load.
- Playback or library scanning fails.
- A maintainer asks for a diagnostics bundle.

## Steps

1. Reproduce the problem in the same app session if possible.
2. Open Settings → System → **Diagnostics**.
3. Choose what to include: system info, hardware, server logs, browser console, plugin diagnostics.
4. Click **Preview Bundle** to see what will be shared, if you want.
5. Click **Export Diagnostics** and save the `.zip` file.
6. Attach the file to your issue or tester thread.

## What To Include With It

- Your platform and build version.
- What you expected to happen.
- What actually happened.
- Steps to reproduce.
- Screenshots if the problem is visual.

## Privacy Note

**Redact paths & song names** is on by default. It removes library paths, song file names, and IP addresses from the bundle. Device names and log lines stay in, so use **Preview Bundle** if you want to check before sharing.

## Related Pages

- [Reporting an Issue](../troubleshooting/report-issue.md)
- [Audio Input Problems](../troubleshooting/audio-input.md)

