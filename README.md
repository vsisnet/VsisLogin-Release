<p align="center">
  <h1 align="center">VsisLogin</h1>
  <p align="center">Anti-Detect Browser Profile Manager — quản lý profile trình duyệt, proxy, giả lập Android & TUN routing cho Game/App</p>
  <p align="center">
    <img src="https://img.shields.io/github/v/release/vsisnet/VsisLogin-Release?label=version&color=blue" alt="Version">
    <img src="https://img.shields.io/badge/platform-Windows%2010%2F11-brightgreen" alt="Platform">
    <img src="https://img.shields.io/badge/.NET-8.0-purple" alt=".NET">
    <img src="https://img.shields.io/badge/license-Proprietary-red" alt="License">
  </p>
</p>

---

## 🇻🇳 Tiếng Việt

### Giới thiệu

**VsisLogin** là phần mềm desktop (Windows) tích hợp 5 module cốt lõi cho công việc multi-account / scaling:

1. **Quản lý Hồ sơ trình duyệt** — tạo profile Chrome (Orbita) với fingerprint riêng biệt cho mỗi profile, không dùng chung cookie/login với Chrome cá nhân.
2. **Quản lý Giả lập Android** — phát hiện LDPlayer / Nox / MEmu / BlueStacks, tự bật ADB, gán proxy theo từng emulator instance qua ADB system-proxy injection.
3. **Quản lý Proxy** — kho proxy HTTP / SOCKS5, kiểm tra live, xác định vị trí IP, làm nguồn proxy cho các module khác.
4. **Quản lý Game / App** *(v4.4.22+)* — route từng `chrome.exe` / `firefox.exe` / game binary qua proxy bằng TUN engine (sing-box + wintun). Dùng cho app/game không hỗ trợ proxy native.
5. **Đồng bộ Cloud** *(mới — v4.5.1+)* — đồng bộ profile (cookie/login), danh sách profile, kho proxy và nhóm lên cloud; dùng chung trên nhiều máy với cùng 1 license.

### Tính năng đầy đủ

#### 🧩 Profile Browser
- Tạo profile không giới hạn với fingerprint riêng biệt: User-Agent, Resolution, CPU, RAM, GPU, WebGL Vendor / Renderer, Canvas, AudioContext, ClientRects, Fonts, Timezone, Language, WebRTC
- Mỗi profile = `--user-data-dir` riêng → cookie / login / lịch sử / extension hoàn toàn cách ly
- Gán proxy HTTP / SOCKS5 (có / không xác thực) cho từng profile
- Nhóm profile (Group) — sắp xếp theo project / khách hàng
- Tạo / xoá / chỉnh sửa / clone / di chuyển nhóm hàng loạt
- Quản lý tiện ích Chrome (extension) — tải từ Chrome Web Store hoặc upload `.crx` local, gán cho từng profile
- Auto-detect khi user đóng browser trực tiếp (X góc cửa sổ) → tự update status DB
- Pagination preserved khi profile flip Run/Stop — không reset về page 1

#### 📱 Quản lý Giả lập Android
- Auto-discovery LDPlayer (v4 / v9 / v64), Nox, MEmu, BlueStacks qua process scan
- Auto-bật `adbDebug = 1` trên mọi LDPlayer instance (kể cả instance vừa tạo qua FileSystemWatcher) — không cần thủ công vào Settings
- Gán proxy theo từng emulator: `adb shell settings put global http_proxy` + broadcast `PROXY_CHANGE` ép app re-read setting
- Hot-apply binding khi engine đang chạy — không cần Stop → Start cycle
- DNS / QUIC / ICMP / IPv6 leak guard (WinDivert) — chặn rò rỉ IP gốc qua các kênh non-HTTP

#### 🌐 Quản lý Proxy
- Hỗ trợ HTTP CONNECT + SOCKS5 (RFC 1928)
- Kiểm tra live (alive / dead / timeout) với concurrent limit 8
- Hiển thị Local Proxy URL — listener `127.0.0.1:<port>` mỗi profile để app khác đọc proxy không xác thực
- Import hàng loạt theo nhiều format: `host:port`, `host:port:user:pass`, `user:pass@host:port`, `socks5://...`
- Filter / search inline

#### 🎮 Quản lý Game / App *(v4.4.22+)*
- Route process Windows qua proxy bằng sing-box TUN inbound (wintun adapter)
- Match theo `process_name` — 1 binding áp cho mọi instance cùng tên process
- Hỗ trợ Chrome / Firefox / Edge / game launcher (Genshin, Valorant launcher, ...) — bất kỳ exe nào trên Windows
- Picker liệt kê process đang chạy theo nhóm + filter system processes
- Block QUIC outbound (forces TCP HTTP/2 fallback → đi qua proxy thay vì lò bỏ qua) + reject IPv6 leak
- "📂 Mở log" button mở `sing-box.log` để debug
- **Yêu cầu chạy as Administrator** lần đầu để install wintun service driver

