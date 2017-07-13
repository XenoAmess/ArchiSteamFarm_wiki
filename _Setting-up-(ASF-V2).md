# Setting up

If you arrived here for the first time, welcome!

ASF is a console app, which means that program itself doesn't have a friendly GUI (yet) that you're in general used to. This means that ASF is standalone executable file that doesn't need installation, but requires configuration prior to launching it.

***

## Quick configuration

This is an option for those who don't want to learn how ASF works in-depth and just want to make it working. It can be achieved in less than 5 minutes. This guide is compacted to the maximum - if something is unclear, visit **[detailed configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up#detailed-configuration)** instead.

- Install latest **[.NET framework](https://www.microsoft.com/en-us/download/details.aspx?id=53345)**. No, don't skip this step.
- Download **ASF.zip** located **[here](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. Don't download source code or anything else (as you don't need it).
- Unpack archive to wherever you want.
- Launch ASF-ConfigGenerator.exe.
- Follow the tutorial (if you want to)
- Hit **+** button on the top.
- Name your bot however you want, I suggest ```YourNickname```, ```1```, ```Primary``` or ```Main```.
- Put your steam login in ```SteamLogin``` (optional - if you don't want to put it on each ASF start).
- Put your steam password in ```SteamPassword``` (optional - if you don't want to put it on each ASF start).
- If you're using Steam parental PIN to protect your account, put it in ```SteamParentalPIN```.
- Switch ```Enabled``` property from ```false``` to ```true```.
- Close ASF-ConfigGenerator.
- Launch ASF.exe.
- If you use SteamGuard or 2FA, input tokens in the console once asked, and confirm with enter.
- ASF should be working. Check out console output to see what is happening.

If you did everything correctly, you should see that ASF is working nicely. You can keep using ASF like this, or head over to detailed configuration below to fully understand what ASF offers, together with every configurable option.

***

## Detailed configuration

This is detailed instruction dedicated for both less advanced, and more advanced users who would like to use ASF. By reading it, you'll learn how to configure and use ASF.

***

First step is ensuring that your OS can launch ASF properly. ASF is written in C# and typically uses framework functions that might not be available in your OS right away. If you're using Windows OS, make sure that you have **[latest .NET framework](https://www.microsoft.com/en-us/download/details.aspx?id=53345)** installed. .NET framework is runtime used by ASF for execution, and for flawless experience you must ensure that your Windows OS has at least minimum supported .NET framework version used by ASF - currently 4.6.1, but it might change with future releases.

If you're not using Windows OS, you should install **[latest Mono](https://github.com/JustArchi/ArchiSteamFarm/wiki/Mono)** that supports many other OSes - including Linux and OS X (but not only). After installing mono you can start all ASF executables by executing ```mono ExecutableName.exe``` from your favourite terminal/shell. Like with .NET, you must ensure that your Mono is at least in the minimum supported version used by ASF, currently 4.6 but it might also change with future releases.

Not using minimum **required** runtime version automatically leads to issues and lack of support - don't ask for support and don't report any bugs if you're not using supported runtime, noone of them will be fixed - you should fix yourself instead.

***

Second step is downloading latest stable release of ASF, which is located **[here](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. Find **ASF.zip** link on the bottom of the page to start download process. ASF comes in zipped archive format, therefore prior to launching it you should unpack the archive, using any tool you want to. Personally I suggest **[7-zip](http://www.7-zip.org/)**, but any tool capable of reading ZIP files will do.

After unpacking archive you should notice executable files **ASF.exe**, **ASF-ConfigGenerator.exe** and **config** directory. Prior to launching ASF you need to configure it. Configuration is really easy process, as long as you read whole documentation carefully and pay attention.

You can configure ASF either manually, by creating required JSON configuration files with proper content, or by using **ASF-ConfigGenerator** - graphical config generator, which includes tutorial and helps you with doing that task.

Now go visit **[configuration](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration)** page and continue reading when you're done :+1:.

***

After you're done with configuration part, you should start **ASF.exe** executable, by double clicking it.

If you did everything correctly, you should notice that ASF starts working and logs in to steam using credentials you provided in the config. If your account needs extra steps to unlock, such as **SteamGuard** or **TwoFactorAuthentication**, ASF will ask you for extra code, that you should type in the console. This has to be done only once, similar like in steam client.

Congratulations, you've just finished basic steps and your ASF is functional! Remember that ASF doesn't require and doesn't interfere in any way with Steam client, which means that you can be logged in to Steam client as your primary account, and launch ASF at the same time, for any number of accounts, including your main one (if needed).

***

## Need more help?

Head over to the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)** :+1: 

### Legacy versions

For legacy ASF versions, including V0.X and V1.X, you should visit **[Setting up (Legacy)](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up-(Legacy))**