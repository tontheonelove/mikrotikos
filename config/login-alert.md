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

พิม hello ใน chat ของ bot 

จากนั้นพิมที่ browser  https://api.telegram.org/bot<Yourbot>/getUpdates

จะได้ Chat ID

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
# ==========================================
# ????????????????: ??? Token ??? ID ??????
# ==========================================
:local BotToken " YOUR BOT TOKEN";
:local ChatID "YOUR CHAT ID";
:local DeviceName [/system identity get name];

:global LastLoginLogID;

# ค้นหา Log ที่เป็นการ Login จริงๆ (ระบุว่าต้องมีคำว่า user นำหน้า)
:local logEvents [/log find where message~"user .* logged in" or message~"login failure"];

:if ([:len $logEvents] > 0) do={
    :local currentLogID [:pick $logEvents ([:len $logEvents]-1)];

    :if ($currentLogID != $LastLoginLogID) do={
        :set LastLoginLogID $currentLogID;
        
        :local logMessage [/log get $currentLogID message];
        :local logTime [/log get $currentLogID time];
        
        # --- ???????????????????????? %20 ??????????????????? ---
        :local encodedMsg "";
        :for i from=0 to=([:len $logMessage] - 1) do={
            :local char [:pick $logMessage $i];
            :if ($char = " ") do={
                :set encodedMsg ($encodedMsg . "%20");
            } else={
                :set encodedMsg ($encodedMsg . $char);
            }
        }
        
        # ????????? URL (??? %20 ????????????????????? ????)
        :local tgText "??%20MikroTik%20Alert%0ADevice:%20$DeviceName%0ATime:%20$logTime%0ADetail:%20$encodedMsg";
        :local tgUrl "https://api.telegram.org/bot$BotToken/sendMessage?chat_id=$ChatID&text=$tgText";
        
        # ????????? (??????????????? Certificate ???????????????? v7)
        /tool fetch url=$tgUrl keep-result=no check-certificate=no;
    }
}
```


## 🧩 Step 6: ตั้ง Scheduler

```
/system scheduler add name=check-login interval=10s on-event=check-login
```






