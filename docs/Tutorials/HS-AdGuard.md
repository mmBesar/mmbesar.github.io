---
template: blog_post.html
title: Home Server - AdGuard Home
description: حماية الأسرة على الإنترنت والتخلص من إزعاج الإعلانات
date: 2026-11-20
---

# <div dir="rtl">حماية الأسرة على الإنترنت والتخلص من إزعاج الإعلانات</div>

![type:video](https://www.youtube.com/embed/r_lv3-eiE14)

<div dir="rtl">
أشارك معكم وسيلتي الأساسية للحماية والتخلص من الإعلانات على الإنترنت، ليس هذا فحسب، بل وإدارة الشبكة المحلية لربط كل أجهزتي.  
شرح تنصيب واستخدام AdGuard Home خطوة بخطوة، على الشبكة المحلية، وعلى VPS على الإنترنت.
</div>

<p hidden>#more</p>

## AdGuard Home + Caddy

> **المتطلبات:** Debian جاهز، Docker وDocker Compose مثبتان، والمستخدم الحالي عضو في مجموعة `docker`. نفّذ أوامر Docker دون `sudo`؛ استخدم `sudo` لأوامر النظام والملكية وFirewall.
>
> لا يتناول الدليل تثبيت Docker أو إنشاء المستخدم.
>
> **مهم:** إعداد AdGuard الأولي يعمل مؤقتًا بالصلاحيات الافتراضية للحاوية. بعد إنهاء المعالج نفعّل `user` و`cap_drop` و`cap_add` في **ملف Compose نفسه**، ثم نضبط ملكية البيانات ونعيد إنشاء الحاوية.

### الإعداد

أنشئ مجلد المشروع:

```bash
mkdir -p ~/adguard-stack
cd ~/adguard-stack
```

أنشئ `.env`، وعدّل القيم حسب جهازك:

```bash
cat > .env <<EOF
PUID=$(id -u)
PGID=$(id -g)
ADGUARD_ROOT=/srv/docker/adguard
CADDY_ROOT=/srv/docker/caddy
ADGUARD_DOMAIN=<ad.SERVER.lan>
SERVER_LAN_IP=<ServerIP>
EOF
chmod 600 .env
nano .env
```

حمّل المتغيرات وأنشئ المجلدات:

```bash
set -a
. ./.env
set +a

sudo mkdir -p \
  "$ADGUARD_ROOT/workdir" \
  "$ADGUARD_ROOT/confdir" \
  "$CADDY_ROOT/conf" \
  "$CADDY_ROOT/data" \
  "$CADDY_ROOT/config"

# Caddy يعمل بالمستخدم العادي من البداية.
sudo chown -R "$PUID:$PGID" "$CADDY_ROOT"
```

تحقق من المنافذ قبل البدء:

```bash
sudo ss -lntup | grep -E ':(53|80|443|3000|3001|853)\b' || true
```

إذا كان المنفذ `53` مستخدمًا، حدّد الخدمة المتعارضة قبل تشغيل AdGuard. لا توقف `systemd-resolved` أو تغيّر `/etc/resolv.conf` دون التأكد من إعداد DNS الحالي.

### ملف Compose

أنشئ `compose.yml`:

```yaml
services:
  adguardhome:
    image: adguard/adguardhome:v1.0.0
    container_name: adguardhome
    restart: unless-stopped
    network_mode: host
    # بعد إنهاء معالج الإعداد الأولي: أزل التعليقات عن الأسطر التالية.
    # user: "${PUID}:${PGID}"
    # cap_drop:
    #   - ALL
    # cap_add:
    #   - NET_BIND_SERVICE
    volumes:
      - ${ADGUARD_ROOT}/workdir:/opt/adguardhome/work
      - ${ADGUARD_ROOT}/confdir:/opt/adguardhome/conf

  caddy:
    image: caddy:2.11.9
    container_name: caddy
    restart: unless-stopped
    network_mode: host
    user: "${PUID}:${PGID}"
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
    environment:
      ADGUARD_DOMAIN: ${ADGUARD_DOMAIN}
    volumes:
      - ${CADDY_ROOT}/conf:/etc/caddy:ro
      - ${CADDY_ROOT}/data:/data
      - ${CADDY_ROOT}/config:/config
```

تحقق من الملف:

```bash
docker compose config --quiet && echo "Compose syntax OK"
```

> **ملاحظة:** كتلة `user` و`cap_drop` و`cap_add` الخاصة بـAdGuard تظل معلّقة أثناء أول تشغيل فقط؛ ترك `user` دون تحديد يستخدم المستخدم الافتراضي للصورة. إعداد Caddy يبقى كما هو.

---


### تشغيل AdGuard أول مرة

```bash
cd ~/adguard-stack
docker compose up -d adguardhome
docker logs --tail=50 adguardhome
```

من جهازك الذي تستخدم عليه المتصفح، افتح نفق SSH إلى الخادم:

```bash
ssh -N -L 3000:127.0.0.1:3000 -L 3001:127.0.0.1:3001 USER@SERVER_LAN_IP
```

> منفذ الإعداد `3000` مؤقت ويكون متاحًا على واجهات الخادم أثناء هذه المرحلة؛ نفّذها على شبكة موثوقة فقط، ولا تعرّض المنفذ للإنترنت.

افتح:

```text
http://127.0.0.1:3000
```

في معالج الإعداد اختر:

| الإعداد | القيمة |
|---|---|
| Admin Web Interface — Listen interface | `127.0.0.1` |
| Admin Web Interface — Port | `3001` |
| DNS Server — Listen interface | All interfaces (`0.0.0.0`) |
| DNS Server — Port | `53` |

أنشئ حساب الإدارة وأكمل المعالج. ستصبح واجهة الإدارة على `127.0.0.1:3001`، بينما يستمع DNS على `0.0.0.0:53`.

### تفعيل التشغيل كمستخدم عادي

أوقف الحاوية، ثم فعّل أسطر الصلاحيات في `compose.yml` بإزالة علامة `#` عنها:

```bash
docker compose stop adguardhome
nano compose.yml
```

بعد حفظ الملف، اضبط ملكية مجلدي البيانات والإعدادات للمستخدم الذي سيشغّل الحاوية:

```bash
sudo chown -R "$PUID:$PGID" "$ADGUARD_ROOT"
```

يجب أن تصبح الأسطر داخل خدمة `adguardhome` هكذا:

```yaml
    user: "${PUID}:${PGID}"
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
```

تحقق ثم أعد إنشاء الحاوية:

```bash
docker compose config --quiet && echo "Compose syntax OK"
docker compose up -d --force-recreate adguardhome

docker inspect adguardhome \
  --format 'user={{.Config.User}} capadd={{json .HostConfig.CapAdd}} capdrop={{json .HostConfig.CapDrop}}'
docker compose exec adguardhome sh -c 'id; grep "^CapEff:" /proc/1/status'
```

المتوقع: `user` يساوي `PUID:PGID`، و`CapEff` يساوي `0000000000000400`؛ أي `CAP_NET_BIND_SERVICE`.

### إضافة أسماء الخدمات المحلية

من **Filters → DNS rewrites** أضف سجلًا لكل خدمة تريد الوصول إليها باسم محلي:

| Domain | Answer |
|---|---|
| `ad.SERVER.LAN` | `192.168.1.1` |
| `jellyfin.SERVER.LAN` | `192.168.1.1` — عنوان Caddy |
| `immich.SERVER.LAN` | `192.168.1.1` — عنوان Caddy |

استبدل العنوان بقيمة `SERVER_LAN_IP` الفعلية. إذا كانت الخدمة خلف Caddy، وجّه DNS إلى عنوان جهاز Caddy، ثم وجّه Caddy إلى IP ومنفذ الخدمة. أما عند الاتصال بالخدمة مباشرة دون Caddy، فاستخدم IP جهاز الخدمة.

اجعل DHCP في الراوتر يوزّع عنوان AdGuard بوصفه DNS للأجهزة المنزلية. اختبر من جهاز عميل:

```bash
dig @192.168.1.1 ad.SERVER.LAN +short
dig @192.168.1.1 jellyfin.SERVER.LAN +short
```

يجب أن يرجع كل استعلام عنوان IP الذي أدخلته. DNS Rewrite يحلّ الاسم إلى IP فقط؛ للوصول إلى الخدمة دون كتابة المنفذ، أضف اسمها إلى Caddy ووجّهها إلى منفذ الخدمة. أضف المثال التالي إلى Caddyfile بجانب كتلة AdGuard؛ غيّر عنوان الوجهة والمنفذ حسب مكان الخدمة. مثال لخدمة Jellyfin على المنفذ `8096`:

```caddyfile
jellyfin.SERVER.LAN {
    reverse_proxy 127.0.0.1:8096
    tls internal
}
```

أضف هذا الجزء بعد إنشاء Caddyfile في الخطوة التالية، ثم أعد تحميل Caddy إذا عدّلت الملف بعد تشغيله.

### إعداد Caddy بشهادة محلية

أنشئ Caddyfile:

```bash
cat > "$CADDY_ROOT/conf/Caddyfile" <<'EOF'
{
    pki {
        ca local {
            name "MMB AdGuard Home"
        }
    }
}

{$ADGUARD_DOMAIN} {
    reverse_proxy 127.0.0.1:3001
    tls internal
}
EOF
```

شغّل Caddy واختبر HTTPS:

```bash
docker compose up -d caddy
docker logs --tail=50 caddy

ROOT_CA="$CADDY_ROOT/data/caddy/pki/authorities/local/root.crt"
curl --cacert "$ROOT_CA" \
  --resolve "$ADGUARD_DOMAIN:443:127.0.0.1" \
  -sSI "https://$ADGUARD_DOMAIN/"
```

المتوقع استجابة `HTTP/2 302` أو تحويل إلى صفحة تسجيل الدخول، دون خطأ في التحقق من الشهادة. بعد إضافة أي Site Block لخدمة محلية إلى Caddyfile، أعد تحميله بالأمر:

```bash
docker compose exec caddy caddy reload --config /etc/caddy/Caddyfile --adapter caddyfile
```

للوثوق بالشهادة على أجهزة الإدارة، انسخ `root.crt` وثبّته في مخزن الشهادات الموثوقة. في Debian:

```bash
cp "$ROOT_CA" "$HOME/adguard-home-local-root.crt"
```

انقله من جهاز العميل عبر SSH:

```bash
scp USER@SERVER_LAN_IP:~/adguard-home-local-root.crt .
```

ثم نفّذ على جهاز Debian العميل:

```bash
sudo install -m 0644 ./adguard-home-local-root.crt \
  /usr/local/share/ca-certificates/adguard-home-local.crt
sudo update-ca-certificates
```

قد يحتاج Firefox إلى استيراد الشهادة يدويًا من **Authorities**. وعلى Android ثبّتها من إعدادات الأمان والشهادات.

### الاختبارات المحلية

```bash
sudo ss -lntup | grep -E ':(53|3001|80|443)\b'
dig @127.0.0.1 example.com A +time=3 +tries=1
docker compose ps
```

اختبر أيضًا DNS Rewrites من جهاز عميل يستخدم AdGuard، وافتح `https://$ADGUARD_DOMAIN` بعد الوثوق بشهادة Caddy.

----


## دليل تشغيل AdGuard Home على VPS مع Caddy وشهادة SSL

تشغيل **AdGuard Home** + **Caddy** عبر Docker بمستخدم عادي `1000:1000`، والوصول إليه من:

```
https://ad.SERVER.CLOUD
```

> بدّل `ad.SERVER.CLOUD` بنطاقك في كل مكان إن اختلف.

---

### الفكرة باختصار

| ماذا | كيف |
|---|---|
| الشهادة (SSL) | Caddy يجلبها من Let's Encrypt تلقائيًا |
| واجهة AdGuard | تعمل على `127.0.0.1:3001` فقط، ويفتحها Caddy للعالم عبر HTTPS |
| منفذ DNS رقم 53 | يعمل على `127.0.0.1` فقط، وبعد الخطوة 11 يُغلق نهائيًا (AdGuard لا يخدم DNS العادي إطلاقًا) |
| DNS للمتصفحات | عبر HTTPS (DoH) على نفس الرابط ونفس الشهادة، ومحمي بمعرّف سري |
| DNS لأندرويد (Private DNS) | عبر TLS (DoT) على المنفذ 853 بنفس الشهادة، **مفتوح لمن يعرف الاسم** (الخطوة 11) |

**لماذا لا نفتح المنفذ 53؟** لأن فتحه يجعل سيرفرك DNS مجانيًا للعالم، ويستغله المهاجمون في هجمات DDoS. قلقك في محله.

**ولماذا لا يتعارض مع `systemd-resolved`؟** هو يستخدم `127.0.0.53` و`127.0.0.54`، وAdGuard سيستخدم `127.0.0.1`. لا تحتاج لإيقاف أي شيء.

**كيف نضمن أن 53 مغلق فعلًا؟** لا نعتمد على جدار حماية فقط. في الخطوة 11 نوقف خدمة DNS العادي داخل AdGuard نفسه، فلا يبقى شيء يستمع على المنفذ 53. والدليل على ذلك أمر `ss` على السيرفر (الخطوة 12)، وليس اختبار `dig` من الخارج، لأن انتهاء المهلة (timeout) لا يثبت شيئًا.

---

### 0. قبل البدء

نفّذ الأوامر على السيرفر، وتأكد من التالي:

```bash
id
docker compose version
getent ahostsv4 ad.SERVER.CLOUD | head -1
curl -4 -s https://ifconfig.me; echo
ip -4 route get 1.1.1.1
```

- `id` يجب أن يُظهر `uid=1000` (مستخدم عادي وليس root).
- `docker compose version` يعمل بدون أخطاء.
- العنوانان الثالث والرابع **متطابقان**. إن لم يظهر شيء للنطاق، أضف سجل `A` باسم `ad` يشير إلى IP السيرفر من إعدادات DNS للنطاق، وانتظر دقائق.
- آخر أمر يطبع عنوان السيرفر بعد كلمة `src`. في الغالب هو نفس العنوان العام.

---

### 1. جدار الحماية

افتح فقط ما يلزم: SSH و80 و443.

على نظام نظيف قد لا يكون `ufw` مثبّتًا (يظهر `command not found`). ثبّته أولًا:

```bash
sudo apt update
sudo apt install -y ufw
```

ثم:

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 443/udp
sudo ufw enable
sudo ufw status
```

يجب أن يظهر `Status: active`.

> ⚠️ إن كان SSH على منفذ غير `22` فغيّره في أول سطر **قبل** `ufw enable` وإلا سينقطع اتصالك.
> وإن كان لدى مزوّد الخدمة جدار حماية في لوحته (Hostinger مثلًا)، افتح فيه `80` و`443` أيضًا، وافتح `853/tcp` إن ستستخدم أندرويد (الخطوة 11).

الحاويات هنا تستخدم `network_mode: host`، لذلك `ufw` يحميها فعليًا.

---

### 2. المجلدات والمتغيرات

```bash
mkdir -p ~/adguard-stack
cd ~/adguard-stack
```

اقرأ عنوان السيرفر (يُستخدم في الخطوة 11):

```bash
SERVER_IP=$(ip -4 route get 1.1.1.1 | awk '{for(i=1;i<=NF;i++) if($i=="src"){print $(i+1); exit}}')
echo "$SERVER_IP"
```

يجب أن يظهر عنوان IP واحد. إن ظهر فراغ فلا تكمل.

أنشئ `.env`:

```bash
cat > .env <<EOF
PUID=1000
PGID=1000
ADGUARD_ROOT=/srv/docker/adguard
CADDY_ROOT=/srv/docker/caddy
ADGUARD_DOMAIN=ad.SERVER.CLOUD
SERVER_IP=$SERVER_IP
DOH_ID=$(openssl rand -hex 16)
EOF
chmod 600 .env
```

> `DOH_ID` معرّف سري عشوائي (يعمل كأنه كلمة مرور لـ DNS). لا تشاركه.

حمّل المتغيرات وأنشئ المجلدات:

```bash
set -a
. ./.env
set +a

sudo mkdir -p \
  "$ADGUARD_ROOT/workdir" \
  "$ADGUARD_ROOT/confdir" \
  "$CADDY_ROOT/conf" \
  "$CADDY_ROOT/data" \
  "$CADDY_ROOT/config"

# Caddy يعمل بالمستخدم العادي من البداية
sudo chown -R "$PUID:$PGID" "$CADDY_ROOT"
```

> في أي نافذة طرفية جديدة، أعد تحميل المتغيرات: `cd ~/adguard-stack && set -a && . ./.env && set +a`

---

### 3. ملف Compose

```bash
cat > compose.yml <<'EOF'
services:
  adguardhome:
    image: adguard/adguardhome:v1.0.0
    container_name: adguardhome
    restart: unless-stopped
    network_mode: host
    # بعد إنهاء معالج الإعداد الأولي: أزل التعليقات عن الأسطر التالية.
    # user: "${PUID}:${PGID}"
    # cap_drop:
    #   - ALL
    # cap_add:
    #   - NET_BIND_SERVICE
    volumes:
      - ${ADGUARD_ROOT}/workdir:/opt/adguardhome/work
      - ${ADGUARD_ROOT}/confdir:/opt/adguardhome/conf

  caddy:
    image: caddy:2.11.9
    container_name: caddy
    restart: unless-stopped
    network_mode: host
    user: "${PUID}:${PGID}"
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
    environment:
      ADGUARD_DOMAIN: ${ADGUARD_DOMAIN}
    volumes:
      - ${CADDY_ROOT}/conf:/etc/caddy:ro
      - ${CADDY_ROOT}/data:/data
      - ${CADDY_ROOT}/config:/config
EOF

docker compose config --quiet && echo "Compose syntax OK"
```

---

### 4. إعداد Caddy

شهادة حقيقية من Let's Encrypt (بدون `tls internal`):

```bash
cat > "$CADDY_ROOT/conf/Caddyfile" <<'EOF'
{$ADGUARD_DOMAIN} {
    reverse_proxy 127.0.0.1:3001
}
EOF
```

---

### 5. أول تشغيل لـ AdGuard (كـ root)

```bash
docker compose up -d adguardhome
docker logs --tail=20 adguardhome
```

معالج الإعداد يعمل على المنفذ `3000` وهو مغلق أمام الإنترنت، لذلك افتحه عبر نفق SSH.
**من جهازك أنت** (وليس السيرفر):

```bash
ssh -N -L 3000:127.0.0.1:3000 root@SERVER_IP
```

ثم افتح في المتصفح: `http://127.0.0.1:3000`

في المعالج اختر:

| الإعداد | القيمة |
|---|---|
| Admin Web Interface — Listen interface | `127.0.0.1` |
| Admin Web Interface — Port | `3001` |
| DNS Server — Listen interface | `127.0.0.1` (وليس All interfaces) |
| DNS Server — Port | `53` |

أنشئ حساب الإدارة وأكمل المعالج.

> بعد الإنهاء قد لا تفتح الصفحة الأخيرة. هذا طبيعي، أكمل للخطوة التالية.

---

### 6. التحويل إلى 1000:1000 وتفعيل DoH

أوقف AdGuard:

```bash
docker compose stop adguardhome
```

فعّل DoH خلف Caddy (يسمح لـ AdGuard باستقبال طلبات DoH القادمة من Caddy):

```bash
sudo sed -i -E "s/(insecure_enabled'?: )false/\1true/; s/(allow_unencrypted_doh'?: )false/\1true/" \
  "$ADGUARD_ROOT/confdir/AdGuardHome.yaml"

sudo grep -nE "insecure_enabled|allow_unencrypted_doh" "$ADGUARD_ROOT/confdir/AdGuardHome.yaml"
```

المتوقع: سطر واحد ينتهي بـ `true`.
إن لم يظهر شيء، أضف هذين السطرين تحت قسم `http:` في الملف (بنفس المسافات):

```yaml
  doh:
    insecure_enabled: true
```

فعّل أسطر المستخدم والصلاحيات في `compose.yml` (إزالة `#` تلقائيًا):

```bash
sed -i -E '/^    # (user:|cap_drop:|cap_add:|  - ALL|  - NET_BIND_SERVICE)/s/^    # /    /' compose.yml
grep -nE "user:|cap_drop|cap_add" compose.yml
```

المتوقع: 6 أسطر بدون `#` (3 لـ AdGuard و3 لـ Caddy).

اضبط الملكية وأعد التشغيل:

```bash
sudo chown -R "$PUID:$PGID" "$ADGUARD_ROOT"
docker compose config --quiet && echo "Compose syntax OK"
docker compose up -d --force-recreate adguardhome
```

تحقق:

```bash
docker inspect adguardhome \
  --format 'user={{.Config.User}} capadd={{json .HostConfig.CapAdd}} capdrop={{json .HostConfig.CapDrop}}'
docker compose exec adguardhome sh -c 'id; grep "^CapEff:" /proc/1/status'
sudo ss -lntup | grep -E ':(53|3001)\b'
```

المتوقع:
- `user=1000:1000`
- `CapEff: 0000000000000400`
- AdGuard يستمع على `127.0.0.1:53` و`127.0.0.1:3001` فقط (بجانب أسطر `systemd-resolve`).

---

### 7. تشغيل Caddy

```bash
docker compose up -d caddy
docker logs --tail=30 caddy
curl -sSI "https://$ADGUARD_DOMAIN/"
```

المتوقع: في السجل `certificate obtained successfully`، والاستجابة `HTTP/2 302` (تحويل لصفحة الدخول).

> إن فشلت الشهادة: راجع الخطوتين 0 و1 (النطاق والمنافذ 80/443) ولا تكرر المحاولة كثيرًا، فـ Let's Encrypt يحدّد عدد المحاولات الفاشلة.

---

### 8. حماية DNS بالمعرّف السري

بدون هذه الخطوة يستطيع أي شخص استخدام الرابط كـ DNS مفتوح.

اعرض المعرّف:

```bash
grep DOH_ID .env
```

ثم افتح `https://ad.SERVER.CLOUD` وسجّل الدخول، واذهب إلى:

**Settings → DNS settings → Access settings → Allowed clients**

الصق **المعرّف فقط** (القيمة بعد `DOH_ID=`) في الحقل، واضغط **Save**.

من الآن لن يُجيب AdGuard إلا الطلبات التي تحمل هذا المعرّف.

> إن كنت ستستخدم أندرويد (Private DNS) فستحتاج لإفراغ هذا الحقل لاحقًا في الخطوة 11، لأن أندرويد لا يستطيع إرسال المعرّف.

---

### 9. الاختبار

```bash
DOH="https://$ADGUARD_DOMAIN/dns-query"
Q="AAABAAABAAAAAAAAB2V4YW1wbGUDY29tAAABAAE"

# مع المعرّف: يجب أن يعمل
curl -s -o /dev/null -H 'accept: application/dns-message' \
  -w 'with ID:    HTTP %{http_code}, %{size_download} bytes\n' "$DOH/$DOH_ID?dns=$Q"

# بدون معرّف: يجب أن يُرفض
curl -s -o /dev/null -H 'accept: application/dns-message' \
  -w 'without ID: HTTP %{http_code}, %{size_download} bytes\n' "$DOH?dns=$Q"
```

- **مع المعرّف:** `HTTP 200` وحجم أكبر من `0`.
- **بدون معرّف:** يجب ألا يكون `HTTP 200` مع إجابة.

ثم تأكد على **السيرفر** أن AdGuard لا يستمع على عنوان عام للمنفذ 53:

```bash
sudo ss -lntup | grep -E ':53\b'
```

المتوقع: عنوان AdGuard الوحيد هو `127.0.0.1`، والباقي أسطر `systemd-resolve`. بعد الخطوة 11 لن يظهر AdGuard هنا إطلاقًا.

---

### 10. استخدامه على أجهزتك

رابط DNS الخاص بك:

```
https://ad.SERVER.CLOUD/dns-query/المعرّف_السري
```

| البرنامج | المسار |
|---|---|
| Firefox | Settings → Privacy & Security → DNS over HTTPS → Custom |
| Chrome / Edge | Settings → Privacy and security → Security → Use secure DNS → Custom |

> أندرويد (Private DNS) يدعم DoT فقط وليس DoH. له الخطوة 11 أدناه.

---

### 11. أندرويد (Private DNS) عبر DoT

النتيجة النهائية: تكتب في أندرويد الاسم التالي فقط:

```
ad.SERVER.CLOUD
```

> ⚠️ **تنبيه:** أندرويد لا يستطيع إرسال معرّف سري، لذلك يجب **إفراغ** حقل Allowed clients (الخطوة 8).
> بعدها يستطيع **أي شخص يعرف اسم النطاق** استخدام DNS الخاص بك على المنفذ 853، ورابط DoH أيضًا يصبح مفتوحًا. لا تشارك الاسم مع أحد.

#### 11.1 افتح المنفذ 853

```bash
sudo ufw allow 853/tcp
```

وإن كان لدى مزوّد الخدمة جدار حماية في لوحته، افتح `853/tcp` فيه أيضًا.

#### 11.2 اربط شهادة Caddy بـ AdGuard وفعّل التشفير

أوقف AdGuard، واربط مجلد شهادات Caddy به (للقراءة فقط). الأمر آمن للتكرار:

```bash
docker compose stop adguardhome

grep -q '/certs:ro' compose.yml || \
  sed -i '/:\/opt\/adguardhome\/conf$/a\      - ${CADDY_ROOT}/data/caddy/certificates:/certs:ro' compose.yml

grep -n '/certs:ro' compose.yml
```

المتوقع: سطر واحد فقط.

حدّد مساري الشهادة والمفتاح:

```bash
CRT=$(find "$CADDY_ROOT/data/caddy/certificates" -name "$ADGUARD_DOMAIN.crt" | head -1 | sed "s|^$CADDY_ROOT/data/caddy/certificates|/certs|")
KEY="${CRT%.crt}.key"
echo "$CRT"
echo "$KEY"
```

يجب أن يظهر مساران يبدآن بـ `/certs/`. إن كانا فارغين فالشهادة لم تصدر بعد (راجع الخطوة 7).

فعّل التشفير في إعدادات AdGuard:

```bash
sudo sed -i -E "/^tls:/,/^[a-z_]+:/{
s|^(  '?enabled'?: ).*|\1true|
s|^(  '?server_name'?: ).*|\1$ADGUARD_DOMAIN|
s|^(  '?port_https'?: ).*|\10|
s|^(  '?port_dns_over_quic'?: ).*|\10|
s|^(  '?certificate_path'?: ).*|\1$CRT|
s|^(  '?private_key_path'?: ).*|\1$KEY|
}" "$ADGUARD_ROOT/confdir/AdGuardHome.yaml"

sudo sed -n -E '/^tls:/,/^[a-z_]+:/p' "$ADGUARD_ROOT/confdir/AdGuardHome.yaml"
```

المتوقع في الناتج:
- `enabled: true`
- `server_name:` اسم نطاقك
- `port_https: 0` (حتى لا يزاحم Caddy على المنفذ 443)
- `port_dns_over_tls: 853`
- مساري الشهادة والمفتاح

#### 11.3 اجعل DoT يستمع على عنوان السيرفر وأوقف DNS العادي

**هذه الخطوة ضرورية.** AdGuard يشغّل DoT على نفس العناوين التي يستمع عليها DNS العادي (`bind_hosts`)، وهي الآن `127.0.0.1` فقط. لذلك يبقى المنفذ 853 غير متاح من الإنترنت حتى تضيف عنوان السيرفر.

ولأن إضافة العنوان تجعل المنفذ 53 يستمع عليه أيضًا، نوقف خدمة DNS العادي بالكامل (`serve_plain_dns: false`)، فيبقى DoT وDoH فقط.

```bash
AGH_YAML="$ADGUARD_ROOT/confdir/AdGuardHome.yaml"

# أضف عنوان السيرفر إلى bind_hosts (مرة واحدة فقط)
sudo grep -q "^    - $SERVER_IP\$" "$AGH_YAML" || \
  sudo sed -i -E "/^  bind_hosts:/a\    - $SERVER_IP" "$AGH_YAML"

# أوقف DNS العادي
sudo sed -i -E 's/^([[:space:]]+serve_plain_dns: )true/\1false/' "$AGH_YAML"

sudo sed -n '/^dns:/,/^  port:/p' "$AGH_YAML"
sudo grep -n 'serve_plain_dns' "$AGH_YAML"
```

المتوقع:
- تحت `bind_hosts:` سطران: `127.0.0.1` وعنوان السيرفر.
- `serve_plain_dns: false`

> إن لم يطبع آخر أمر شيئًا فنسختك لا تدعم هذا الخيار، ولن يُغلق المنفذ 53 بهذه الطريقة. في هذه الحالة تأكد من تفعيل `ufw` (الخطوة 1) وجدار حماية المزوّد.

#### 11.4 شغّل AdGuard وتحقق

```bash
docker compose config --quiet && echo "Compose syntax OK"
docker compose up -d --force-recreate adguardhome
docker logs --tail=20 adguardhome
sudo ss -lntup | grep -E ':(53|853)\b'
```

المتوقع:
- `127.0.0.1:853` و`عنوان_السيرفر:853` يستمع عليهما AdGuard.
- **لا يظهر AdGuard على المنفذ 53 إطلاقًا**، فقط أسطر `systemd-resolve`.

#### 11.5 أفرغ Allowed clients

افتح `https://ad.SERVER.CLOUD` ← **Settings → DNS settings → Access settings → Allowed clients** ← احذف المعرّف واترك الحقل فارغًا ← **Save**.

#### 11.6 تجديد الشهادة تلقائيًا

Caddy يجدد الشهادة قبل انتهائها بنحو شهر، لكن AdGuard يقرأها عند التشغيل فقط. أعد تشغيله شهريًا.

على نظام نظيف قد لا يكون `cron` مثبّتًا (يظهر `crontab: command not found`):

```bash
sudo apt install -y cron
sudo systemctl enable --now cron
```

ثم أضف المهمة (آمن للتكرار):

```bash
crontab -l 2>/dev/null | grep -q 'restart adguardhome' || \
  (crontab -l 2>/dev/null; echo '0 4 1 * * docker restart adguardhome >/dev/null 2>&1') | crontab -

crontab -l
```

المتوقع: يظهر سطر `docker restart adguardhome` مرة واحدة.

#### 11.7 اختبر ثم فعّله في أندرويد

من **جهازك أنت** (يجب أن يظهر `Let's Encrypt` في المُصدِر):

```bash
openssl s_client -connect ad.SERVER.CLOUD:853 -servername ad.SERVER.CLOUD </dev/null 2>/dev/null | openssl x509 -noout -issuer -dates
```

في أندرويد:

**Settings → Network & internet → Private DNS → Private DNS provider hostname**
واكتب: `ad.SERVER.CLOUD` ثم **Save**.

> أسماء القوائم تختلف قليلًا بين الهواتف. ابحث عن "Private DNS" في الإعدادات.
> إن ظهر "Couldn't connect": راجع جدول المشاكل أدناه.

---

### 12. الفحص النهائي

على **السيرفر**:

```bash
cd ~/adguard-stack && set -a && . ./.env && set +a

docker compose ps
sudo ss -lntup | grep -E ':(53|853|3001)\b'
sudo ufw status | head -15
crontab -l
```

المتوقع:
- الحاويتان `adguardhome` و`caddy` بحالة `running`.
- `:853` على `127.0.0.1` وعلى عنوان السيرفر، و`:3001` على `127.0.0.1`.
- **لا AdGuard على `:53`**.
- `ufw` فعّال والمنافذ المفتوحة: 22 و80 و443 و853/tcp.
- مهمة cron موجودة.

من **جهازك أنت**:

```bash
openssl s_client -connect ad.SERVER.CLOUD:853 -servername ad.SERVER.CLOUD </dev/null 2>/dev/null | openssl x509 -noout -issuer -dates
curl -sSI https://ad.SERVER.CLOUD/ | head -1
```

ثم على الهاتف: افتح أي موقع بعد تفعيل Private DNS، وراجع **Query Log** في لوحة AdGuard وتأكد أن الطلبات تظهر.

> لا تعتمد على `dig @SERVER_IP` من الخارج لإثبات إغلاق 53: انتهاء المهلة قد يكون بسبب جدار حماية المزوّد أو شبكتك. الدليل هو `ss` على السيرفر.

---

### أوامر مفيدة

```bash
cd ~/adguard-stack
docker compose ps                      # الحالة
docker compose logs -f --tail=50       # السجلات
docker compose pull && docker compose up -d   # التحديث (بعد تغيير رقم النسخة في compose.yml)
docker compose restart adguardhome     # إعادة التشغيل
```

### مشاكل شائعة

| المشكلة | الحل |
|---|---|
| Caddy لا يجلب الشهادة | النطاق لا يشير لـ IP السيرفر، أو 80/443 مغلقة |
| الصفحة تعطي `502` | AdGuard متوقف، أو لا يستمع على `127.0.0.1:3001` (راجع الخطوة 6) |
| DoH لا يعطي `200` مع المعرّف | تأكد أن `insecure_enabled` أو `allow_unencrypted_doh` قيمته `true`، وأن المعرّف مضاف في Allowed clients |
| AdGuard لا يبدأ بعد التحويل | تأكد من `sudo chown -R 1000:1000 /srv/docker/adguard` ثم أعد التشغيل |
| AdGuard لا يبدأ بعد الخطوة 11 | شغّل `docker logs adguardhome`؛ غالبًا مسار الشهادة خاطئ أو `port_https` ليس `0` |
| أمر `openssl` للمنفذ 853 لا يطبع شيئًا | شغّل `sudo ss -lntp \| grep ':853'`؛ إن ظهر `127.0.0.1:853` فقط فنفّذ الخطوة 11.3. إن ظهر عنوان السيرفر فراجع جدار حماية المزوّد، ثم السجلات |
| أندرويد لا يتصل | المنفذ `853/tcp` مغلق (جدار الحماية أو لوحة المزوّد)، أو الخطوة 11.3 لم تُنفّذ، أو حقل Allowed clients غير فارغ |
| `ufw: command not found` | `sudo apt install -y ufw` ثم أعد الخطوة 1 |
| `crontab: command not found` | `sudo apt install -y cron && sudo systemctl enable --now cron` |
