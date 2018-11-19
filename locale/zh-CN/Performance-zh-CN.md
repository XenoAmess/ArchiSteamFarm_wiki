# 性能

The primary objective of ASF is to farm as effectively as possible, based on two types of data it can operate on - small set of user-provided data that is impossible for ASF to guess/check on its own, and larger set of data which can be automatically checked by ASF.

In automatic mode, ASF does not allow you to choose the games that should be farmed, neither allows you to change cards farming algorithm. **ASF knows better than you what it should do and what decisions it should make in order to farm as fast as possible**. Your objective is to set config properties properly, as ASF can't guess them on its own, everything else is covered.

* * *

Some time ago Valve changed the algorithm for card drops. From that point onwards, we can categorize steam accounts by two categories: those **with** card drops restricted, and those **without**. The only difference between those two types is the fact that accounts with restricted card drops can't get any card from given game until they play given game for at least `X` hours. It seems that older accounts that never asked for refund have **unrestricted card drops**, while new accounts and those who did ask for refund have **restricted card drops**. This is however only theory, and should not be taken as a rule. That's why there is **no obvious answer**, and ASF relies on **you** telling it which case is appropriate for your account.

* * *

ASF currently includes two farming algorithms:

**Simple** algorithm works best for accounts that have unrestricted card drops. This is primary algorithm used by ASF. Bot finds games to farm, and farms them one-by-one until all cards are dropped. This is because card drops rate when farming more than one game is close to zero and totally ineffective.

**Complex** is new algorithm that has been implemented to help restricted accounts to maximize their profits as well. ASF will firstly use standard **Simple** algorithm on all games that passed `HoursUntilCardDrops` hours of playtime, then, if no games with >= `HoursUntilCardDrops` hours are left, it will farm all games (up to `32` limit) with < `HoursUntilCardDrops` hours left simultaneously, until any of them hits `HoursUntilCardDrops` hours mark, then ASF will continue the loop from beginning (use **Simple** on that game, return to simultaneous on < `HoursUntilCardDrops` and so on). We can use multiple games farming in this case for bumping hours of the games we need to farm to appropriate value firstly. Keep in mind that during farming hours, ASF **does not** farm cards, therefore it also won't check for any card drops during that period (for reasons stated above).

Currently, ASF chooses cards farming algorithm based purely on `HoursUntilCardDrops` config property (which is set by **you**). If `HoursUntilCardDrops` is set to `0`, **Simple** algorithm will be used, otherwise, **Complex** algorithm will be used instead.

* * *

### **哪种算法更适合您并没有一个明确的答案**。

This is one of the reasons why you do not choose cards farming algorithm, instead, you tell ASF if account has restricted drops or not. If account has non-restricted drops, **Simple** algorithm will **work better** on that account, as we won't be wasting time on bringing all games to `X` hours - cards drop ratio is close to 0% when farming multiple games. On the other hand, if your account has card drops restricted, **Complex** algorithm will be better for you, as there's no point in farming solo if game didn't reach `HoursUntilCardDrops` hours yet - so we'll farm **playtime** first, **then** cards in solo mode.

Don't blindly set `HoursUntilCardDrops` only because somebody told you to - do tests, compare results, and based on data you get, decide which option should be better for you. If you put some minimal effort into that, you'll ensure that ASF is working with maximum possible efficiency for your account, which is probably what you want, considering that you're reading this wiki page right now. If there was a solution that works for everybody, you'd not be given a choice - ASF would decide itself.

* * *

### 检查您的账号是否被限制的最好方法是什么？

Make sure you have some games to farm, preferably 5+, and run ASF with `HoursUntilCardDrops` of `0`. It would be a good idea if you didn't play anything during farming period for more accurate results (best to run ASF during the night). Let ASF farm those 5 games, and after that check out the log for results.

ASF clearly states when a card for given game was dropped. You're interested in fastest card drop achieved by ASF. For example, if your account is unrestricted then a first card drop should happen after around 30 minutes since you started farming. If you notice **at least one** game that did drop card in those initial 30 minutes, then this is an indicator that your account is **not** restricted and should use `HoursUntilCardDrops` of `0`.

On the other hand, if you notice that **every** game is taking at least `X` amount of hours before it drops its first card, then this is an indicator to you what you should set `HoursUntilCardDrops` to. Majority (if not all) of restricted users require at least `3` hours of playtime for cards to drop, and this is also the default value for `HoursUntilCardDrops` setting.

Remember that games can have different drop rate, this is why you should test if your theory is right with **at least** 3 games, preferably 5+ to ensure that you're not running into false results by a coincidence. A card drop of one game in less than an hour is a confirmation that your account **is not** restricted and can use `HoursUntilCardDrops` of `0`, but for confirming that your account **is** restricted, you need at least several games that are not dropping cards until you hit a fixed mark.

