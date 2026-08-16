# مركز دعم عملاء ذكي متعدد المسارات (RAG) | Multi-Path AI Customer Support Hub

## 📋 الوصف | Description
نظام دعم عملاء احترافي متكامل بأربعة مسارات متخصصة: الإجابة على الأسئلة الشائعة عبر تقنية RAG وقاعدة بيانات متجهات حقيقية (Supabase/pgvector)، التحقق من حالة الطلبات، تسجيل وتصعيد الشكاوى تلقائياً لموظف بشري، وتحليل رضا العملاء.

A comprehensive professional customer support system with four specialized paths: FAQ answering via RAG with a real vector database (Supabase/pgvector), order status tracking, automated complaint logging and human escalation, and customer satisfaction analysis.

## ⚙️ كيف يعمل | How It Works
1. **Chat Trigger + AI Agent** - نقطة الدخول والتوجيه الذكي
2. **مسار RAG**: Google Gemini Embeddings + Supabase Vector Store للإجابة الدقيقة من مستندات حقيقية
3. **مسار حالة الطلب**: Google Sheets Tool للبحث عن حالة الطلب
4. **مسار الشكاوى**: تسجيل في قاعدة بيانات + إشعار Gmail فوري للموظف المسؤول
5. **مسار التقييم**: حفظ تقييم رضا العميل لتحليل الجودة

## 🎯 المشكلة التي يحلها | Problem Solved
خدمة العملاء التقليدية إما بطيئة (بشرية بالكامل) أو غير دقيقة (بوتات تعتمد على معلومات ثابتة قديمة). هذا النظام يجمع دقة RAG مع سرعة الأتمتة والتصعيد الذكي للحالات المعقدة.

## 📁 الملفات | Files
- `workflow.json` - ملف الـ Workflow الرئيسي
- `sql-setup.sql` - كود إعداد قاعدة بيانات Supabase (pgvector)
