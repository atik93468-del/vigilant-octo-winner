# Hospital Duty Roster — Android starter project

এই প্রজেক্টে আছে:
- Email/password login (Firebase Authentication)
- Staff CRUD-এর প্রাথমিক সুবিধা (সর্বোচ্চ ১০ জন যোগ করা)
- ৩টি shift: Morning, Evening, Night
- ২টি duty type: Regular, Extra
- Firestore live listener — একাধিক মোবাইলে তথ্য আপডেট
- বাংলা/ইংরেজি UI toggle

## ১. যা লাগবে
- Android Studio (সাম্প্রতিক stable version)
- JDK 17
- Google account
- Internet
- Firebase project

## ২. Firebase সেটআপ
1. https://console.firebase.google.com এ নতুন project তৈরি করুন।
2. Android app যোগ করুন; package name অবশ্যই `com.example.hospitaldutyroster` দিন।
3. `google-services.json` ডাউনলোড করে এই প্রজেক্টের `app/` ফোল্ডারে রাখুন।
4. Authentication > Sign-in method-এ Email/Password enable করুন।
5. Authentication > Users থেকে প্রত্যেক অনুমোদিত কর্মীর জন্য user email/password তৈরি করুন। এই অ্যাপে public sign-up নেই।
6. Firestore Database তৈরি করুন।
7. Firebase Console > Firestore > Rules-এ `firestore.rules` ফাইলের নিয়ম বসিয়ে Publish করুন।

**নিরাপত্তা:** অন্তর্ভুক্ত rules হলো development starter মাত্র। এতে যে কোনো authenticated account সব roster data read/write করতে পারে। হাসপাতালের বাস্তব ডেটা ব্যবহারের আগে admin/roster-editor/viewer role, প্রতিষ্ঠানের পরিচয় যাচাই, audit log ও সীমিত access rules যোগ করুন। রোগীর তথ্য এই অ্যাপে রাখবেন না।

## ৩. Android Studio-তে খুলুন
1. ZIP extract করুন।
2. Android Studio > Open > `HospitalDutyRoster` ফোল্ডার নির্বাচন করুন।
3. `google-services.json` `app/`-এ আছে কি না যাচাই করুন।
4. Gradle sync শেষ হওয়া পর্যন্ত অপেক্ষা করুন।
5. ফোনে Developer options > USB debugging চালু করে USB দিয়ে যুক্ত করুন, অথবা emulator চালান।
6. Run ▶ চাপুন এবং Firebase-এ তৈরি করা email/password দিয়ে লগইন করুন।

## ৪. APK তৈরি
Android Studio-তে:
- Build > Build Bundle(s) / APK(s) > Build APK(s)
- APK সাধারণত `app/build/outputs/apk/debug/app-debug.apk`-এ তৈরি হবে।
- ফোনে কপি করে ইনস্টল করুন। ফোনে অনুমতি চাইলে ওই ফাইল ম্যানেজার অ্যাপের জন্য Install unknown apps অনুমতি দিন।

Release APK-এর জন্য:
- Build > Generate Signed Bundle / APK > APK
- নতুন keystore তৈরি করুন এবং নিরাপদে সংরক্ষণ করুন।
- Release APK তৈরি করে পরীক্ষার পর বিতরণ করুন। Keystore/password কাউকে দেবেন না।

## ৫. গুরুত্বপূর্ণ সীমাবদ্ধতা
এটি একটি runnable starter, সম্পূর্ণ production-ready হাসপাতাল সিস্টেম নয়। তারিখটি `YYYY-MM-DD` হিসেবে লিখতে হয়। এই সংস্করণে roster list দেখা, যোগ করা ও মুছতে পারা যায়; পূর্ণ edit form, PDF export/print, admin roles, shift overlap validation, audit history এবং backup/restore পরবর্তী উন্নয়ন হিসেবে যোগ করতে হবে। অনলাইনে সিঙ্কের জন্য ইন্টারনেট প্রয়োজন।

## ৬. GitHub Actions দিয়ে মোবাইল থেকে APK বানানো
1. GitHub-এ `HospitalDutyRoster` নামে **Private** repository তৈরি করুন।
2. ZIP extract করে ZIP ফাইল নয়, ভেতরের সব ফাইল ও `.github` ফোল্ডার repository-তে upload করুন।
3. Firebase Android app package `com.example.hospitaldutyroster` তৈরি করে পাওয়া `google-services.json` ফাইলটি `app/` ফোল্ডারে upload করুন।
4. Commit changes করুন। `main` branch-এ push হলে workflow চলবে; অথবা Actions > Build Hospital Duty Roster APK > Run workflow চাপুন।
5. সফল হলে workflow run-এর পৃষ্ঠার নিচের Artifacts থেকে `HospitalDutyRoster-debug-apk` ডাউনলোড করুন, ZIP extract করুন, তারপর APK ইনস্টল করুন।

**গুরুত্বপূর্ণ:** Firebase config ছাড়া workflow ইচ্ছাকৃতভাবে থেমে যাবে। Firebase config অ্যাপের পরিচয় সেট করে; সিঙ্কের জন্য Firebase Authentication ও Firestore-ও Console-এ চালু থাকতে হবে। Repository Private রাখুন। এই starter-এর Firestore rules production-এর জন্য যথেষ্ট নিরাপদ নয়; হাসপাতালের বাস্তব ব্যবহারের আগে role-based access rules যোগ করুন।
