# 交易

ASF支援Steam非互動式（離線）交易。 您可以即刻收到（接受/拒絕）以及發送交易提案，並且不需要特殊配置，但顯然需要不受限制的 Steam 帳戶（已經在商店中花費了5$ 的帳戶）。 對於受限制的帳戶，交易模組不可用。

* * *

## 邏輯

ASF 將始終接受所有來自對機械人有`Master`（或更高）訪問權限的用戶的交易項目。 這不僅可以輕鬆地拾取機械人實例的掛卡所獲（Steam卡片），還可以輕鬆地管理機械人物品庫中的Steam物品。

ASF將拒絕任何來自交易模組黑名單中的用戶（對master無效）的交易報價，無論其內容如何。 黑名單存儲在標準 `BotName.db` 數據庫中，可通過 `bl`、`bladd` 和 `blrm` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)** 進行管理。 這應該可以替代Steam 提供的標準用戶屏蔽模塊，請謹慎使用。

除非在 `TradingPreferences` 中指定 `DontAcceptBotTrades`，否則 ASF 將接受跨機械人發送的所有 `loot`類型交易。 簡而言之，預設 `TradingPreferences`為 `None` 將導致 ASF 自動接受對機械人具有 `Master` 訪問許可權的用戶的交易（如上文所述），以及參與 ASF 流程的其他機械人的所有捐贈交易。 如果您想禁用來自其他機械人的捐贈交易，那麼應當在 `TradingPreferences` 中啟用 `DontAcceptBotTrades` 。

當您在 `TradingPreferences` 中啟用 `AcceptDonations` 時，ASF將接受任何捐贈交易，在這種交易中，機械人帳戶不會損失任何物品。 此屬性僅影響非機械人帳戶，因為機械人帳戶受 `DontAcceptBotTrades` 的影響。 `AcceptDonations` 可以讓你輕鬆接受來自其他用戶（包括不在同一ASF進程中的機械人）的捐贈。

值得一提的是，`AcceptDonations` 不需要 **[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication)**，因為如果我們不會損失任何物品，則不需要確認交易。

