# Yurak — tanishuv demo

Mobilga mos, GitHub Pages’da ishlaydigan tanishuv sayti prototipi. GitHub Actions workflow `main` branch-dagi o‘zgarishlarni avtomatik Pages deploy qiladi.

## Nima ishlaydi

- Tanishuv profillarini ko‘rish va yoqtirish.
- Matchlar va chat xabarlarining brauzerda saqlanishi (`localStorage`).
- O‘yinlar: zar tashlash va xotira mini-o‘yini.
- Profilni saqlash.

## Muhim cheklov

Bu frontend demo bo‘lib, haqiqiy foydalanuvchi autentifikatsiyasi, telefon/Google kirish, real-time chatlar, admin panel va online/offline statistikasi hali mavjud emas. Ma’lumotlar faqat bitta qurilmada saqlanadi.

Haqiqiy production versiyasi uchun Firebase/Auth/Firestore yoki Supabase + admin panel + security rules kerak bo‘ladi.

## GitHub Pages

`main` branchga push qilingandan so‘ng `.github/workflows/pages.yml` avtomatik ishlaydi. Repozitoriyning **Settings → Pages** bo‘limidan **GitHub Actions** tanlagan bo‘lishi lozim.

Agar deployment muvaffaqiyatli bo‘lsa, sayt URL ko‘rinishi:

`https://baburshakh.github.io/Tanishuvlar/`
