# 보안

## 스팀 암호(SteamPassword)

ASF는 현재 4가지 종류의 암호를 지원합니다. 평문(`PlainText`), `AES`, `ProtectedDataForCurrentUser` 그리고 없음(None, `null` / `""`)입니다.

암호화된(encrypted) 암호(password)를 사용하기 위해서 처음에는 평문(`PlainText`)를 사용해서 평소처럼 로그인하고 `password` **[명령어](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ko-KR)** 를 이용해서 암호화된 암호를 생성해야 합니다. 좋아하는 암호화 방법을 선택하고 봇 환경설정의 `SteamPassword`에 암호화된 암호를 입력하십시오. 마지막으로 선택한 암호화 방법과 맞도록 `PasswordFormat`를 변경하는 것을 잊지 마십시오.

* * *

### 평문(PlainText)

암호를 저장하는 가장 간단하고 보안에 취약한 방식입니다. `PasswordFormat`는 `0`으로 정의되 있습니다. ASF는 `SteamPassword` 항목이 스팀에 로그인할 때 사용되는 암호가 직접 입력하는 것처럼 평문으로 되어있을 것이라고 생각합니다. 사용하기 가장 쉽고 모든 설치방법과 100% 호환되므로 기본값입니다.

* * *

### 고급 암호화 표준(AES)

