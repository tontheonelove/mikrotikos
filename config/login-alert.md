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


## 🧩 Step 3: สร้าง Script แจ้งเตือน

```
# 1. แก้ชื่อให้ตรงกับชื่อ Script ของคุณใน Winbox
:local ScriptName "LoginAlert"; 

:local BotToken "YOUR BOT TOKEN";
:local ChatID "YOUR BOT ID";
:local DeviceName [/system identity get name];

# ดึง ID ล่าสุดจาก Comment (ต่อให้ Reboot ค่านี้ก็ไม่หาย)
:local LastID [/system script get [find name=$ScriptName] comment];

# ค้นหาใน Log Topic account
:local logEvents [/log find where topics~"account"];

:if ([:len $logEvents] > 0) do={
    :local currentLogID [:pick $logEvents ([:len $logEvents]-1)];

    if ($currentLogID != $LastID) do={
        
        # บันทึก ID ลง Comment ของ Script นี้
        /system script set [find name=$ScriptName] comment=$currentLogID;
        
        :local logMessage [/log get $currentLogID message];
        :local logTime [/log get $currentLogID time];
        
        :local encodedMsg "";
        :for i from=0 to=([:len $logMessage] - 1) do={
            :local char [:pick $logMessage $i];
            :if ($char = " ") do={ :set encodedMsg ($encodedMsg . "%20") } else={ :set encodedMsg ($encodedMsg . $char) }
        }
        
        :local tgText "[SEC-ALERT]%20Device:%20$DeviceName%0ATime:%20$logTime%0ADetail:%20$encodedMsg";
        :local tgUrl "https://api.telegram.org/bot$BotToken/sendMessage?chat_id=$ChatID&text=$tgText";
        
        /tool fetch url=$tgUrl keep-result=no check-certificate=no;
    }
}
```


## 🧩 Step 4: ตั้ง Scheduler

```
/system scheduler
add interval=10s name=schedule1 on-event=LoginAlert policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
```


# Result 

<img width="620" height="270" alt="image" src="https://github.com/user-attachments/assets/a16826e6-7295-466f-aae8-e1861c671689" />




