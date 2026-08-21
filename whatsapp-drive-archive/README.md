# WhatsApp Files Auto-Archive to Google Drive

> أتمتة n8n تستقبل الملفات الواردة عبر واتساب (صور، فيديو، مستندات) وتؤرشفها تلقائيًا في Google Drive، منظّمة في مجلدات حسب التاريخ واسم جهة الاتصال، مع الحفاظ على الاسم الأصلي للملف ومعالجة التكرار دون أي تدخل يدوي.

---

## 🇸🇦 نظرة عامة (العربية)

### المشكلة
الملفات الواردة عبر واتساب (فواتير، مستندات، صور، تسجيلات) تُفقد بسهولة أو تبقى مبعثرة داخل التطبيق دون أرشفة منظمة، ما يصعّب الرجوع إليها لاحقًا.

### الحل
سير عمل n8n يعمل تلقائيًا في الخلفية:
1. يستقبل كل رسالة واردة تحتوي ملفًا عبر Webhook متصل بواتساب.
2. يتحقق من نوع الملف (صورة / فيديو / مستند) ويتجاهل الرسائل النصية.
3. ينزّل الملف كبيانات ثنائية من رابط الوسائط.
4. يبحث عن مجلد بصيغة `YYYY-MM-DD - اسم المرسل` في Google Drive، وينشئه إن لم يكن موجودًا.
5. يفحص وجود ملف بنفس الاسم في المجلد؛ إن وُجد تكرار، يُلحق طابعًا زمنيًا باسم الملف الجديد بدل الكتابة فوق الملف الأصلي.
6. يرفع الملف بالاسم النهائي إلى المجلد الصحيح.

### التقنيات المستخدمة
| الأداة | الدور |
|---|---|
| **n8n** | محرك الأتمتة (منطق سير العمل بالكامل) |
| **Whapi.Cloud** | بوابة اتصال بواتساب (وضع Sandbox المجاني) |
| **Google Drive API** | التخزين والتنظيم التلقائي للملفات |

### مخطط سير العمل
```
Webhook (واتساب) → If (نوع الملف؟) → HTTP Request (تنزيل الملف)
    → Edit Fields (تجهيز الاسم والمجلد)
    → Search Folder → If (المجلد موجود؟)
         ├─ نعم → Set targetFolderId
         └─ لا  → Create Folder → Set targetFolderId
    → (التقاء المسارين) → Search Duplicate → If (تكرار؟)
         ├─ نعم → Code (إلحاق طابع زمني)
         └─ لا  → Set (الاسم كما هو)
    → (التقاء المسارين) → Merge (دمج البيانات الثنائية مع الأسماء)
    → Upload to Google Drive
```

### أهم التحديات التقنية التي حُلّت
- **فروع If التي لا تُرجع بيانات**: عقد بحث Google Drive ترجع مصفوفة فارغة عند عدم وجود نتائج، ما يمنع تنفيذ أي فرع بعدها — تم الحل بتفعيل خيار `Always Output Data`.
- **فقدان البيانات الثنائية عبر سلسلة عقد Set/Code طويلة**: تم حلها بإضافة عقدة `Merge` تجمع البيانات الثنائية من عقدة `HTTP Request` مع البيانات النصية (الاسم والمجلد) في عنصر واحد قبل الرفع.
- **أخطاء "Multiple matching items" بعد الدمج**: تم حلها باستخدام `.first()` والإشارة الصريحة لاسم العقدة المصدر بدل `$json` العام.
- **الخلط بين حقلي Drive وFolder** في عقد Google Drive: معرّف المجلد يجب أن يوضع في حقل *Folder*، وليس *Drive* (المخصص لمساحات Google Drive المشتركة).

