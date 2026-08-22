# AI-Powered Accounting System (n8n + Gemini + Google Sheets)

> نظام محاسبي متكامل مبني على n8n، يجمع بين وكيل ذكاء اصطناعي (Gemini) بأربع أدوات محاسبية، وGoogle Sheets كقاعدة بيانات، وتقرير مالي شهري تلقائي يُرسل عبر Gmail — كل ذلك بلغة طبيعية دون أي نماذج إدخال يدوية.

---

## 🇸🇦 نظرة عامة (العربية)

### المشكلة
التسجيل المحاسبي اليدوي (دفاتر أو جداول تُملأ يدويًا) عرضة للأخطاء والتأخير، ولا يوفر رؤية فورية عن الوضع المالي دون مجهود إضافي لتجميع البيانات وحساب الأرباح والخسائر.

### الحل
نظام يتكوّن من جزأين مترابطين:

**1. وكيل محادثة ذكي (تفاعلي)**
واجهة دردشة ويب متصلة بنموذج Gemini، تفهم الطلبات بلغة طبيعية وتنفذها عبر أربع أدوات:
- **تسجيل معاملة**: تحويل جملة مثل *"سجل مصروف 3000 دج كهرباء"* إلى سطر منظم في جدول البيانات.
- **استعلام مالي**: الإجابة عن أسئلة مثل *"كم صافي ربحي هذا الشهر؟"* بحساب فعلي من البيانات المخزّنة.
- **تصنيف ذكي**: اختيار الفئة المحاسبية الأقرب من قائمة معتمدة مسبقًا، بدل اختراع فئات غير متسقة.
- **تعديل/حذف معاملة**: تصحيح الأخطاء بلغة طبيعية دون العودة لتعديل الجدول يدويًا.

**2. تقرير مالي مجدول (تلقائي بالكامل)**
سير عمل منفصل يعمل أول كل شهر: يقرأ كل المعاملات، يحسب إجمالي الدخل والمصروفات وصافي الربح للشهر الحالي، ويُرسل ملخصًا كاملاً عبر Gmail دون أي تدخل بشري.

### التقنيات المستخدمة
| الأداة | الدور |
|---|---|
| **n8n** | محرك الأتمتة (منطق النظام بالكامل) |
| **Google Gemini** | نموذج اللغة المشغّل لوكيل المحادثة |
| **Google Sheets** | قاعدة بيانات محاسبية (جدولا المعاملات والفئات) |
| **Gmail** | إرسال التقرير الشهري التلقائي |

### هيكل البيانات
**جدول `المعاملات`**: `date | type | category | amount | description | source`
**جدول `الفئات`**: `category_name | type`

> ملاحظة تقنية: أسماء الأعمدة إنجليزية عمدًا (وليست عربية) — راجع قسم "أهم التحديات" أدناه لمعرفة السبب.

### أهم التحديات التقنية التي حُلّت
- **تعارض "Duplicate key" عند استخدام Google Sheets كأداة لوكيل AI**: أسماء الأعمدة العربية بالكامل قد تُختزل جميعها لنفس المفتاح الداخلي الفارغ (`_`) أثناء تحويلها لمعاملات برمجية، ما يمنع تنفيذ الأداة تمامًا. الحل: استخدام أسماء أعمدة تقنية بالإنجليزية (`date`, `type`, `category`...) مع إبقاء الشرح والأوصاف بالعربية في حقول Description المنفصلة.
- **خلط النموذج بين القيم عند التسجيل** (مثل وضع نص الوصف كامل في حقل المبلغ): حُلّ بإضافة توصيف دقيق لنوع ونطاق كل حقل على حدة (رقم فقط، فئة فقط دون أرقام...) بدل الاعتماد على وصف عام واحد للأداة كلها.
- **تحديد الصف المستهدف عند التعديل**: باستخدام العمود الضمني `row_number` الذي توفره عقدة Google Sheets تلقائيًا، بدل الحاجة لعمود معرّف فريد إضافي في هذه المرحلة التدريبية.

