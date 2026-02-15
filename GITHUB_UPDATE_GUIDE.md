# 🔧 GitHub Update Guide - Recent Changes Only

## Option 1: Download Full Updated bot.js
The easiest way is to download the complete updated `bot.js` file from the package and replace it on GitHub.

## Option 2: Manual Changes (If you want to edit on GitHub directly)

### Change 1: Rate Limit (Line 24)
```javascript
// OLD:
const RATE_LIMIT_WINDOW = 60000; // 60 seconds

// NEW:
const RATE_LIMIT_WINDOW = 15000; // 15 seconds
```

### Change 2: Error Message (Line 241)
```javascript
// OLD:
return { error: '⏰ Please wait 60 seconds before requesting another number.' };

// NEW:
return { error: '⏰ Please wait 15 seconds before requesting another number.' };
```

### Change 3: No Numbers Message (Line 252)
```javascript
// OLD:
return { error: `❌ No ${COUNTRY_FLAGS[country]} ${COUNTRY_NAMES[country]} numbers available right now. Please try another country.` };

// NEW:
return { error: '❌ Numbers not available. Try another country.' };
```

### Change 4: Remove Country Restriction (Line 477-482)
```javascript
// OLD:
if (!COUNTRY_FLAGS[country]) {
  await bot.sendMessage(chatId, 
    `❌ Invalid country code.\n\n` +
    `Supported countries:\n` +
    `${Object.keys(COUNTRY_FLAGS).map(code => `${COUNTRY_FLAGS[code]} ${code} - ${COUNTRY_NAMES[code]}`).join('\n')}`
  );
  return;
}

// NEW:
// Accept any 2-3 letter country code
if (country.length < 2 || country.length > 3 || !/^[A-Z]+$/.test(country)) {
  await bot.sendMessage(chatId, 
    `❌ Invalid country code format.\n\n` +
    `Use 2 or 3 letter country codes:\n` +
    `Examples: PK, IN, US, ZW, etc.\n\n` +
    `Common countries:\n` +
    `${Object.keys(COUNTRY_FLAGS).map(code => `${COUNTRY_FLAGS[code]} ${code} - ${COUNTRY_NAMES[code]}`).join('\n')}`
  );
  return;
}

// Get flag and name if available, otherwise use defaults
const countryFlag = COUNTRY_FLAGS[country] || '🌍';
const countryName = COUNTRY_NAMES[country] || country;
```

### Change 5: Update Message Text (Line 485)
```javascript
// OLD:
`📝 Adding numbers for ${COUNTRY_FLAGS[country]} ${COUNTRY_NAMES[country]}\n\n` +

// NEW:
`📝 Adding numbers for ${countryFlag} ${countryName}\n\n` +
```

### Change 6: Success Message (Line 510-520)
```javascript
// OLD:
`${COUNTRY_FLAGS[country]} Country: *${COUNTRY_NAMES[country]}*\n\n` +

// NEW:
`${countryFlag} Country: *${countryName}*\n` +
`Code: ${country}\n\n` +
```

### Change 7: Auto-Delete After OTP (Line 800-810)
ADD these lines after logging successful OTP:
```javascript
// AUTO-DELETE: Mark number as deleted after successful OTP
await connection2.query(
  'UPDATE phone_numbers SET deletedAt = NOW(), isAvailable = 0 WHERE number = ?',
  [phoneNumber]
);
console.log(`🗑️ Auto-deleted number ${phoneNumber} after successful OTP`);

// Clear user's assigned number
await connection2.query(
  'UPDATE telegram_users SET currentPhoneNumberId = NULL WHERE telegramId = ?',
  [userId.toString()]
);
```

## ⚡ Quick Summary of Changes:

1. ✅ 60 sec → 15 sec delay
2. ✅ Simple error messages  
3. ✅ Any country codes accepted (not just 10)
4. ✅ Auto-delete numbers after OTP
5. ✅ Broadcast command improvements
6. ✅ Dynamic country display

## 🎯 Recommended: Just Replace bot.js

Instead of manual edits, easiest is:
1. Download `bot.js` from the new package
2. Go to GitHub repo
3. Delete old `bot.js`
4. Upload new `bot.js`
5. Commit!

Railway will auto-deploy! 🚀
