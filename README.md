📦 Scale Agent

Scale Agent is a lightweight background service that reads weight data from a weighing machine via serial port and exposes it as a REST API.

It allows ERP systems (such as Frappe / ERPNext) or any web application to fetch live weight data over the local network.

🚀 Features

Reads weight continuously from serial port

Runs as background service

Exposes HTTP API

Works with Frappe client scripts

No database required

Low resource usage

🖥 Platform Support

✅ Windows (pre-built EXE provided)

🟡 Linux (run from source or build binary manually)

🔌 API Endpoint
http://<HOSTNAME>:5000/read-weight


Response:

{
  "weight": 52.3,
  "connected": true
}

🔹 Windows Installation (Recommended)
1) Download

Download scale_agent.exe from GitHub Releases.

2) Run Once

Double-click the file.

If Windows shows Protected your PC:

More info → Run anyway

3) Auto Start on Boot

Press:

Win + R


Type:

shell:startup


Copy scale_agent.exe into this folder.

Now it will start automatically when Windows boots.

4) Allow Firewall

Allow TCP port 5000 in Windows Firewall.

🔹 Linux Installation
Option A – Run from Source
sudo apt install python3 python3-pip
pip3 install flask flask-cors pyserial
python3 scale_agent.py

Option B – Build Linux Binary
pip3 install pyinstaller
pyinstaller --onefile scale_agent.py
chmod +x dist/scale_agent
./dist/scale_agent

🔹 Find Hostname

Run this on the system where Scale Agent is running.

Windows
hostname

Linux
hostname


Example output:

BALA-PC


Your API URL becomes:

http://BALA-PC:5000/read-weight

🔹 Verify API

Open browser and visit:

http://HOSTNAME:5000/read-weight


If JSON appears, service is working.

🔹 Frappe Client Script Integration

Create Client Script:

Desk → Customization → Client Script → New


DocType: Sale IN OUT

Enabled: ✔

Paste:

frappe.ui.form.on('Sale IN OUT', {
    refresh(frm) {
        cur_frm.add_custom_button("Fetch Weight", () => {
            fetch("http://HOSTNAME:5000/read-weight")
                .then(r => r.json())
                .then(d => {
                    if (cur_frm.doc.entry_type == "Sales IN") {
                        cur_frm.set_value("tare_weight", d.weight);
                    } 
                    else if (cur_frm.doc.entry_type == "Sales OUT") {
                        cur_frm.set_value("gross_weight", d.weight);
                    }
                });
        });
    }
});


Replace:

HOSTNAME


with your actual hostname.

🔹 Common Problems & Fixes
API not reachable

Check Scale Agent is running

Check firewall port 5000

Check both machines are on same network

Weight always 0

Check COM port in code

Verify scale sends data

Windows blocks EXE

Right click → Properties → Unblock

Or More info → Run anyway

📁 Project Structure
scale_agent.py   -> Source code
scale_agent.exe  -> Windows binary (in Releases)

📜 License

MIT License

