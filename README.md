# DIY Raspberry Pi camera for DAM

Build your own outdoor Raspberry Pi camera and connect it to
**[DAM — days in a minute](https://chase-nova.com)**: the camera takes a
photo every few seconds, and every day of photos becomes a one-minute
time-lapse video of your place.

This guide takes you from parts on a desk to a sealed camera on a wall,
step by step: choosing the hardware, choosing a lens, building a
weatherproof package, installing the software, and registering the camera
with your DAM account.

*[Photo 00 — images/00-finished-camera.jpg: the finished, packaged camera mounted outdoors]*

| | |
| --- | --- |
| **Time** | about 3 hours (plus waiting for parts) |
| **Skills** | basic computer use, copying commands into a terminal, a drill and a screwdriver |
| **Result** | a camera that uploads to your own Location on DAM and a new video every day |
| **Software** | [rpi-camera-agent](https://github.com/chase-nova/rpi-camera-agent) (open source, Apache-2.0) |

## Contents

1. [Get a DAM account](#1-get-a-dam-account)
2. [Choose the hardware](#2-choose-the-hardware)
   - [2a. Raspberry Pi](#2a-raspberry-pi) · [2b. Camera module](#2b-camera-module) · [2c. Lens](#2c-lens) · [2d. Everything else](#2d-everything-else)
3. [Plan the outdoor package](#3-plan-the-outdoor-package)
4. [Assemble on the bench](#4-assemble-on-the-bench)
5. [Install the operating system](#5-install-the-operating-system)
6. [Install the camera software](#6-install-the-camera-software)
7. [Register the camera with DAM](#7-register-the-camera-with-dam)
8. [Focus, test and seal](#8-focus-test-and-seal)
9. [Mount it outdoors](#9-mount-it-outdoors)
10. [Troubleshooting](#10-troubleshooting)
11. [Privacy, safety and license](#11-privacy-safety-and-license)

---

## 1. Get a DAM account

To connect a camera you need a DAM account with the **post_staff** role.
It lets you:

- create your own **Locations** (the places your cameras film),
- **register** cameras and hold them (you are their *custodian*),
- assign a camera to one of your Locations, start/stop it, and manage its
  videos.

Accounts are created by the DAM operator; each account has a limit on how
many Locations and cameras it can hold.

> **TODO (operator):** how to request an account — contact address or
> form. Until then: ask the person who runs your DAM instance.

When your account is ready, sign in at [chase-nova.com](https://chase-nova.com)
and open **Manage**. If you see **Devices → Register device**, you are
set.

---

## 2. Choose the hardware

A camera is three independent choices: a **Raspberry Pi**, a **camera
module**, and (for the HQ camera) a **lens**. Any Pi works with any
camera below.

### 2a. Raspberry Pi

Both models below use little power — the camera only takes one photo
every few seconds, so a small Pi is plenty.

| | **Raspberry Pi Zero 2 W** | **Raspberry Pi 3 (B / B+)** |
| --- | --- | --- |
| Power use | lowest | a little higher |
| Power supply | 5 V, 2.5 A USB (micro-USB) | 5 V, **2.5 A** USB (micro-USB) — a weak supply causes under-voltage |
| Camera connector | small 22-pin → needs a **22-pin to 15-pin camera cable** | standard 15-pin — the camera's own cable fits |
| Network | 2.4 GHz Wi-Fi | 2.4 GHz Wi-Fi (3B+: also 5 GHz) and Ethernet |
| Size | tiny — fits small boxes | credit-card size |
| Good for | small, low-power packages | easier first builds (Ethernet for setup) |

Add a small **heatsink** for the processor on either board (§3).

*[Photo 01 — images/01-pi-zero-and-pi3.jpg: a Pi Zero 2 W and a Pi 3 side by side]*

### 2b. Camera module

| | **Raspberry Pi HQ Camera (IMX477), M12 mount** | **Arducam IMX462 low-light (night vision)** |
| --- | --- | --- |
| Best for | daytime scenery; sharp photos; any view angle | dark places; colour images at night |
| Sensor | Sony IMX477, 12.3 MP | Sony IMX462 STARVIS, 2 MP |
| Lens | **interchangeable M12 lens** — you choose it in §2c | comes with a very bright F0.95 lens, about 92° wide |
| Setup | works out of the box | needs extra setup steps (§5.4): an overlay, a tuning file and a one-time camera firmware update |
| Night | city lights and dusk look good; truly dark scenes stay dark | real night images with the agent's night mode |

Buy the HQ Camera **M12** version (not the CS-mount version) if you want
to use the M12 lenses in §2c. The camera comes with a 15-pin ribbon cable;
for the Pi Zero 2 W you need the 22-pin to 15-pin cable instead.

*[Photo 02 — images/02-cameras.jpg: HQ Camera M12 and Arducam IMX462]*

### 2c. Lens

For the HQ Camera M12. Pick the lens by **how wide** you want the view
(horizontal field of view):

| Lens | View (horizontal, approx.) | Use it for |
| --- | --- | --- |
| **1.56 mm fisheye** | ~180° (curved lines) | the whole sky, a courtyard |
| **2.8 mm wide** | ~110–115° | wide landscapes, streets |
| **4 mm "starlight" F1.5** | ~70–80° | **a good default** — natural perspective, bright lens for dusk |
| **8 mm** | ~40–45° | a distant subject: a skyline, a mountain |

Tips:

- Shop listings often quote the **diagonal** angle — the horizontal view
  is 20–25 % narrower.
- The angle also depends on the sensor size; the numbers above are for
  the HQ Camera.
- Prefer lenses rated **5 MP or more**; a 2 MP lens looks soft.
- "**Starlight**" lenses (F1.2–F1.8) collect more light — better at dusk
  and dawn. "IR" on a lens only means it keeps focus under infrared light;
  it does not make it better behind the HQ Camera's IR filter.
- M12 lenses screw into the holder; you focus by turning the lens (§8).

*[Photo 03 — images/03-lenses.jpg: the four lens types]*

### 2d. Everything else

| Part | Notes |
| --- | --- |
| microSD card, 32 GB or more | an "endurance" / high-endurance card lasts longer in a camera |
| USB power supply, 5 V 2.5 A | indoor or weather-protected; stays **outside** the box |
| USB power cable (micro-USB) | **short and thick** — 2–3 m at most; long thin cables drop the voltage |
| Waterproof box | see §3 |
| Acrylic dome or flat acrylic window | see §3 |
| Cable gland (PG7/PG9) + outdoor silicone | seals the cable entry |
| Heatsink for the Pi | stick-on |
| Silica-gel pack | keeps the inside dry |
| Standoffs/screws, mounting bracket | to fix the Pi and camera inside and the box outside |
| Insect mesh, a small plate for a sunshade | see §3 |

*[Photo 04 — images/04-all-parts.jpg: everything laid out before assembly]*

---

## 3. Plan the outdoor package

The package has three jobs: keep water out, keep the sun's heat out, and
let the camera see. Heat is the one people underestimate — a clear box in
direct sun reaches **80 °C inside**, where the camera software pauses to
protect itself.

**Box**
- A plastic food container with a good lid, or an IP65 junction box.
- **White or covered with aluminium foil tape** on the top and sunny
  sides — the biggest single improvement, nearly free.

**Window for the camera**
- An **acrylic dome** works; keep it small, because a dome traps heat and
  can fog.
- A **flat acrylic window** traps less heat and distorts less —
  especially better for fisheye lenses.
- Seal the window to the box with outdoor silicone.

**Heat**
- A **second roof**: a small plate 1–2 cm above the box, with an air gap.
  The sun heats the plate, not the box — often 10–20 °C cooler.
- **Vents**: small holes low on the shaded side and high on the opposite
  side, covered with insect mesh and placed under the lid's edge so rain
  cannot enter.
- A **heatsink** on the Pi; mount the board upright so warm air rises
  across it.
- **No fan** — it draws power and invites under-voltage.

**Water and damp**
- Bring the cable in through a **cable gland** at the bottom, never the
  top.
- One small **drain hole** at the lowest point.
- A **silica-gel pack** inside against condensation at night.

**Power**
- The USB power supply stays **outside the box** in a dry place (it makes
  heat of its own).
- Keep the USB cable short; if you need distance, move the power supply
  closer, not the cable longer.

*[Photo 05 — images/05-box-prepared.jpg: box with window, gland, vents and drain hole]*
*[Photo 06 — images/06-sunshade.jpg: the second roof with its air gap]*

---

## 4. Assemble on the bench

Build and test everything **on a table first**; seal the box only after
the camera works (§8).

1. **Connect the camera cable** with the Pi switched off. Lift the
   connector latch, insert the ribbon fully (contacts facing the board as
   printed on the connector), press the latch down. Do the same on the
   camera board.
   - Pi Zero 2 W: use the 22-pin to 15-pin cable.
2. **Fit the lens** (HQ Camera M12): screw it in until it is roughly in
   focus; fine focus comes later.
3. **Stick the heatsink** on the Pi's processor.
4. **Fix the camera behind the window** so the lens nearly touches it
   (reflections disappear) and nothing of the box edge is in view.
5. **Fix the Pi** on standoffs, upright if possible.

*[Photo 07 — images/07-cable-connected.jpg: ribbon cable in the Pi and camera connectors]*
*[Photo 08 — images/08-inside-wired.jpg: camera behind the window, Pi on standoffs]*

---

## 5. Install the operating system

### 5.1 Write the SD card

1. Install **[Raspberry Pi Imager](https://www.raspberrypi.com/software/)**
   on your computer.
2. Choose your Pi model, then **Raspberry Pi OS (Legacy, 64-bit) Lite**
   (Bookworm). The newer release also works for the HQ Camera, but some
   first-boot settings are ignored there, and the IMX462 steps below are
   tested on Bookworm.
3. Open the **settings** (gear / "Edit settings") and set:
   - a hostname, e.g. `dam-cam-1`,
   - a user name and password,
   - your **Wi-Fi** name, password and **country**,
   - **SSH enabled** — preferably with your public key,
   - your time zone.
4. Write the card.

*[Photo 09 — images/09-imager-settings.png: the Imager settings screen]*

### 5.2 First boot

Put the card in the Pi and power it on. After a minute or two, connect
from your computer:

```bash
ssh <user>@dam-cam-1.local        # or ssh <user>@<pi-ip>
sudo apt update && sudo apt full-upgrade -y
sudo reboot
```

### 5.3 Check the camera

```bash
rpicam-hello --list-cameras
```

You should see your camera (`imx477` for the HQ Camera). If the list is
empty, switch off and re-seat the ribbon cable (§10).

### 5.4 Only for the Arducam IMX462

This camera needs three extra steps (tested on Pi 3 with Bookworm):

1. **Enable the camera overlay.** In `/boot/firmware/config.txt` set
   `camera_auto_detect=0` and add `dtoverlay=arducam-pivariety`, then
   reboot.
2. **Install the tuning file** so colours and exposure are right:
   ```bash
   cd /usr/share/libcamera/ipa/rpi/vc4/
   sudo cp imx462.json arducam-pivariety.json
   ```
3. **Update the camera's firmware once** — without it the camera reports
   nonsense sensor modes (e.g. `2147485568x1080`) and refuses to start.
   Use Arducam's firmware package for **your exact camera model (SKU)**
   from the [Arducam IMX462 documentation](https://docs.arducam.com/Raspberry-Pi-Camera/Pivariety-Camera/IMX462/).
   The updater needs the I2C bus:
   ```bash
   sudo raspi-config nonint do_i2c 0
   sudo modprobe i2c-dev
   # then run Arducam's firmware_update tool as its instructions say, and reboot
   ```
   > **Warning:** firmware is specific to the camera model — the wrong
   > package can brick the camera.

Afterwards `rpicam-hello --list-cameras` shows a normal 1920×1080 mode.
Avoid blind upgrades of the camera libraries later; re-check the camera
after big system updates.

---

## 6. Install the camera software

On the Pi:

```bash
sudo apt install -y git
git clone https://github.com/chase-nova/rpi-camera-agent.git
cd rpi-camera-agent
./scripts/install.sh
```

The installer sets up the camera libraries, a small Python environment in
`/opt/dam-agent`, a settings file `/opt/dam-agent/.env.prod`, and the
`dam-agent` background service. It does **not** start the camera yet —
that needs your registration token (§7).

Open the settings file and set your **time zone** (the camera's local
time names the daily videos):

```bash
nano /opt/dam-agent/.env.prod
```

```ini
TIMEZONE=Asia/Seoul          # your IANA time zone, e.g. Europe/Berlin
```

**Only for the IMX462** — also turn on night mode (a good starting
point; tune per site):

```ini
NIGHT_EXPOSURE_MS=250
NIGHT_GAIN=2
MAX_EXPOSURE_MS=5000
```

All other settings have sensible defaults; `.env.example` in the
software repository explains each one.

---

## 7. Register the camera with DAM

### 7.1 Create your Location

In **Manage → Locations → New location**:

- **Name**: letters, digits, `-` or `_` (e.g. `River-View`). It becomes
  part of the video file names and cannot be changed while a camera is
  assigned.
- **Time zone**: the place's time zone.
- **Coordinates** (optional but recommended): enables the dawn/dusk
  schedule. *Use my location* fills them in from your browser.

### 7.2 Register the camera and get its token

In **Manage → Devices → Register device**, give it a label (e.g.
"Rooftop east"). You get an **enrollment token** starting with `dame_`:

- it is shown **only once** — copy it now,
- it works **once**, and **expires after 48 hours**.

*[Photo 10 — images/10-register-dialog.png: the token dialog]*

### 7.3 Give the token to the camera

On the Pi, add it to the settings file and start the service:

```bash
nano /opt/dam-agent/.env.prod      # ENROLLMENT_TOKEN=dame_...
sudo systemctl start dam-agent
journalctl -u dam-agent -f         # watch it: "enrolled as dam-..."
```

On its first connection the camera creates its own secret key, trades the
token for it, and removes the token from the file. The key is stored in
`/var/lib/dam-agent/credential.json` — treat it like a password.

> Other ways to hand over the token: put a line `ENROLLMENT_TOKEN=dame_...`
> into a file `dam-agent.env` on the SD card's boot partition before the
> first start, or run
> `cd /opt/dam-agent && STAGE=prod .venv/bin/python -m agent.enroll <token>`.

### 7.4 Assign it to your Location

Open the device in **Manage → Devices** (it shows *active* once
enrolled), choose your Location and click **Assign**. Within a capture
interval the **latest frame** appears on the device page.

Optional, on the same page:

- **Video window** — all day, fixed hours, or the presets *Daylight
  video (dawn → dusk)* / *Night video (dusk → dawn)* (need coordinates).
- **Boosted dawn / Boosted dusk** — four times more photos around sunrise
  and sunset (the video gets a little longer).

*[Photo 11 — images/11-device-page.png: device page with the first frame]*

---

## 8. Focus, test and seal

1. **Focus**: while the Pi is on your network, open
   `http://<pi-ip>:8080/` in a browser (find the address with
   `hostname -I` on the Pi). Turn the M12 lens slowly until distant
   details are sharpest, then lock it (a drop of glue or the lock ring).
2. **Test for an hour**: frames keep arriving on the device page, the
   temperature stays reasonable, and there are no errors in
   `journalctl -u dam-agent`.
3. **Seal**: silica-gel pack in, lid closed, cable gland tightened,
   silicone on the window edge.

*[Photo 12 — images/12-focus-view.png: the live view used for focusing]*
*[Photo 13 — images/13-sealed-unit.jpg: the fully packaged camera]*

---

## 9. Mount it outdoors

- **Placement**: the lens needs the view, the box does not need the sun —
  under an eave or on a shaded wall is ideal.
- Mount firmly: any wobble shows in the time-lapse.
- Power supply indoors or in a weatherproof spot; cable entering the box
  from below with a drip loop.
- Check the Wi-Fi signal at the final spot before fixing everything.
- Next day, watch your first video on your Location's page.

*[Photo 14 — images/14-mounted.jpg: mounted camera with sunshade]*
*[Photo 15 — images/15-first-video.png: the first daily video]*

---

## 10. Troubleshooting

| Symptom | Likely cause and fix |
| --- | --- |
| `rpicam-hello --list-cameras` shows nothing / device page says **camera not detected** | ribbon cable loose or reversed — power off, re-seat both ends; IMX462: check §5.4 |
| IMX462 shows modes like `2147485568x1080` | camera firmware not updated — §5.4 step 3 |
| **camera off (thermal)** on sunny afternoons | box too hot — white/foil box, second roof, vents (§3); capture resumes by itself when it cools |
| Random restarts, blurry freezes, `vcgencmd get_throttled` not `0x0` | under-voltage — stronger 5 V supply, shorter/thicker USB cable |
| **disconnected** | Wi-Fi out of reach or power lost; the camera keeps photos on the SD card and uploads them when it is back |
| `enrollment refused` in the log | token used, expired (48 h) or cancelled — *Re-issue token* on the device page |
| `not enrolled` in the log | no token set — §7.3 |
| Uploads refused, device says **unassigned** | assign it to a Location — §7.4 |
| Window fogs up | fresh silica gel, drain hole open, vents not blocked |

More help: open an issue in this repository.

---

## 11. Privacy, safety and license

- Point your camera at **landscapes, skylines and public scenery** — not
  into neighbours' windows, gardens or at people. Check the rules for
  cameras where you live.
- Keep your account and the camera's key (`credential.json`) private.
- Mains power belongs to a proper outdoor-rated socket or an indoor
  outlet — not inside the camera box.

> **TODO (operator):** choose the license for this guide (proposal:
> CC BY 4.0 for text and photos).
