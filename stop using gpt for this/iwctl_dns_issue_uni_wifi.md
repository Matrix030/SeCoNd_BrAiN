ss## 📝 Fixing “DNS lookup failed on ::1:53” in Arch (quick run-down)

1. **Enable systemd-resolved** (DNS service in systemd):

   ```bash
   sudo systemctl enable --now systemd-resolved
   ```

2. **Point `/etc/resolv.conf` to resolved’s stub file** (so apps use it):

   ```bash
   sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
   ```

3. **Find your Wi-Fi interface name** (usually `wlan0`, `wlp2s0`, etc.):

   ```bash
   ip link show
   ```

4. **Set working DNS servers on that interface**:

   ```bash
   sudo resolvectl dns <iface> 1.1.1.1 8.8.8.8
   sudo resolvectl domain <iface> "~."
   sudo resolvectl flush-caches
   ```

5. **Verify DNS is working**:

   ```bash
   resolvectl status
   cat /etc/resolv.conf
   getent hosts proxy.golang.org
   ```

6. **Retry your Go command**:

   ```bash
   go install github.com/spf13/cobra/cobra@latest
   ```

### Why this works

* Before, `/etc/resolv.conf` was an **empty file from NetworkManager**, but NM wasn’t running → so no DNS.
* By enabling `systemd-resolved` and symlinking `/etc/resolv.conf`, DNS requests go through resolved.
* By explicitly setting DNS servers (Cloudflare + Google), you bypass campus/local misconfigs.




That’s the full fix cycle. Next time you hit this problem, just follow **steps 1–4**, then verify.

Do you want me to write this out as a **tiny script** (`fix-dns.sh`) you can run whenever DNS breaks?



Perfect — since you’re on Arch with Hyprland, you can also do this with **`iwd`** (iNet wireless daemon). `iwd` can handle WPA2/WPA3-Enterprise (802.1X) directly, but the setup is a little more manual than with NetworkManager.

Here’s the step-by-step:

---

## 1. Install and enable `iwd`

```bash
sudo pacman -S iwd
sudo systemctl enable --now iwd
```

---

## 2. Start the interactive shell

```bash
iwctl
```

Inside `iwctl`, check your device:

```bash
device list
```

Say it’s `wlan0`.

---

## 3. Scan for networks

```bash
station wlan0 scan
station wlan0 get-networks
```

Find your university SSID (e.g. `eduroam`).

---

## 4. Create a config file for 802.1X

Enterprise Wi-Fi requires an `iwd` **network profile**. These live in `/var/lib/iwd/`.

Create a file:

```bash
sudo nvim /var/lib/iwd/eduroam.8021x
```

Put this in (adjust `EAP-Method`, `Phase2-Method`, and your identity):

```ini
[Security]
EAP-Method=PEAP
EAP-Identity=your_netid@university.edu
EAP-PEAP-Phase2-Method=MSCHAPV2
Passphrase=yourpassword
```

Some universities use TTLS/PAP instead:

```ini
[Security]
EAP-Method=TTLS
EAP-Identity=your_netid@university.edu
EAP-TTLS-Phase2-Method=PAP
Passphrase=yourpassword
```

If your school requires a CA certificate, add:

```ini
EAP-PEAP-CACert=/etc/ssl/certs/your_university_ca.pem
```

---

## 5. Connect

Back in `iwctl`:

```bash
station wlan0 connect eduroam
```

If the profile is written correctly, `iwd` will use it and authenticate.

---

✅ After this, `iwd` will auto-connect to that SSID in the future.

---

👉 Do you know if your university Wi-Fi is **eduroam**, and do they require uploading a CA certificate, or is it just username + password? That determines whether we can keep it simple (just PEAP/MSCHAPv2) or whether we need to fetch certs.