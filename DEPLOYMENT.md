# Math5 Expert - نشر التطبيق (Deployment Guide)

## متطلبات نشر PWA

### 1. **الخادم والاستضافة**
```
✅ HTTPS مطلوب (لا يعمل PWA بـ HTTP عادي)
✅ Domain (نطاق الويب)
✅ Hosting (استضافة الويب)
```

### 2. **الملفات المطلوبة**
```
- index.html                           (صفحة العودة)
- مغامرة الرياضيات - نسخة احترافية محسّنة.html  (التطبيق الرئيسي)
- manifest.json                        (بيانات التطبيق)
- service-worker.js                    (معالج الخدمة)
- browserconfig.xml                    (إعدادات Windows)
- design-system/                       (نظام التصميم)
- README.md                            (التوثيق)
```

---

## خيارات الاستضافة الموصى بها

### 🟢 **Google Firebase Hosting** (مجاني ومضمون)
```bash
# التثبيت
npm install -g firebase-tools
firebase login
firebase init hosting

# النشر
firebase deploy

# HTTPS: تلقائي ✅
# Performance: ممتاز ✅
# Cost: مجاني ✅
```

**الميزات:**
- HTTPS مضمون
- CDN عالمي
- تحديثات فورية
- شهادات SSL مجانية

---

### 🔵 **GitHub Pages**
```bash
# إنشاء repository
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/math5-expert.git
git push -u origin main

# تفعيل Pages في الإعدادات
# Settings → Pages → Source: main
```

**الميزات:**
- مجاني تماما
- HTTPS تلقائي
- سهل جدا

**العيب:**
- قد يكون أبطأ قليلا

---

### 🟠 **Vercel**
```bash
npm i -g vercel
vercel

# Follow the prompts
# HTTPS: تلقائي ✅
# Performance: ممتاز ✅
```

---

### 🔴 **Netlify**
```bash
npm i -g netlify-cli
netlify deploy --prod --dir=.

# أو drag & drop في الموقع
```

---

## مراحل النشر

### المرحلة 1: الإعدادات الأساسية

#### أ) تحديث معلومات PWA
```json
// في manifest.json
{
  "start_url": "/",
  "scope": "/",
  "name": "مغامرة الرياضيات ٥",
  // ...
}
```

#### ب) إضافة HTTPS
- تأكد من شهادة SSL صحيحة
- اختبر بـ https://yourdomain.com

#### ج) اختبار Service Worker
```bash
# افتح DevTools (F12)
# انظر إلى التبويب Application
# تأكد من تسجيل Service Worker
```

---

### المرحلة 2: الاختبار قبل النشر

#### اختبار محلي:
```bash
# استخدم server محلي بـ HTTPS
# Python 3:
python -m http.server 8000

# Node:
npx http-server -p 8000 --cors

# مع HTTPS (اختياري):
# استخدم mkcert أو
# https://github.com/http-party/http-server
```

#### اختبار في المتصفح:
```javascript
// في Console:
navigator.serviceWorker.getRegistrations()
  .then(registrations => {
    console.log('SWs registered:', registrations.length);
  });
```

#### اختبار الإشعارات:
```javascript
// طلب إذن
Notification.requestPermission().then(permission => {
  if (permission === 'granted') {
    new Notification('اختبار الإشعار');
  }
});
```

---

### المرحلة 3: النشر على الإنترنت

#### Firebase Hosting (موصى):

1. **إنشاء Project**
```bash
firebase init
# اختر Hosting
# اختر مشروع Firebase جديد أو موجود
```

2. **تكوين firebase.json**
```json
{
  "hosting": {
    "public": ".",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "headers": [{
      "source": "**",
      "headers": [{
        "key": "Cache-Control",
        "value": "public, max-age=3600"
      }]
    }]
  }
}
```

3. **النشر**
```bash
firebase deploy
# سيظهر رابط الموقع
```

#### GitHub Pages:

