---
layout: post
title: Setup Windows as a Time Server
description: I have an Ubuntu VM running under Hyper-V on Windows 10. Time synchronisation is enabled, however after hybernation, or suspending my laptop, time is not resynched to my VM. 
imgurl: /images/blog-banner.jpg
---

I have an Ubuntu VM running under Hyper-V on Windows 10. Time synchronisation is enabled, however after hybernation, or suspending my laptop, time is not resynched to my VM.

To make ubuntu synchronised more regularly, I have setup my Windows host as a time (NTP) server, enabled firewall rules and created a cron job to check time every minute.

I created the cron job using webmin. See below for the command to use to update time from the time server.

Note, Microsoft explicitly states it does not guarantee the accuracy of the service, as it is used for Kerberos purposes (which only requires synchronisation accuracy of 5 minutes). However I have found it to work reasonable for my dev machine. For more info, see Microsoft KB939322.

First, to setup the time server, make the following registry edits to enable the NTP server.

Enable the W32Time service to announce itself as a NTP server. Open the registry and navigate to:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32TimeConfig1
```

Set the “AnnounceFlags” from 10 (or ‘a’ in hex) to 5.

Next, enable the NTPServer. Open the registry and navigate to:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer
```

Set “Enabled” to 1.

Now, restart the Time service. Open command prompt as administrator and run the following:

```
C:>net stop w32time && net start w32time
```

To confirm the settings were changed correctly, run the following (in an admin command prompt):

```
C:>w32tm /query /configuration
```

Confirm you should see something similar to the following:

```
[Configuration]

EventLogFlags: 2 (Local)
AnnounceFlags: 5 (Local)
TimeJumpAuditOffset: 28800 (Local)
MinPollInterval: 10 (Local)
MaxPollInterval: 15 (Local)
MaxNegPhaseCorrection: 54000 (Local)
MaxPosPhaseCorrection: 54000 (Local)
MaxAllowedPhaseOffset: 1 (Local)

FrequencyCorrectRate: 4 (Local)
PollAdjustFactor: 5 (Local)
LargePhaseOffset: 50000000 (Local)
SpikeWatchPeriod: 900 (Local)
LocalClockDispersion: 10 (Local)
HoldPeriod: 5 (Local)
PhaseCorrectRate: 1 (Local)
UpdateInterval: 360000 (Local)
[TimeProviders]

NtpClient (Local)
DllName: C:Windowssystem32w32time.dll (Local)
Enabled: 1 (Local)
InputProvider: 1 (Local)
AllowNonstandardModeCombinations: 1 (Local)
ResolvePeerBackoffMinutes: 15 (Local)
ResolvePeerBackoffMaxTimes: 7 (Local)
CompatibilityFlags: 2147483648 (Local)
EventLogFlags: 1 (Local)
LargeSampleSkew: 3 (Local)
SpecialPollInterval: 604800 (Local)
Type: NTP (Local)
NtpServer: time.windows.com,0x9 (Local)

NtpServer (Local)
DllName: C:Windowssystem32w32time.dll (Local)
Enabled: 1 (Local)
InputProvider: 0 (Local)
AllowNonstandardModeCombinations: 1 (Local)

VMICTimeProvider (Local)
DllName: C:WindowsSystem32vmictimeprovider.dll (Local)
Enabled: 1 (Local)
InputProvider: 1 (Local)
```

The NTP service should now be setup. The final step is to allow the service through the firewall.

Open Windows Firewall Settings. Click Inbound Rules. Right-cick and select “New Rule…”.

Select “Port”. Click Next.

Select “UDP”, and Specific local ports: “123”.
Click Next.

Make sure “Allow the connection” is selected, then click Next.

Enter the profiles to setup the rule (for example, only Private and Domain).

Click Next

Enter the name (or your preferred name): w32time – ntp server. Click Finish.

You should now be able to access the time server.
Here is the command to test it from Ubuntu:

```
$ sudo ntpdate [ServerName]
```

You should see something like this:

```
 7 Jan 14:10:38 ntpdate[3328]: adjust time server 192.168.0.2 offset -0.000822 sec
```
