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

## ☕ Help My Team

MASS Portal is free and open-source under AGPLv3. If it helps you out and you'd like to support our team, even a **$1** coffee means a lot:
Not a big fan of funding links, honestly—but time is tight. If MASS Portal saves you some hassle and you have a spare buck, it helps keep the project moving.

[[https://img.shields.io/badge/Help%20My%20Team-$1%20Coffee-FFDD00?style=flat-square&logo=buy-me-a-coffee&logoColor=black]]([https://buymeacoffee.com/proffware](https://buymeacoffee.com/proffware?status=1))
