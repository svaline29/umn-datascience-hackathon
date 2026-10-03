---
name: mcp-video-analyzer
description: Analyze uploaded videos or video URLs with the mcp-video-analyzer tool, returning transcript, key frames, OCR text, metadata, timeline, warnings, and timestamped answers.
---

Use this skill whenever the user uploads a video file, provides a local video path, or asks you to analyze a video URL with transcript + frames + OCR rather than only reading metadata.

This skill is based on `guimatheus92/mcp-video-analyzer` and should prefer the one-shot CLI unless this environment already has the MCP server configured for the current session.

## Inputs this skill handles

- Uploaded local video files such as `.mp4`, `.mov`, `.mkv`, `.webm`, `.avi`, `.m4v`
- Absolute local paths or `file://` URIs
- Public direct video URLs
- Supported platform URLs such as YouTube, Vimeo, TikTok, Instagram, X, Twitch, Dailymotion, Facebook, and Loom

## Core workflow

1. Resolve the video source.
   - If the user uploaded a file, use its local path.
   - Prefer absolute paths for local files.
2. Run the analyzer with Bash using the CLI:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>"
```

3. Parse the JSON from stdout.
4. Inspect these fields as needed:
   - `metadata`
   - `transcript`
   - `ocrResults`
   - `timeline`
   - `warnings`
   - `frames` (`{ time, filePath, mimeType }`)
5. If the question depends on visuals, read the frame image files referenced in `frames[].filePath`.
6. Answer with timestamps in `M:SS` format whenever making claims about events in the video.
7. Relay relevant warnings to the user when they affect interpretation or completeness.

## Recommended command patterns

General analysis:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>"
```

Fast transcript-first pass:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>" --detail brief
```

Dense UI / screen recording where small text matters:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>" --max-width 0
```

Limit output fields when only text is needed:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>" --detail brief --fields metadata,transcript
```

Use a dedicated output directory for easier frame inspection:

```bash
npx -y mcp-video-analyzer@latest analyze "<video-path-or-url>" --out /workspace/.cache/video-analysis
```

## When to adjust options

- Use `--detail brief` for quick summarization or transcript-only questions.
- Use default `standard` for most videos.
- Use `--max-width 0` or a high value like `1568` for IDEs, terminals, dashboards, spreadsheets, or UI bug recordings.
- Use `--force-refresh` if the user says the source changed or previous cached results look stale.
- Use `--language <code>` when the spoken language is known and transcription quality matters.

## Interpretation guidance

- Treat `warnings` as actionable caveats, not automatic failure.
- An empty transcript plus a silent-audio warning usually means the video truly has no speech.
- For platform URLs, missing `yt-dlp` may reduce or prevent frame extraction.
- Only public `http(s)` URLs are accepted by default. Do not retry private-network URLs unless the environment is intentionally configured for that.

## Response style

- Be explicit about what came from transcript, OCR, and visual frames.
- Cite timestamps for factual claims about moments in the video.
- If analysis is partial, say what succeeded and what was unavailable.

## Environment notes

- Requires Node.js 22.12+.
- `ffmpeg` is bundled by the package.
- Local files and direct video URLs work without `yt-dlp`; many platform URLs need it.
- The first `npx` run may take longer because the package is downloaded.

## Optional MCP route

If this environment later gains a configured MCP server for this tool, you may use the richer MCP tools instead of the CLI. Otherwise, the CLI path above is the default and sufficient.
