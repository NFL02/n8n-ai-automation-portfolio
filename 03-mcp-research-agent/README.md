# وكيل بحث متعدد المصادر (MCP) | Multi-Source Research Agent (MCP)

## 📋 الوصف | Description
وكيل ذكاء اصطناعي متصل بخوادم MCP (Model Context Protocol) خارجية، قادر على البحث في الإنترنت والحصول على معلومات حديثة، بالإضافة لتحليل وشرح توثيق مشاريع GitHub تلقائياً - كل هذا دون بناء أي أداة يدوياً، فقط بربط الوكيل بخوادم جاهزة.

An AI agent connected to external MCP (Model Context Protocol) servers, capable of web search for up-to-date information and automatic analysis of GitHub project documentation - all without manually building any tool, simply by connecting the agent to ready-made servers.

## ⚙️ كيف يعمل | How It Works
1. **Chat Trigger** - يستقبل سؤال المستخدم
2. **AI Agent (Google Gemini)** - يقرر أي مصدر معرفة يستخدم
3. **MCP Client Tool #1** - يتصل بخادم بحث/توثيق ويب
4. **MCP Client Tool #2** - يتصل بخادم DeepWiki لتحليل مشاريع GitHub

## 🎯 المشكلة التي يحلها | Problem Solved
النماذج التقليدية محدودة بمعرفتها السابقة (Training Cutoff). هذا الوكيل يصل لمعلومات حية ومحدّثة عبر بروتوكول موحّد وقابل للتوسع لأي عدد من المصادر.

## 📁 الملفات | Files
- `workflow.json` - ملف الـ Workflow الكامل القابل للاستيراد في n8n