It's important to note that in the past `HoursUntilCardDrops` was only `0` or `2`, and this is why ASF had a single `CardDropsRestricted` property that allowed to switch between those two values. With recent changes we noticed that not only majority of users require `3` hours in place of previous `2` now, but also that `HoursUntilCardDrops` is now dynamic and can hit any value on per-account basis.

In the end, of course, decision is up to you.

And to make it even worse - I experienced cases when people switched from restricted to unrestricted state and vice versa - either because of Steam bug (oh yeah, we have many of those), or because of some logic adjustments by Valve. So even if you confirmed that your account is restricted (or not), do not believe that it'll stay like that - in order to switch from unrestricted to restricted it's enough to ask for a refund. If you feel like previously set value is no longer appropriate, you can always do a re-test and update it accordingly.

* * *

By default, ASF assumes that `HoursUntilCardDrops` is `3`, as the negative effect of setting this to `3` when it should be less is smaller than done the other way. This is because of the fact that in the worst possible case we'll waste `3` hours of idling per `32` games, compared to wasting `3` hours of idling per every single game if `HoursUntilCardDrops` was set to `0` by default. However, you should still tune this variable to match your account for maximum efficiency, as this is only a blind guess based on potential drawbacks and majority of users (so we're trying to choose "lesser evil" by default).

At the moment two above algorithms are enough for all currently possible account scenarios, in order to farm as effectively as possible, therefore it's not planned to add any other ones.

It's nice to note that ASF also includes `Manual` farming mode that can be activated by `play` command. 您可以阅读**[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN#备注)**章节了解详情。

* * *

## Steam 漏洞

Cards drop algorithm does not always work the way it should, and it's entirely possible for various Steam glitches to happen, such as cards being dropped on restricted accounts, cards being dropped on closing/switching the game, cards not dropping at all when game is being played, and likewise.

This section is mainly for people that are wondering why ASF doesn't do **X**, such as rapidly switching games to idle cards faster.

What is a **Steam glitch** - a specific action triggering **undefined** behaviour, which is **not intended, undocumented, and considered as a logic flaw**. It's **unreliable by definition**, which means that it can't be reproduced reliably with clean testing environment, and therefore, coded without resorting to hacks that are supposed to guess when glitch is happening and how to fight with it / abuse it. Typically it's temporary until developers fix the logic flaw, although some misc glitches can go unnoticed for a very long period of time.

A good example of what is considered as a **Steam glitch** is not that uncommon situation of dropping a card when game is being closed, which can be abused to some degree with idle master's game skip function.

- **Undefined behaviour** - you can't say if there will be 0 or 1 cards being dropped when you trigger the glitch.
- **Not intended** - based on past experience and behaviour of Steam network that doesn't result in same behaviour when sending a single request.
- **Undocumented** - it's clearly documented on Steam website how cards are being obtained, and **in every single place** it's clearly stated that it's obtained through **playing**, NOT closing games, getting achievements, games switching or launching 32 games concurrently.
- **Considered as a logic flaw** - closing game(s) or switching them should have no outcome on cards being dropped which are clearly stated to be obtained through **gaining playtime**.
- **Unreliable by definition, can't be reproduced reliably** - it doesn't work for everybody, and even if it did work for you once, it might no longer work for the second time.

Now once we realized what Steam glitch is, and the fact that cards being dropped when game gets closed **is** one, we can move on to the second point - **ASF is not abusing Steam network in any way by definition, and it's doing its best to comply with Steam ToS, its protocols and what is generally accepted**. 持续向 Steam 网络发送大量启动/停止游戏的请求会被视为一种 **[DoS 攻击](https://en.wikipedia.org/wiki/Denial-of-service_attack)**并且**直接违反 [Steam 在线行为准则](https://store.steampowered.com/online_conduct/)**。

> 作为一名 Steam 用户，您同意遵守以下行为准则。
> 
> 您将不会：
> 
> 对 Steam 服务器展开攻击或以其他方式破坏 Steam。

It doesn't matter whether you're able to trigger Steam glitch with other programs (such as IM), and it also doesn't matter if you agree with us and consider such behaviour as DoS attack, or not - it's up to Valve to judge this, but if we consider it as exploiting/abusing non-intended behaviour through excessive Steam network requests, then you can be pretty sure that Valve will have similar view on this.

ASF is **never** going to take advantage of Steam exploits, abuses, hacks or any other activity that we see as **illegal or unwanted** according to Steam ToS, Steam Online Conduct or any other trusted source that could indicate that ASF activity is unwanted by Steam network, as stated in **[contributing](https://github.com/JustArchiNET/ArchiSteamFarm/blob/master/.github/CONTRIBUTING.md)** section.

如果您仍然愿意为了几分钱的卡牌冒险加快挂卡速度，ASF 永远不会支持自动执行这样的操作。但您仍然可以手动执行 `play` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-zh-CN)**进行您想要对 Steam 网络做的任何事情。 We do not recommend taking advantage of Steam glitches and exploiting them for your own gain - not only with ASF, but with any other tool as well. In the end however, it's your account, and your choice what you want to do with it - just keep in mind that we warned you.