#### ☁️ Đồng bộ Cloud *(v4.5.1+)*
- Đồng bộ **profile** (cookie / login / Local Storage — toàn bộ trạng thái đăng nhập) lên cloud, mở lại trên máy khác vẫn còn đăng nhập
- Đồng bộ **danh sách profile + cấu hình** (proxy, fingerprint, nhóm) + **kho proxy** + **nhóm** giữa nhiều máy dùng chung 1 license
- Chọn nơi lưu **theo từng profile** hoặc **áp dụng toàn bộ**: Local (chỉ máy này) · VSIS.net Cloud (managed) · Storage riêng (Google Drive / OneDrive / Dropbox / ...)
- **Tự đồng bộ**: kéo bundle về trước khi Run, đẩy lên khi đóng profile / đóng app — không cần thao tác; kèm nút **"Sync ngay"** để đồng bộ hàng loạt thủ công
- **Đa thiết bị an toàn**: khoá (lock) chống mở cùng 1 profile trên 2 máy cùng lúc; badge ☁️ (đã sync) / 🔒 (đang mở ở máy khác) trên danh sách
- **Cách ly & mã hoá**: mỗi license có vùng lưu trữ riêng (credential riêng theo key); bundle mã hoá **AES-256-GCM** trước khi upload (tùy chọn)
- **Hạn mức theo gói**: số profile được đồng bộ cloud theo gói license (hiển thị "đã dùng / tổng" + nút nâng cấp)
- Profile chọn **Local** thì chỉ nằm trên máy đó; dữ liệu giả lập luôn giữ local — không đẩy lên cloud

### Yêu cầu hệ thống

