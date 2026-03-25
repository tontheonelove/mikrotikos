#  Mikrotik Login Alert to Telegrams

## 🧩 Step 1: Create Telegram Bot

1. goto  Telegram

2. find @BotFather

3. chat /start → /newbot

4. set botname and copy BOT TOKEN


```
Example:

123456789:AAxxxxxxxxxxxxxxxxxxxx
```

## 🧩 Step 2: หา Chat ID

ค้นหา @userinfobot

กด start → จะได้ Chat ID

```
123456789
```


## 🧩 Step 3: ทดสอบส่งข้อความจาก MikroTik

เข้า Winbox → New Terminal 

```
/tool fetch url="https://api.telegram.org/bot<BOT_TOKEN>/sendMessage?chat_id=<CHAT_ID>&text=TEST_MIKROTIK" keep-result=no
```

ถ้าข้อความเด้งใน Telegram = ผ่าน ✅


## 🧩 Step 4: เปิด Log สำหรับ Login

```
/system logging add topics=account
```

## 🧩 Step 5: สร้าง Script แจ้งเตือน

```
/system script add name=telegram-login-alert policy=read,write,test source="
:local user \$user;
:local address \$address;
:local msg (\"MikroTik Login: \" . \$user . \" from \" . \$address);

/tool fetch url=(\"https://api.telegram.org/bot<BOT_TOKEN>/sendMessage?chat_id=<CHAT_ID>&text=\" . \$msg) keep-result=no;
"
```

## 🧩 Step 6: ใช้ Log Trigger Script

```
/system script add name=check-login source="
:foreach i in=[/log find where topics~\"account\"] do={
    :local msg [/log get \$i message];
    
    :if (\$msg~\"logged in\") do={
        /system script run telegram-login-alert;
    }
}
"
```

## 🧩 Step 7: ตั้ง Scheduler

```
/system scheduler add name=check-login interval=10s on-event=check-login
```






