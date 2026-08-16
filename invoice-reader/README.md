# قارئ فواتير ذكي | Smart Invoice Reader

## 📋 الوصف | Description
نظام يستقبل صورة فاتورة (مصوّرة بالهاتف)، يحللها باستخدام الذكاء الاصطناعي متعدد الوسائط (Vision AI)، يستخرج البيانات الأساسية (المورد، التاريخ، المبلغ الإجمالي، قائمة المنتجات) بصيغة منظمة، ويحفظها تلقائياً في سجل محاسبي.

A system that receives a photographed invoice image, analyzes it using multimodal Vision AI, extracts key data (supplier, date, total amount, product list) in structured form, and automatically saves it into an accounting log.

## ⚙️ كيف يعمل | How It Works
1. **Form Trigger** - نموذج ويب لرفع صورة الفاتورة
2. **Google Gemini (Analyze Image)** - يقرأ الصورة ويستخرج البيانات كـ JSON
3. **Code (JavaScript)** - ينظّف ويحوّل النص الخام لبيانات منظمة
4. **Google Sheets** - يحفظ البيانات في سجل منظم بأعمدة منفصلة

## 🎯 المشكلة التي يحلها | Problem Solved
الإدخال اليدوي لبيانات الفواتير الورقية يستهلك وقتاً طويلاً وعرضة للأخطاء البشرية.

## 📁 الملفات | Files
- `workflow.json` - ملف الـ Workflow الكامل القابل للاستيراد في n8n
