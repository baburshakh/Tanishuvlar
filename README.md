# Yurak — tanishuv va o‘yinlar

Yurak — mobilga mos tanishuv interfeysi. Bu repozitoriyaga frontend va Firebase xavfsizlik qoidalari qo‘shilgan. **Saytni haqiqiy foydalanuvchilar ishlatishi uchun Firebase loyihasi va hostingni o‘zingiz sozlashingiz kerak.** Bu repozitoriydan hozircha tayyor public sayt havolasi yaratilmagan; kodni hostingga deploy qilmaguncha va Firebase sozlanmaguncha autentifikatsiya/chat ishlamaydi.

## Nimalar bor

- Email/parol bilan ro‘yxatdan o‘tish va kirish.
- Google hisob bilan kirish.
- Telefon raqamiga SMS kod yuborish orqali tasdiqlash (Firebase Phone Auth).
- Profil tahrirlash, tanishuv profillarini ko‘rish va chat boshlash.
- Xabarlar Firestore’da saqlanadi; shu hisobga qayta kirganda chat tarixi tiklanadi.
- O‘yinlar: zar tashlash va xotira mini-o‘yini. Hozirgi zar o‘yini botga qarshi demo; real multiplayer emas.
- Admin panel: foydalanuvchi emaili/telefonini va oxirgi faolligini ko‘rsatadi. Admin huquqi faqat Firebase konsolida qo‘lda beriladi.
- Firestore xavfsizlik qoidalari: `firestore.rules`.

## 1. Firebase loyihasini sozlash

1. [Firebase Console](https://console.firebase.google.com/) da loyiha yarating.
2. **Authentication → Sign-in method** bo‘limida Email/Password, Google va Phone provayderlarini yoqing. SMS test uchun Firebase test raqamlari va ruxsat etilgan domenlarni sozlang.
3. **Project settings → Your apps → Web app** orqali web app yarating. Ko‘rsatilgan `apiKey`, `authDomain`, `projectId`, `appId` qiymatlarini `index.html` ichidagi `firebaseConfig` qiymatlariga kiriting. Firebase web config identifikatorlari client tarafida ishlatiladi; admin paroli yoki service-account kalitini HTML/репозиториyaга hech qachon yozmang.
4. **Firestore Database** yarating va `firestore.rules` mazmunini Firestore Rules bo‘limiga publish qiling.
5. Authentication’dagi **Authorized domains** ro‘yxatiga hosting domeningizni qo‘shing.
6. Firebase Authentication uchun qo‘shimcha xavfsizlik sozlamalari va billing/kvotalarni ko‘rib chiqing. Phone Auth SMS narx/limitlariga ega bo‘lishi mumkin.

## 2. Adminni xavfsiz tayinlash

1. O‘zingiz oddiy foydalanuvchi sifatida ro‘yxatdan o‘ting.
2. Firebase Console → Authentication’dan UID’ingizni oling.
3. Firestore’da `admins` kolleksiyasini yarating va document ID sifatida UID’ingizni kiriting (masalan `admins/<UID>`). Hujjat mazmuni `{ "role": "admin" }` bo‘lishi mumkin.
4. Admin tugmasi keyingi login/yuklanishda ko‘rinadi.

Admin roli sayt ichidan berilmaydi. Firebase loyiha egasi hisobini 2FA-р хамгаалж, service-account/private keys-ийг хэнд ч битгий илгээгээрэй. Admin panel заавал шаардлагатай хэмжээнд л мэдээлэл харуулах ёстой; мессежийн агуулгыг админд нээхгүй.

## 3. Hosting

`index.html` нь ES module ашигладаг тул HTTPS hosting хэрэгтэй. GitHub Pages асаах бол репозиторийн **Settings → Pages → Deploy from a branch → main / root** сонгоно. Тохиргооны дараа URL нь `https://baburshakh.github.io/Tanishuvlar/` хэлбэртэй байж болно (GitHub Pages идэвхжиж deploy амжилттай болсны дараа л нээгдэнэ). Firebase Authorized domains-д `baburshakh.github.io` домэйныг нэмнэ.

## Турших

Firebase config утгуудыг тохируулж, дээрх providers ба Firestore-ийг идэвхжүүлсний дараа HTTPS hosting дээр нээнэ. Доод тал нь хоёр тусдаа хэрэглэгч үүсгээд профайл, мессеж болон буцаж нэвтэрсний дараах түүхийг шалгаарай. Firebase Emulator Suite ашиглан production өгөгдөлд хүрэхгүйгээр rules-ийг турших нь зөв.

## Хамгаалалтын тайлбар

- HTML-д нууц үг/серверийн түлхүүр оруулахгүй; нууц үгийг Firebase Authentication удирдана.
- Firestore Security Rules нь client талын UI-аар хязгаарлагдахгүй, шууд өгөгдлийн хүсэлтийг шалгана. Deploy-оос өмнө Emulator Suite-ээр rules-ийг заавал туршина.
- Утас/имэйл нь зөвхөн тухайн хэрэглэгч болон админд уншигдана; нийтэд харагдах профайлд оруулахгүй.
- Онлайны тоо нь хэрэглэгчийн `lastSeen`-ийг 60 секунд тутам шинэчилж, 2 минутын босгоор ойролцоолно; браузер хаагдсан мөчийг яг таг батлахгүй.
- Нээлттэй chat эхлүүлэх боломжтой тул хэрэглэгчийн блок/мэдээлэх хэрэгсэл болон abuse moderation-ыг production хувилбарт нэмэх шаардлагатай.
- Хэрэглэгчдийн зураг URL нь third-party зургууд руу хүсэлт үүсгэж болзошгүй; privacy бодлого/зөвшөөрөлтэй эх сурвалж ашиглаарай.

## Одоогоор хийгдээгүй

Энэ commit сервер эсвэл домэйн үүсгэхгүй, GitHub Pages-ийг автоматаар асаахгүй. Гуравдагч талын сервер рүү нэвтрэх, Google/SMS төлбөр, domain болон admin credentials-ийг хэрэглэгч өөрийн account дээр тохируулна. Multiplayer battle, real payment, report/block, production moderation ба legal/privacy pages хэрэгжээгүй.
