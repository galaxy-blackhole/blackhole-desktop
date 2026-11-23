<!--
 author Bùi Trọng Hiếu
 email kevinbui210191@gmail.com
 create date 23/11/2026
 modify date 23/11/2026
 desc Nơi phát hành app desktop Galaxy Blackhole cho macOS, Windows và Linux — MỘT link duy nhất.
-->

# Galaxy Blackhole Desktop

App desktop của Galaxy Blackhole, đóng gói sẵn runtime Node + host harness nên mở là chạy, không cần
cài node/bun/repo. **Tải ở [Releases](https://github.com/galaxy-blackhole/blackhole-desktop/releases)**.

| Nền | Tệp | Ghi chú |
|---|---|---|
| macOS (Apple Silicon + Intel) | `Galaxy-Blackhole-<version>-macOS-arm64.zip` / `-x64.zip` | Chưa ký bằng chứng chỉ Apple: mở lần đầu cần chuột phải → **Open**, hoặc `xattr -dr com.apple.quarantine "/Applications/Galaxy Blackhole.app"` |
| Windows x64 | `Galaxy-Blackhole-<version>-Windows-x64-setup.exe` | Bộ cài NSIS, cài theo user, có shortcut |
| Linux x64 | `Galaxy-Blackhole-<version>-Linux-x64.deb` | `sudo apt install ./Galaxy-Blackhole-<version>-Linux-x64.deb` |

Version app = version của CLI `@galaxy-stack/blackhole-cli` trên npm tại thời điểm phát hành.

## Tệp này KHÔNG chứa mã nguồn

Repo này chỉ chứa workflow đóng gói. Mã vỏ Tauri nằm ở
[`galaxy-blackhole/blackhole-harness`](https://github.com/galaxy-blackhole/blackhole-harness)
(`apps/desktop` cho macOS, `apps/desktop-windows` cho Windows + Linux); host harness đóng vào app
lấy từ npm theo đúng version đã publish.

## Phát hành

```bash
gh workflow run release-desktop.yml -f version=3.0.34 -f harness_version=0.1.18
```

Workflow tự: chốt version → dựng 3 nền → chạy thử host **trong gói** (in `?token=`, `curl` đòi HTTP 200)
→ đăng mọi tệp lên release `desktop-v<version>`.

macOS để `continue-on-error`: tài khoản chưa có runner macOS (GitHub trả
`The configuration 'macos-14-arm64-us-default' is not supported`), nên bản macOS dựng trên máy dev
rồi tải lên cùng release. Khi có runner macOS, bỏ dòng `continue-on-error` là CI dựng được cả 3 nền.