您還可以通過修改相應的 `TradingPreferences` 來進一步自訂 ASF 交易功能。 其中一個主要的 `TradingPreferences` 功能是 `SteamTradeMatcher` 選項，這將允許 ASF 使用內置邏輯來接受交易，以幫助您収集合成徽章所需的卡片，這在與**[SteamTradeMatcher](https://www.steamtradematcher.com)**公開清單的配合使用中特別有用，但也可以單獨使用。 下面將進一步介紹此功能。

* * *

## `SteamTradeMatcher`

當啟用`steamtrademcher`時，ASF將使用複雜算法來檢查交易是否遵循 STM規則，並且結果至少對我們保持中立。 當前邏輯如下：

- 如果我們失去任何`MatchableTypes`類型之外的物品，則拒絕交易。
- 如果我們未收到每個遊戲和每種類型下相同數量的物品，則拒絕交易。
- 如果用戶要求交易特殊的Steam夏季/冬季特賣卡片，並有交易暫掛，則拒絕交易。
- 如果交易暫掛時間超過全域配置中`MaxTradeHoldDuration` 屬性的值，則拒絕交易。
- 如果我們沒有啟用`MatchEverything`，則拒絕一切對我們不利的交易。
- 如果交易未被以上所有規則拒絕，則接受交易。

值得一提的是，ASF 還支援超額支付——只要滿足上述所有條件，邏輯就會正常工作，接受用戶向交易添加的額外物品。

前4個拒絕相關的判斷條件是有目共見的。 最後一個包括實際上重覆的邏輯，用於檢查我們的庫存狀態，並決定交易狀態。

- 如果我們的收集度取得進展，交易**有利**。 A A（交易前） <-> A B（交易後）
- 如果我們的收集度維持現狀，交易**中立**。 A B（交易前） <-> A C（交易後）
- 如果我們的收集度出現倒退，交易**不利**。 A C（交易前） <-> A A（交易後）

STM 的運行規則僅會匹配對我們有利的交易，這意味著將 STM 用於匹配冗餘卡片的用戶總會發起對我們有利的交易。 然而，ASF 更加包容，它也接受中立的交易 ，因為在這些交易中，我們實際上並沒有任何損失，所以沒有必要拒絕它們。 這對朋友間的交易特別有用，因為他們可以在不使用 STM 的情況下交換您的冗餘卡片，而不會影響您的卡牌收集進度。

預設情況下，ASF 將拒絕不利交易——作為一個用戶，這恰恰正是您想要的。 但是，您也可以選擇在`TradingPreferences`中啟用 `MatchEverything`，以讓ASF接受所有的冗餘物品交易，包括**不利交易**。 只有當您想要在您的帳戶下運行 1:1 交易機械人時，這一特性才有用，因為您瞭解 **ASF 將不再帮您完成徽章進度，反而可能會使您因 N 張相同卡片而損失收集進度**。 除非您有意運行一個交易機械人，該機械人**並不**期待集齊卡片，否則您不應啟用此選項。

無論您選擇哪種 `TradingPreferences`，交易被 ASF 拒絕並不意味著您自己無法手動接受它。 如果您保留 `BotBehaviour` 的預設值（並未啟用 `RejectInvalidTrades`），ASF 將忽略這些交易，允許您自行決定。 同樣適用于一切 `MatchableTypes` 之外的物品交易，該模組僅幫助您自動化 STM 交易，而不會決定什麼是好的交易，什麼不是。 此規則的唯一例外是，當交易來自您使用 `bladd` 命令從交易模組中列入黑名單的用戶，無論 `BotBehaviour` 設置如何，這些交易都會立即被拒絕。

強烈建議在啟用此選項時使用 **ASF 2FA</0 >, 因為如果您決定手動確認每個交易, 則此功能將失去其全部潛力。 `SteamTradeMatcher` 即使沒有確認交易功能也能正常工作, 但如果您未及時手動接受, 它可能會產生積壓的確認請求。</p> 

* * *

### `MatchActively`

`MatchActively` 設置是擴展版本的 `SteamTradeMatcher`， 除了該選項提供的被動匹配外，還包括主動匹配，機械人將主動向其他人發送交易。

為了使用該選項，您有一組需要滿足的要求。 首先，您需要啟用 `SteamTradeMatcher`（因為該特性是此功能的擴展），並確保您已**禁用**`MatchEverything`（因為交易機械人永遠不會參與主動匹配）。 之後，您必須有資格被列入我們的 **[ASF STM 清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)**，這不需要您擁有100個物品。 這意味着您至少應啟用 `Statistics`，帳號**[未受限](https://support.steampowered.com/kb_article.php?ref=3330-IAGK-7663)**，啟用**[ASF 2FA](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Two-factor-authentication#asf-2fa)**，**[物品庫狀態為公開](https://steamcommunity.com/my/edit/settings)**並至少在`MatchableTypes`中設置一個有效值，如集換式卡片。

如果您滿足上述所有要求，ASF 將定期與我們的 **[ASF STM 公開清單](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#public-asf-stm-listing)**通信，以便積極匹配目前可用的類型為`Any`（`MatchEverything`）的機械人。

- 每次匹配由若幹「回合」組成，每次匹配最多可進行`10`回合。
- 在每一輪匹配中， ASF 將獲取我們和清單中可選機械人的物品庫以找到符合 `匹配類型` 的物品。 如果找到匹配項，ASF 將自動發送並確認交易報價。
- 每個集合（項目類型和appID的組合）只能在一回合中參與一次匹配。 這樣做是為了最大限度地避免「物品不再可用」的發生，且無需在發送交易之前等待每個機械人做出反應。
- ASF 在單個交易中發送的物品不會超過 `255`個，並且對單個用戶發送的交易將不會超過 `5` 個。 這是由 Steam 限制以及我們自己的負載平衡所決定的。
- 當我們試圖匹配機械人`40`次，或者未能匹配連續`20`個不同機械人，回合終止。
- 如果最後一回合中有至少一個交易被發送，下一回合將於上一回合結束`5` 分鐘後開始（考慮到等待所有機械人對我們的交易作出反應的冷卻時間），否則匹配結束，並在`8`小時後再次運行。

此模組是完全透明的。 匹配進程會在 ASF 啟動 `1` 小時後開始，並每 `8` 小時重複執行（如果需要）。 `MatchActively` 特性旨在作為一個長期的週期性措施來保障我們集齊卡片的進程持續推進，但如果我們將其作為命令使用，就會造成短期內的資源壓力。 此模組的目標用戶是主帳戶和「倉儲型」備用帳戶，它亦可以用於未啟用 `MatchEverything` 的任何機械人。

ASF 將盡一切努力減少使用此功能產生的請求數並減輕系統壓力，同時最大化的提升匹配效率。 選擇機械人匹配的精確算法是 ASF 的實現細節，在此不予詳述，目前 ASF 傾向于優先選擇物品所屬遊戲類型更多的機械人。

`MatchActively` 會識別您以`bladd` **[命令](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**加入交易模組黑名單中的機械人，不會試圖匹配它們。 這可以用來告訴 ASF 它永遠不應該匹配哪些機械人，即使它們有潛在的匹配物品可供我們使用。