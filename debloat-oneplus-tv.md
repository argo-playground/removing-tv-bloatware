# How to Debloat a OnePlus TV Using ADB

*A practical guide to removing preinstalled apps and making your Android TV feel faster.*

---

## TL;DR

- Enable USB debugging on the TV, connect from a laptop with ADB, and uninstall unwanted preinstalled apps for the current user.
- The method is reversible, free, and needs no root.
- Expect a modest, noticeable improvement, not "10x faster" as social media claims.
- You do not need an AI tool for this; the commands are simple.

---

## What Is This?

Most Android TVs ship with preinstalled apps (bloatware) that run background services, consume RAM and storage, and sometimes push sponsored content to the home screen. ADB (Android Debug Bridge) is Google's official command-line tool for communicating with an Android device. It can uninstall system apps for the current user without rooting the device.

## Why Do It?

| Benefit | Detail |
|---|---|
| Lower RAM usage | Fewer background processes compete for limited memory |
| More free storage | Low storage is a common cause of TV lag |
| Cleaner home screen | Fewer promoted apps and recommendations |
| Reversible | Removed apps can be restored with one command |

**Reality check:** Gains depend on hardware. Entry-level OnePlus TV models (around 2 GB RAM) benefit most. Higher-spec models will see a smaller difference.

---

## How To

### Step 1: Enable Developer Options on the TV

1. Go to **Settings > Device Preferences > About**.
2. Select **Build** seven times until a message confirms Developer mode.
3. Go back to **Device Preferences > Developer options**.
4. Enable **USB debugging** (and **Network debugging** if available).

### Step 2: Install ADB on Your Laptop

Download **Android SDK Platform-Tools** from the official Android developer site and extract it to a folder. Open a terminal in that folder.

### Step 3: Connect to the TV

Both devices must be on the same Wi-Fi network.

1. Find the TV's IP address under **Settings > Network**.
2. Run:

```bash
adb connect <TV_IP>:5555
```

3. Accept the authorisation prompt on the TV screen.
4. Verify the connection:

```bash
adb devices
```

### Step 4: List Installed Apps

```bash
# Preinstalled (system) apps
adb shell pm list packages -s

# User-installed apps
adb shell pm list packages -3
```

### Step 5: Remove an App

```bash
adb shell pm uninstall -k --user 0 <package.name>
```

The `-k` flag keeps app data and `--user 0` removes the app only for the main user, which is what makes the change reversible.

### Step 6: Restore an App (If Needed)

```bash
adb shell cmd package install-existing <package.name>
```

---

## What to Remove (and What to Leave Alone)

**Generally safe to consider:**
- Preloaded streaming or shopping apps you do not use
- Games, demo apps, and promotional apps
- Third-party apps you installed and no longer need

**Do not remove:**
- Anything with `launcher`, `settings`, `inputs`, or `tvprovision` in the name
- Google core services (`gms`, `gsf`, `vending`)
- OnePlus system-update and core service packages

Removing critical packages can cause a boot loop. Search each package name online before removing it, and remove **one package at a time**, confirming the TV still works normally.

---

## Bonus: Extra Speed-Ups

### Reduce Animations

```bash
adb shell settings put global window_animation_scale 0.5
adb shell settings put global transition_animation_scale 0.5
adb shell settings put global animator_duration_scale 0.5
```

Use `1` to restore the defaults.

### Other Quick Wins

- Clear the cache of heavy apps (**Settings > Apps**).
- Keep at least 20% of storage free.
- Restart the TV periodically.
- Disable autostart or background activity for apps you rarely use.

### Remove Sponsored Home Screen Content

Removing the stock launcher is risky. A safer alternative is to install a third-party launcher such as **Projectivy** or **FLauncher** and set it as the default.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `adb connect` fails | Confirm the TV IP, same Wi-Fi network, and that debugging is enabled |
| No authorisation prompt | Run `adb kill-server`, then reconnect |
| Package not found | Re-check the exact name using `pm list packages` |
| TV behaves oddly after removal | Restore the last removed package with `install-existing` |
| Apps return after an update | Re-run the removal commands; major OTA updates can restore them |

---

## Risks and Caveats

- A factory reset restores all removed apps.
- Software updates may reinstall some apps.
- Removing the wrong system package can cause instability. Go slowly.
- Warranty or support implications are generally minimal for user-level removal, but check your terms if this matters.

---

## Conclusion

Debloating via ADB is a low-risk, reversible way to reclaim memory and storage on a OnePlus TV. Combine it with reduced animations and sensible storage management for the best result, and be conservative about what you remove.

---

*Last updated: October 2026*
