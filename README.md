# AMSIBypass.dll

**AMSIBypass.dll** is a Windows x64 dynamic-link library (DLL) that disables the AMSI (Antimalware Scan Interface) in-memory for the current process by patching the `AmsiScanBuffer` function. This is commonly used to allow unrestricted script execution in environments where Defender or AMSI-integrated AV solutions inspect runtime buffers (e.g., PowerShell, C# in-memory execution, etc).

This project is for educational and research purposes, such as evaluating endpoint visibility, testing security software, or understanding runtime patching in Windows processes.

---

## ⚙️ Features

- Patches `amsi.dll!AmsiScanBuffer` at runtime
- Sets `eax = 0; ret` to force AMSI to always return "clean"
- Auto-executes via `DllMain()` on `DLL_PROCESS_ATTACH`
- Built in C++ (Visual Studio 2022)
- x64-only (Windows 10/11)

---

## 🧪 Usage

**1. Compile**

Open in **Visual Studio 2022**:
- Build as a DLL (Release/x64 recommended)
- Output: `AMSIBypass.dll`

**2. Inject**

Inject into a target process using any standard method (e.g., `CreateRemoteThread`, `rundll32`, or a custom loader):

```cpp
CreateRemoteThread(hProc, 0, 0, (LPTHREAD_START_ROUTINE)LoadLibraryA, (LPVOID)"C:\Path\AMSIBypass.dll", 0, 0);
```

Once loaded, the patch will immediately disable AMSI scanning in the host process.

---

## ✅ Confirmed Working In

- `powershell.exe`
- `pwsh.exe`
- `wscript.exe`
- .NET apps executing dynamic code

---

## 🛑 Limitations

- Only affects **current process**; injection must be repeated for each new instance.
- Will not survive reboot or restart of the host process.
- May need updating if Windows internals change (function layout).

---

## 📁 File Structure

```
/AMSIBypass
├── amsi_bypass.cpp
├── AMSIBypass.vcxproj
├── AMSIBypass.sln
└── README.md
```

---

## 📜 License

This project is provided **as-is** for private use, testing, and research.

> You are free to use this software for personal or internal testing purposes.  
> Redistribution, repackaging, modification, or commercial use is **not permitted**.  
> This includes uploading altered versions or including it in third-party tools.

If you need broader usage rights, contact the original author.

---

## ⚠️ Disclaimer

This project is intended for legitimate security testing, red team simulation, or education. Misuse of this tool outside those contexts may violate terms of service, software agreements, or laws. The author takes no responsibility for unintended or malicious use.

---

## 📬 Contact

Issues, pull requests, or questions? Open one on GitHub or reach out via the listed email/contact in the repo.
