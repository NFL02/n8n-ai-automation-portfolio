# نظام تذكير مواعيد ذكي | Smart Appointment Reminder System

## 📋 الوصف | Description
أتمتة تقرأ المواعيد من Google Sheets يومياً، تتحقق من المواعيد المستحقة غداً، وتُرسل تذكير WhatsApp جاهز عبر إيميل لصاحب العمل، ثم تحدّث حالة الإرسال لمنع التكرار.

Daily automation that reads appointments from Google Sheets, filters those due tomorrow, sends a ready-made WhatsApp reminder link via email to the business owner, and updates the status to prevent duplicate reminders.

## ⚙️ كيف يعمل | How It Works
1. **Schedule Trigger** - يعمل تلقائياً كل صباح
2. **Google Sheets** - يقرأ كل المواعيد
3. **Filter** - يصفّي المواعيد المستحقة غداً فقط
4. **Code (JavaScript)** - يبني رابط WhatsApp جاهز مع رسالة مخصصة
5. **Gmail** - يرسل قائمة الروابط لصاحب العمل
6. **Google Sheets (Update)** - يحدّث حالة الإرسال

## 🎯 المشكلة التي يحلها | Problem Solved
الأعمال الصغيرة (ورش، عيادات، صالونات) تفقد عملاء بسبب نسيان تذكيرهم بمواعيدهم يدوياً.

## 📁 الملفات | Files
- `workflow.json` - ملف الـ Workflow الكامل القابل للاستيراد في n8n