### الإعداد
1. أنشئ ملف Google Sheets بجدولين: `المعاملات` و `الفئات` بالأعمدة الموضحة أعلاه.
2. استورد `workflow.json` في n8n واربط بيانات اعتماد Google Gemini، Google Sheets، وGmail الخاصة بك.
3. فعّل السير (Publish)، واستخدم رابط الدردشة (Open Chat) للتفاعل مع الوكيل.
4. تأكد أن Schedule Trigger مضبوط على التوقيت المطلوب (افتراضيًا: أول كل شهر الساعة 9 صباحًا).

### قيود وتحسينات مستقبلية
- الحذف الفعلي للصفوف (وليس فقط تصفير القيم) يحتاج عملية `Delete` منفصلة — غير مطبق في هذه النسخة التدريبية.
- استخدام `row_number` للتعديل غير مستقر إذا حُذفت صفوف لاحقًا — يُنصح بإضافة عمود معرّف فريد (UUID) لكل معاملة في نسخة إنتاجية.
- يمكن إضافة أداة "قراءة فاتورة" (بالاستفادة من مشروع قارئ الفواتير السابق بـ Gemini Vision) لتسجيل المصروفات مباشرة من صورة فاتورة.

---

## 🇬🇧 Overview (English)

### Problem
Manual accounting (paper ledgers or manually-filled spreadsheets) is error-prone and slow, and gives no instant view of financial health without extra effort to aggregate data and compute profit/loss.

### Solution
A two-part system:

**1. Interactive AI Chat Agent**
A web chat interface powered by Gemini that understands natural-language requests and executes them through four tools:
- **Register transaction**: turns a sentence like *"log a 3000 DZD electricity expense"* into a structured spreadsheet row.
- **Financial query**: answers questions like *"what's my net profit this month?"* by computing real totals from stored data.
- **Smart categorization**: picks the closest matching category from a pre-approved list instead of inventing inconsistent ones.
- **Update/delete transaction**: corrects mistakes in natural language without manually editing the sheet.

**2. Scheduled Financial Report (fully automatic)**
A separate workflow that runs on the 1st of every month: reads all transactions, computes total income, total expenses, and net profit for the current month, and emails a full summary via Gmail with no human involvement.

### Tech Stack
| Tool | Role |
|---|---|
| **n8n** | Automation engine (full system logic) |
| **Google Gemini** | LLM powering the chat agent |
| **Google Sheets** | Accounting database (Transactions and Categories tables) |
| **Gmail** | Sends the automatic monthly report |

### Data Structure
**`Transactions` sheet**: `date | type | category | amount | description | source`
**`Categories` sheet**: `category_name | type`

> Technical note: column names are English on purpose, not Arabic — see "Key Challenges" below for why.

### Key Technical Challenges Solved
- **"Duplicate key" conflict when using Google Sheets as an AI agent tool**: fully Arabic column names can all collapse to the same empty internal key (`_`) when converted to programmatic parameters, blocking tool execution entirely. Fixed by using English technical column names (`date`, `type`, `category`...) while keeping human-readable Arabic explanations in separate Description fields.
- **Model confusing field values on registration** (e.g. putting the full description text into the amount field): fixed by adding a precise type/range description to each individual field (number only, category only with no digits...) instead of relying on one general tool description.
- **Targeting the correct row for updates**: using the implicit `row_number` column that Google Sheets nodes provide automatically, instead of requiring a dedicated unique-ID column at this training stage.

### Setup
1. Create a Google Sheets file with two tabs: `Transactions` and `Categories`, using the columns described above.
2. Import `workflow.json` into n8n and connect your own Google Gemini, Google Sheets, and Gmail credentials.
3. Publish the workflow and use the "Open Chat" link to interact with the agent.
4. Confirm the Schedule Trigger timing (default: 1st of every month at 9 AM).

### Limitations & Future Improvements
- Actually deleting rows (not just clearing values) requires a separate `Delete` operation — not implemented in this training version.
- Using `row_number` for updates becomes unstable once rows are deleted — a unique ID (UUID) column per transaction is recommended for a production version.
- An "invoice reader" tool could be added (reusing the earlier Gemini Vision invoice-reader project) to log expenses directly from a photographed receipt.

---

## 📁 Project Structure
```
ai-accounting-system/
├── README.md          # هذا الملف / this file
├── workflow.json       # ملف n8n قابل للاستيراد (بدون بيانات اعتماد)
└── screenshots/         # لقطات شاشة اختيارية للسير العمل
```
