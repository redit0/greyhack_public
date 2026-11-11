# Grey Hack Public Scripts by Redit0

Various scripts and resources for the hacking simulation game [Grey Hack](https://store.steampowered.com/app/605230/Grey_Hack/). Everything here is GreyScript (`.src`), the game's in-world scripting language. Build each file with the in-game code editor or `build` command and run the resulting binary from a terminal.

## Script Inventory

| Script | Purpose |
| --- | --- |
| [defaults.src](defaults.src) | Compiles the game's default source code for common binaries into `/bin`. |
| [find.src](find.src) | Reusable `find()` function for locating files and folders by pattern and permission flags. |
| [instant_rshell.src](instant_rshell.src) | Hunts random hosts, roots them, and stands up a reverse shell (rshell) server automatically. |
| [nmap.src](nmap.src) | Expanded nmap replacement with WHOIS, firewall rules, and recursive LAN port mapping. |
| [nomad.src](nomad.src) | Rotates the host through random known Wi-Fi networks on a timed interval (use with xwifi.src). |
| [overflow.src](overflow.src) | Scans a target's library for vulnerabilities and runs every overflow it finds. |
| [ranip.src](ranip.src) | Generates a random, routable public IP address and prints it. |
| [rollingpin.src](rollingpin.src) | Scans and mass-exploits library vulnerabilities to trigger new library versions. |
| [sitefinder.src](sitefinder.src) | Searches random hosts for a given site type (shop, bank, mail, hackshop) by port signature. |
| [xwifi.src](xwifi.src) | Automatically cracks every nearby Wi-Fi network and saves the passwords to `wifi.txt`(compatible with nomad.src). |

## Script Details

### defaults.src
Compiles the game's built-in default source for a chosen binary and places it in `/bin`. Run with no arguments for an interactive menu, or pass a program name directly. Supported programs include `nmap`, `chat-server`, `ftp-server`, `http-server`, `repository-server`, `smtp-user-list`, `ssh-server`, and `wallet`. Prompts before overwriting an existing binary.

### find.src
A library-style `find()` function (not a standalone command) that recursively searches a starting file or folder for names matching a pattern, with `*` as a wildcard. Flags filter the results: `b` binary files, `f` folders only, `t` text files, `r` readable, `w` writable, `x` executable, and `l` include symlinks. Copy this function into other scripts that need file searching.

### instant_rshell.src
End-to-end automation that picks random IP addresses, scans each with metaxploit, and overflows vulnerabilities until it lands a root shell. Once rooted, it copies `librshell.so` and `metaxploit.so` to the target, installs and starts the rshell service, then loops as a server, launching a terminal for every client that connects back. Requires `librshell.so` at `/root/SecureLibs/` and `metaxploit.so` at `/lib/`.

### nmap.src
A fuller alternative to the default nmap. Accepts an IP or URL (resolving `www` hosts via nslookup), prints WHOIS details, ESSID/BSSID, and firewall rules, then maps open, closed, internal, and routed ports. With `FullScan` enabled it recursively walks routers and switches to enumerate the entire LAN, and it groups results by network class.

### nomad.src
Keeps the host moving between Wi-Fi networks. It reads known networks from a readable `wifi.txt`, then connects to a random one every interval (default 60 seconds, or pass a custom interval in seconds), never repeating the network it is currently on. Requires an active WIFI network card. Pairs well with the output of `xwifi.src`.

### overflow.src
A hands-on exploitation helper. Given an IP and port (and an optional password or LAN address), it connects with metaxploit, dumps the target library, scans every address for vulnerabilities, and runs each overflow it discovers, reporting the type of object returned. The result object is left in a variable so you can extend the script to act on the shell, computer, or file it yields.

### ranip.src
A small utility that loops until it finds a reachable router at a randomly generated IPv4 address, skipping reserved and non-routable ranges, then returns and prints that router's public IP. Useful on its own or as a drop-in function for other scripts.

### rollingpin.src
A GBI tool (written by redit0) that scans a selected local library for vulnerabilities and repeatedly overflows a chosen exploit to pressure the repository into releasing a new version. It logs activity to `/root/rollingpin.log`, manages its own temporary working folder, auto-upgrades installed libraries via aptclient, and must be run as root. Fully commented with per-function documentation.

### sitefinder.src
Searches for a website of a chosen type by scanning random public hosts for characteristic open ports: `hackshop` (1542 + 80), `bank` (141 + 80), `mail` (25 + 80), or `shop` (80 only). On a match it opens the site in the in-game browser and shows the URL and IP so you can confirm before stopping or retrying.

### xwifi.src
Automated Wi-Fi cracking. It starts monitor mode on `wlan0`, loads previous results from `wifi.txt`, drops stale networks, then works through every in-range network strongest-first, running aireplay/aircrack and skipping ones already cracked or too weak. Cracked credentials are written back to `wifi.txt` in a formatted table. Requires `crypto.so` and an active WIFI network card.
