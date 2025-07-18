Project: Ansible Laravel Server
This project provides an Ansible playbook to automatically set up a production-ready server for Laravel applications on a fresh Ubuntu 24 installation.

این پروژه یک اسکریپت Ansible برای راه‌اندازی خودکار یک سرور آماده به کار برای اپلیکیشن‌های لاراول، روی یک اوبونتو ۲۴ خام، فراهم می‌کند.

۱. ساختار نهایی پروژه در گیت‌هاب
Final Project Structure on GitHub

ابتدا، این ساختار را در پوشه server-automation روی کامپیوتر شخصی خود ایجاد کنید و فایل‌ها را با محتوای زیر پر کنید.

First, create this structure in the server-automation folder on your local machine and fill the files with the content below.

server-automation/
├── README.md                 # راهنمای پروژه (فایل زیر)
├── inventory.ini             # تعریف سرور هدف
├── playbook.yml              # اسکریپت اصلی اتوماسیون
├── templates/                # الگوهای فایل‌های کانفیگ
│   ├── docker-compose.yml.j2
│   ├── Dockerfile.j2
│   ├── nginx.conf.j2
│   └── certbot-renew.sh.j2
└── vars/                     # فایل متغیرهای پروژه
    └── main.yml
۲. محتوای فایل‌ها
File Contents

README.md
(این فایل را در ریشه پروژه خود قرار دهید)

Markdown

# Ansible Laravel Server Setup

This playbook automates the setup of a secure, production-ready server for Laravel applications on **Ubuntu 24**. It uses Docker to containerize services (Nginx, PHP 8.3, MySQL 8) and configures SSL using Let's Encrypt (Certbot).

این اسکریپت، فرآیند راه‌اندازی یک سرور امن و آماده به کار برای اپلیکیشن‌های لاراول را روی **اوبونتو ۲۴** خودکار می‌کند. این پروژه از داکر برای ایزوله‌سازی سرویس‌ها (Nginx, PHP 8.3, MySQL 8) استفاده کرده و گواهینامه SSL را با Let's Encrypt (Certbot) پیکربندی می‌کند.

---

## Features | امکانات

-   **Ubuntu 24 LTS** preparation (UFW firewall, basic packages).
-   **Docker & Docker Compose** installation.
-   **Containerized Services:** Nginx, PHP 8.3-FPM, MySQL 8.
-   **Laravel:** Downloads and sets up the latest version.
-   **Certbot:** Automatically obtains and configures a free SSL certificate from Let's Encrypt.
-   **Security:** Sets correct file permissions and configures auto-renewal for SSL.

---

## Prerequisites | پیش‌نیازها

1.  A fresh server running **Ubuntu 24 LTS**. (یک سرور خام با اوبونتو ۲۴)
2.  A **domain name** pointed to your server's IP address. (یک دامنه که به IP سرور اشاره می‌کند)
3.  A **Cloudflare** account managing your domain's DNS. (یک حساب کلودفلر برای مدیریت DNS)
4.  A **GitHub account** and a **Personal Access Token (PAT)** with `repo` scope. (یک حساب گیت‌هاب و یک توکن دسترسی شخصی با دسترسی `repo`)

---

## Step-by-Step Guide | راهنمای قدم به قدم

### Step 1: Prepare Server & Clone Script
### قدم ۱: آماده‌سازی سرور و دانلود اسکریپت

Connect to your new server as `root` and install the necessary tools. Then, clone this repository.
به عنوان کاربر `root` به سرور جدید خود SSH بزنید، ابزارهای لازم را نصب کرده و سپس این مخزن را کلون کنید.

```bash
# Connect to the new server
ssh root@YOUR_NEW_SERVER_IP

# Install Git and Ansible
apt update
apt install git ansible -y

# Create a non-root user and give it sudo privileges
adduser your_user_name
usermod -aG sudo your_user_name

# Clone this automation script
git clone https://github.com/amozeshfarsi/server-automation.git server-automation
cd server-automation
(Note: For the rest of the steps, you can stay logged in as root to run the playbook, or log in as your new user.)
(نکته: برای ادامه مراحل می‌توانید به عنوان root باقی بمانید یا با کاربر جدید خود وارد شوید.)

Step 2: Configure Cloudflare
قدم ۲: پیکربندی کلودفلر
Log in to your Cloudflare account and go to the DNS settings for your domain. Ensure you have two A records pointing to your server's IP, and critically, set their proxy status to "DNS Only" (Grey Cloud) for the SSL certificate generation to succeed.

وارد حساب کلودفلر خود شده و به تنظیمات DNS دامنه بروید. دو رکورد A بسازید که به IP سرور شما اشاره کنند و بسیار مهم: وضعیت پراکسی آنها را روی "DNS Only" (ابر خاکستری ⚪) قرار دهید تا صدور گواهینامه SSL با موفقیت انجام شود.

Type	Name	Content (IP Address)	Proxy Status
A	@	YOUR_NEW_SERVER_IP	DNS Only
A	www	YOUR_NEW_SERVER_IP	DNS Only

صادر کردن به «کاربرگ‌نگار»
Step 3: Configure Variables
قدم ۳: پیکربندی متغیرها
Edit the vars/main.yml file and fill it with your new server and domain information.
فایل vars/main.yml را ویرایش کرده و آن را با اطلاعات سرور و دامنه جدید خود پر کنید.

Bash

nano vars/main.yml
YAML

# vars/main.yml
server_user: "your_user_name"  # The non-root user you created in Step 1
domain_name: "your-new-domain.com"
letsencrypt_email: "your-email@example.com"
# ... set strong database passwords ...
Step 4: Run the Playbook!
قدم ۴: اسکریپت را اجرا کنید!
Execute the Ansible playbook. This single command will provision your entire server.
اسکریپت Ansible را اجرا کنید. این دستور تمام سرور شما را آماده‌سازی می‌کند.

Bash

ansible-playbook -i inventory.ini playbook.yml
This process will take about 5-10 minutes. Go grab a coffee! ☕
این فرآیند حدود ۵ تا ۱۰ دقیقه طول می‌کشد.

Step 5: Finalize Cloudflare & Test
قدم ۵: نهایی‌سازی کلودفلر و تست
After the playbook completes successfully (failed=0), go back to your Cloudflare DNS settings and re-enable the proxy by changing the grey clouds back to orange (Proxied).
پس از اتمام موفقیت‌آمیز اسکریپت (failed=0)، به تنظیمات کلودفلر برگشته و پراکسی را با تغییر ابرهای خاکستری به نارنجی (Proxied) 🔶 دوباره فعال کنید.

Now, open your browser and navigate to https://your-new-domain.com. You should see the Laravel welcome page, fully secured with SSL.
اکنون با مراجعه به آدرس https://your-new-domain.com در مرورگر، باید صفحه خوش‌آمدگویی لاراول را با قفل امنیتی مشاهده کنید.