1. **دفع الملفات**
```bash
git push origin main
```

2. **تفعيل Pages**
- اذهب إلى Settings
- Pages
- اختر main branch

3. **انتظر التفعيل** (2-5 دقائق)

---

## التحقق بعد النشر

### 1️⃣ **اختبر HTTPS**
```
https://yourdomain.com ✅
```

### 2️⃣ **اختبر Service Worker**
```
DevTools → Application → Service Workers
يجب أن ترى "Active and running"
```

### 3️⃣ **اختبر Manifest**
```
DevTools → Application → Manifest
يجب أن ترى بيانات التطبيق كاملة
```

### 4️⃣ **اختبر الأداء**
```
DevTools → Lighthouse
اختر PWA واضغط Analyze
يجب أن تحصل على درجة عالية
```

### 5️⃣ **اختبر على الهاتف**
1. افتح الموقع على هاتفك
2. يجب أن ترى زر "تثبيت" أو "إضافة إلى الشاشة الرئيسية"

---

## أفضل الممارسات

### 🔒 **الأمان**
```bash
# استخدم HTTPS دائما
# لا تخزن بيانات حساسة محليا
# تحقق من الأمان بـ Content Security Policy
```

### ⚡ **الأداء**
```bash
# ضغط الصور
# تقليل حجم CSS/JS
# استخدم CDN للخطوط
```

### 📱 **التوافق**
```bash
# اختبر على أجهزة مختلفة
# اختبر على متصفحات مختلفة
# تحقق من iOS و Android
```

### 📈 **المراقبة**
```bash
# استخدم Google Analytics
# راقب أخطاء Service Worker
# تتبع استخدام المستخدمين
```

---

## إضافة Google Analytics

```html
<!-- في head -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_ID');
</script>
```

---

## إضافة PWA Analytics

```javascript
// في service-worker.js
self.addEventListener('install', event => {
  gtag('event', 'app_installed');
  // ...
});

self.addEventListener('activate', event => {
  gtag('event', 'app_updated');
  // ...
});
```

---

## معالجة الأخطاء الشائعة

### ❌ "Service Worker not registered"
```
✅ تحقق من HTTPS
✅ تحقق من تسجيل الملف في الرابط الصحيح
✅ افحص DevTools Console للأخطاء
```

### ❌ "لا يظهر زر التثبيت"
```
✅ تأكد من HTTPS
✅ تحقق من manifest.json صحيح
✅ تحقق من Service Worker تم التسجيل
✅ جرب متصفح Chrome أو Edge
```

### ❌ "لا يعمل بدون إنترنت"
```
✅ تحقق من Cache في Service Worker
✅ افحص Storage في DevTools
✅ تحقق من كيفية حفظ البيانات
```

---

## نصائح أخرى

### 🎯 **قياس النجاح**
- عدد التثبيتات
- معدل الاستخدام
- رضا المستخدمين
- الأداء والسرعة

### 📊 **التحليلات**
```javascript
// تتبع تثبيت التطبيق
window.addEventListener('beforeinstallprompt', (e) => {
  gtag('event', 'app_install_prompt_shown');
});
```

### 🔄 **التحديثات**
- تحديث Service Worker تلقائيا
- إعلام المستخدم بالتحديثات
- تحديث لا يفسد البيانات القديمة

---

## المواارد المفيدة

- [PWA Checklist](https://web.dev/pwa-checklist/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Web Vitals](https://web.dev/vitals/)
- [Firebase Docs](https://firebase.google.com/docs)

---

## الدعم والمساعدة

إذا واجهت مشاكل في النشر:

1. افحص Console للأخطاء
2. جرب Lighthouse لتحديد المشاكل
3. اقرأ التوثيق الرسمية
4. اطلب مساعدة في Stack Overflow

---

**تم الإعداد**: أكتوبر 2026  
**النسخة**: 1.0

استمتع بنشر تطبيقك! 🚀
