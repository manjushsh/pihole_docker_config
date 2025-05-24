
---

## 🧱 Your Devices:

* 🧠 **Router 1**: The main router (connected to the internet)
* 🧠 **Router 2**: The second router (currently in router mode)
* 🍓 **Pi-hole**: Your ad-blocking superhero (on a Raspberry Pi, connected to Router 1)

---

## 🎯 The Goal:

* Make all devices on both routers use **Pi-hole** to block ads.
* Make **Router 2 just extend the Wi-Fi** and not create its own separate network.
* Still be able to log in to both routers' dashboards.

---

## ✅ What You’ll Do:

### 1. **Log in to Router 2**

* Open a browser and type its IP (probably `192.168.0.1` or `192.168.1.1`)
* Login with the username/password (check the label under it if unsure)

---

### 2. **Set a Static IP for Router 2**

This makes sure you can always access Router 2’s settings later.

* Go to **LAN settings** or **Network > LAN**
* Change the **IP Address** of Router 2 to something like:
  👉 `192.168.1.2` (same first 3 numbers as Router 1 but not the same IP)
* **Save but DO NOT reboot yet**

---

### 3. **Turn Off DHCP on Router 2**

You don’t want it to give out IP addresses — Router 1 will handle that.

* Go to **DHCP settings**
* Disable or turn off **DHCP server**
* **Save** (again, don’t reboot yet)

---

### 4. **Change Operation Mode to Access Point (AP)**

* Go to **Operation Mode** or **System Mode**
* Select **Access Point Mode**
* Confirm and **now reboot** the router

💡 It might reboot and change its IP to the one you set in step 2 (e.g., `192.168.1.2`)

---

### 5. **Reconnect Everything Properly**

* **LAN port from Router 1 → LAN port on Router 2**

  * (⚠️ NOT WAN port anymore — AP mode uses LAN-to-LAN)
* Pi-hole stays connected to Router 1's LAN port

---

### 6. **Set Router 1 to Use Pi-hole for DNS**

* Log in to Router 1
* Go to **DHCP Settings**
* Find where it says **DNS server** and type your **Pi-hole’s IP**
  👉 e.g., `192.168.1.100`
* Save & reboot Router 1

---

## 🧪 Test It’s Working

1. Connect your phone or computer to Wi-Fi from **Router 2**
2. Go to [pi.hole/admin](http://pi.hole/admin) or check the Pi-hole dashboard
3. You should see your device show up and DNS queries getting blocked!

---

## 🚀 Summary:

| Step | Action                                           |
| ---- | ------------------------------------------------ |
| 1    | Set Router 2 to static IP like `192.168.1.2`     |
| 2    | Turn off DHCP on Router 2                        |
| 3    | Switch Router 2 to **Access Point** mode         |
| 4    | Connect LAN-to-LAN (Router 1 LAN → Router 2 LAN) |
| 5    | Tell Router 1 to use Pi-hole as DNS              |
| 6    | Enjoy ad-free internet everywhere 🎉             |

---
