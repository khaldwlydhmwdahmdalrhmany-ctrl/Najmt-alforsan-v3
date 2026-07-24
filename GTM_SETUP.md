# 📊 دليل تفعيل Google Tag Manager — نجمة الفرسان للنقليات

## 1️⃣ كيف تضيف كود GTM (بدون أي خطأ)

كل صفحة من صفحات الموقع (52 صفحة) تحتوي **مكانين جاهزين** بالضبط بهذا الشكل:

```html
<!-- ============ GTM_HEAD_START ============ -->
<!-- ضع هنا كود Google Tag Manager الخاص بـ <head> -->
<!-- ============ GTM_HEAD_END ============ -->
```

و مباشرة بعد `<body>`:

```html
<!-- ============ GTM_BODY_START ============ -->
<!-- ضع هنا كود Google Tag Manager الخاص بـ <noscript> -->
<!-- ============ GTM_BODY_END ============ -->
```

### طريقة الإضافة الموصى بها (لكل الصفحات دفعة واحدة، بدون فتح كل ملف يدويًا)

إن كنت تستخدم أداة بحث واستبدال في محرر أكواد (مثل VS Code → Find & Replace in Files):

1. ابحث عن: `<!-- ضع هنا كود Google Tag Manager الخاص بـ <head> فقط... -->`
   استبدله بكود GTM الخاص بـ `<head>` من حساب GTM الخاص بك
2. ابحث عن: `<!-- ضع هنا كود Google Tag Manager الخاص بـ <noscript>... -->`
   استبدله بكود الـ `<noscript>` الخاص بـ GTM

كلا الجزأين تحصل عليهما من Google Tag Manager عند إنشاء الحاوية (Container) → Install GTM.

---

## 2️⃣ الأحداث المُجهّزة مسبقًا للتتبع (Data Attributes)

هذه العناصر تحمل بالفعل خصائص `data-gtm-event` جاهزة — كل ما تحتاجه في GTM هو إنشاء **Trigger من نوع Click** يستهدف هذه الخصائص:

| العنصر | `data-gtm-event` | `data-gtm-location` | الصفحات |
|---|---|---|---|
| الزر العائم — واتساب | `cta_whatsapp_click` | `floating_button` | كل الصفحات |
| الزر العائم — اتصال | `cta_call_click` | `floating_button` | كل الصفحات |
| زر الهيدر — اتصل الآن | `cta_call_click` | `header` | كل الصفحات |
| زر الهيدر — واتساب | `cta_whatsapp_click` | `header` | كل الصفحات |
| زر الفوتر — واتساب | `cta_whatsapp_click` | `footer` | كل الصفحات |

### كيفية إنشاء Trigger في GTM لهذه العناصر
1. Triggers → New → Click - All Elements
2. Fire on: Some Clicks
3. Shart: `Click Element` → `matches CSS selector` → `[data-gtm-event="cta_whatsapp_click"]`
4. كرّر لكل حدث بنفس الطريقة، مع تغيير القيمة

---

## 3️⃣ حدث تحويل مهم: إرسال نماذج الطلب (الأقيَم في الموقع)

**كل نماذج طلب الخدمة الستة** (اطلب عرض سعر، اطلب توريد ديزل، اطلب عقد توريد، اطلب توصيل، اطلب تأجير داينا، طلب خدمة عاجلة) تُرسل تلقائيًا حدثًا لـ `dataLayer` عند الإرسال الناجح:

```javascript
dataLayer.push({
  event: 'form_submit_whatsapp',
  form_name: '[عنوان الصفحة]'  // مثال: "اطلب عرض سعر"
});
```

### لتتبعه في GTM (هذا هو أهم حدث تحويل في الموقع بأكمله)
1. Triggers → New → **Custom Event**
2. Event name: `form_submit_whatsapp`
3. اربطه بـ Google Ads Conversion Tracking أو GA4 Event Tag لقياس عدد طلبات العملاء الفعلية

---

## 4️⃣ توصية: أهم 3 تحويلات يجب قياسها بالضبط

| الأولوية | الحدث | لماذا |
|---|---|---|
| 🥇 | `form_submit_whatsapp` | العميل أرسل طلبًا فعليًا — أقوى إشارة نية شراء |
| 🥈 | `cta_whatsapp_click` (أي موقع) | نية تواصل، حتى لو لم يكمل النموذج |
| 🥉 | `cta_call_click` (أي موقع) | نية اتصال مباشر |

---

## 5️⃣ ملاحظة تقنية
لم أضف الكود الفعلي لـ GTM لأنني لا أملك رقم الحاوية (GTM-XXXXXXX) الخاص بحسابك. بمجرد إرسالك للرقم أو الكود، أدمجه فورًا في كل الصفحات الـ52 دفعة واحدة.
