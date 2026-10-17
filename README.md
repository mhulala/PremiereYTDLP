# PremiereYTDLP
YTDLP integration for Adobe Premiere Pro

## Install

Close Premiere, open **YouTube-Import-Setup.exe**, and click **Install**. Restart Premiere and open **Window > Extensions > YouTube Import** (or **Extensions (Legacy)**).

The Windows installer embeds the panel and reuses working dependencies. Missing yt-dlp, FFmpeg/ffprobe, Deno, and Node.js are downloaded, checked against upstream SHA-256 hashes, and installed under `%LOCALAPPDATA%\YouTubeImport\tools`. It does not change global path. 

## Usage

Paste a YouTube link, choose **Video** or **Audio**, and click **Download**. On first use, choose a download folder under **Settings** where the file will be stored. Completed media is imported into the current project's bin. Disable automatic import to download without a project open.

While a download runs, you can still paste other links and **Add to queue**. Downloads run one at a time, in submission order. Each queued item keeps the settings and target project captured when it was added. The queue is kept for the current panel session and is discarded when the panel closes. Files needing manual import can be imported together after the queue finishes.

- **Video** is selected by default. The default preset limits sources to 1080p/60 fps, excludes AV1, prefers AVC/M4A, requests MP4 recoding with H.264/AAC and 320 kb/s audio, and uses Node.js for YouTube JavaScript.
- **Audio** aims to use the highest quality audio available from the source link.
- You cna edit or replace the command for each preset individually to your linking. Clearing the custom command field will revert to the original preset.
- Paste commands without a URL; the `yt-dlp` prefix is optional. Pasted `--path`, `--paths`, `-P`, `--output`, `-o`, and `--ffmpeg-location` options and their values are removed because the panel manages them.
- A pasted command beginning with `ffmpeg` applies encoder options to the selected preset's media. Its input/output paths and overwrite switches are discarded. Compatible mode checks the resulting codecs before import.
- **Premiere compatible** checks real media streams with ffprobe. Existing H.264/yuv420p/AAC MP4 is imported without another encode. Other video is converted; supported MP3/AAC/PCM audio is kept and other audio becomes PCM WAV. **Keep custom output** skips the compatibility safeguard for yt-dlp commands.
- Finished media is saved directly in the selected download folder. 

Settings are saved locally. Avoid storing credentials in custom command text. The command field runs without a shell, so pipelines and shell substitutions are not supported. Execution hooks, external config files, extra input files, and reporting overrides are also unavailable.

## Uninstalling

Close Premiere and remove its installed extension folder. Private tools/logs can be removed from `%LOCALAPPDATA%\YouTubeImport`; reused tools elsewhere are left alone. If no other unsigned panels need them, remove `PlayerDebugMode` from `HKCU\Software\Adobe\CSXS.11` and `.12`. Downloaded media is unaffected from uninstalling. 

Sources: [yt-dlp](https://github.com/yt-dlp/yt-dlp), [FFmpeg builds](https://www.gyan.dev/ffmpeg/builds/), [Deno](https://github.com/denoland/deno/releases), [Node.js](https://nodejs.org/dist/latest-v24.x/), [Adobe CEP](https://github.com/Adobe-CEP/CEP-Resources).

