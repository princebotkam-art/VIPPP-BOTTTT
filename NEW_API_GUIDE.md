# 🔄 New API Integration Guide

## 🆕 API Details:

### **New API Endpoint:**
```
URL: http://147.135.212.197/crapi/st/viewstats
Token: RVZUQ0pBUzR5d3NZgYuPiEN0hkRoYpVXiE6BVnJRiVtIlohqU4hmaw==
```

### **Request Format:**
```
GET http://147.135.212.197/crapi/st/viewstats?token=XXX&records=100
```

### **Response Format:**
```json
[
  ["WhatsApp", "263782230040", "# Akun WhatsApp Business... Kode 401-442...", "2026-02-15 04:48:30"],
  ["WhatsApp", "263776198555", "# Your WhatsApp... code 644-745...", "2026-02-15 03:18:24"],
  ["WhatsApp", "263784445626", "# Kode WhatsApp... 706-924...", "2026-02-15 03:04:07"],
  ["WhatsApp", "263773457055", "# Your WhatsApp code 499-152...", "2026-02-15 00:24:54"],
  ["Facebook", "263775883857", "Facebook Your code is 568035...", "2026-02-15 00:20:44"]
]
```

**Array Format:**
- Index 0: Service (WhatsApp, Facebook, etc.)
- Index 1: Phone Number
- Index 2: SMS Message (contains OTP)
- Index 3: Timestamp

---

## 🔍 How Bot Processes:

### **Step 1: Fetch All Records**
```javascript
GET /viewstats?token=XXX&records=100
→ Returns last 100 SMS messages
```

### **Step 2: Find User's Number**
```javascript
User number: +263782230040

Bot searches in array:
- record[1] contains "263782230040"?
- Match! ✅
```

### **Step 3: Extract OTP from Message**
```javascript
Message: "# Akun WhatsApp... Kode 401-442..."

Bot patterns:
- XXX-XXX format → Finds "401-442" ✅
- XXXXXX format → Finds "401442"
- "code: XXX-XXX" → Finds code
- "is XXX-XXX" → Finds code

Result: OTP = "401-442" ✅
```

---

## 📊 OTP Pattern Examples:

### **Pattern 1: XXX-XXX (with dash)**
```
Message: "Your WhatsApp code 401-442"
Extracted: 401-442 ✅
```

### **Pattern 2: XXXXXX (no dash)**
```
Message: "Your code is 568035"
Extracted: 568035 ✅
```

### **Pattern 3: Code keyword**
```
Message: "code: 644-745"
Extracted: 644-745 ✅
```

### **Pattern 4: Is keyword**
```
Message: "Your code is 499-152"
Extracted: 499-152 ✅
```

---

## ✅ Advantages of New API:

### **1. Batch Retrieval**
```
Old API: Check one number at a time
New API: Gets 100 recent messages ✅
         More efficient
```

### **2. Multiple Services**
```
Supports:
- WhatsApp ✅
- Facebook ✅
- Telegram
- Other services
```

### **3. Recent History**
```
Shows last 100 messages
Better chance of finding OTP
Timestamps included
```

---

## 🔧 Configuration:

### **Environment Variables:**
```bash
OTP_API_URL=http://147.135.212.197/crapi/st/viewstats
OTP_API_TOKEN=RVZUQ0pBUzR5d3NZgYuPiEN0hkRoYpVXiE6BVnJRiVtIlohqU4hmaw==
```

### **Railway Setup:**
```
1. Go to bot service → Variables
2. Update:
   OTP_API_URL=http://147.135.212.197/crapi/st/viewstats
   OTP_API_TOKEN=RVZUQ0pBUzR5d3NZgYuPiEN0hkRoYpVXiE6BVnJRiVtIlohqU4hmaw==
3. Save
4. Bot auto-restarts ✅
```

---

## 📝 Example Flow:

### **User Requests OTP:**
```
User number: +263782230040
User clicks: GET OTP CODE
```

### **Bot Makes API Call:**
```
GET http://147.135.212.197/crapi/st/viewstats?token=XXX&records=100

Response: [
  ["WhatsApp", "263782230040", "code 401-442", "2026-02-15 04:48:30"],
  ... 99 more records
]
```

