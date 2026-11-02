# Dr.Security v2.0

**Native C# Protection Suite with AI** - System cleaner, process manager, security scanner, plugin system, and AI assistant.

## Features

- **Dashboard** - System info, disk usage, device protection score, quick actions, Dr.Helper diagnostics
- **Process Manager** - Search, view, kill processes with real-time stats
- **System Cleaner** - Clean temp files, browser caches, Windows cache + custom folder cleaning
- **Security Scanner** - Process analysis, startup checks, registry scan, hosts file audit, hash-based malware detection
- **Plugin System** - Steam, Epic Games, Discord management via DLL (detect, launch, close)
- **AI Assistant** - Built-in AI chatbot (5 questions/day via Groq API)
- **Device Protection** - Firewall, UAC, Windows Update, Defender status + protection score
- **VirusTotal Integration** - SHA256 hash checking against known malware database
- **Auto-Update** - Checks GitHub Releases on startup, standalone Dr.Updater.exe
- **Multi-language** - Russian, English, Chinese (Simplified)

## Project Structure

```
Dr.Security/
  Dr.Security.exe       # Main native C# WinForms application
  Dr.Updater.exe        # Auto-updater (checks GitHub releases)
  config.json           # User settings (auto-saved)
  api ai.txt            # Groq API key (not in git)
  assets/
    icon.ico            # Green shield icon
    icon.png
  lib/
    Dr.Helper64.dll     # Diagnostics, error detection/fixing
    Dr.Core.dll         # Scanning, hashing, VT, system checks
    Dr.Crypto.dll       # AES-256 encryption/decryption
    Dr.Plugins.dll      # Plugin system (Steam, Epic, Discord)
  plugins/              # Directory for external plugin DLLs (plugin_*.dll)
  lang/
    ru.json             # Russian translation
    en.json             # English translation
    zh.json             # Chinese (Simplified) translation
  src/
    Dr.Security.App.cs  # Main application source
    Dr.Updater.cs       # Updater source
    Dr.Helper64.cs      # Dr.Helper64.dll source
    Dr.Core.cs          # Dr.Core.dll source
    Dr.Crypto.cs        # Dr.Crypto.dll source
    Dr.Plugins.cs       # Dr.Plugins.dll source
  data/                 # Additional databases
```

## Components

| Component | Purpose |
|-----------|---------|
| **Dr.Security.exe** | Main native WinForms app (7 tabs, dark UI, AI, plugins) |
| **Dr.Updater.exe** | Standalone updater, checks GitHub releases, downloads & installs |
| **Dr.Helper64.dll** | System diagnostics, error codes (1001-3005), auto-fix, memory optimization |
| **Dr.Core.dll** | SHA256/MD5 hashing, VirusTotal API, malware hash DB, startup scan, firewall/UAC/update checks |
| **Dr.Crypto.dll** | AES-256 file/string encryption, script obfuscation, password generation |
| **Dr.Plugins.dll** | IPlugin interface, Steam/Epic/Discord built-in plugins, external plugin loader |

## Building

```powershell
$csc = "C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe"

# DLLs
& $csc /target:library /out:lib\Dr.Helper64.dll /reference:System.Management.dll src\Dr.Helper64.cs
& $csc /target:library /out:lib\Dr.Core.dll src\Dr.Core.cs
& $csc /target:library /out:lib\Dr.Crypto.dll src\Dr.Crypto.cs
& $csc /target:library /out:lib\Dr.Plugins.dll src\Dr.Plugins.cs

# Updater
& $csc /target:winexe /out:Dr.Updater.exe src\Dr.Updater.cs

# Main app
& $csc /target:winexe /out:Dr.Security.exe /reference:lib\Dr.Helper64.dll /reference:lib\Dr.Core.dll /reference:lib\Dr.Crypto.dll /reference:lib\Dr.Plugins.dll /reference:System.Management.dll /win32icon:assets\icon.ico src\Dr.Security.App.cs
```

## Creating Plugins

Create a C# DLL named `plugin_*.dll` implementing `DrSecurity.Plugins.IPlugin`:
- `Name`, `Description`, `Version`, `IconChar` (properties)
- `Detect()`, `GetPath()`, `IsRunning()`, `Launch()`, `Close()`, `GetInfo()` (methods)

Place in the `plugins/` directory - loaded automatically on startup.

## Requirements

- Windows 10/11
- .NET Framework 4.x
