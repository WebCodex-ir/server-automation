# Ansible Laravel Server Setup | راه‌اندازی سرور لاراول با Ansible

**Creator:** Meisam Shadkam ([WebCodexPro])  
**Contact:** [meisamshadkam@gmail.com] | **Website:** [https://webcodex.ir] | **GitHub:** [@WebCodex-ir]

---

## About This Project
### درباره این پروژه

This project was born out of the need to simplify and automate the complex, error-prone process of deploying a production-ready Laravel server. After facing numerous challenges with manual setups—from firewall misconfigurations to complex SSL setups and dependency issues—I decided to codify the entire process into a single, reliable Ansible playbook.

این پروژه از نیازی واقعی سرچشمه گرفت: ساده‌سازی و خودکارسازی فرآیند پیچیده و مستعد خطای راه‌اندازی یک سرور حرفه‌ای برای لاراول. پس از مواجهه با چالش‌های متعدد در راه‌اندازی دستی—از خطاهای پیکربندی فایروال گرفته تا تنظیمات پیچیده SSL و مشکلات وابستگی‌ها—تصمیم گرفتم کل این فرآیند را به یک اسکریپت Ansible قابل اعتماد تبدیل کنم.

### What Does It Do?
### این اسکریپت چه کاری انجام می‌دهد؟

This script transforms a bare-bones **Ubuntu 24** server into a secure, high-performance platform for Laravel applications with a single command. It handles everything from initial server hardening to deploying the Laravel application within a Docker environment.

این اسکریپت یک سرور خام **اوبونتو ۲۴** را با یک دستور به یک پلتفرم امن و بهینه برای اپلیکیشن‌های لاراول تبدیل می‌کند. همه چیز، از آماده‌سازی اولیه سرور گرفته تا استقرار لاراول در یک محیط داکر، توسط این اسکریپت انجام می‌شود.

### Key Benefits
### مزایای کلیدی

* **🚀 Speed:** Deploy a complete server in minutes, not hours.
    **(سرعت:** راه‌اندازی یک سرور کامل در چند دقیقه به جای چند ساعت.)
* **⚙️ Consistency:** Every server is configured identically, eliminating human error.
    **(ثبات:** تمام سرورها به صورت یکسان پیکربندی می‌شوند و خطاهای انسانی حذف می‌گردد.)
* **🔒 Security:** Implements best practices like a non-root user, a configured firewall, and automated SSL.
    **(امنیت:** بهترین شیوه‌های امنیتی مانند کاربر غیر-روت، فایروال و SSL خودکار اجرا می‌شود.)
* **✅ Reliability:** The script is the result of extensive debugging and is designed to run flawlessly on the first try.
    **(قابلیت اطمینان:** این اسکریپت نتیجه عیب‌یابی‌های متعدد است و برای اجرای بی‌نقص در همان تلاش اول طراحی شده است.)

---

## Getting Started | شروع به کار

Follow this guide to deploy your own server.
برای راه‌اندازی سرور خود، این راهنما را دنبال کنید.

### Prerequisites | پیش‌نیازها

1.  A fresh server running **Ubuntu 24 LTS**. (یک سرور خام با اوبونتو ۲۴)
2.  A **domain name** pointed to your server's IP address. (یک دامنه که به IP سرور اشاره می‌کند)
3.  A **Cloudflare** account managing your domain's DNS. (یک حساب کلودفلر برای مدیریت DNS)
4.  A **GitHub account** and a **Personal Access Token (PAT)** with `repo` scope to clone your private repository. (یک حساب گیت‌هاب و یک توکن دسترسی شخصی)

---

## Step-by-Step Guide | راهنمای قدم به قدم

### Step 1: Prepare Server & Create User
### قدم ۱: آماده‌سازی سرور و ساخت کاربر

Connect to your new server as `root`. First, install the essential tools, then create a new non-root user for running the project.

به عنوان کاربر `root` به سرور جدید خود SSH بزنید. ابتدا ابزارهای لازم را نصب کرده و سپس یک کاربر جدید غیر-روت برای اجرای پروژه بسازید.

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
