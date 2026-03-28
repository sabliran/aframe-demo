# A-Frame Demo — Local & Network Setup

## Run locally (same machine only)

- [ ] Run `npm start`
- [ ] Open `https://localhost:3000` in browser
- [ ] Accept the self-signed certificate warning ("Advanced" → "Proceed anyway")

---

## Run on local network (other devices)

### First time setup

- [ ] Find your machine's local IP:
  ```bash
  ip a | grep "inet " | grep -v 127.0.0.1
  ```
- [ ] Generate a self-signed SSL certificate (valid 365 days):
  ```bash
  openssl req -x509 -newkey rsa:2048 \
    -keyout key.pem -out cert.pem \
    -days 365 -nodes \
    -subj "/CN=localhost" \
    -addext "subjectAltName=IP:<YOUR_LOCAL_IP>,DNS:localhost"
  ```
  Replace `<YOUR_LOCAL_IP>` with your actual IP (e.g. `192.168.1.10`)

- [ ] Make sure `package.json` start script includes SSL flags:
  ```json
  "start": "npx serve . -l 3000 --ssl-cert cert.pem --ssl-key key.pem"
  ```

- [ ] Allow port 3000 through firewall:
  ```bash
  sudo ufw allow 3000
  ```

### Every time

- [ ] Run `npm start`
- [ ] On each other device, open `https://<YOUR_LOCAL_IP>:3000`
- [ ] Accept the certificate warning on each device once

---

## Certificate expired?

Re-run the `openssl` command above to generate a new `cert.pem` and `key.pem`, then restart the server.

---

## Notes

- `localhost:3000` only works on the host machine
- Other devices must use the LAN IP (e.g. `192.168.1.10:3000`)
- HTTPS is required for WebXR / VR features to work in the browser
- Certificate warnings are normal for self-signed certs on a local network