### الإعداد
1. أنشئ حساب Whapi.Cloud مجاني (وضع Sandbox)، وامسح رمز QR لربط رقم واتساب.
2. فعّل `Auto Download` لأنواع الوسائط: Image, Video, Document.
3. اربط رابط Webhook الخاص بـ n8n في إعدادات القناة.
4. استورد ملف `workflow.json` في n8n، واربط بيانات اعتماد Google Drive الخاصة بك.
5. فعّل السير (Publish) واستخدم رابط الـ Production.

### ملاحظات أمنية
- لا يحتوي ملف `workflow.json` المرفق على أي مفاتيح API أو Tokens — يجب ربط بيانات الاعتماد يدويًا بعد الاستيراد.
- خطة Whapi.Cloud Sandbox محدودة بـ 150 رسالة/يوم، 5 محادثات/شهر، 1000 طلب API/شهر — مناسبة للتدريب والعرض فقط، وليست للإنتاج الفعلي.

---

## 🇬🇧 Overview (English)

### Problem
Files shared via WhatsApp (invoices, documents, photos, recordings) are easily lost or left scattered inside the app with no structured archive, making them hard to retrieve later.

### Solution
An n8n workflow that runs automatically in the background:
1. Receives every incoming message containing a file via a WhatsApp-connected webhook.
2. Checks the file type (image / video / document) and ignores text messages.
3. Downloads the file as binary data from the media link.
4. Searches for a `YYYY-MM-DD - sender name` folder in Google Drive, creating it if it doesn't exist.
5. Checks whether a file with the same name already exists in the folder; if a duplicate is found, a timestamp is appended to the new file's name instead of overwriting the original.
6. Uploads the file under its final name to the correct folder.

### Tech Stack
| Tool | Role |
|---|---|
| **n8n** | Automation engine (full workflow logic) |
| **Whapi.Cloud** | WhatsApp gateway (free Sandbox mode) |
| **Google Drive API** | Automatic file storage and organization |

### Workflow Diagram
```
Webhook (WhatsApp) → If (file type?) → HTTP Request (download file)
    → Edit Fields (build name & folder)
    → Search Folder → If (folder exists?)
         ├─ yes → Set targetFolderId
         └─ no  → Create Folder → Set targetFolderId
    → (branches merge) → Search Duplicate → If (duplicate?)
         ├─ yes → Code (append timestamp)
         └─ no  → Set (keep original name)
    → (branches merge) → Merge (combine binary data with names)
    → Upload to Google Drive
```

### Key Technical Challenges Solved
- **If branches receiving no data**: Google Drive search nodes return an empty array on no matches, which stops any downstream branch from executing — fixed by enabling `Always Output Data`.
- **Binary data lost across a long chain of Set/Code nodes**: solved by adding a `Merge` node that combines the binary data from `HTTP Request` with the text data (filename, folder id) into a single item before upload.
- **"Multiple matching items" errors after merging**: solved using `.first()` and explicit node references instead of the generic `$json`.
- **Confusing the Drive and Folder fields** in Google Drive nodes: the folder ID must go in the *Folder* field, not *Drive* (reserved for Shared Drives).

### Setup
1. Create a free Whapi.Cloud account (Sandbox mode) and scan the QR code to link a WhatsApp number.
2. Enable `Auto Download` for Image, Video, and Document media types.
3. Set your n8n webhook URL in the channel's webhook settings.
4. Import `workflow.json` into n8n and connect your own Google Drive credentials.
5. Publish the workflow and use the Production URL.

### Security Notes
- The included `workflow.json` contains no API keys or tokens — credentials must be linked manually after import.
- The Whapi.Cloud Sandbox plan is limited to 150 messages/day, 5 chats/month, 1000 API requests/month — suitable for training and demos only, not production use.

---

## 📁 Project Structure
```
whatsapp-drive-archive/
├── README.md          # هذا الملف / this file
├── workflow.json       # ملف n8n قابل للاستيراد (بدون بيانات اعتماد)
└── screenshots/         # لقطات شاشة اختيارية للسير العمل
```
