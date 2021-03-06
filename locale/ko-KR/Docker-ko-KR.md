# 도커

ASF는 3.0.3.2 버전부터 **[도커 컨테이너](https://www.docker.com/what-container)** 로도 사용가능합니다. ASF를 도커 컨테이너로 실행하는 것은 일반적인 사용자에게는 보통 장점이 없습니다. 하지만 ASF가 다른 모든 앱으로부터 분리된 샌드박스 환경에서 실행을 보장해주는, 서버에서 ASF 사용시에는 훌륭한 방법일 수 있습니다. 도커 repo는 **[여기에](https://hub.docker.com/r/justarchi/archisteamfarm)** 있습니다.

* * *

## 태그

ASF는 4가지 주요 유형의 **[태그](https://hub.docker.com/r/justarchi/archisteamfarm/tags)** 상태를 갖습니다.

### `master`

이 태그는 항상 master 분기의 최신 커밋으로 빌드된 ASF를 가리킵니다. 이는 **[릴리스 주기](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-ko-KR)**에서 묘사된 실험적 AppVeyor 빌드와 동일하게 동작합니다. 일반적으로 이 태그는 피해야 합니다. 이는 개발목적을 위해 개발자와 고급사용자용의 최고 수준으로 버그가 많은 소프트웨어입니다. GitHub master 분기의 모든 커밋마다 업데이트되며, AppVeyor 빌드처럼 업데이트가 매우 잦고 일부 깨진것이 있을 것입니다. 릴리스 주기에서 이야기한 것 처럼 이는 ASF 프로젝트의 현재 상태를 나타내기 위해 있는것이지, 안정적이거나 테스트완료되었음을 보장하는 것이 아닙니다. 이 태그는 제작 환경에서 사용해서는 안됩니다.

### `released`

위와 매우 유사하지만, 이 태그는 사전 릴리스를 포함한 최신 **[릴리스](https://github.com/JustArchiNET/ArchiSteamFarm/releases)** 버전을 항상 가리킵니다. `master` 태그와 비교하면, GitHub 태그가 제출될때마다 업데이트 됩니다. 안정적이면서도 새로운, 최첨단에 있기를 좋아하는 고급 사용자를 위한 것입니다. `latest`를 사용하고 싶지 않은 경우 추천합니다. 이 태그의 사용은 **[사전 릴리스](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Release-cycle-ko-KR)** 의 사용과 동일합니다.

### `latest`

This tag in comparison with others, as the only one includes ASF auto-updates feature and will typically point to the one of the stable versions, but not necessarily the latest one. The objective of this tag is to provide a sane default Docker container that is capable of running self-updating, OS-specific build of ASF. 이 때문에 가능한만큼 자주 업데이트 될 필요가 없습니다. 포함된 ASF 버전은 필요하다면 스스로 업데이트될 수 있습니다. 물론 `UpdatePeriod`는 안전하게 꺼도 됩니다(`0`으로 설정). 이 경우 `A.B.C.D` 릴리스를 사용해야 할 것입니다. 마찬가지로, 그대신 자동으로 업데이트 되는 `released` 태그를 만들기 위해 기본 `UpdateChannel`을 변경할 수도 있습니다.

Due to the fact that the `latest` image comes with capability of auto-updates, it includes bare OS with OS-specific ASF version, contrary to all other tags that include OS with .NET Core runtime and generic ASF version. This is because newer (updated) ASF version might also require newer runtime than the one the image could possibly be built with, which would otherwise require image to be re-built from scratch, nullifying the planned use-case.

### `A.B.C.D`

위의 태그와 비교하자면, 이 태그는 완전히 동결되었습니다. 즉, 한번 릴리스되면 업데이트 되지 않습니다. 최초 릴리스 후 한번도 건드리지 않은 GitHub 릴리스와 유사하게 동작하고, 안정적이고 동결된 환경을 보장합니다. `latest`보다 오래된 특정한 ASF 릴리스를 사용하고 싶지만 예를들어 `latest` 태그에서 제공하는 것과 같은 모든 종류의 자동업데이트를 사용하고 싶지 않은 경우 이 태그를 사용합니다.

* * *

## 어떤 태그가 최선입니까?

찾고 있는 것에 따라 다릅니다. 대부분의 사용자에게 `latest` 태그가 최선입니다. 이는 서비스로 되어있는 특별한 도커 컨테이너에 담겨있을 뿐 데스크탑 ASF가 제공하는 것을 정확하게 동일하게 제공합니다. 해당 릴리스에 ASF 버전이 묶여있는 대신 이미지를 꽤 자주 다시 빌드하는 사람은 `released` 태그를 사용하십시오. 당신의 명확한 의도 없이 어떤것도 바뀌지 않는 특정한 동결 버전을 사용하길 원한다면 `A.B.C.D` 릴리스가 당신이 항상 돌아갈 수 있는 고정된 ASF 마일스톤이 될 것입니다.

자동화된 AppVeyor 빌드처럼 `master` 빌드의 사용은 추천하지 않습니다. 이 빌드는 ASF 프로젝트의 현재 상태를 나타내기 위한 것입니다. 그 상태는 어떤것도 정상작동을 보장하지 않지만, ASF 개발에 관심이 있다면 시도해보는 것도 좋습니다.

* * *

## 아키텍쳐

ASF docker image is currently available for 3 architectures - `x64`, `arm` and `arm64`. 더 많은 정보는 **[호환성](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility-ko-KR)** 항목을 참고하십시오.

다중 아키텍처 태그는 아직 작업중이므로, 기본 `x64`가 아닌 다른 아키텍처용 빌드는 태그 이름에 `-{ARCH}`를 붙이면 됩니다. 즉, `arm` 아키텍처에서 `latest` 태그를 사용하고 싶다면 `latest-arm`를 사용하면 됩니다.

* * *

## 사용법

완전한 참조는 **[도커 공식 문서](https://docs.docker.com/engine/reference/commandline/docker)**를 참고하시고, 이 가이드에서는 간단한 사용법만 다룹니다. 더 깊이 공부하는 것도 좋습니다.

### 안녕하세요 ASF!

먼저 도커가 정상적으로 작동하는지 확인해야합니다. 이것이 ASF의 "Hello World!" 입니다.

```shell
docker pull justarchi/archisteamfarm
docker run -it --name asf --rm justarchi/archisteamfarm
```

캐시에 오래된 사본이 있을수 있으므로 `docker pull` 명령으로 최신의 `justarchi/archisteamfarm` 이미지를 사용하고 있음을 확인합니다. `docker run` 으로 당신의 새로운 ASF 도커 컨테이너를 생성하고 전경에서 실행되도록 합니다(`-it`). 지금은 잘 작동하는지 단지 테스트하는 중이므로, `--rm` 으로 컨테이너가 멈추면 제거하도록 합니다.

모든것이 성공적으로 끝나면, 모든 레이어를 당기고 컨테이너를 시작한 뒤 ASF가 정상적으로 시작해서 정의된 봇이 없다고 알리고 있음을 알게 됩니다. ASF가 도커에서 정상적으로 작동하고 있습니다. `CTRL+P`와 `CTRL+Q`를 순서대로 눌러서 전경 도커 컨테이너에서 빠져나옵니다. 그리고 `docker stop asf`를 입력해 ASF를 중지합니다.

명령어를 자세히 봤다면 태그를 선언하지 않았음을 알게 되었을 겁니다. 태그는 자동으로 기본값인 `latest` 가 됩니다. `latest` 말고 `latest-arm` 같은 다른 태그를 사용하고 싶다면 명시적으로 선언해야 합니다.

```shell
docker pull justarchi/archisteamfarm:latest-arm
docker run -it --name asf --rm justarchi/archisteamfarm:latest-arm
```

* * *

## 볼륨 사용

ASF를 도커 컨테이너에서 사용하고 있다면 프로그램 자체를 환경설정할 필요가 있습니다. 다양한 방법으로 할 수 있지만, 추천하는 것은 로컬 기기에 ASF `config` 디렉토리를 만들어서 ASF 도커 컨테이너에 공유 볼륨으로 마운트 하는 것입니다.

예를 들어, 당신의 ASF 환경설정 폴더가 `/home/archi/ASF/config` 디렉토리에 있다고 가정합니다. 이 디렉토리는 핵심 `ASF.json`과 실행하려는 봇이 담겨 있습니다. 이제 할일은 단순히 이 디렉토리를 도커 컨테이너에 공유볼륨으로 붙이는 것입니다. ASF는 여기에 환경설정 디렉토리(`/app/config`)가 있다고 생각합니다.

```shell
docker pull justarchi/archisteamfarm
docker run -it -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

그러면 끝입니다. 이제 ASF 도커 컨테이너는 로컬 기기에 있는 공유 디렉토리를 읽기-쓰기 모드로 사용하고, 이는 ASF를 설정하는데 필요한 전부입니다. In similar way you can mount other volumes that you'd like to share with ASF, such as `/app/logs` or `/app/plugins`.

물론, 이는 우리가 원하는 바를 달성하기 위한 하나의 방법일뿐이고, 자신만의 `Dockerfile`을 만들어서 환경설정 파일을 ASF 도커 컨테이너 안의 `/app/config` 디렉토리로 복사하는 등 당신이 하려는 것을 막을수 있는 것은 없습니다. 이 가이드에서는 기본적인 것만을 다룹니다.

### 볼륨 권한

ASF는 기본적으로 컨테이너 내부에서 기본값인 `root` 사용자로 실행됩니다. 우리는 이미 도커 컨테이너 안에 있으므로 보안측면에서 문제가 되지는 않습니다. 하지만 새로 생성된 공유 볼륨은 보통 `root`의 소유가 되고 이는 공유 볼륨을 사용할 때 원하는 상황은 아닙니다.

도커는 `docker run` 명령어에 `--user` **[플래그](https://docs.docker.com/engine/reference/run/#user)** 를 전달하여 ASF를 실행할 기본 사용자를 정의할 수 있습니다. 예를 들어 `id` 명령어를 통해 `uid`와 `gid`를 확인하고 이를 명령어의 일부로 줄 수 있습니다. 예를 들어, 해당 사용자의 `uid`와 `gid`가 1000 인 경우,

```shell
docker pull justarchi/archisteamfarm
docker run -it -u 1000:1000 -v /home/archi/ASF/config:/app/config --name asf justarchi/archisteamfarm
```

기본적으로 ASF가 사용하는 `/app` 디렉토리는 여전히 `root`의 소유임을 명심하십시오. ASF를 다른 사용자로 실행하면 ASF 프로세스는 파일에 쓰기 권한을 갖지 못합니다. 쓰기 권한은 작업에 필수적인 것은 아니지만 자동 업데이트 기능 등에서는 치명적입니다. 이를 수정하기 위해서는 모든 ASF 파일을 기본 `root`에서 새 사용자로 소유권을 변경하는 것만으로 충분합니다.

```shell
docker exec -u root asf chown -hR 1000:1000 /app
```

ASF 프로세스에 일반 사용자를 사용하기로 정한 경우에만, 컨테이너를 `docker run`으로 생성한 후 한번만 하면 됩니다. 위의 `1000:1000` 인자를 ASF가 실행될 실제 `uid`와 `gid`로 변경하는 것을 잊지 마십시오.

* * *

## Multiple instances synchronization

ASF includes support for multiple instances synchronization, as stated in **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#multiple-instances)** section. When running ASF in docker container, you can optionally "opt-in" into the process, in case you're running multiple containers with ASF and you'd like for them to synchronize with each other.

By default, each ASF running inside a docker container is standalone, which means that no synchronization takes place. In order to enable synchronization between them, you must bind `/tmp/ASF` path in every ASF container that you want to synchronize, to one, shared path on your docker host, in read-write mode. This is achieved exactly the same as binding a volume which was described above, just with different paths:

```shell
mkdir -p /tmp/ASF-g1
docker pull justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/archi/ASF/config:/app/config --name asf1 justarchi/archisteamfarm
docker run -v /tmp/ASF-g1:/tmp/ASF -v /home/john/ASF/config:/app/config --name asf2 justarchi/archisteamfarm
# And so on, all ASF containers are now synchronized with each other
```

We recommend to bind ASF's `/tmp/ASF` directory also to a temporary `/tmp` directory on your machine, but of course you're free to choose any other one that satisfies your usage. Each ASF container that is expected to be synchronized should have its `/tmp/ASF` directory shared with other containers that are taking part in the same synchronization process.

As you've probably guessed from example above, it's also possible to create two or more "synchronization groups", by binding different docker host paths into ASF's `/tmp/ASF`.

Mounting `/tmp/ASF` is completely optional and actually not recommended, unless you explicitly want to synchronize two or more ASF containers. We do not recommend mounting `/tmp/ASF` for single-container usage, as it brings absolutely no benefits if you expect to run just one ASF container, and it might actually cause issues that could otherwise be avoided.

* * *

## 명령줄 인자

ASF는 환경변수를 통해 **[명령줄 인자](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-ko-KR)** 를 도커 컨테이너로 넘겨줄 수 있습니다. 지원하는 스위치를 위해서는 특별한 환경변수를, 그 외에는 `ASF_ARGS` 를 사용해야 합니다. `docker run` 에 `-e` 스위치를 붙이면 됩니다. 다음은 예시입니다.

```shell
docker pull justarchi/archisteamfarm
docker run -it -e "ASF_CRYPTKEY=MyPassword" -e "ASF_ARGS=--process-required" --name asf justarchi/archisteamfarm
```

이렇게 하면 `--cryptkey` 인자 뿐 아니라 다른 인자들도 도커 컨테이너 내부에서 실행되는 ASF 프로세스로 전달할 것입니다. Of course, if you're advanced user then you can also modify `ENTRYPOINT` or add `CMD` and pass your custom arguments yourself.

사용자지정 암호화 키나 다른 고급 옵션을 주고싶은 것이 아니라면 보통은 어떤 특별한 환경 변수를 포함하지 않습니다. 도커 컨테이너는 이미 기본값으로 `--no-restart` `--process-required` `--system-required`로 실행하도록 설정되어있습니다. 따라서 위의 `ASF_ARGS` 는 필요없고, `ASF_CRYPTKEY` 만이 적용됩니다.

* * *

## IPC

IPC를 사용하려면, 먼저 `IPC` **[일반 환경설정 속성값](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-ko-KR#global-config)** 을 `true`로 변경해야 합니다. 그 외에도, `localhost`로 되어있는 기본 리스닝 주소도 **수정해야 합니다**. 도커는 외부 트래픽을 루프백 인터페이스로 라우팅할 수 없습니다. 모든 인터페이스를 리슨하는 설정값의 예시입니다. `http://*:1242`. 물론 로컬 LAN이나 VPN 네트워크 같은 더 제한적인 바인딩을 사용할 수도 있지만, 외부에서 라우팅 가능해야 합니다. `localhost`는 게스트 기기안에서만 라우팅하므로 이것이 불가능합니다.

위와 같이 하려면, 아래와 같은 **[사용자 설정 IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-ko-KR#custom-configuration)**를 사용해야 합니다.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

IPC를 루프백이 아닌 인터페이스에 설정하고 나면, `-P` 나 `-p` 스위치를 사용해서 도커에게 ASF의 `1242/tcp` 포트를 알려주어야 합니다.

예를 들어, 이 명령어는 ASF의 IPC 인터페이스를 호스트 기기에 열 것입니다.

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 --name asf justarchi/archisteamfarm
```

모든 것을 정확하게 설정했다면 위의 `docker run` 명령어는 호스트 기기의 **[IPC](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC-ko-KR)** 인터페이스가 게스트 기기로 리다이렉트된 표준 `localhost:1242` 라우트에서동작하도록 할 것입니다. 이 라우트를 더 많이 노출하지 않음을 알아 두십시오. 연결은 오직 도커 호스트안에서만 이루어지고 안전하게 유지됩니다.

* * *

### 완전한 예제

위의 지식을 모두 합치면 완전한 설치 예시는 다음과 같습니다.

```shell
docker pull justarchi/archisteamfarm
docker run -it -p 127.0.0.1:1242:1242 -p [::1]:1242:1242 -v /home/archi/asf:/app/config --name asf justarchi/archisteamfarm
```

This assumes that you'll use a single ASF container, with all ASF config files in `/home/archi/asf`. You should modify the config path to the one that matches your machine. 다음과 같은 `IPC.config` 를 환경설정 디렉토리에 넣기로 결정했다면 추가로 IPC 사용도 가능합니다.

```json
{
    "Kestrel": {
        "Endpoints": {
            "HTTP": {
                "Url": "http://*:1242"
            }
        }
    }
}
```

* * *

## 프로 팁

ASF 도커 컨테이너가 준비되어있다면 `docker run`를 매번 실행할 필요가 없습니다. `docker stop asf`와 `docker start asf`로 ASF 도커 컨테이너를 쉽게 멈추고 시작할 수 있습니다. `latest` 태그를 사용하고 있지 않다면 ASF를 업데이트 하는 경우 `docker stop`, `docker rm`, `docker pull`, `docker run` 을 다시 한번 실행해야 함을 명심하십시오. 이는 매 버전마다 새로운 ASF 도커 이미지로부터 다시 빌드해야 하기 때문입니다. `latest` 태그에서는 ASF가 스스로 자동 업데이트할 수 있으며, 최신 ASF를 사용하는데 이미지를 다시 빌드할 필요가 없습니다. 물론 새로운 .NET Core 런타임과 그 밑의 OS를 사용하기 위해 때때로 다시 빌드하는 것도 좋은 생각입니다.

위에서 암시하였듯이, `latest` 가 아닌 ASF 태그는 스스로 자동 업데이트하지 않습니다. 즉, **당신이** 최신의 `justarchi/archisteamfarm` 저장소를 사용할 주체입니다. 보통 앱이 실행중에는 코드를 건드려서는 안되기 때문에 많은 장점을 갖지만, 도커 컨테이너에 있는 ASF는 걱정할 필요가 없다는 편리함도 있습니다. 좋은 사례와 정확한 도커 사용례를 잘 살핀다면 우리가 추천하는 것은 `latest` 태그가 아닌 `released` 태그입니다. 하지만 그게 귀찮고 ASF가 동작도 잘하고 자동 업데이트도 하길 원하면 `latest` 태그가 그 답입니다.

보통 ASF 도커 컨테이너는 `Headless: true` 일반 설정상태로 실행해야 합니다. 이는 ASF에게 세부적인 내용이 빠져있어도 당신이 줄 수 없으며 묻지 말 것을 알립니다. 물론 초기 설치단계에서는 이 옵션을 `false`로 해서 쉽게 설정할 수 있지만, 장기적으로는 ASF 콘솔에 매여있지 않으므로 ASF에게 이를 알리고 필요가 있으면 `input` **[명령어](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands-ko-KR)** 를 사용하는 것이 맞습니다. 이렇게 해서 ASF는 일어나지 않을 사용자 입력을 무한히 기다리지 않을 수 있고 이에 소요되는 자원낭비를 막을 수 있습니다. It will also allow ASF to run in non-interactive mode inside container, which is crucial e.g. in regards to forwarding signals, making it possible for ASF to gracefully close on `docker stop asf` request.