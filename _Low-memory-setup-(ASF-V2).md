# Low-memory setup (ASF V2)

***

ASF is extremely lightweight on resources by definition, depending on your usage even 128 MB VPS with Linux is capable of running it, although going that low is not recommended and can lead to issues. While being light, ASF is not afraid of asking OS for more memory, if such memory is needed for ASF to operate with optimal speed.

ASF as an application tries to be as much optimized and efficient as possible, which also takes in mind resources being used during execution. When it comes to memory, ASF prefers performance over memory consumption, which can result in temporary memory "spikes", which can be noticed e.g. with accounts having 3+ badge pages, as ASF will fetch and parse first page, read from it total number of pages, then launch fetch task for every extra page, which results in concurrent fetching and parsing of remaining pages. This speeds up execution, for cost of increased memory usage. Similar thing is happening e.g. with parsing active trade offers, ASF is also parsing them all concurrently. On top of all of that, ASF (C# runtime) doesn't return unused memory back to OS immediately. Huh? What's going on?

ASF is extremely well optimized, and makes use of available resources as much as possible. High memory usage of ASF doesn't mean that ASF actively **uses** that memory and **needs it**. Very often ASF will keep some memory allocated for some "room" for future actions, as by not asking OS for every memory chunk we're improving performance. The runtime (.NET, or Mono) should automatically release unused ASF memory back to OS when OS will **truly** need it. Remember - **[unused memory is wasted memory](http://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full/)**. You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra for having free space for functions that will execute in a moment. You run into problems when Linux kernel is killing Mono process due to OOM (out of memory), not when you see ASF as top memory consumer in ```htop```.

***

Below suggestions are divided into three categories, from simple ASF tricks, through fine-tuning the runtime, and finishing at full runtime recompilation.

***

## ASF (Easy)

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of ```ShutdownOnFarmingFinished```. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You need only one active bot to keep ASF instance running, and you can always bring up other bots if needed.
- Keep your bots number low. Not ```Enabled``` bot instance takes less resources, as ASF doesn't bother starting it. Also keep in mind that ASF has to create a bot for each of your configs, therefore if you don't need to ```!start``` given bot and you want to save some extra memory, you can temporarily rename ```Bot.json``` to e.g. ```Bot.json.bak``` in order to avoid creating state for your disabled bot instance in ASF. This way you won't be able to ```!start``` it without rename and ASF restart, but ASF also won't bother keeping state of this bot in memory, leaving room for other things (very small save, in 99.9% cases you shouldn't bother with it, just keep your bots with ```Enabled``` of ```false```).
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing ```LoginLimiterDelay``` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time. The less work that has to be done at the same time - the less memory used.
- As last resort, you can tune ASF for ```MinMemoryUsage``` through ```OptimizationMode``` **[global config property](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration#global-config)**. Read carefully its purpose, as this is serious performance degradation for nearly no memory benefits.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:
- Badge page parsing
- Inventory parsing

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with its inventory (e.g. sending trade or dealing with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can help :+1: 

***

## Mono Runtime (Advanced)

If you're running ASF on Windows, then there is nothing more that can help you, as Windows is not optimized by definition, so instead of looking at ASF, start looking at your OS. Squishing every megabyte out of runtime is pointless when your OS allocates minimum of 2 GB by definition.

If you're not using Windows, then you should know that Mono is highly customizable and you can use many switches and parameters to keep its memory low. I strongly suggest to check out ```man mono``` to find out which of the recommended options are available for you, because they can differ from one version to another.

Some interesting features that could help you:
- Take a look at ```MONO_GC_PARAMS```. This is the most important thing that directly affects how GC operates, properly setting things such as ```nursery-size```, ```soft-heap-limit``` or ```evacuation-threshold``` can really help you, by tuning GC to be more aggressive and try to keep memory low, as opposed to default tuning for performance.
- Use ```--desktop``` parameter which will tune Mono for desktop usage, slightly improving garbage collector which will act more aggressively.
- Experiment with ```-O``` also known as ```--optimize```. Enabling optimizations in general should increase performance for cost of increased memory usage, so logically you should **avoid** using optimizations if you intend to keep memory low. However, there are certain optimizations that might improve memory efficiency, such as ```deadce``` - dead code elimination. If you want, go ahead and experiment, you can also turn on all optimizations with ```-O=all```, although that probably won't help with memory.

A good example of excellent ```MONO_GC_PARAMS``` for keeping memory as low as possible:

```
export MONO_GC_PARAMS="nursery-size=1m,soft-heap-limit=128m,evacuation-threshold=90,save-target-ratio=0.1,default-allowance-ratio=1.0"
mono --desktop ASF.exe
```

I suggest to further tune ```soft-heap-limit``` to size that you expect from ASF to occupy at most, and also read about other variables I put in ```man mono```. **Note:** ```soft-heap-limit``` doesn't specify maximum allowed memory for ASF to use, as we can't put any hard limit on GC, we can only suggest GC how much we can expect from ASF to use, but if there will be a need, GC is free to ignore our tip to satisfy ASF needs. I suggest to set this parameter to 75-90% of free memory you expect to have.

For optimal performance, you should also adapt ```nursery-size``` to fit ```soft-heap-limit```. I suggest ```nursery-size``` of ```1m``` for ```128m``` soft-heap-limit, ```2m``` for ```256m``` and ```4m``` for above (although you probably don't have memory problems when you're going that high).

**Also keep in mind that above ```MONO_GC_PARAMS``` will heavily affect ASF performance, you can't expect ASF to be fast if you expect from GC to be as conservative as possible at the same time. I strongly suggest to fine-tune ```MONO_GC_PARAMS``` to your needs.** In my case, using above MONO_GC_PARAMS decreased performance of parsing badge pages from **20** to **32** seconds, but also decreased memory footprint from maximum of **24.9%** to maximum of **19.8%**, so it's **47.5%** lower performance for **20.5%** smaller memory footprint. Decide yourself if it's a good tradeoff for you.

ASF by default is optimized for performance. If you use ```run.sh``` script from ASF tree, then you'll notice that Mono is started with ```--server -O=all``` arguments. Mono optimized for memory should avoid ```-O=all```, in addition to that it should use ```--desktop``` instead of ```--server```. In the end, you should experiment a bit to find out which setup suits you best.

***

## Mono Recompilation (Expert / Developer)

Finally, you can recompile your Mono to disable some unused features and generate code that is optimized for size and not performance, which could help you even further. This is very complex process, and you should not do that in general, unless you're feeling comfortable with recompiling and using packages from source, as opposed to binary ones.

Here is a script of mine which should greatly help you with the process:
```
#!/bin/bash
set -eu

PREFIX="/opt/mono-unstable"
JOBS="$(grep -c processor /proc/cpuinfo)"

ADCFLAGS=(-Os -pipe -march=native -s)
ADMONOCFLAGS=("--prefix=$PREFIX")

# Below flags are unsupported, you use them at your own risk
ADMONOFLAGS+=(--disable-boehm --disable-libraries --disable-nls --with-gc=none --with-mcs-docs=no --with-ikvm-native=no --with-shared_mono=no --with-xen-opt=no)
ADMONOFLAGS+=(--enable-minimal=profiler,pinvoke,debug,reflection_emit_save,large_code,logging,shadowcopy,attach,verifier,soft_debug,perfcounters,normalization,shared_perfcounters,appdomains,security,lldb,mdb)

export PATH="$PREFIX/bin:$PATH"
mkdir -p "$PREFIX" "${HOME}/.mono"

git pull
git submodule update -j "$JOBS" --init --recursive

export CFLAGS="${ADCFLAGS[@]} -std=gnu11"
export CXXFLAGS="${ADCFLAGS[@]} -std=gnu++14 -fvisibility=hidden"
export LDFLAGS="${ADLDFLAGS[@]}"

if [[ ! -x "autogen.sh" ]]; then
        echo "ERROR: autogen.sh could not be found!"
        exit 1
fi

if [[ -f "Makefile" ]]; then
        make clean || true
fi

./autogen.sh "${ADMONOFLAGS[@]}"
make -j "$JOBS"
make install -j "$JOBS"
```

Firstly clone mono repository:
```git clone https://github.com/mono/mono --depth 10```

Then execute my script, it's possible that you will need to install some extra dependencies required by ```autogen.sh```, so don't be afraid of getting errors at first.

If your compilation succeeded, then you can find your brand new mono in ```$PREFIX``` location, which is ```/opt/mono-unstable``` in my case. Navigate there, and create one more file, I suggest to name it ```envsetup.sh```:

```
#!/bin/bash

MONO_PREFIX=/opt/mono-unstable

# Don't forget to tune these
export MONO_GC_PARAMS="nursery-size=1m,soft-heap-limit=128m,evacuation-threshold=90,save-target-ratio=0.1,default-allowance-ratio=1.0"
export MONO_ENV_OPTIONS="--desktop"

export DYLD_FALLBACK_LIBRARY_PATH=$MONO_PREFIX/lib:$DYLD_LIBRARY_FALLBACK_PATH
export LD_LIBRARY_PATH=$MONO_PREFIX/lib:$LD_LIBRARY_PATH
export C_INCLUDE_PATH=$MONO_PREFIX/include
export ACLOCAL_PATH=$MONO_PREFIX/share/aclocal
export PKG_CONFIG_PATH=$MONO_PREFIX/lib/pkgconfig
export PATH=$MONO_PREFIX/bin:$PATH
```

Now when you will want to run ASF with our self-compiled stripped Mono, simply execute:

```
source /opt/mono-unstable/envsetup.sh
mono ASF.exe
```

And done.

***

### So what we're exactly doing?

We're recompiling Mono with ```-Os``` - optimization for size, as opposed to default flag of ```-O2``` - optimization for speed. We also use ```-march=native``` to instruct gcc into generating code designed specially for our current machine, therefore that Mono won't run on any other machine, but as an advantage we're making use of all our available CPU instructions, and also L1/L2 CPU cache sizes, which can improve performance and memory usage. Lastly, we disable Mono features we do not use, so resulting binary is smaller, faster and has less memory footprint. I suggest to do ```./configure --help``` to read about them all if you're interested.

**Notice:** We also disable XEN optimizations/compatibility with ```--with-xen-opt=no```. If you plan to use your self-compiled Mono on XEN VPS, please remove this line or switch to ```yes``` (default). Mono without XEN optimizations will be a little faster and smaller on non-XEN systems, but will run significantly worse on XEN systems. If you have VPS and you're unsure which virtualization you're using (OpenVZ/KVM/XEN), then remove this line as well just in case.

Last word, what works for me, might not work for you. This is only example of mono recompiling process, it's **unsupported** by both me and Mono development team, therefore if you decide to compile yourself and use any of unsupported flags, you should be an expert that doesn't require any support and is completely fine dealing with broken packages. If you want to make ASF "just work", don't do any of that, because it doesn't even guarantee any significant gain on performance neither memory usage. Stick with ASF and Mono runtime instructions, and let maintainers do their job with compiling packages.

Finally, optimizing for memory usage degrades performance almost always. Recompilation of Mono should be the last step, when you're extremely constrained by available memory and you don't mind much slower execution for every damn kilobyte saved. At some point you must accept the fact that you simply don't have enough memory for all tasks you expect ASF to do, therefore either decrease amount of those tasks, or add more memory :+1:.

But hey, using self-compiled package is always rewarding, right? :smile:. 