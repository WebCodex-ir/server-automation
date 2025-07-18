# Ansible Laravel Server Setup | راه‌اندازی سرور لاراول با Ansible

**Creator:** Meisam Shadkam ([WebCodexPro])  
**Contact:** [meisamshadkam@gmail.com] | **Website:** [https://webcodex.ir] | **GitHub:** [@WebCodex-ir]

---

## About This Project
### درباره این پروژه

This project was born out of the need to simplify and automate the complex, error-prone process of deploying a production-ready Laravel server. After facing numerous challenges with manual setups—from firewall misconfigurations to complex SSL setups and dependency issues—I decided to codify the entire process into a single, reliable Ansible playbook.

این پروژه از نیازی واقعی سرچش-مه گرفت: ساده‌سازی و خودکارسازی فرآیند پیچیده و مستعد خطای راه‌اندازی یک سرور حرفه‌ای برای لاراول. پس از مواجهه با چالش‌های متعدد در راه‌اندازی دستی—از خطاهای پیکربندی فایروال گرفته تا تنظیمات پیچیده SSL و مشکلات وابستگی‌ها—تصمیم گرفتم کل این فرآیند را به یک اسکریپت Ansible قابل اعتماد تبدیل کنم.

### What Does It Do?
### این اسکریپت چه کاری انجام می‌دهد؟

This script transforms a bare-bones **Ubuntu 24** server into a secure, high-performance platform for Laravel applications with a single command. It handles everything from initial server hardening to deploying the Laravel application within a Docker environment.

این اسکریپت یک سرور خام **اوبونتو ۲۴** را با یک دستور به یک پلتفرم امن و بهینه برای اپلیکیشن‌های لاراول تبدیل می‌کند. همه چیز، از آماده‌سازی اولیه سرور گرفته تا استقرار لاراول در یک محیط داکر، توسط این اسکریپت انجام می‌شود.

### Key Benefits
### مزایای کلیدی

* **🚀 Speed:** Deploy a complete server in minutes, not hours.
* **⚙️ Consistency:** Every server is configured identically, eliminating human error.
* **🔒 Security:** Implements best practices like a non-root user, a configured firewall, and automated SSL.
* **✅ Reliability:** The script is the result of extensive debugging and is designed to run flawlessly on the first try.

---

## Getting Started (English Guide)

### Prerequisites

1.  A fresh server running **Ubuntu 24 LTS**.
2.  A **domain name** pointed to your server's IP address.
3.  A **Cloudflare** account managing your domain's DNS.
4.  A **GitHub account** and a **Personal Access Token (PAT)** with `repo` scope to clone your private repository.

### Step-by-Step Guide

#### Step 1: Prepare Server & Create User
Connect to your new server as `root`. First, install the essential tools, then create a new non-root user for running the project.
```bash
# Connect to the new server
ssh root@YOUR_NEW_SERVER_IP

# Install Git and Ansible
apt update
apt install git ansible -y

# Create a non-root user (replace 'webcodex' with your desired username)
adduser webcodex
usermod -aG sudo webcodex

# Exit and log back in as the new user to continue
exit
ssh webcodex@YOUR_NEW_SERVER_IP
````

#### Step 2: Configure Cloudflare

Log in to Cloudflare. Create two `A` records for your domain (`@` and `www`) pointing to your server's IP. **Critically, set their proxy status to "DNS Only" (Grey Cloud) ⚪**. This is temporary and required for SSL generation.

#### Step 3: Clone & Configure the Script

Clone the automation script from your GitHub repository onto the server.

```bash
# Clone your automation script repository (use your PAT as the password)
git clone https://github.com/amozeshfarsi/server-automation.git server-automation
cd server-automation

# Edit the variables file
nano vars/main.yml
```

In `vars/main.yml`, update `server_user` to the username you created in Step 1 (e.g., `webcodex`), and fill in your domain, email, IP, and new database passwords.

#### Step 4: Run the Playbook\!

Execute the Ansible playbook. You will be asked for your user's `sudo` password.

```bash
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
```

The process will take about 5-10 minutes.

#### Step 5: Finalize Cloudflare & Test

After the playbook completes successfully (`failed=0`), go back to Cloudflare and **re-enable the proxy by changing the grey clouds back to orange (Proxied) 🔶**.

Now, navigate to `https://your-new-domain.com`. Your server is live\!

-----

## شروع به کار (راهنمای فارسی)

### پیش‌نیازها

۱. یک سرور خام با **اوبونتو ۲۴**.
۲. یک **دامنه** که به IP سرور اشاره می‌کند.
۳. یک حساب **کلودفلر** برای مدیریت DNS.
۴. یک حساب **گیت‌هاب** و یک **توکن دسترسی شخصی (PAT)**.

### راهنمای قدم به قدم

#### قدم ۱: آماده‌سازی سرور و ساخت کاربر

به عنوان کاربر `root` به سرور جدید خود SSH بزنید. ابتدا ابزارهای لازم را نصب کرده و سپس یک کاربر جدید غیر-روت برای اجرای پروژه بسازید.

```bash
# اتصال به سرور جدید
ssh root@YOUR_NEW_SERVER_IP

# نصب گیت و Ansible
apt update
apt install git ansible -y

# ساخت کاربر غیر-روت (نام webcodex را با نام دلخواه جایگزین کنید)
adduser webcodex
usermod -aG sudo webcodex

# خارج شده و با کاربر جدید دوباره وارد شوید
exit
ssh webcodex@YOUR_NEW_SERVER_IP
```

#### قدم ۲: پیکربندی کلودفلر

وارد حساب کلودفلر شوید. دو رکورد `A` برای دامنه اصلی (`@`) و `www` بسازید که به IP سرور شما اشاره کنند. **مهم: وضعیت پراکسی را روی "DNS Only" (ابر خاکستری ⚪) قرار دهید.** این یک مرحله موقتی و برای صدور گواهینامه SSL الزامی است.

#### قدم ۳: دانلود و پیکربندی اسکریپت

اسکریپت اتوماسیون را از گیت‌هاب خود روی سرور کلون کنید.

```bash
# کلون کردن مخزن اسکریپت (از توکن به عنوان پسورد استفاده کنید)
git clone https://github.com/amozeshfarsi/server-automation.git server-automation
cd server-automation

# ویرایش فایل متغیرها
nano vars/main.yml
```

در فایل `vars/main.yml`، متغیر `server_user` را به نام کاربری که در قدم ۱ ساختید (مثلاً `webcodex`) تغییر داده و بقیه اطلاعات (دامنه، ایمیل، IP، و رمزهای دیتابیس) را به‌روز کنید.

#### قدم ۴: اسکریپت را اجرا کنید\!

اسکریپت Ansible را اجرا کنید. از شما رمز عبور `sudo` کاربرتان پرسیده خواهد شد.

```bash
ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
```

این فرآیند حدود ۵ تا ۱۰ دقیقه طول می‌کشد.

#### قدم ۵: نهایی‌سازی کلودفلر و تست

پس از اتمام موفقیت‌آمیز اسکریپت (`failed=0`)، به کلودفلر برگشته و **پراکسی را با تغییر ابرهای خاکستری به نارنجی (Proxied) 🔶 دوباره فعال کنید.**

اکنون به آدرس `https://your-new-domain.com` بروید. سرور شما فعال است\!

```
```
