## Prerequisites

Before running the playbook, ensure the following are in place:

**Control machine (where you run Ansible):**
- Ansible 2.9 or later installed
- SSH access to the target host(s)
- Sudo/root privileges on the target host(s)

**Files you must supply:**
- `config.ovpn` — Your OpenVPN client configuration file (`.ovpn`), placed in the same directory as the playbook
- Your VPN account username and password (entered interactively at runtime)

**Target host requirements:**
- Ubuntu or Debian-based Linux
- Internet access to download packages via `apt`
- Python 3 installed (required by Ansible)

---

## Directory Structure

Place both files in the same directory before running:

```
your-project/
├── openvpn.yml      ← The Ansible playbook
└── config.ovpn         ← Your OpenVPN client config file
```

---

## Configuration

Before running, open `openvpn.yml` and update the two variables under the `vars` section:

| Variable             | Default         | Description                                                                                              |
| -------------------- | --------------- | -------------------------------------------------------------------------------------------------------- |
| `ssh_bypass_network` | `8.8.8.0`       | The network address of your SSH client. Traffic to this network will bypass the VPN, preventing lockout. |
| `ssh_bypass_netmask` | `255.255.255.0` | The subnet mask for the bypass network (`/24` in the default example).                                   |

**Example:** If you are SSHing in from IP `192.168.1.50`, set:
```yaml
ssh_bypass_network: "192.168.1.0"
ssh_bypass_netmask: "255.255.255.0"
```

> ⚠️ **Critical:** If you skip this step and the SSH client network is not excluded from VPN routing, your SSH session may drop as soon as the VPN connects and you could lose access to the host.

---

## File Locations on the Target Host

| File                  | Path                       |
| --------------------- | -------------------------- |
| OpenVPN client config | `/etc/openvpn/client.conf` |
| VPN credentials       | `/etc/openvpn/auth.txt`    |

Both files are created with `0600` permissions, readable only by root.

---

## Verifying the VPN Connection

After the playbook completes, SSH into your host and check the service status:

```bash
sudo systemctl status openvpn@client
```

To check that the VPN tunnel interface is up:

```bash
ip addr show tun0
```

To verify your public IP has changed:

```bash
curl icanhazip.com
```

---

## Troubleshooting

**Service fails to start:**
Check OpenVPN logs for errors:
```bash
sudo journalctl -u openvpn@client -n 50
```

---

## Security Notes

- The auth file at `/etc/openvpn/auth.txt` contains your VPN password in plaintext. It is protected by `0600` permissions (root-only), but be mindful of this when operating in shared environments.