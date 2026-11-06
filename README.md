# Elysium-V2-Trojan-Proof

File : Elysium V2.exe + DL_SCEWIN.exe  (fileport.io RAR v5, 11 MB)
Elysium V2 : PE32 x86 .NET GUI, unsigned, ConfuserEx obfuscated
DL_SCEWIN  : PE32+ x86-64 CUI, PyInstaller Python 3.12, unsigned
MD5 Elys : 397838937383299db82816ea4b0f242b
SHA256 Elys: 4dca0e8da171e5d7d02d1c210c90ab43c4f42333276ba4e3aa7dc6517c6ad326
MD5 DLSCE: 445a4c7c7b2e18875e341eff378c5705
SHA256 DLSCE: 1664c92a10909a2c137ed33b05973cf4986efab96a7bd3e6004e8dfe80815292

WHAT IT IS
Elysium V2.exe = MALWARE. .NET loader obfuscated (ConfuserEx v1.0.0), built on KeyAuth C# WinForm template. Fake Fortnite / gaming tweak wrapper. Steals HWID, tokens, passwords, cookies. Exfiltrates via Discord webhook. Bypasses SSL cert validation.
DL_SCEWIN.exe = LEGIT TOOL (SCEWIN BIOS modder from GitHub ab3lkaizen/SCEHUB, g0sheto/SCEWIN). Included as lure. Not the malware.

KEY SECTIONS
Elysium:
  big blob ent 7.9998 rs 887296  (ConfuserEx encrypted code)
  .text ent 5.24  rs 118272
  .rsrc ent 4.20  rs 1536
DL_SCEWIN:
  .text ent 6.50  rs 176640
  .rsrc ent 7.35  rs 61440  (PyInstaller archive overlay)
  .data ent 1.83
No Rich Header, no signature, checksum 0 on both.

KEY IMPORTS
Elysium (.NET): mscoree.dll _CorExeMain only. Uses WebClient, HttpWebRequest, Newtonsoft.Json (JToken).
DL_SCEWIN:
  USER32 (14) : windowing, message loop
  KERNEL32 (107): CreateProcessW, LoadLibraryExW, GetProcAddress, DeleteFileW, filesystem
  ADVAPI32 (4): OpenProcessToken, GetTokenInformation, ConvertSidToStringSidW -> token/SID theft

NETWORK / IOCs
URLs :
  keyauth.win / keyauth.cc  <- KeyAuth license API (C2 / license check)
  youtu.be/qWtZv82YftI  <- legit YouTube lure (monitor overclocking)
bstlar.com / discord.gg NOT in this file (was PopCornFN).
IPs : none real in statice (URLs obfuscated / server-side via KeyAuth).
Domains : keyauth.win, keyauth.cc

BEHAVIOR
1. Elysium V2 runs, ConfuserEx deobfuscates at runtime.
2. KeyAuth init(): checks license on keyauth.win before unlocking payload.
3. Steals HWID (get_hwid/set_hwid), tokens (tokenPath, TokenHash), passwords (passwordField).
4. Exfiltrates via webhook (sendWebhookBtn_Click_1, webhookBaseURL, webhookID) -> Discord webhook likely.
5. Bypasses cert validation (RemoteCertificateValidationCallback, ServerCertificateValidationCallback) -> MITM possible.
6. DL_SCEWIN drops innoextract.exe, extracts MSI Center from download.msi.com, reads cookies (http.cookiejar, http.cookies, requests.cookies), steals tokens via OpenProcessToken/GetTokenInformation.
7. Creates temp files in %TEMP%, drops Python runtime modules (PyInstaller overlay).

IOC LIST
Hashes :
  Elysium SHA256 4dca0e8da171e5d7d02d1c210c90ab43c4f42333276ba4e3aa7dc6517c6ad326
  Elysium MD5   397838937383299db82816ea4b0f242b
  DL_SCEWIN SHA256 1664c92a10909a2c137ed33b05973cf4986efab96a7bd3e6004e8dfe80815292
Domains : keyauth.win, keyauth.cc
Files   : temp MSI Center files, innoextract.exe
Reg     : HKLM\...\CurrentVersion\Internet Settings (proxy, from PopCornFN pattern)
Strings : KeyAuthApp, enckey, iv_key, numKeys, webhook, webhookBaseURL, webhookID, hwid, tokenPath, TokenHash, passwordField, RemoteCertificateValidationCallback

VERDICT
Class : Trojan / stealer / loader (fake gaming tweak, paid cheat wrapper)
Risk  : HIGH
Reasons : unsigned, packed/obfuscated (ent ~8.0), anti-analysis (ConfuserEx), KeyAuth license gate, HWID/token/password/cookie theft, webhook exfil, SSL cert bypass, spread via fileport.io warez + gaming tweak camouflage (BIOS tweaks, power plans, screenshots).
