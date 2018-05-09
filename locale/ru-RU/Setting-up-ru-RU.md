# Установка

Если вы пришли сюда впервые - Добро Пожаловать! Рады видеть что ещё один путник заинтересован в нашем проекте, но просим вас не забывать, что с большой силой приходит большая ответственность - ASF может сделать в Steam многое, но только если **вам не лень учиться, как им пользоваться**. Кривая обучения довольно крутая, и поэтому от вас ожидается что вы прочтёте эту wiki, которая подробно описывает как всё работает. Если вы ищете программу для фарма в Steam, которая не требует чтения и понимания - воспользуйтесь **[Idle Master](https://www.steamidlemaster.com)**, потому что никто не будет делать за вас домашнее задание по чтению wiki.

Если вы всё ещё читаете, значит вас не напугало вступление выше, и это радует. Конечно, если вы не пропустили его, в противном случае у вас скоро будут **[большие проблемы](https://www.youtube.com/watch?v=WJgt6m6njVw)**... Anyway, ASF is a console app, which means that the program itself doesn't have a friendly GUI that you're in general used to. ASF was mainly supposed to be run on servers, so it acts as a service (daemon) and not a desktop app.

This however doesn't mean that you can't use it on your PC or using it is in some way more complicated than usual, nothing like that. ASF is a standalone program that doesn't need installation, and works out of the box right away, but requires configuration prior to becoming useful. Configuration is telling ASF what it should in fact do after you launch it. If you launch it without configuration, then ASF won't do anything, simple.

* * *

## Quick video setup

If you absolutely hate reading and you'd like to watch a video instead, then you can take a look at the one recorded by **[@GamingTaylor](https://www.youtube.com/channel/UCTjrsQgjZmBzYzWaAh0zI3Q)** under **[this link](https://www.youtube.com/watch?v=gi2UjXtGWgc)**. Please note that you should still refer to the wiki for further explanation and up-to-date setting up guide. While we consider YouTube video as a good material for actually showing how things are configured and launched, we can't easily update it when things are changed, so it should be a reference material only. If you care about detailed explanation, documentation and complete setup, then you should continue reading our **[OS-specific setup](#os-specific-setup)** instead, using YouTube video as optional reference material only. Still, we note it here, as it can be useful in **some** places, but we recommend reading our wiki over watching anyway.

* * *

## OS-specific setup

In general, here is what we'll do in the next few minutes:

- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in appropriate OS-specific variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm` if you're on Linux/OS X).
- **[Configure ASF](Configuration)**.
- Launch ASF and see the magic.

Sounds simple enough, right? So let's get through it.

* * *

### .NET Core prerequisites

First step is ensuring that your OS can even launch ASF properly. ASF is written in C#, based on .NET Core and might require native libraries that are not available on your platform yet. Depending on whether you use Windows, Linux or OS X, you will have different requirements, although all of them are listed in **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** document that you should follow. Simply follow the instructions, there is a chance that you already have all required libraries, but you should double check.

For example on Windows, all you need to do is downloading and installing `Microsoft Visual C++ 2015 Redistributable Update 3 RC`, which **could be even already installed by some other game/software that you're using**. On Linux, you have a list of libraries that can be obtained with `apt-get install` or any other package manager that you're using for your distribution. OS X doesn't have any mandatory dependencies for now, but it might change in the future.

Keep in mind that you don't need to do anything else for OS-specific builds, especially installing .NET Core SDK or even runtime, since OS-specific package includes all of that already. You need only .NET Core prerequisites (dependencies) to run .NET Core runtime included in ASF.

Since it might be quite difficult to extract the info you're looking for, we listed required dependencies also here, but please refer to original .NET Core source in case dependencies change in the future.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- If you're running Windows 7, ensure that all Windows updates are already installed, especially Service Pack 1. Do this before trying to install above package.

It's possible that redist package was already installed by some other software/game you're using, but you should still double-check by running the installer to be sure. ASF won't run without this dependency being present.

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Package name depends on the distribution, we listed most common ones. You should obtain them with native package manager for your OS (such as `apt-get` for Debian or `yum` for CentOS).

- libunwind8 (libunwind)
- liblttng-ust0 (lttng-ust)
- libcurl3 (libcurl)
- libssl1.0.2 (libssl, openssl-libs, latest 1.0.X version for your distribution)
- libuuid1 (libuuid)
- libkrb5-3 (krb5-libs)
- libicu57 (libicu, latest version for your distribution)
- zlib1g (zlib)

At least a few of those should be already natively available on your system (such as zlib1g that is required in almost every Linux distro today).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites)**:

- None for now, although you might need to **[increase the maximum open file limit](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x#increase-the-maximum-open-file-limit)**. Shouldn't be required for ASF alone, but keep that in mind if you encounter any issues.

* * *

### Downloading

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](Compatibility)** section. ASF is also able to run on OSes that we're not building OS-specific package for, such as **32-bit Windows**, head over to **[generic setup](#generic-setup)** for that.

![Assets](https://i.imgur.com/Ym2xPE5.png)

Once you get your package and extract the zip file (we recommend using **[7-zip](https://www.7-zip.org)**), you'll have a huge mess of folders and files. Don't worry, we'll clean it up in a second.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm`, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which might lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

    C:\ASF (where you put your own things)
        ├── ASF shortcut.lnk (optional)
        ├── Config shortcut.lnk (optional)
        ├── Commands.txt (optional)
        ├── MyExtraScript.bat (optional)
        ├── ... (any other files of your choice, optional)
        └── Core (dedicated to ASF only, where you extract the archive)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

This is also a structure we'd recommend, so you don't need to go through a massive number of files and folders included in ASF, since for usage you only need a shortcut to config folder and main binary.

Okay, we'll now prepare ASF folder for usage. If you want to, you can now skip to the next step, since cleaning up ASF structure is not required, but it will make your life a bit easier.

Open ASF folder and find core executable file, this will be `ArchiSteamFarm.exe` on Windows, and `ArchiSteamFarm` on Linux/OS X. Right click it and select "copy". Now navigate to the place you actually want to have ASF shortcut in (such as your desktop), right click and choose "paste shortcut here". You can rename your shortcut if you'd like to, such as giving it "ASF" name. Now do the same with `config` directory that you can find in the same place as ASF binary.

After a small cleanup, you'll now have a very convenient structure similar to the one below:

![Structure](https://i.imgur.com/k85csaZ.png)

This will allow you to easily access ASF binary and config files without much hassle. In my case I decided to use the structure mentioned above, so my ASF files are in "Core" directory directly inside. You can adapt this structure to your liking, such as having ASF + config shortcuts on the desktop and ASF directory e.g. in `C:\ASF` instead, it's up to you.

Linux/OS X users are advised to do the same, you can use excellent symbolic links mechanism available through `ln -s`.

* * *

### Конфигурация

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

First and foremost, there is **[configuration](Configuration)** page that explains **everything** that relates to configuration, but it's a massive amount of new information, a lot of which we don't need to know right away. Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in two ways - either by using our web config generator, or manually. This is explained in-depth in **[configuration](Configuration)** section, so refer to that if you want more detailed information. We'll use web config generator way, since it's much easier.

Navigate to our **[web config generator](https://justarchi.github.io/ArchiSteamFarm)** page with your favourite browser, you'll need to have javascript enabled in case you manually disabled it. We recommend Chrome or Firefox, but it should work on all most popular browsers.

After opening the page, switch to "Bot" tab. You should now see a page similar to the one below:

![Bot tab](https://i.imgur.com/BUkUEYt.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There are only 3 words you can't use, those are: `ASF`, `example` and `minimal`. In addition to that we recommend that you avoid using spaces, you can use `_` as a word separator.

After you decided about your name, change `Enabled` switch to be on, this defines whether your bot is supposed to be started by ASF automatically after launch (of the program).

Now you can decide upon two things:

- You can put your login in `SteamLogin` field and your password in `SteamPassword` field
- Or you can leave them empty

Doing the first thing will allow ASF to automatically use your account credentials during startup, so you won't need to input them manually each time ASF needs them. You can however decide to omit them, in which case they're not being saved, so ASF won't be able to automatically start without your help and you'll need to input them during runtime.

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental PIN to unlock the account, you'll need to toggle advanced settings and put it into `SteamParentalPIN` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/BUmF0Wr.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name:

![Bot tab 3](https://i.imgur.com/ylyvzvL.png)

Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![Structure 2](https://i.imgur.com/doYnbB9.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Running ASF

You're now ready to launch the program for the first time. Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](FAQ#so-how-it-exactly-works)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery 

* * *

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalPIN` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

#### Changing settings

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is idling, instead of showing actual game.

So let's do that, if you run ASF and start idling, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/sCdSMZj.png)

Let's change that then. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 4](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/NeFYrdU.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, then downloading generated config and replacing core `ASF.json` file.

* * *

### Summary

You've successfully set up ASF to use your Steam accounts and you've even customized it slightly to your liking already. Now is a good time to read entire **[configuration](Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **<FAQ>** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over the rest of **[our wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki)**. Have fun! 

* * *

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- Install **[.NET Core prerequisites](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**.
- Install **[.NET Core SDK](https://www.microsoft.com/net/download/core#/sdk)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchi/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[Configure ASF](Configuration)**.
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.