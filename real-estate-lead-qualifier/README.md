# نظام تأهيل عملاء عقاريين بالذكاء الاصطناعي | AI Real Estate Lead Qualifier

## 📋 الوصف | Description
نظام أتمتة يستقبل رسائل العملاء المهتمين بالعقارات عبر Telegram، يتحاور معهم بالذكاء الاصطناعي لجمع بياناتهم الأساسية (نوع العقار، الميزانية، المنطقة، موعد المعاينة)، يحفظ كل محادثة بشكل دائم، ويرسل ملخصاً جاهزاً للوكيل العقاري فور اكتمال التأهيل.

An automation system that receives messages from real estate leads via Telegram, engages them in AI-driven conversation to collect key data (property type, budget, area, viewing date), permanently logs every conversation, and sends a ready summary to the agent once qualification is complete.

## ⚙️ كيف يعمل | How It Works
1. **Telegram Trigger** - يستقبل رسائل العملاء | Receives customer messages
2. **AI Agent (Google Gemini)** - يسأل تدريجياً ويجمع المعلومات | Progressively asks questions and gathers data
3. **Simple Memory** - يحافظ على سياق كل محادثة عبر معرف Chat ID الخاص بكل عميل | Maintains conversation context per customer via unique Chat ID
4. **Telegram (Send Message)** - يرد على العميل مباشرة | Replies directly to the customer

## 🎯 المشكلة التي يحلها | Problem Solved
الوكلاء العقاريون يفقدون عملاء محتملين بسبب التأخر في الرد وغياب فرز منظم بين العملاء الجادين وغيرهم.

Real estate agents lose potential leads due to delayed responses and lack of systematic filtering between serious and casual inquiries.

## 🛠️ الأدوات المستخدمة | Tools Used
- n8n (Workflow Automation)
- Telegram Bot API
- Google Gemini (LLM)

## 📁 الملفات | Files
- `workflow.json` - ملف الـ Workflow الكامل القابل للاستيراد في n8n | Full n8n workflow file, ready to import
