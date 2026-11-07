# media/

Put the two files a share post carries **in this folder**, at the repository root:

```
media/share-poster.jpg     the picture (vertical 4:5 or square, under 25 MB)
media/share-clip.mp4       the clip  (10-15 seconds, under 25 MB)
```

Then tell `rewards.json` about them — name **and** fingerprint, in the same
commit:

```json
"media": {
  "image": { "file": "share-poster.jpg", "sha256": "<64 characters>" },
  "video": { "file": "share-clip.mp4", "sha256": "<64 characters>" }
}
```

## How to get the 64-character fingerprint

Windows PowerShell, in the folder holding the file:

```powershell
Get-FileHash .\share-poster.jpg -Algorithm SHA256
```

Copy the long value, make it lowercase (`Ctrl+H` → Replace, or just type it
lowercase), and paste it into `sha256`.

## Rules, and why

- The app downloads a file only to
  `https://up-games-config.pages.dev/media/<that exact name>` — it cannot be
  pointed at another host.
- Every downloaded file is re-hashed on the phone and compared with your
  `sha256`. **Match → it is used. No match → it is deleted and the post goes out
  with the text alone.** That is what protects your users from a replaced file.
- A name without a fingerprint is treated as "no media", on purpose: the app will
  not hand an unverified binary to TikTok or WhatsApp.
- File names: letters, digits, `.` `-` `_` only, no folders, no `..`, no leading
  dot, at most 80 characters.
- Anything over 25 MB is refused before it is downloaded.
- Everything the phone keeps of this is capped (default 120 MB); past the cap the
  oldest file is dropped and fetched again when something needs it.

## خطوات الرفع للمبتدئين

1. افتح المستودع ← `media` ← **Add file** ← **Upload files**.
2. اسحب الصورة و الفيديو. ⚠️ تأكد إنك داخل مجلد `media` و مو في جذر المستودع.
3. احسب البصمة بالأمر فوق، و حط الاسم و البصمة في `rewards.json`، و زيّد
   `version`.
4. افتح `https://up-games-config.pages.dev/media/share-poster.jpg` في المتصفح ←
   لو بان الصورة، الرفع تمام.
5. غيرت الصورة و أبيها تتبدل في الهواتف: لازم تغيّر `sha256` في نفس التعديل، وإلا
   الهواتف ترمي الملف الجديد و تشارك بدون صورة.