오늘날의 기준으로 보안성이 있다고 간주되는 **[고급 암호화 표준(AES)](https://ko.wikipedia.org/wiki/%EA%B3%A0%EA%B8%89_%EC%95%94%ED%98%B8%ED%99%94_%ED%91%9C%EC%A4%80)** 은 `PasswordFormat`이 `1`로 정의되어 있습니다. ASF는 `SteamPassword` 항목이 번역후에는 AES-암호화된 바이트 배열이 되는 **[베이스64로 인코딩된](https://ko.wikipedia.org/wiki/%EB%B2%A0%EC%9D%B4%EC%8A%A464)** 문자열이라고 생각합니다. 이 문자열은 포함된 **[초기화 벡터](https://ko.wikipedia.org/wiki/%EC%B4%88%EA%B8%B0%ED%99%94_%EB%B2%A1%ED%84%B0)** 와 ASF 암호화 키를 이용해서 복호화되어야 합니다.

위의 방식은 공격자가 암호의 복호화와 암호화에 사용되는 내장된 ASF 암호화 키를 알지 못하는 한 보안을 보장합니다. ASF는 `--cryptkey` **[명령줄 인자](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-Line-Arguments-ko-KR)** 를 통해 최대한의 보안을 위해서 특정 키의 사용을 허락합니다. 이를 생략하기로 했다면 ASF는 **알려져있고** 어플리케이션에 하드코딩된 자체 키를 사용할 것입니다. 이는 누구나 ASF 암호화를 뒤집어서 복호화된 암호를 얻을 수 있다는 뜻힙니다. 여전히 약간 노력이 필요하고 하기 쉽지는 않지만, 가능한 일입니다. 이것이 `AES` 암호화는 비밀리에 보관하고 있는 자신만의 `--cryptkey`를 사용해야 하는 이유입니다. ASF에서 사용하는 AES 방법은 만족스러운 보안성을 제공하고 평문(`PlainText`)의 단순함과 `ProtectedDataForCurrentUser`의 복잡성 간의 균형점입니다. 하지만 사용자의 `--cryptkey`와 함께 사용하는 것을 강력하게 권장합니다.

* * *

### ProtectedDataForCurrentUser

ASF가 제공하는 가장 보안성이 좋은 암호 저장방식으로, 위에서 설명한 `AES` 방법보다 훨씬 안전합니다. `PasswordFormat`은 `2`로 정의되어 있습니다. 이 방법의 주된 장점은 동시에 주된 단점입니다. `AES` 등 암호화 키를 사용하는 대신 현재 로그인 된 사용자의 로그인 자격을 이용하여 데이터를 암호화합니다. 즉, **오직** 암호화된 기기에서만, 또한 **오직** 암호화한 사용자만 데이터를 복호화할 수 있습니다. 만약 `Bot.json` 파일을 다른 누군가에게 보낸다고 해도 당신의 PC에 접근하지 않고는 암호를 복호화 할 수 없음을 보장합니다. 이것은 훌륭한 보안 방식입니다만, 동시에 가장 호환성이 없는 주된 단점을 가지고 있습니다. 이 방법을 사용해 암호화된 암호는 다른 어떤 사용자나 기기에서도 호환되지 않습니다. 심지어 운영체제를 재설치하기로 결정한 경우 **자신의** 기기에서도 호환되지 않습니다. 하지만 여전히 암호를 저장하는 최고의 방법중 하나입니다. `평문`의 보안성이 걱정되고 `없음(None)`옵션으로 매번 암호를 넣길 원하지 않는다면, 당신의 것이 아닌 다른 기기에서 환경설정에 접근할 필요가 없는 한 이것이 최선입니다.

**이 옵션은 윈도우 OS를 사용중인 기기에서만 가능함을 유의하십시오.**

* * *

### 없음(None)

100% 보안을 보장하고 누구도 Steam 암호를 훔칠 수 없음을 확신하는 유일한 방법입니다. 이 옵션을 사용하려면 `SteamPassword`을 빈 문자열(`""`)이나 `null` 값으로 설정하십시오. ASF는 필요할때 Steam 암호를 물어볼 것입니다. 그리고 어디에도 저장하지는 않지만 현재 실행중인 프로세스를 종료할 때 까지 메모리에는 보관합니다. 암호를 다루는 가장 안전한 방법이지만, ASF를 실행할때마다 수동으로 암호를 입력해야해서 가장 귀찮기도 합니다. 문제가 안된다면 가장 보안측면에서 최선의 방법입니다.

* * *

## 권장사항

호환성이 문제되지 않고, `ProtectedDataForCurrentUser` 방법의 동작 방법이 괜찮다면 ASF에서 암호를 저장하는 **권장** 옵션입니다. 이는 최고의 보안을 제공합니다. `AES` 방법은 원하는 어느 기기에서나 환경설정을 사용하길 원하는 사람들에게 좋은 선택입니다. 그리고 `평문`은 누구나 JSON 환경설정 파일을 들여다 볼 수 있다는 것을 개의치 않는다면 가장 단순한 암호 저장방식입니다.

공격자가 PC에 접근권한을 가진다면 이 세가지 방법은 모두 **보안에 취약**함을 명심하십시오. ASF는 암호화된 암호를 복호화할 수 있어야만 하며, 당신의 기기에서 실행되는 프로그램이 그것을 할 수 있다면 같은 기기에서 실행되는 다른 프로그램도 또한 그럴 수 있습니다. `ProtectedDataForCurrentUser`는 **심지어 같은 PC를 이용하는 다른 사용자도 복호화를 할 수 없을만큼** 가장 안전하지만, 누군가가 로그인 자격과 기기정보, ASF 환경설정 파일을 숨친다면 여전히 복호화는 가능합니다.

ASF를 어쩌다 한번 실행하거나 매 ASF 실행시마다 암호넣는 것을 귀찮아하지 않는 사람이라면, Steam 암호를 어디에도 저장하지 않는 없음(`None`) 방식이 가장 안전합니다.

* * *

# 복호화

ASF는 이미 암호화된 암호를 복호화하는 어떠한 방법도 지원하지 않습니다. 복호화 방법은 프로세스 안에서 데이터에 접근하기 위해 내부적으로만 사용됩니다. `ProtectedDataForCurrentUser`를 사용하는 ASF를 다른 기기로 옮기는 등 암호화 절차를 되돌리고 싶다면, 단순히 `PasswordFormat`을 `0` (평문)으로 되돌려놓고, `SteamPassword`를 적절히 입력하십시오. ASF를 평소처럼 실행하고, 처음부터 절차를 반복하십시오.