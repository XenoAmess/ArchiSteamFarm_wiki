# 命令列參數

ASF 支援一些能夠影響程式運行時的命令列參數。 高級用戶可使用這些參數以定義程式運行方式。 與`ASF.json `配置文件的預設方式相比，命令列參數可用於核心初始化（例如`--path`）、平台特定設置（例如`--system-required`）或敏感性資料（例如`--cryptkey`）。

* * *

## 使用方法

使用方法取決於您的操作系統和ASF版本。

Generic（通用）:

```shell
dotnet ArchiSteamFarm.dll --參數 -- 另一個參數
```

Windows:

```powershell
.\ArchiSteamFarm.exe --參數--另一個參數
```

Linux/OS X

```shell
./ArchiSteamFarm --參數--另一個參數
```

命令列參數也可用於通用助手腳本中，例如`ArchiSteamFarm.cmd`或`ArchiSteamFarm.sh`。 除此之外, 如我們的**[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker#command-line-arguments)**&#8203;部分中所述，在使用助手腳本時, 您還可以使用 `ASF_ARGS` 環境屬性,。

若您的參數包含空格，請務必使用引號將其括住。 兩個錯誤示例：

```shell
./ArchiSteamFarm --path /home/archi/My Downloads/ASF # Bad!
./ArchiSteamFarm --path=/home/archi/My Downloads/ASF # Bad!
```

兩個正確示例：

```shell
./ArchiSteamFarm --path "/home/archi/My Downloads/ASF" # OK
./ArchiSteamFarm "--path=/home/archi/My Downloads/ASF" # OK
```

## 參數

`--cryptkey<key>`或`--cryptkey=<key>`──將使用值為`<key>`的自訂密鑰啟動 ASF。 此選項會影響**[安全性](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Security)**，並將導致ASF使用您的自訂密鑰 `<key>`，而不是硬編碼在程式中的預設值。 請謹記使用此密鑰加密的密碼在每次 ASF 運行時傳遞相同的密鑰才能正確解密。

* * *

`--no-restart`──此開關主要用於**[Docker](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Docker)**&#8203;容器並將 `AutoRestart` 強制設置為 `false`。 除非有特殊的需要，否則您應直接在配置中配置 `AutoRestart` 屬性。這個開關的作用是使 Docker 腳本不必修改您的全域配置即可適應環境。 當然，如果您在腳本中運行 ASF，也可以使用此開關（否則您最好使用全域配置屬性）。

* * *

`--path <path>`或`--path=<path>`──ASF在啟動時始終會導航至自身所在的目錄。 通過指定此參數，ASF 將在初始化後導航到給定的目錄，這允許您對 `config`（還可選`plugins`或 `www`）目錄使用自訂路徑，而無需複製二進位檔案至預設目録。 如果您想將二進位檔案和實際配置檔案分開，這可能會非常有用, 類似Linux 打包機制──這樣您就可以在多個設置中共用一個（最新的）二進位檔案。 此路徑既可以是基於當前 ASF 二進位檔案所在位置的相對路徑，也可以是絕對路徑。 請留意，使用同一個二進位檔案運行多個實例時，通常應禁用自動更新, 因為它們之間沒有同步。 也請注意，該命令指向新的「ASF 主目錄」──與原始的 ASF 具有相同結構的目錄，其中包含 `config` 目錄。

範例：

```shell
dotnet /opt/ASF/ArchiSteamFarm.dll --path
/opt/TargetDirectory # 絕對路徑
dotnet /opt/ASF/ArchiSteamFarm.dll --path
../TargetDirectory # 相對路徑
```

    ├── /opt
    │     ├── ASF
    │     │     ├── ArchiSteamFarm.dll
    │     │     └── ...
    │     └── TargetDirectory
    │           ├── config
    │           ├── plugins (optional)
    │           └── www (optional)
    └── ...
    

* * *

`--process-required`──預設情況下，ASF 將會在無機械人運行的情況下關閉，聲明此開關將禁用這一行為。 阻止自動關機功能在和**[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC)**配合下特別有用，大多數用戶都希望他們的 Web 服務正常運行，無論啟用了多少個機械人。 如果您在使用 IPC 或者需要 ASF 進程持續運行直至手動關閉它，這就是正確的選項。

如果您不打算運行 IPC，此選項對您來說將相當無用， 因為您可以在需要時再次啟動該過程 (而不是 ASF 的 web 伺服器，在那裏您需要它一直偵聽以發送命令)。

* * *

`--system-required`──聲明此開關將導致 ASF 嘗試通知操作系統「此進程要求系統在其存留期內處於啟動狀態並正常運行」。 當前，此開關僅對 Windows 電腦有效，只要ASF進程正在運行，它就會阻止您的系統進入睡眠模式。 當您在夜間使用 PC 或者筆記本電腦掛卡時，這一功能是相當實用的，因為 ASF 能夠在掛卡時保持系統處於喚醒狀態，然後，一旦掛卡完成，ASF 就會像平常一樣自動關閉，允許您的系統再次進入睡眠模式，因此，一旦掛卡完成，即可立刻節省電力。

請注意，要让 ASF正確地自動關閉，您還需要其他設置──特別是避免 `--process-required`，且確保所有機械人都已啟用 `ShutdownOnFarmingFinished`。 當然，自動關機只是這個參數的用法之一，而非必需，因為您還可以將此參數配合 `--process-required` 使用，從而有效地使您的系統在 ASF 啟動之後無限運行下去。