| | Tối thiểu | Đề nghị |
|---|---|---|
| OS | Windows 10 1909 (64-bit) | Windows 11 22H2 hoặc 10 22H2 |
| CPU | Bất kỳ x64 ≥ 2 core | 4+ core (số core ảnh hưởng số profile chạy song song) |
| RAM | 4 GB | 8+ GB (mỗi profile Chrome ăn ~150–300 MB) |
| Disk | 1 GB cho install + ~150 MB cho Data bundle | 5+ GB nếu dùng nhiều profile |
| .NET Runtime | **.NET 8.0 Desktop Runtime** (https://dotnet.microsoft.com/download/dotnet/8.0) | — |
| Quyền | User thường | **Administrator** nếu dùng "Quản lý Game/App" |
| Khác | LDPlayer / Nox / MEmu / BlueStacks (tùy chọn — chỉ cần khi dùng tab Quản lý Giả lập) | — |

### Cài đặt

VsisLogin cung cấp **2 cách cài** — chọn 1 trong 2:

#### Cách 1 — Installer (khuyến nghị)

Phù hợp đa số user — Windows nhận diện app trong "Apps & features", có shortcut Desktop / Start menu, uninstall qua Settings.

1. Tải `VsisLogin-Setup-<version>.exe` ở [Releases mới nhất](https://github.com/vsisnet/VsisLogin-Release/releases/latest)
2. Chạy file `.exe` — chọn Yes ở UAC prompt (cần admin để install wintun service driver cho tab Game/App)
3. Chọn ngôn ngữ (Tiếng Việt / English) → Next → Install
4. Tick "Launch VsisLogin" ở cuối wizard → Finish

#### Cách 2 — Portable Zip

Không cần install, chạy trực tiếp từ folder, tiện copy qua máy khác / USB.

1. Tải `VsisLogin_v<version>.zip` ở [Releases mới nhất](https://github.com/vsisnet/VsisLogin-Release/releases/latest)
2. Giải nén ra folder bất kỳ (tránh `C:\Program Files` vì cần quyền ghi cho proxy-config.json)
3. **Click chuột phải `VsisLogin.exe` → "Run as administrator"** (nếu sẽ dùng tab Quản lý Game/App) → click Yes ở UAC
4. Ở các lần chạy sau: nếu chỉ dùng Profile / Emulator / Proxy thì chạy bình thường, không cần admin

> ⚠️ **Lưu ý về quyền Administrator**
> - Profile / Emulator / Proxy chạy được không cần admin
> - **Tab "Quản lý Game/App" bắt buộc admin** để wintun driver bind TUN adapter; chạy không admin → sing-box vẫn start nhưng proxy không có hiệu lực (IP browser vẫn là IP gốc)
> - Installer (cách 1) elevation tự xử lý lần đầu, sau đó wintun service đã install, nhưng vẫn phải chạy app as admin để Start được TUN engine

### License Key

VsisLogin là phần mềm **bản quyền (proprietary)** — cần license key để đăng nhập và sử dụng.

Có 2 cách lấy license key:

1. **Mua riêng license**
   Liên hệ qua các kênh chính thức:
   - Website: [vsis.net](https://vsis.net)
   - Hỗ trợ kỹ thuật / sale: [vsis.net/contact](https://vsis.net/contact/)

2. **Tặng kèm khi mua proxy tại [vsis.net](https://vsis.net)**
   Mỗi gói proxy mua tại vsis.net được tặng kèm license VsisLogin tương ứng — không cần trả phí riêng cho phần mềm. Hỏi rõ thông tin gói khi đặt mua.

License được kích hoạt theo email + key — đăng nhập 1 lần ở app, key cache lại cho lần khởi động sau. Tính năng auto-update tích hợp sẵn — không cần download manual khi có bản mới.

### Auto Update

VsisLogin có 2 đường update:

- **In-app updater (v4.4.4+)** — kiểm tra `version.json` ở repo này, hiện banner đỏ "Có phiên bản mới" ở góc trên, click 1 phát tự download + replace + restart.
- **Legacy update_tool (v4.3.1 trở xuống)** — đọc `vsislogin.vsis.net/version` qua HTTP, dùng `update_tool.exe` cũ. Server vẫn được sync mỗi release.

### Troubleshooting

| Vấn đề | Nguyên nhân / Fix |
|---|---|
| Bind Firefox/Chrome trong tab Game/App, click Start mà IP không đổi | Chưa run-as-admin → wintun không bind TUN. Thoát app, click chuột phải `VsisLogin.exe` → Run as administrator |
| "TUN Engine: No process bindings configured" | Click vào cột "Proxy profile" của row → ProxyPickerDialog → chọn 1 proxy → OK trước khi Start |
| Browser đã start qua tab Game/App nhưng connection cũ vẫn dùng IP gốc | TUN route chỉ áp cho connection MỚI — restart browser sau khi Start Proxy để mọi connection đi qua proxy |
| Login fail "Invalid license key" | Kiểm tra key + email match với gói đã mua. Liên hệ [vsis.net/contact](https://vsis.net/contact/) nếu key bị reset |
| App crash khi click "Quản lý giả lập" | LDPlayer install path không chuẩn — check log ở `%APPDATA%\VsisLogin\logs\app-*.log` |
| Proxy hiển thị "Dead" nhưng test online thì alive | Proxy chỉ alive với IPv4 / một số region — VsisLogin test qua HTTPS handshake, một số proxy block CONNECT to test server |

### Liên hệ

- 🌐 Website: [vsis.net](https://vsis.net)
- 📚 Docs: [vsislogin.vsis.net/docs](https://vsislogin.vsis.net/docs/)
- 🎫 Support: [vsis.net/contact](https://vsis.net/contact/)
- 🐛 Bug report: GitHub Issues của repo này

---

## 🇬🇧 English

### About

**VsisLogin** is a Windows desktop tool that bundles 5 modules used for multi-account / scaling work:

1. **Browser Profile Manager** — independent Chrome (Orbita) profiles with unique fingerprints; each profile has its own `--user-data-dir` so cookies / logins / extensions are fully isolated.
2. **Android Emulator Manager** — auto-discovers LDPlayer / Nox / MEmu / BlueStacks, auto-enables ADB, assigns a proxy per emulator via ADB system-proxy injection.
3. **Proxy Manager** — HTTP / SOCKS5 proxy inventory with live validation, IP geolocation, used as the proxy source by the other modules.
4. **Game / App Manager** *(v4.4.22+)* — routes individual Windows processes (`chrome.exe`, game binaries, etc.) through a proxy via a sing-box subprocess + wintun TUN adapter. Useful for apps/games that don't expose a proxy setting.
5. **Cloud Sync** *(new — v4.5.1+)* — syncs profiles (cookies/logins), the profile list, proxy inventory and groups to the cloud; use them across multiple machines with one license.

### Full feature list

#### 🧩 Browser Profile
- Unlimited profiles, each with independent fingerprint: User-Agent, Resolution, CPU, RAM, GPU, WebGL Vendor / Renderer, Canvas, AudioContext, ClientRects, Fonts, Timezone, Language, WebRTC
- Per-profile `--user-data-dir` → cookies / logins / history / extensions fully isolated
- Per-profile HTTP / SOCKS5 proxy (with or without auth)
- Profile groups (folders) — organize by project / customer
- Bulk create / delete / edit / clone / move
- Chrome extension manager — download from Chrome Web Store or upload local `.crx`, assign per profile
- Auto-detect when user closes the browser directly (window-X) → DB status auto-syncs
- Pagination preserved on Run/Stop flips — no reset to page 1

#### 📱 Android Emulator Manager
- Auto-discovery of LDPlayer (v4 / v9 / v64), Nox, MEmu, BlueStacks via process scan
- Auto-enables `adbDebug = 1` on every LDPlayer instance (including newly-created ones via FileSystemWatcher) — no manual Settings dive
- Per-emulator proxy: `adb shell settings put global http_proxy` + `PROXY_CHANGE` broadcast to force app re-read
- Hot-apply binding while the engine is running — no Stop → Start cycle needed
- DNS / QUIC / ICMP / IPv6 leak guard (WinDivert) — blocks real-IP leaks via non-HTTP channels

#### 🌐 Proxy Manager
- HTTP CONNECT + SOCKS5 (RFC 1928)
- Live check (alive / dead / timeout) with concurrency 8
- Local Proxy URL display — a `127.0.0.1:<port>` listener per profile that other apps can talk to without auth
- Bulk import in multiple formats: `host:port`, `host:port:user:pass`, `user:pass@host:port`, `socks5://...`
- Inline filter / search

#### 🎮 Game / App Manager *(v4.4.22+)*
- Routes Windows processes through a proxy via sing-box TUN inbound (wintun adapter)
- Matches by `process_name` — one binding covers every instance with the same exe name
- Works for Chrome / Firefox / Edge / game launchers (Genshin, Valorant launcher, …) — any Windows exe
- Process picker lists running processes (grouped) with a system-process filter
- Blocks QUIC outbound (forces TCP HTTP/2 fallback → goes through the proxy) + rejects IPv6 leak
- "📂 Open log" button to inspect `sing-box.log`
- **Requires Administrator** on first start to install the wintun service driver

#### ☁️ Cloud Sync *(v4.5.1+)*
- Sync **profiles** (cookies / logins / Local Storage — the full login state) to the cloud; reopen on another machine still logged in
- Sync the **profile list + config** (proxy, fingerprint, groups) + **proxy inventory** + **groups** across machines sharing one license
- Choose storage **per profile** or **apply to all**: Local (this machine only) · VSIS.net Cloud (managed) · Your own storage (Google Drive / OneDrive / Dropbox / ...)
- **Auto-sync**: pulls the bundle before Run, pushes it on profile close / app close — no manual step; plus a **"Sync now"** button for manual bulk sync
- **Multi-device safe**: a lock prevents opening the same profile on two machines at once; ☁️ (synced) / 🔒 (open elsewhere) badges on the list
- **Isolation & encryption**: each license gets its own storage area (per-key credentials); bundles are **AES-256-GCM** encrypted before upload (optional)
- **Plan quota**: number of cloud-synced profiles per license plan (shows "used / total" + upgrade button)
- Profiles set to **Local** stay on that machine only; emulator data always stays local — never uploaded

### System Requirements

| | Minimum | Recommended |
|---|---|---|
| OS | Windows 10 1909 (64-bit) | Windows 11 22H2 or 10 22H2 |
| CPU | Any x64 ≥ 2 cores | 4+ cores |
| RAM | 4 GB | 8+ GB (each Chrome profile uses ~150–300 MB) |
| Disk | 1 GB for install + ~150 MB for the Data bundle | 5+ GB if using many profiles |
| .NET | **.NET 8.0 Desktop Runtime** (https://dotnet.microsoft.com/download/dotnet/8.0) | — |
| Permissions | Standard user | **Administrator** if using the Game/App tab |

### Installation

VsisLogin ships two install options — pick one:

#### Option 1 — Installer (recommended)

Best for most users — registers under "Apps & features", creates Desktop / Start menu shortcuts, uninstalls via Settings.

1. Download `VsisLogin-Setup-<version>.exe` from the [latest release](https://github.com/vsisnet/VsisLogin-Release/releases/latest)
2. Run the `.exe` — click Yes on the UAC prompt (admin is needed to install the wintun driver for the Game/App tab)
3. Pick a language (Vietnamese / English) → Next → Install
4. Tick "Launch VsisLogin" at the end → Finish

#### Option 2 — Portable Zip

No install, runs straight from a folder, easy to copy across machines / USB.

1. Download `VsisLogin_v<version>.zip` from the [latest release](https://github.com/vsisnet/VsisLogin-Release/releases/latest)
2. Extract anywhere (avoid `C:\Program Files` — the app needs write access for `proxy-config.json`)
3. **Right-click `VsisLogin.exe` → "Run as administrator"** (if you'll use the Game/App tab) → click Yes on UAC
4. Subsequent launches: if only using Profile / Emulator / Proxy, no admin needed

### License Key

VsisLogin is **proprietary** software — a license key is required to log in.

Two ways to get a key:

1. **Buy a standalone license**
   Contact through the official channels:
   - Website: [vsis.net](https://vsis.net)
   - Sales / support: [vsis.net/contact](https://vsis.net/contact/)

2. **Bundled free with proxy purchase at [vsis.net](https://vsis.net)**
   Every proxy package sold on vsis.net includes a matching VsisLogin license at no extra cost — ask about it when ordering.

The key activates per email + key. Logged in once, the key is cached for subsequent launches. Auto-update is built in — no manual download needed.

---

## 📋 Changelog

### v4.6.0

**🇻🇳 Tính năng mới — Proxy giả lập qua WinDivert (không cần ADB) 🛡️**
- Gắn **proxy riêng cho từng giả lập** (HTTP & SOCKS5) bằng **WinDivert** theo tiến trình — bỏ phụ thuộc ADB shell (hết lỗi trùng port / nhận diện chậm)
- Hỗ trợ **LDPlayer · NoxPlayer · MEmu** — tự dò đường dẫn, liệt kê giả lập **đang chạy lẫn đang tắt**
- **Bật / Tắt** giả lập ngay trong app + **Bật/Tắt tất cả** (hoặc theo các giả lập được chọn)
- **Chọn nhiều** giả lập (chọn tất cả) để thao tác hàng loạt
- **Cài đặt giả lập**: bật/tắt loại giả lập, số giả lập mỗi proxy, ẩn/hiện install (vd giả lập trên ổ clone)
- Chống lộ IP thật: DNS qua DoH, chặn UDP/QUIC + IPv6 ra ngoài proxy

**🇬🇧 New feature — Emulator proxy via WinDivert (no ADB) 🛡️**
- Assign a **dedicated proxy per emulator** (HTTP & SOCKS5) with **WinDivert** at the process level — no more ADB shell (no port clashes / slow detection)
- Supports **LDPlayer · NoxPlayer · MEmu** — auto-detect paths, list **running and stopped** instances
- **Start / Stop** emulators in-app + **Start/Stop all** (or only the selected ones)
- **Multi-select** emulators (select all) for bulk actions
- **Emulator settings**: toggle emulator types, emulators per proxy, hide/show installs (e.g. installs on a clone drive)
- Real-IP leak protection: DNS over DoH, block UDP/QUIC + IPv6 outside the proxy

---

### v4.5.1

**🇻🇳 Tính năng mới — Đồng bộ Cloud ☁️**
- Đồng bộ **profile** (cookie / login / trạng thái đăng nhập) lên cloud — mở lại trên máy khác vẫn còn đăng nhập
- Đồng bộ **danh sách profile + cấu hình + kho proxy + nhóm** giữa nhiều máy dùng chung 1 license
- Chọn nơi lưu **theo từng profile** hoặc **toàn bộ**: Local · VSIS.net Cloud · Storage riêng (Google Drive / OneDrive / ...)
- **Tự đồng bộ** khi Run/Stop profile và khi đóng app; thêm nút **"Sync ngay"** để đồng bộ thủ công
- **Đa thiết bị an toàn**: khoá chống mở trùng profile + badge ☁️/🔒; mỗi license có vùng lưu riêng + mã hoá AES-256
- **Hạn mức số profile đồng bộ theo gói license**

**🇬🇧 New feature — Cloud Sync ☁️**
- Sync **profiles** (cookies / logins / login state) to the cloud — reopen on another machine still signed in
- Sync the **profile list + config + proxy inventory + groups** across machines sharing one license
- Storage **per profile** or **for all**: Local · VSIS.net Cloud · Your own storage (Google Drive / OneDrive / ...)
- **Auto-sync** on profile Run/Stop and app close; plus a **"Sync now"** button for manual sync
- **Multi-device safe**: lock against duplicate-open + ☁️/🔒 badges; per-license isolated storage + AES-256 encryption
- **Per-plan cloud-sync profile quota**

---

<p align="center">
  <b>Latest release:</b><br>
  <a href="https://github.com/vsisnet/VsisLogin-Release/releases/latest">⬇️ Download VsisLogin</a>
</p>
