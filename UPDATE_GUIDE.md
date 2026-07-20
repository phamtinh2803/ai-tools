# Hướng dẫn update version pt.Studio

## Files cần sửa (tăng version, ví dụ 1.0.3 → 1.0.4)

| File | Chỗ cần sửa |
|------|-------------|
| `app.py` | `CURRENT_VERSION = "1.0.4"` (~dòng 96) |
| `app.py` | `UPDATE_URL` giữ nguyên, KHÔNG sửa |
| `version_info.txt` | 4 chỗ: `filevers`, `prodvers`, `FileVersion`, `ProductVersion` |
| `setup.iss` | `#define AppVersion "1.0.4"` |
| `requirements.txt` | Comment đầu file `# pt.Studio v1.0.4` |
| `README.md` | `# pt.Studio v1.0.4` |

## Thứ tự thực hiện

1. **Sửa version** ở tất cả file trên
2. **Build installer** bằng `setup.bat`
3. **Upload installer lên Google Drive** → lấy file ID từ link share
4. **Update `update.json` trên GitHub** (repo `phamtinh2803/ai-tools`):

```bash
git clone https://github.com/phamtinh2803/ai-tools.git /tmp/ai-tools
cat > /tmp/ai-tools/update.json << 'EOF'
{
  "version": "1.0.4",
  "download_url": "https://drive.google.com/uc?export=download&id=FILE_ID_MOI",
  "changelog": "Mô tả ngắn gọn",
  "sha256": ""
}
EOF
cd /tmp/ai-tools
git config user.name "phamtinh2803"
git config user.email "phamtinh2803@users.noreply.github.com"
git add update.json && git commit -m "Update to v1.0.4" && git push
```

5. **Verify**: `curl -s https://raw.githubusercontent.com/phamtinh2803/ai-tools/main/update.json`

## Lưu ý
- `UPDATE_URL` trong app.py phải luôn trỏ raw URL GitHub, KHÔNG đổi
- GitHub repo: `phamtinh2803/ai-tools`, file `update.json` trên nhánh `main`
- Git config: `user.name="phamtinh2803"`, `user.email="phamtinh2803@users.noreply.github.com"`
- User từ v1.0.3 trở đi mới tự update được
