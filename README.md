# Odoo 17 with Docker Compose

مستودع بسيط لتشغيل نسخة واحدة من Odoo 17 مع PostgreSQL باستخدام Docker Compose فقط. لا يحتاج إلى Ansible أو Jenkins.

## المتطلبات

- Docker Engine
- Docker Compose v2 (`docker compose`)

## التثبيت والتشغيل

1. انسخ المستودع وانتقل إلى مجلده:

   ```bash
   git clone <repository-url>
   cd InstallingOdoo17-Docker-Deployment
   ```

2. أنشئ ملف الإعدادات وغيّر كلمتي مرور PostgreSQL وOdoo الرئيسية:

   ```bash
   cp .env.example .env
   ```

3. شغّل Odoo وقاعدة البيانات:

   ```bash
   docker compose up -d
   ```

4. تحقق من حالة الحاويات وافتح Odoo:

   ```bash
   docker compose ps
   ```

   افتح `http://localhost:10017`. منفذ المحادثة المباشرة هو `20017`.

## الإعدادات

عدّل القيم في `.env` لتغيير صور الحاويات، كلمات المرور، المنافذ أو اسم الشبكة. لا ترفع `.env` إلى المستودع؛ الملف مستثنى من Git. تُمرر إعدادات قاعدة البيانات وكلمة مرور Odoo الرئيسية إلى ملف إعداد وقت تشغيل الحاوية.

ضع إضافات Odoo المخصصة داخل `addons/`. ملف إعداد Odoo موجود في `etc/odoo.conf`، وبيانات PostgreSQL محفوظة في `postgresql/`.

## أوامر الإدارة

```bash
docker compose logs -f       # عرض السجلات
docker compose restart       # إعادة التشغيل
docker compose down          # إيقاف الحاويات مع الإبقاء على البيانات
```

لا تحذف مجلد `postgresql/` إذا كنت تريد الاحتفاظ بقواعد البيانات.

## استخدام Traefik (اختياري)

يتوفر ملف Docker Compose إضافي للربط مع شبكة Traefik موجودة مسبقًا:

```bash
docker compose -f docker-compose.traefik.yml up -d
```

اضبط `ODOO_DOMAIN` و`DOCKER_PROXY_NETWORK` في `.env`، وأنشئ شبكة Traefik الخارجية قبل التشغيل.
