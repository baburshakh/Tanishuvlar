# Yurak — Firebase ulanishi va ishga tushirish

Repo sahifasi frontendni GitHub Pages’da host qiladi. **Haqiqiy hisoblar, Firestore chat va global foydalanuvchi ma’lumotlari Firebase loyihangizni sozlaganingizdan keyingina ishlaydi.** Firebase akkaунт үүсгэх, төслийн config оруулах, rules publish хийхийг би таны Firebase console-д нэвтрэх эрхгүй тул таны өмнөөс гүйцэтгэж чадахгүй.

## Firebase-ийг нэг удаа тохируулах

1. [Firebase Console](https://console.firebase.google.com/) дээр өөрийн Google хаягаар шинэ project үүсгэнэ.
2. Project settings → Your apps → Web app нэмж, Web app config утгуудыг хуулна.
3. Repo-ийн `firebase-config.js` файлыг edit хийж `REPLACE_ME` утгуудыг Firebase-ийн `apiKey`, `authDomain`, `projectId`, `appId`-аар солиод commit хийнэ. Эдгээр browser config утга нь нууц биш; харин service account JSON, private key, нууц үгийг огт commit хийж болохгүй.
4. Firebase Console → Authentication → Sign-in method дээр Email/Password, Google, Phone-ийг тус тус идэвхжүүлнэ. Google-ийн support email болон Phone Auth-ийн тохиргоог шалга.
5. Authentication → Settings → Authorized domains хэсэгт `baburshakh.github.io` домэйныг нэмнэ.
6. Firestore Database үүсгэнэ. **Firestore → Rules** дотор `firestore.rules` файлын агуулгыг paste хийгээд Publish дар.
7. GitHub Pages workflow амжилттай deploy хийсний дараа `https://baburshakh.github.io/Tanishuvlar/` хаягийг нээнэ.

## Admin хэрэглэгчийг аюулгүй нэмэх

1. Сайт дээр өөрийн аккаунт үүсгэж нэвтэр.
2. Firebase Console → Authentication → Users хэсгээс өөрийн UID-г хуул.
3. Firestore Data дээр `admins` collection үүсгээд document ID-г яг UID болго (жишээ нь `admins/your-auth-uid`). `{ "role": "admin" }` талбар нэмж болно.
4. Дахин нэвтрэхэд Admin tab гарна.

Admin эрхийг web UI-аар хэзээ ч өөртөө олгохгүй. Admin хэрэглэгчийн бүртгэлийн жагсаалтад email/phone харагдана. Нууц үг хадгалахгүй.

## Одоогоор ажиллах ба хязгаарлалт

- Email/password, Google popup, Firebase Phone Auth/SMS.
- Public profile, likes, харилцан like хийсэн хосын match.
- Match хоёр хэрэглэгчийн persistent Firestore чат.
- Admin зөвшөөрөлтэй account-ийн жагсаалт ба сүүлийн идэвх дээр суурилсан ойролцоо online тоо (2 минутын босго).
- Bot-ийн zar болон memory мини тоглоом. **Хүмүүс хоорондын multiplayer battle хараахан хийгдээгүй.**
- Authentication болон Firestore-ийн эрхийг Firebase өөрөө шалгана. `firestore.rules`-ийг production-д хэрэглэхээсээ өмнө Emulator Suite-ээр шалга.

## Хэрэв алдаа гарвал

- “Firebase hali sozlanmagan” — `firebase-config.js` дахь placeholder үлдсэн.
- `auth/unauthorized-domain` — hosting hostname-ийг Firebase Auth Authorized domains-д нэм.
- `permission-denied` — Firestore Database асаалттай, `firestore.rules` нийтлэгдсэн эсэхийг шалга.
- SMS ирэхгүй — Phone provider enabled, domain authorized, Firebase SMS квота/billing, reCAPTCHA-г шалга. SMS үйлчилгээний хязгаар/төлбөр үйлчилж болно.
- Profile save дээр насны алдаа — профайл 18+ нас шаарддаг.

Энэ бол MVP: block/report, content moderation, тусгайлал/нас батлах, multiplayer battle, нууцлалын хууль зүйн хуудас хараахан ороогүй. Өөр хүний зөвшөөрөлгүй зураг эсвэл мэдээлэл бүү нийтэл.
