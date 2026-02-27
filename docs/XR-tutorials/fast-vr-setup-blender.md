---
sidebar_position: 1
---

# Fast VR setup for Linux (Meta Quest 2/3 -> ALVR -> Blender VR)

> Faculty disclaimer: this guide assumes the lab machines already have the required software installed (Steam, SteamVR, ALVR streamer, Blender, ADB). If something is missing, ask a supervisor/TA.

Goal: start from a Meta Quest 2 or 3 and end with Blender showing the 3D viewport inside the headset through ALVR.

Pipeline overview:

Meta Quest (ALVR viewer) ↔ Wi-Fi (or USB) ↔ ALVR streamer (Linux PC) ↔ SteamVR ↔ OpenXR ↔ Blender VR Scene Inspection add-on

---

## Fast lab overview

### Standalone VR vs PCVR

- Standalone VR: the Quest runs apps directly on the headset.
- PCVR: the PC runs the VR app; the headset is the display and tracking device.

### What [ALVR](https://github.com/alvr-org/ALVR/wiki/Linux-troubleshooting) is

ALVR is open-source software that turns a standalone headset (like Meta Quest) into a wireless PCVR headset by streaming the PC VR view to the headset and sending tracking and controller input back to the PC.

ALVR has two parts:

1) Streamer (PC, Linux)
- Runs on the computer.
- Hooks into SteamVR.
- Encodes the VR view to a video stream.
- Receives head tracking and controller input.

2) Viewer or client (Quest)
- Runs on the headset.
- Decodes the streamed video.
- Sends tracking data back to the PC.

### How this connects to Blender

Blender can run a VR session using OpenXR via the built-in VR Scene Inspection add-on. SteamVR provides OpenXR runtime support on many systems, and ALVR makes the Quest appear as the VR headset to SteamVR.

So the chain is:

Blender (OpenXR VR session) -> SteamVR -> ALVR streamer -> headset (ALVR viewer)

---

## Quick checklist before you start

- Quest 2 or Quest 3 charged.
- PC and Quest on the same network (recommended) and ideally close to the Wi-Fi access point.
- Controllers paired to the headset.

Optional but useful:
- USB cable to install the ALVR viewer (if not already installed) or to troubleshoot.

---

## Step 1: Connect Quest and PC (network basics)

1. Put the Linux PC and the Quest on the same Wi-Fi network.
2. If the stream is choppy, move closer to the Wi-Fi access point or use the recommended lab network.

Tip: the best experience usually comes from a strong 5 GHz or Wi-Fi 6 connection.

---

## Step 2: ALVR on the Linux PC (streamer)

1. Launch ALVR on the Linux PC (ALVR Launcher or ALVR Dashboard).
2. In ALVR, open the Installation section and register the ALVR driver if needed.
   - This step makes SteamVR detect ALVR as a headset driver.
3. Start SteamVR once if it has never been opened before on that machine.

When ALVR is ready, it will wait for a headset connection.

---

## Step 3: ALVR on the Quest (viewer)

### If the ALVR viewer is already installed

1. Put on the headset.
2. Open the ALVR app.
3. On the PC, in ALVR Devices, you should see the headset appear.
4. Click Trust or Allow for that device.

### If the ALVR viewer is not installed

Use ADB (Android Debug Bridge) to install the ALVR viewer APK.

You need:
- Developer mode enabled on the headset.
- USB debugging allowed.
- A USB cable connection to the Linux PC.

---

## ADB essentials (installing and debugging the Quest connection)

ADB is a command-line tool to communicate with Android devices (the Quest runs Android under the hood). You will use it to verify the USB connection and to install the ALVR viewer APK.

### 1) Connect and verify the headset

1. Plug the Quest into the Linux PC via USB.
2. Put on the headset and accept the prompts:
   - Allow USB debugging
   - Optionally Always allow from this computer

Then in a terminal on the Linux PC run:

```bash
adb devices
```

Expected outcomes:

- You see a line like: `<serial> device`  
  Good: ADB is connected.

- You see: `unauthorized`  
  Put on the headset and accept the USB debugging prompt, then run `adb devices` again.

- The list is empty  
  Try a different cable/port, unplug/replug, and run `adb devices` again.

Useful reset commands if ADB gets stuck:

```bash
adb kill-server
adb start-server
adb devices
```

### 2) Install the ALVR viewer APK

If you have the ALVR viewer APK file on the PC (example name `ALVRClient.apk`), install it with:

```bash
adb -d install -r ALVRClient.apk
```

What this does:
- `-d` targets a single device connected via USB.
- `-r` reinstalls/updates if the app is already installed.

If you get `INSTALL_FAILED_VERSION_DOWNGRADE`, uninstall first:

```bash
adb uninstall alvr.client
```

Then install again:

```bash
adb -d install ALVRClient.apk
```

Note: the package name can vary depending on the ALVR build. If you are unsure, list installed packages and search:

```bash
adb shell pm list packages | grep -i alvr
```

### 3) Optional: view logs for troubleshooting

```bash
adb logcat
```

This can help if the ALVR viewer crashes immediately or fails to start.

---

## Step 4: Start SteamVR and confirm the headset is connected

1. On the Quest, open the ALVR viewer.
2. On the PC, in ALVR Devices, Trust the headset if requested.
3. Start SteamVR (either from ALVR or manually).

If everything is correct:
- SteamVR shows a headset connected.
- The Quest shows the streamed VR environment.

---

## Step 5: Blender VR (VR Scene Inspection) through ALVR

1. Start Blender on the Linux PC.
2. Enable the VR add-on:
   - Edit -> Preferences -> Add-ons
   - Search: VR Scene Inspection
   - Enable it
3. Open your .blend scene.
4. Find the VR panel (usually in the 3D Viewport sidebar, press `N`) and start a VR session.

If SteamVR and ALVR are running correctly, the Blender VR view appears in the headset.

---

## Fast fixes

### Headset showing black screen

[Check official troubleshooting](https://github.com/alvr-org/ALVR/wiki/Linux-troubleshooting)

### Headset not showing in SteamVR

- In ALVR, register the ALVR driver (Installation section).
- Restart SteamVR.
- Restart ALVR streamer.

### Quest not detected by ADB

- Run `adb devices` and check for `unauthorized`.
- Put on the headset and allow USB debugging.
- Try `adb kill-server` then `adb start-server`.
- Try a different USB cable or port.

### Streaming is laggy or blurry

- Improve Wi-Fi signal (closer to access point, reduce congestion).
- Close heavy downloads/streams on the PC network.

### Blender VR option missing

- Confirm VR Scene Inspection add-on is enabled.
- Confirm SteamVR is running and the headset is connected via ALVR.

---

## References

- ALVR (project): https://github.com/alvr-org/ALVR
- ALVR wiki (installation guide): https://github.com/alvr-org/ALVR/wiki/Installation-guide
- ALVR on SideQuest (Quest app listing): https://sidequestvr.com/app/9/alvr
- Meta Quest developer mode: https://developers.meta.com/horizon/documentation/android-apps/enable-developer-mode/
- Blender VR (VR Scene Inspection, OpenXR): https://docs.blender.org/manual/en/latest/addons/3d_view/vr_scene_inspection.html
- SteamVR (Steam): https://store.steampowered.com/app/250820/SteamVR/
