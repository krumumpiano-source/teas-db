# Frontend Configuration

## ⚠️ สำคัญ: ต้องตั้งค่า API URL ก่อน Deploy

ก่อนที่จะ deploy frontend ไปยัง GitHub Pages หรือ static hosting ใดๆ  
**ต้องแทนที่ `YOUR_GAS_WEB_APP_URL` ในทุกไฟล์ HTML**

### ไฟล์ที่ต้องแก้ไข (8 ไฟล์):

1. `index.html`
2. `upload.html`
3. `search.html`
4. `checker.html`
5. `sar.html`
6. `view.html`
7. `download.html`
8. `about.html`

## วิธีแก้ไข

### วิธีที่ 1: ใช้ Helper Script (แนะนำ)

#### Windows (PowerShell):
```powershell
cd frontend
.\update-api-url.ps1 -ApiUrl "https://script.google.com/macros/s/YOUR_ID/exec"
```

#### macOS/Linux (Bash):
```bash
cd frontend
chmod +x update-api-url.sh
./update-api-url.sh "https://script.google.com/macros/s/YOUR_ID/exec"
```

### วิธีที่ 2: แก้ไขด้วยมือ

1. Deploy GAS Backend และได้ Web App URL เช่น:
   ```
   https://script.google.com/macros/s/AKfycbxxxx/exec
   ```

2. เปิดไฟล์ HTML แต่ละไฟล์

3. หาบรรทัดนี้:
   ```javascript
   window.API_BASE_URL = 'YOUR_GAS_WEB_APP_URL';
   ```

4. แทนที่ด้วย URL จริง:
   ```javascript
   window.API_BASE_URL = 'https://script.google.com/macros/s/AKfycbxxxx/exec';
   ```

### วิธีที่ 3: ใช้ PowerShell แบบ Manual

```powershell
# ค้นหาไฟล์ที่มี YOUR_GAS_WEB_APP_URL
Get-ChildItem -Path frontend -Filter *.html | Select-String "YOUR_GAS_WEB_APP_URL"

# แทนที่ทั้งหมด
$apiUrl = "https://script.google.com/macros/s/YOUR_ACTUAL_ID/exec"
Get-ChildItem -Path frontend -Filter *.html | ForEach-Object {
    $content = Get-Content $_.FullName -Raw -Encoding UTF8
    $newContent = $content -replace "YOUR_GAS_WEB_APP_URL", $apiUrl
    Set-Content -Path $_.FullName -Value $newContent -Encoding UTF8 -NoNewline
    Write-Host "Updated: $($_.Name)"
}
```

### ตรวจสอบหลังแก้ไข:

```powershell
# ตรวจสอบว่าไม่มี YOUR_GAS_WEB_APP_URL เหลืออยู่
Get-ChildItem -Path frontend -Filter *.html | Select-String "YOUR_GAS_WEB_APP_URL"
```

ควรไม่มีผลลัพธ์ (ไม่มีไฟล์ใดมี YOUR_GAS_WEB_APP_URL เหลืออยู่)

## หมายเหตุ

- URL ต้องเป็น URL เต็มรูปแบบ (รวม `https://`)
- ต้องไม่มี trailing slash (`/`) ที่ท้าย URL
- ตรวจสอบว่า URL ถูกต้องก่อน deploy
