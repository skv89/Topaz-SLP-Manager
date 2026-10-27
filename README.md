# SKV89's SLP Manager

**SKV89's SLP Manager** is the successor to the original **Topaz SLP Tuning Launcher**, which I created in response to the Topaz community's calls for faster SLP processing and better use of their hardware. Many users felt that SLP was too slow and could not take full advantage of their systems; the original launcher was my answer to that frustration. This more powerful successor adds the features users have requested since that first Topaz SLP Tuning Launcher release: batch processing and queues, custom output resolutions beyond Topaz's standard resolution lock, pause/resume, automatic resume and recovery, manual recovery from partial videos, and configurable out-of-memory (OOM) protection with retry and resume options to improve reliability during long, unattended runs. It also adds expanded tuning, optimization and monitoring features. After Topaz expressed concern that the original Topaz Tuning Launcher app might be mistaken for an official Topaz product, I replaced "Topaz" with "SKV89's" to make its independent origins clear. **This is a community-developed app by me, SKV89, not an app developed or endorsed by Topaz.**

A portable Windows application for queuing, tuning and resuming **Topaz Starlight Precise (SLP)** video processing. Manager uses your licensed Topaz installation and runs its processing backend directly, without opening the Topaz Video interface.

![SKV89's SLP Manager v1.0.0 showing the processing queue, per-file output settings and partial continuation files](docs/images/slp-manager-v1.0.0-queue.png)

## Download

Download **SKV89-SLP-Manager-v1.0.0-windows-x64.zip** from the [v1.0.0 release](https://github.com/skv89/Topaz-SLP-Manager/releases/tag/v1.0.0), extract it to a writable folder, and run **SKV89's SLP Manager.exe**. Keep the included notices and `licenses` folder with the program. Python does not need to be installed separately.

## Features

- **Queue multiple videos** with output settings saved separately for each file. Drag videos into the queue, reorder waiting files, or apply settings to selected rows.
- **Pause and continue long jobs.** Resume a saved Manager job or select an existing partial video, including a Topaz-generated partial. Earlier source frames provide temporal context at the continuation point.
- **Automatic recovery.** Compatible encoded video is retained when possible. Manager checks damaged partials, attempts repair and regenerates an irreparable ending when it can establish a usable prefix.
- **Choose your output.** H.264, HEVC NVENC, AV1 software or NVENC, FFV1, FFV1 All Intra, ProRes and DNxHR are available with codec-specific quality, bit-depth and chroma choices. Available profiles depend on your FFmpeg build and GPU.
- **Tune processing for your system.** AutoTune compares supported settings and derives conservative and aggressive presets. Ready presets use bold, slowly shimmering Save buttons until saved. BF16 Fusion is optional and experimental.
- **Optimize verification and encoding separately.** Measure supported CPU/GPU checking routes and encoder resources using representative video, then apply compatible recommendations explicitly.

## Requirements

- A 64-bit Windows system and an NVIDIA GPU suitable for your selected SLP workload.
- A licensed, compatible Topaz Video installation with the SLP model and its Neuroserver runtime available locally. Sign-in, model entitlement and licensing remain part of Topaz Video.
- FFmpeg and FFprobe, including the encoders you intend to use. NVENC options also require compatible NVIDIA hardware and drivers.
- Enough system memory, GPU memory and free disk space for the chosen resolution, processing settings and working video. Optional lossless backups require substantially more disk space.

Use **Dependencies** to check your installation. Manager does not include Topaz Video, model weights, FFmpeg, NVIDIA drivers or a Topaz license. Do not run competing Topaz/SLP jobs while processing or running AutoTune.

## First job

1. Open **Settings**, select the Topaz installation, choose your processing settings, and click **Apply Settings**.
2. On **Queue**, choose the output resolution, codec, profile and quality. A blank output folder saves beside each source.
3. Add files or drag them into the queue. Each file keeps the settings that were committed when it was added.
4. To change waiting rows, select them and use **Apply Current Settings to Selected Rows**.
5. Click **Start / Resume Queue**. Use a short representative clip first to check output appearance and resource needs.

Adding a file reads metadata quickly. Manager establishes the required frame count and timing before processing or attaching a partial. Qualified files use index/packet checks with boundary decoding; other files may require a full decode.

## Pause, close and resume

**Pause Queue** finishes the current processing chunk and closes the working video. **Stop After Current File** finishes that file without starting the next. **Stop Queue Now** requests a short writer close before stopping the worker; recovery may be needed afterward.

Closing while busy offers **Pause safely and exit**, **Stop now and exit**, or **Cancel**. Safe exit can take time because it waits for the current chunk or media operation and checks saved work.

To use a partial video, add its original source to the queue, right-click the row and select **Resume from partial video**. Choose the actual video; you do not need to find a JSON checkpoint.

- **Keep selected file unchanged — copy compatible video** creates a working copy and continues from it. This is the default.
- **Append directly to selected file** extends a compatible live MKV in place. Other containers, including MOV and MP4, use a working copy and replace the selected file after completion, retaining the original beside it. A required container change produces a file with the appropriate extension beside the selection.

Both choices retain compatible encoded frames without re-encoding. Repair or conversion runs automatically when needed. Conversion can take longer and may add compression loss for lossy formats. Choose a partial that begins at the start of the source and uses the intended SLP model and sharpness; Manager cannot prove that an unrelated partial belongs to that source.

A row with saved progress can accept a longer replacement partial. Failed or cancelled validation keeps the previous selection. A shorter usable partial shows both frame counts for confirmation.

## Files, settings and updates

Manager keeps settings, queue records, tuning results and diagnostics in **Data** beside the executable. Keep this folder when updating.

The growing working video is stored beside the source, normally as `<source>_temp.mkv`. Optional **Continuous video + lossless backup** adds FFV1 backup sections in a source-adjacent folder. Completed output is validated before owned temporary media is cleaned up. Selected originals and protected recovery copies are retained.

Job diagnostics use readable folders under `Data/jobs/<video filename> [short ID]/`. Ordinary successful jobs retain a job record, final-output receipt and compressed log. These diagnostics do not expire automatically; use completed-history controls to inspect or remove history. Finished videos are separate from diagnostic history.

For a manual update, close Manager, extract the new portable package into its folder and replace the supplied program files. Preserve **Data** and any separately downloaded runtimes. Do not replace files while a job is running.

## Supported scope

- The processing pipeline expects **constant-frame-rate SDR video**. Variable/discontinuous timing and HDR/BT.2020 conversion are not supported by this pipeline.
- Quick admission checks do not inspect every interior picture. Unresolved count/timing or detected damage can require deeper scanning. Final output receives a full decode check.
- Recovery cannot repair every missing index, damaged bitstream or uncertain source alignment. Working files and selected originals are retained when recovery cannot complete safely.
- AutoTune and Optimize measurements apply to the tested hardware, tools, format and settings. Scanner/encoder gains are not promises of faster complete SLP jobs. CPU verification starts with up to 16 logical threads; encoder/filter defaults use Auto.
- Experimental Fusion and optional runtime choices require compatible hardware and models. Start with defaults and a short clip before a long run.

## Help and attribution

Hover controls for guidance, press **F1** for expanded help, or use **Guide**. Include relevant diagnostic logs when reporting a problem through [Issues](https://github.com/skv89/Topaz-SLP-Manager/issues), and review logs for private information before sharing them.

This is independent community tooling and is not endorsed by Topaz Labs or NVIDIA. Third-party notices are included with the download. Topaz software, models and NVIDIA components remain subject to their own licenses.
