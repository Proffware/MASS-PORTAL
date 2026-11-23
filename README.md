<img width="3168" height="1344" alt="Gemini_Generated_Image_212abq212abq212a" src="https://github.com/user-attachments/assets/60f10a44-b208-47cf-902f-54697e40b8da" />

## MASS Portal

MASS Portal is a self-hosted tool for monitoring and controlling Windows and Linux computers from an Android phone over Wi-Fi or mobile data.

This is a casual side project built out of personal interest to manage multiple PCs and connect them to local AI models without relying on third-party cloud platforms. Because it has direct access to your system and terminal, it is and should always remain open-source.

How It Works

One computer runs the Master service (along with its own local Node), and any other Windows or Linux PCs run the Node service. The Android app connects directly to the Master to monitor and control all registered machines.

Wi-Fi (LAN) & Mobile Data (Remote Access):
At Home (Wi-Fi): Connects directly to the Master PC using its local LAN IP (e.g., 192.168.1.x:6277).
Outside (Mobile Data / 4G / 5G): The Android app stores both a Local (Wi-Fi) IP and a Remote (Mobile Data) IP (such as a Tailscale/WireGuard VPN IP, static public IP, or port-forwarded address). You can switch between Wi-Fi and Mobile Data modes manually or let the app select the connection automatically based on your network.
Authentication & Encryption:
Master ↔ Node: Uses mutual TLS (mTLS) with shared local certificates (certs/).
Phone ↔ Master: Uses HTTPS (:6277). On first pairing, the Android app registers an ECDSA P-256 public key stored in the Android Keystore and signs every request with a timestamp, single-use nonce, and SHA-256 body hash—keeping the connection verified even when accessed remotely over mobile data.
External AI Support (SI via Ollama):
MASS Portal does not include a built-in AI model. Instead, it connects to external models running on your machine through Ollama (127.0.0.1:11434).
You can send text prompts from the Android app (such as "Tile open windows", "Show top RAM processes", "List files on Desktop", or "Launch Notepad") to run actions on the selected PC from anywhere.
Reasoning View (<think>): Tapping a message bubble in the Android chat reveals the model's reasoning steps and the tool parameters it used.
Access Modes:
Restricted Mode (Default): Actions requested by the AI wait for manual Approve / Reject confirmation on your phone before running.
Full Access Mode: The AI runs requested actions directly without asking for confirmation each time.
PC Control Features (C++20):
Windows: List, focus, minimize, maximize, restore, close, tile, or cascade open windows (Win32 API on Windows; X11, Hyprland, and Sway on Linux).
Processes & Files: View or stop running processes, start applications, and list, read, write, move, or delete files.
Remote Desktop & Shell: View the desktop screen, send mouse/keyboard input, and run commands in a persistent PowerShell or bash session.
EUS (Easy Update Script):
Single-file .bat and .sh update scripts that back up existing files, apply source changes, compile the project, run unit tests, and revert to the backup if the build or tests fail. Existing certificates (certs/) and the SQLite database (mass_state.db) are left untouched.
Structure
text
[ Android App (Wi-Fi LAN or Mobile Data / VPN / WAN) ]
       |
       | HTTPS + ECDSA Request Signing (:6277)
       v
[ Master Server ] <---> [ Ollama (:11434) ] (External AI Models)
       |
       +--- mTLS (:8444/:8445) ---> [ Node #1 (Main PC) ]
       |
       +--- mTLS (:8444/:8445) ---> [ Node #2..N (Other PCs) ]
Setup & Update
1. Main PC (Master + Node)
Windows: start_mass_v3.bat
Linux: ./start_mass_v3.bash (Prints both the local Wi-Fi IP and detected Remote/WAN IP on startup so you can enter them in the Android app.)
2. Additional PCs (Node Only)
Windows: setup_mass_node_v3.bat (enter the Main PC's IP when prompted)
Linux: ./setup_mass_node_v3.bash (enter the Main PC's IP when prompted)
3. Updating (v3.4.0)
Windows: MASS_EUS_v3.4.0.bat
Linux: ./MASS_EUS_v3.4.0.sh

## Why Open-Source?

A program that can run shell commands, read files, capture the screen, and let an AI model trigger OS actions on your computers—especially one accessible remotely over mobile data—should not be closed-source.

MASS Portal was written as an experimental project for personal use. Keeping the C++20 backend and the Kotlin / Jetpack Compose Android app open-source allows anyone to read the code, verify what runs on their machine, and modify it as they see fit.

## ☕ Help My Team — Buy Them a Turkish Coffee ($2)


<img width="418" height="177" alt="image" src="https://github.com/user-attachments/assets/d493dfbe-4d35-4dc4-acdf-b764c9f44c2f" />

I admit I’m not a fan of asking for donations, but time isn’t free either. MASS Portal stays free and open-source under AGPLv3 either way—if it saves you some hassle and you’ve got a couple of bucks lying around, feel free to buy our team a real Turkish coffee from one of the most popular coffee chains here in Türkiye (Kahve Dünyası):

| Menu Item | Price (TRY) | Approx. USD *(Nov 23, 2026)* |
| :--- | :---: | :---: |
| Türk Kahvesi (80 ml) | 125 TL | ~$2.47 |
| Türk Kahvesi Double (160 ml) | 140 TL | ~$2.77 |
| Sütlü Türk Kahvesi (80 ml) | 130 TL | ~$2.57 |
| Sütlü Türk Kahvesi Double (160 ml) | 145 TL | ~$2.87 |

Or honestly, **just drop a star on the repo—we don’t care about money :)**, a star is more than enough!
[https://img.shields.io/badge/BUY%20US%20A%20TURKISH%20COFFEE-$2-C68B59?style=for-the-badge&logo=github-sponsors&logoColor=F4E8D8&labelColor=15181D]]

(https://github.com/sponsors/Proffware)

[https://img.shields.io/badge/OR%20JUST-DROP%20A%20STAR-6BA3F7?style=for-the-badge&logo=github&logoColor=E4E7EC&labelColor=15181D]


