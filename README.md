# Android Studio Wireless Debugging (Wi-Fi ADB)

বাংলা গাইড: USB Cable ছাড়া Wi-Fi ব্যবহার করে Android ফোনে Android Studio থেকে অ্যাপ Run ও Debug করার পদ্ধতি।

## 1. প্রয়োজনীয়তা

- Android Studio ইনস্টল করা থাকতে হবে।
- Android ফোন ও কম্পিউটার একই Wi-Fi নেটওয়ার্কে যুক্ত থাকতে হবে।
- ফোনে Developer Options চালু থাকতে হবে।
- Android SDK Platform-Tools (ADB) ইনস্টল থাকতে হবে।

## 2. ফোনে Wireless Debugging চালু করুন

Redmi Note 13 ফোনে নিচের ধাপগুলো অনুসরণ করুন:

1. Settings খুলুন।
2. Developer options-এ যান।
3. Wireless debugging চালু করুন।
4. ফোনের IP Address & Port দেখুন।

উদাহরণ:

```text
IP Address & Port:
192.168.1.101:42155
```

**নোট:** IP Address এবং Port প্রতিবার পরিবর্তিত হতে পারে। তাই ফোনে প্রদর্শিত বর্তমান Port ব্যবহার করতে হবে।

## 3. Android Studio দিয়ে Pair করুন

1. Android Studio খুলুন।
2. উপরের Device Dropdown-এ ক্লিক করুন।
3. `Pair Devices Using Wi-Fi` নির্বাচন করুন।
4. `Pair using pairing code` ট্যাবে ক্লিক করুন।
5. ফোনে যান:

```text
Settings
    └── Developer options
        └── Wireless debugging
            └── Pair device with pairing code
```

6. ফোনে প্রদর্শিত ৬ সংখ্যার Pairing Code Android Studio-তে লিখুন।
7. Pair সফল হলে ফোনটি Android Studio-তে যুক্ত হবে।

## 4. Windows PowerShell-এ ADB ব্যবহার করুন

যদি Terminal-এ নিচের Error আসে:

```text
adb is not recognized as the name of a cmdlet
```

তাহলে ADB-এর ফোল্ডারে যেতে হবে।

### Step 1: Platform-Tools ফোল্ডারে যান

```powershell
cd "$env:LOCALAPPDATA\Android\Sdk\platform-tools"
```

### Step 2: ADB পরীক্ষা করুন

```powershell
.\adb.exe devices
```

### Step 3: Wi-Fi দিয়ে ফোন Connect করুন

ফোনের Wireless Debugging পেজে প্রদর্শিত বর্তমান IP Address & Port ব্যবহার করুন।

```powershell
.\adb.exe connect 192.168.1.101:42155
```

উদাহরণস্বরূপ সফল হলে:

```text
connected to 192.168.1.101:42155
```

### Step 4: Connected Device পরীক্ষা করুন

```powershell
.\adb.exe devices
```

আউটপুট:

```text
List of devices attached
192.168.1.101:42155    device
```

## 5. Android Studio থেকে অ্যাপ Run করুন

1. Android Studio-তে Device Dropdown খুলুন।
2. Redmi Note 13 নির্বাচন করুন।
3. Run Configuration থেকে `app` নির্বাচন করুন।
4. সবুজ Run (▶) বাটনে ক্লিক করুন।
5. অ্যাপ ফোনে Install ও Run হবে।

## 6. ADB Connection Reset করার কমান্ড

যদি `Unavailable on device` Error আসে, PowerShell-এ নিচের কমান্ডগুলো চালান।

প্রথমে Platform-Tools ফোল্ডারে যান:

```powershell
cd "$env:LOCALAPPDATA\Android\Sdk\platform-tools"
```

ADB Disconnect:

```powershell
.\adb.exe disconnect
```

ADB Server বন্ধ করুন:

```powershell
.\adb.exe kill-server
```

ADB Server চালু করুন:

```powershell
.\adb.exe start-server
```

বর্তমান IP Address & Port দিয়ে আবার Connect করুন:

```powershell
.\adb.exe connect 192.168.1.101:42155
```

Device পরীক্ষা করুন:

```powershell
.\adb.exe devices
```

## 7. Troubleshooting

### Problem: adb is not recognized

সমাধান:

```powershell
cd "$env:LOCALAPPDATA\Android\Sdk\platform-tools"
```

তারপর:

```powershell
.\adb.exe devices
```

### Problem: Unavailable on device

সমাধান:

- ফোনে Wireless Debugging চালু আছে কি না পরীক্ষা করুন।
- ফোন ও কম্পিউটার একই Wi-Fi-তে আছে কি না নিশ্চিত করুন।
- ফোনের বর্তমান IP Address & Port ব্যবহার করুন।
- Android Studio থেকে Pair Devices Using Wi-Fi দিয়ে পুনরায় Pair করুন।
- প্রয়োজনে ADB Server Restart করুন।

### Problem: Connection refused

সমাধান:

- ফোনের Wireless Debugging OFF করে আবার ON করুন।
- নতুন IP Address & Port ব্যবহার করুন।
- ADB Disconnect করে আবার Connect করুন।

### Problem: Pairing সফল, কিন্তু Run হচ্ছে না

Pairing Port এবং Debugging Port আলাদা হতে পারে।

- Pairing Port: Pairing Code স্ক্রিনে প্রদর্শিত Port।
- Debugging Port: Wireless Debugging মূল পেজে IP Address & Port-এর পাশে প্রদর্শিত Port।

Pairing এবং Connect করার সময় সঠিক Port ব্যবহার করতে হবে।

## 8. গুরুত্বপূর্ণ ADB Commands

```powershell
# ADB devices check
.\adb.exe devices

# Connect phone over Wi-Fi
.\adb.exe connect IP_ADDRESS:PORT

# Disconnect devices
.\adb.exe disconnect

# Restart ADB server
.\adb.exe kill-server
.\adb.exe start-server

# Connect again
.\adb.exe connect IP_ADDRESS:PORT
```

## Conclusion

Android Studio Wireless Debugging ব্যবহার করে USB Cable ছাড়াই Wi-Fi-এর মাধ্যমে Android ফোনে অ্যাপ Install, Run ও Debug করা যায়।

এই পদ্ধতিতে Redmi Note 13 এবং Windows PC ব্যবহার করে Android Development করা সম্ভব।
