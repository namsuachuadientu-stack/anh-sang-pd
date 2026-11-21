# Ảnh sang PDF: bản cài máy tính và bản Android

Ứng dụng gộp một hay nhiều ảnh thành một file PDF. Chạy offline, ảnh không rời khỏi thiết bị.

- `web/`: giao diện dùng chung (HTML, CSS, JavaScript)
- `desktop/`: bản máy tính, đóng gói bằng Electron. Windows ra file `.exe` cài đặt, macOS ra `.dmg`, Linux ra `.AppImage`
- `mobile/`: bản Android, đóng gói bằng Capacitor. Ra file `.apk`
- `.github/workflows/build.yml`: quy trình tự động để GitHub build tất cả các file trên

> Lưu ý: dự án này được viết nhưng chưa được build thử ở nơi soạn ra nó (nơi đó không có Android SDK và không có mạng). Nếu một bước build báo lỗi, hãy gửi lại nội dung lỗi để sửa.

## Cách 1: Để GitHub build giúp (không cần cài gì)

1. Tạo tài khoản miễn phí tại github.com, rồi tạo một repository mới (chọn Private nếu muốn riêng tư).
2. Đưa toàn bộ nội dung thư mục này lên repository đó.
   - Dùng web: bấm **Add file > Upload files**, kéo toàn bộ file và thư mục đã giải nén vào.
   - Thư mục `.github` bắt đầu bằng dấu chấm nên có thể bị bỏ sót khi kéo thả. Nếu vào repository không thấy `.github/workflows/build.yml`, bấm **Add file > Create new file**, gõ tên `.github/workflows/build.yml` rồi dán nội dung file `build.yml` vào.
   - Dùng dòng lệnh: `git init`, `git add .`, `git commit -m "Ảnh sang PDF"`, `git branch -M main`, `git remote add origin <địa chỉ repo>`, `git push -u origin main`.
3. Mở tab **Actions** của repository. Quy trình tên **Build** sẽ tự chạy khi bạn đưa mã lên. Nếu không thấy chạy, chọn **Build** rồi bấm **Run workflow**.
4. Đợi khoảng 5 đến 15 phút. Khi xong, bấm vào lần chạy đó, kéo xuống mục **Artifacts** và tải về:
   - `AnhSangPDF-may-tinh-windows-latest`: giải nén ra file `.exe` để cài trên Windows
   - `AnhSangPDF-may-tinh-macos-latest`: file `.dmg` cho macOS
   - `AnhSangPDF-may-tinh-ubuntu-latest`: file `.AppImage` cho Linux
   - `AnhSangPDF-android-apk`: giải nén ra file `app-debug.apk` để cài trên Android

## Cách cài đặt

**Windows:** chạy file `.exe`. Vì bộ cài chưa được ký số, Windows có thể hiện cảnh báo "Windows protected your PC". Bấm **More info**, rồi **Run anyway**.

**Android:** chép file `.apk` sang điện thoại và mở nó. Điện thoại sẽ hỏi quyền cài ứng dụng từ nguồn không rõ, bạn cho phép cho đúng ứng dụng đang mở file. Google Play Protect có thể cảnh báo vì file chưa được ký bằng khóa phát hành, chọn cài đặt tiếp. Khi bấm "Tạo và lưu PDF", điện thoại mở bảng chia sẻ để bạn chọn nơi lưu (ví dụ Files, Drive) hoặc ứng dụng để gửi.

**macOS:** mở file `.dmg`, kéo ứng dụng vào Applications. Lần đầu mở, bấm chuột phải vào ứng dụng và chọn **Open**, vì ứng dụng chưa được ký.

## Cách 2: Tự build trên máy của bạn

Cần cài Node.js 20 trở lên.

**Bản máy tính** (build ra file cài cho hệ điều hành đang dùng):

```
cd desktop
npm install
npm run dist
```

File cài nằm trong `desktop/dist`. Muốn chạy thử không đóng gói: `npm start`.

**Bản Android** cần thêm JDK 21 và Android Studio (để có Android SDK):

```
cd mobile
npm install
npm run android:setup
cd android
./gradlew assembleDebug
```

Trên Windows dùng `gradlew.bat assembleDebug`. File APK nằm ở `mobile/android/app/build/outputs/apk/debug/app-debug.apk`. Bạn cũng có thể mở thư mục `mobile/android` bằng Android Studio và chọn **Build > Build APK(s)**.

Sau khi sửa giao diện trong `web/`, chạy `npm run android:sync` (trong `mobile`) hoặc `npm run dist` (trong `desktop`) để cập nhật.

## Ghi chú

- File APK này là bản debug, ký bằng khóa thử nghiệm của Android. Dùng cá nhân thì đủ. Muốn đăng lên Google Play cần tạo khóa phát hành và build bản release.
- Biểu tượng bản Android đang là biểu tượng mặc định của Capacitor. Có thể đổi bằng công cụ `@capacitor/assets`.
- Thư viện tạo PDF là jsPDF, được tải về lúc build và đóng gói vào ứng dụng nên không cần mạng khi dùng.
