# 🔐 سازنده پسورد PWA

یک برنامه پیشرفته وب (PWA) برای تولید پسوردهای امن و قوی با استفاده از Vue.js

## ✨ ویژگی‌ها

- 🎯 **تولید پسوردهای امن**: الگوریتم‌های پیشرفته برای تولید پسوردهای قوی
- 📱 **PWA کامل**: قابلیت نصب روی موبایل و دسکتاپ
- 🎨 **طراحی مدرن**: رابط کاربری زیبا و واکنش‌گرا
- ⚡ **عملکرد سریع**: بدون نیاز به سرور، کاملاً آفلاین
- 🔒 **امنیت بالا**: پسوردها در مرورگر ذخیره نمی‌شوند
- 📊 **نمایش قدرت پسورد**: تحلیل و نمایش سطح امنیت پسورد
- 📋 **تولید چندین پسورد**: امکان تولید همزمان چندین پسورد
- 🌙 **حالت تاریک**: پشتیبانی از تم تاریک
- 📱 **واکنش‌گرا**: سازگار با تمام دستگاه‌ها

## 🚀 نصب و راه‌اندازی

### پیش‌نیازها
- Node.js (نسخه 14 یا بالاتر)
- npm یا yarn

### مراحل نصب

1. **کلون کردن پروژه**
```bash
git clone <repository-url>
cd password-generator-pwa
```

2. **نصب وابستگی‌ها**
```bash
npm install
```

3. **اجرای برنامه در حالت توسعه**
```bash
npm run dev
```

4. **ساخت نسخه تولید**
```bash
npm run build
```

5. **پیش‌نمایش نسخه تولید**
```bash
npm run preview
```

## 🛠️ تکنولوژی‌های استفاده شده

- **Vue.js 3**: فریم‌ورک اصلی
- **Vite**: ابزار ساخت و توسعه
- **PWA Plugin**: برای تبدیل به PWA
- **Workbox**: برای مدیریت Service Worker
- **Vazirmatn Font**: فونت فارسی زیبا

## 📱 ویژگی‌های PWA

- **قابلیت نصب**: نصب روی صفحه اصلی دستگاه
- **عملکرد آفلاین**: کارکرد بدون اینترنت
- **Push Notifications**: اعلان‌های فوری (آینده)
- **App-like Experience**: تجربه مشابه اپلیکیشن

## 🎛️ تنظیمات پسورد

### گزینه‌های موجود:
- **طول پسورد**: 8 تا 64 کاراکتر
- **حروف بزرگ**: A-Z
- **حروف کوچک**: a-z
- **اعداد**: 0-9
- **نمادها**: !@#$%^&*()_+-=[]{}|;:,.<>?
- **حذف کاراکترهای مشابه**: l, 1, I, O, 0

### سطوح قدرت پسورد:
- 🔴 **ضعیف**: کمتر از 3 معیار
- 🟡 **متوسط**: 3-4 معیار
- 🟢 **قوی**: 5-6 معیار
- 🟢 **خیلی قوی**: 7+ معیار

## 📁 ساختار پروژه

```
password-generator-pwa/
├── src/
│   ├── App.vue          # کامپوننت اصلی
│   ├── main.js          # نقطه ورود
│   └── style.css        # استایل‌ها
├── public/              # فایل‌های استاتیک
├── index.html           # فایل HTML اصلی
├── vite.config.js       # تنظیمات Vite
├── package.json         # وابستگی‌ها
└── README.md           # مستندات
```

## 🔧 تنظیمات PWA

فایل `vite.config.js` شامل تنظیمات PWA است:

```javascript
VitePWA({
  registerType: 'autoUpdate',
  workbox: {
    globPatterns: ['**/*.{js,css,html,ico,png,svg}']
  },
  manifest: {
    name: 'Password Generator PWA',
    short_name: 'PassGen',
    // ... سایر تنظیمات
  }
})
```

## 🎨 سفارشی‌سازی

### تغییر رنگ‌ها
در فایل `src/style.css` می‌توانید رنگ‌های اصلی را تغییر دهید:

```css
:root {
  --primary-color: #4f46e5;
  --secondary-color: #10b981;
  --background-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

### تغییر فونت
برای تغییر فونت، لینک Google Fonts را در `style.css` تغییر دهید:

```css
@import url('https://fonts.googleapis.com/css2?family=YOUR_FONT:wght@300;400;500;600;700&display=swap');
```

## 📱 تست PWA

1. برنامه را در مرورگر باز کنید
2. در Chrome DevTools به تب **Application** بروید
3. در بخش **Manifest** تنظیمات PWA را بررسی کنید
4. در بخش **Service Workers** وضعیت SW را چک کنید

## 🚀 استقرار

### GitHub Pages
```bash
npm run build
# فایل‌های dist را در GitHub Pages آپلود کنید
```

### Netlify
```bash
npm run build
# پوشه dist را در Netlify آپلود کنید
```

### Vercel
```bash
npm run build
# پروژه را در Vercel متصل کنید
```

## 🤝 مشارکت

1. پروژه را Fork کنید
2. یک شاخه جدید ایجاد کنید (`git checkout -b feature/amazing-feature`)
3. تغییرات را Commit کنید (`git commit -m 'Add amazing feature'`)
4. به شاخه اصلی Push کنید (`git push origin feature/amazing-feature`)
5. یک Pull Request ایجاد کنید

## 📄 مجوز

این پروژه تحت مجوز MIT منتشر شده است. برای اطلاعات بیشتر فایل `LICENSE` را مطالعه کنید.

## 🙏 تشکر

- Vue.js Team برای فریم‌ورک عالی
- Vite Team برای ابزار ساخت سریع
- PWA Plugin Team برای پلاگین PWA

## 📞 پشتیبانی

اگر سوال یا مشکلی دارید، لطفاً یک Issue ایجاد کنید.

---

**نکته امنیتی**: این برنامه پسوردها را در مرورگر ذخیره نمی‌کند و تمام محاسبات در سمت کلاینت انجام می‌شود. 