### **Bot Processes:**
```
1. Loop through 100 records
2. Find record where number = "263782230040"
3. Extract message: "code 401-442"
4. Parse OTP: "401-442"
5. Return to user ✅
```

### **User Sees:**
```
✅ OTP RECEIVED!
🔐 CODE: 401-442

Service: WhatsApp
Time: 2026-02-15 04:48:30
```

---

## 🎯 Smart Features:

### **1. Flexible Pattern Matching**
```javascript
Patterns supported:
- 401-442 (XXX-XXX)
- 401442 (XXXXXX)
- code: 401-442
- code 401-442
- is 401-442
- Your code is 401-442
```

### **2. Last 8 Digits Matching**
```javascript
API number: 263782230040
User number: +263782230040

Match on: "82230040" (last 8 digits) ✅
Handles + sign difference
```

### **3. Multiple Services**
```javascript
WhatsApp: ✅ Supported
Facebook: ✅ Supported
Telegram: ✅ Supported
Any service in API: ✅ Works
```

---

## 🔍 Debugging:

### **Console Logs:**
```
✅ Found OTP for +263782230040: 401-442
   Service: WhatsApp
   Timestamp: 2026-02-15 04:48:30

OR

❌ No OTP found for +263773457055 in 100 records
```

### **If No OTP Found:**
```
Possible reasons:
1. SMS not yet received by API
2. Number doesn't match (check format)
3. OTP older than last 100 messages
4. Pattern not recognized (add new pattern)
```

---

## 📊 API Response Example (Full):

```json
[
  [
    "WhatsApp",
    "263782230040",
    "# Akun WhatsApp Business Anda sedang didaftarkan di perangkat baru\n\nJangan bagikan kode dengan siapa pun\n\nKode WhatsApp Business Anda: 401-442\n\nrJbAXP1KV",
    "2026-02-15 04:48:30"
  ],
  [
    "WhatsApp",
    "263776198555",
    "# Your WhatsApp Business account is being registered on a new device\n\nDo not share this code with anyone\n\nYour WhatsApp Business code: 644-745\n\nrJbAXP1KV",
    "2026-02-15 03:18:24"
  ],
  [
    "WhatsApp",
    "263784445626",
    "# Kode WhatsApp Business: 706-924\n\nJangan bagikan kode ini dengan orang lain\n\nrJbAXP1KV",
    "2026-02-15 03:04:07"
  ]
]
```

---

## ✅ Testing:

### **Test 1: Known Number**
```
Number in API: 263782230040
Bot search: +263782230040
Expected: OTP found ✅
```

### **Test 2: Pattern Recognition**
```
Message: "Kode 401-442"
Expected: Extract "401-442" ✅

Message: "code is 568035"
Expected: Extract "568035" ✅
```

### **Test 3: No Match**
```
Number: +923001234567
Not in API records
Expected: "No OTP found" message ✅
```

---

## 🚀 Deployment:

### **Update Steps:**
```
1. Update .env or Railway variables:
   OTP_API_URL=http://147.135.212.197/crapi/st/viewstats
   OTP_API_TOKEN=RVZUQ0pBUzR5d3NZgYuPiEN0hkRoYpVXiE6BVnJRiVtIlohqU4hmaw==

2. Deploy new bot.js

3. Test with real number

4. Monitor logs for success/errors

5. Done! ✅
```

---

## 💡 Pro Tips:

### **1. Records Parameter**
```
?records=100 → Last 100 messages
Can increase if needed:
?records=200 → Last 200 messages
```

### **2. Number Matching**
```
Uses last 8 digits for matching
More reliable than full number
Handles international format differences
```

### **3. Service Info**
```
Bot now shows which service:
"Service: WhatsApp"
"Service: Facebook"
Helpful for users
```

---

## ✅ Summary:

**New API:**
- ✅ Batch retrieval (100 records)
- ✅ Multiple services supported
- ✅ Flexible OTP patterns
- ✅ Timestamp included
- ✅ More efficient

**Bot Integration:**
- ✅ Smart pattern matching
- ✅ Last 8 digit matching
- ✅ Service display
- ✅ Better error handling
- ✅ Detailed logging

**Ready to use!** 🎉
