---
title: "[Setting] 듀얼부팅 환경에서 블루투스 페어링 키 동기화"
categories:
- develop
- setting
img_path: "/assets/img/posts/develop/setting/2026-08-13-bluetooth-setting"
image:
  path: "/assets/img/posts/develop/setting/2026-08-13-bluetooth-setting/communication3.png"
---

## **배경**
저는 `Marshall`사의 `EMBERTON` 블루투스 스피커를 애용하고 있습니다.

![1]({{ page.img_path }}/emberton.png){: .shadow .rounded-10}

음질이 좋고 생활 방수도 되고 내구성이 좋아서 거의 5년간 사용했는데 잔고장이 하나도 없었습니다. 

정말 잘 쓰고 있지만 굳이 문제를 하나 꼽는다면, `EMBERTON` 블루투스 스피커의 전용 동글(`Dongle`)이 없어서 제 컴퓨터와 같은 듀얼부팅(`Windows`, `Ubuntu`) 환경에서는 매번 페어링을 새로 해줘야 한다는 점입니다.

> **그래서 전용 동글이 없더라도 듀얼부팅 환경에서 링크 키(Link Key)를 동일하게 설정함으로서 매번 페어링하는 번거로움을 없애도록 하겠습니다.**

## **환경**
저는 아래의 환경에서 작업을 하고 있습니다.
- 듀얼부팅(`Windows 11`, `Ubuntu 20.04`)
- `EMBERTON` 블루투스 스피커
- 다이소 블루투스 동글(품명: 액센_블루투스 동글(VER5.3))

## **개발 지원 도구**
`ChatGPT GPT-5.6 sol`의 도움을 받아서 작성했습니다.

## **문제점**
일단은 왜 이런 문제가 생기는지 살펴보겠습니다. 그 전에 먼저 `MAC`과 `Link key`라는 개념을 알고 있어야 합니다.

블루투스 스피커와 동글은 고유의 `MAC`을 가지고 있습니다. 그런데 `MAC`만으로 블루투스 장비 간에 페어링 하기에는 보안상으로 문제가 있습니다. `MAC`은 공공연하게 노출 되어 있는 주소이기 때문에, `MAC`만으로 페어링이 가능하다면 공격자가 `MAC` 주소를 쉽게 찾아내고 연결을 가로챌 수 있습니다. 그래서 `Link key`라는 것이 중요합니다. 

> `MAC`이란?
>: `Media Access Control Address`으로 랜카드 등 네트워크 장비에 부여된 고유한 물리적 식별 번호입니다. 말 그대로 각 기기마다 가지고 있는 고유의 번호라고 생각하시면 되겠습니다.
{: .prompt-info}

`Link key`는 블루투스 장비 간에 처음 페어링을 할 때 생성합니다. 그리고 각 장비들은 `Link key`를 기억함으로서 나중에도 다시 페어링을 할 수 있는 것입니다.

> **페어링을 로그인으로 예를 들어 설명한다면, MAC이 ID이고 Link key는 비밀번호라고 생각하시면 되겠습니다.**

다시 돌아와서 듀얼부팅 환경에서 왜 매번 페어링을 새로 해줘야하는지를 설명드리겠습니다. 듀얼부팅 환경에서는 `Link key`를 생성하는 운영체제가 2개나 있어서 운영체제들이 각자 다른 `Link key`를 생성하게 됩니다. 아래의 그림을 보면서 설명하도록 하겠습니다.

![2]({{ page.img_path }}/communication1.png){: .shadow .rounded-10}

먼저 `Windows 11`와 블루투스 스피커 간에 `Link key`를 생성했습니다. 블루투스 스피커와 서로 같은 `Link key`를 공유하고 있습니다. 이제 `Ubuntu 20.04`와 블루투스 스피커를 연결해보겠습니다.

![3]({{ page.img_path }}/communication2.png){: .shadow .rounded-10}

블루투스 스피커는 `Windows 11`과 약속한 `Link key`를 가지고 있는 상태입니다. 하지만 `Ubuntu 20.04`는 `Link key`가 따로 없기 때문에 위의 그림과 같이 블루투스 스피커와 새로운 `Link key`를 생성하고 공유해야합니다. 하지만 이러면 블루투스 스피커를 `Windows 11`에 다시 연결하는 경우에는 문제가 생깁니다.

![4]({{ page.img_path }}/communication3.png){: .shadow .rounded-10}

위의 그림처럼 블루투스 스피커는 `Ubuntu 20.04`와의 `Link key`를 기억하고 있기 때문에 `Windows 11`이 기억하고 있는 `Link key`와 달라서 페어링이 안 됩니다. 그래서 처음 페어링을 했던 것처럼 `Windows 11`과 공유할 수 있는 `Link key`를 또 생성해야 합니다.

> **즉, 듀얼부팅 환경에서는 운영체제를 전환하는 경우에 블루투스 스피커와의 `Link key`를 매번 생성해줘야 하는 문제점이 있습니다.**

## **해결방법**
이러한 문제점을 해결하기 위해서는 모든 운영체제에서의 `Link key`를 고정시키는 것입니다.

1. **`Ubuntu` 환경에서 우리가 사용하고 있는 블루투스 스피커와 동글의 `MAC` 주소를 확인해보도록 하겠습니다.**
    ```bash
    # Ubuntu 환경에 장착된 블루투스 컨트롤러 확인
    bluetoothctl list
    ```

    ![5]({{ page.img_path }}/c1.png){: .shadow .rounded-10}

    그럼 결과가 위와 같이 나오게 되는데, 여기서 나온 `MAC` 주소가 블루투스 동글의 `MAC` 주소입니다. 블루투스 스피커의 `MAC` 주소도 확인해보도록 하겠습니다.

    ```bash
    # 블루투스 통신으로 연결된 장치 확인
    bluetoothctl devices
    ```

    ![6]({{ page.img_path }}/c2.png){: .shadow .rounded-10}

    그럼 결과가 위와 같이 나오고 마찬가지로 명시되어 있는 `MAC` 주소가 제 `EMBERTON` 스피커의 `MAC` 주소입니다.

2. **`Windows` 운영체제에서 설정한 `Link key`를 `Ubuntu` 운영체제에 덮어 씌우겠습니다.**
    
    반대로 `Ubuntu`에서 생성한 `Link key`를 `Windows`에서 덮어 씌워도 되지만 비교적 `Ubuntu` 운영체제에서 설정을 변경하는게 더 쉽기때문에 이 포스팅에는 그렇게 하겠습니다.

    일단은 `Windows` 운영체제에서 블루투스 스피커와 다시 페어링을 해서 `Link key`를 생성할 수 있도록 합니다. 이후 `PowerShell`을 관리자 권한으로 실행시켜 아래의 명령어들을 순서대로 입력합니다.

    ```bash
    # Link Key가 저장된 Windows 레지스트리를 조회하고, 그 결과와 오류 메시지를 "C:\btkeys.txt" 파일에 저장하도록 작업 스케줄러의 실행 동작을 정의하는 명령어
    $action = New-ScheduledTaskAction -Execute "cmd.exe" -Argument '/c reg query "HKLM\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys" /s > C:\btkeys.txt 2>&1'

    # Link Key가 저장된 보호된 레지스트리를 읽을 수 있도록, 작업을 SYSTEM 계정의 최고 권한으로 실행하도록 설정하는 명령어
    $principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest

    # 앞서 생성한 실행 동작($action)과 권한 설정($pricipal)을 Windows 작업 스케줄러에 "BluetoothKeyRead"라는 이름의 작업을 등록하는 명령어
    Register-ScheduledTask -TaskName "BluetoothKeyRead" -Action $action -Principal $principal -Force
    ```
    ![7]({{ page.img_path }}/c3.png){: .shadow}

    위와 같이 이제 `BluetoothKeyRead`라는 작업이 `Windows 작업 스케쥴러`에 등록되었음을 확인할 수 있습니다.

    이제 `BluetoothKeyRead` 작업을 수행하고 `C:\btkeys.txt`을 읽어서 `Link key`를 확인해보겠습니다.

    ```bash
    # "BluetoothKeyRead" 시작
    Start-ScheduledTask -TaskName "BluetoothKeyRead"

    # 2~3초 정도 작업을 기다려줍니다.

    # "C:\btkeys.txt" 파일 확인
    Get-Content C:\btkeys.txt
    ```
    ![8]({{ page.img_path }}/c4.png){: .shadow}

    위와 같이 `EMBERTON`의 `MAC` 주소와 같은 줄에 `Link key`를 확인할 수 있습니다.

    > `Ubuntu`에서는 MAC 주소를 `:`와 같은 구분기호로 구분해놨다면, Windows에서는 구분기호가 따로 없습니다.
    > : 예시) Ubuntu -> `01:23:45:67:89:AB`, Windows -> `0123456789AB`
    {: .prompt-warning}

    이제는 `BluetoothKeyRead` 작업과 `C:\btkeys.txt`는 필요 없어졌으니 삭제해주도록 하겠습니다.

    ```bash
    # "BluetoothKeyRead" 작업 삭제
    Unregister-ScheduledTask -TaskName "BluetoothKeyRead" -Confirm:$false

    # "C:\btkeys.txt" 삭제
    Remove-Item C:\btkeys.txt
    ```

3. **이제 블루투스 스피커에 대한 `Link key`를 `Ubuntu 20.04`에서 고정시키도록 하겠습니다.**

    지금 상황은 1번에서 `Ubuntu` 환경에서 한번 페어링을 했기 때문에 `Link key`는 다르더라도 블루투스 설정은 남아있는 상태입니다. 그래서 설정 파일을 직접 열어서 `Link key`를 수정해줄 것입니다.

    그 전에 설정을 잘못 건들여서 문제가 생길 수 있으니 백업을 먼저 만들어주도록 하겠습니다.
    ```bash
    sudo cp -a /var/lib/bluetooth /var/lib/bluetooth.backup
    ```
    
    지금 작동하고 있는 블루투스를 중지시킵니다.
    ```bash
    sudo systemctl stop bluetooth
    ```

    `/var/lib/bluetooth`에 접근하여 설정 파일인 `info`를 수정합니다.
    ```bash
    sudo nano /var/lib/bluetooth/(블루투스 동글의 MAC 주소)/(블루투스 스피커의 MAC 주소)/info
    ```

    ![9]({{ page.img_path }}/c5.png){: .shadow}

    위의 그림처럼 `[LinkKey]`에서 `Key=` 옆에 `Link key`을 수동입력합니다.

    이제 다시 작동을 멈췄던 블루투스를 작동시킵니다.
    ```bash
    sudo systemctl start bluetooth
    ```

    일단은 재페어링을 먼저 시도하지 말고 블루투스 시스템을 먼저 열어서 연결만 시도해보도록 하겠습니다.
    ```bash
    bluetoothctl
    ```
    ![10]({{ page.img_path }}/c6.png){: .shadow}

    블루투스 시스템이 열렸으니 먼저 연결을 시도하겠습니다.

    ```bash
    connect  (EMBERTON MAC 주소)
    ```

    > 블루투스 연결이 안 되는 경우
    > : ![11]({{ page.img_path }}/fail1.png){: .shadow}
    > : 위의 그림과 같이 `connect` 명령어로 연결이 안 되는 경우도 있습니다. 페어링을 요구하는 장치와 컴퓨터 간에 `Trust` 상태가 아니기 때문에 발생하는 문제입니다. 그래서 아래와 같은 명령어를 입력하여 `Trust` 상태로 변경합니다.
    > ```bash
    > trust  (EMBERTON MAC 주소)
    > ```
    {: .prompt-info}

    이제 아래와 같이 장치명이 뜨면 제대로 연결이 된 것입니다.

    ![12]({{ page.img_path }}/c7.png){: .shadow}

    추가적으로 아래의 명령어를 입력하여 `Paired: yes`, `Trusted: yes`, `Connected: yes`가 확인이 되면 문제가 없는 상태입니다.

    ```bash
    info  (EMBERTON MAC 주소)
    ```

    ![13]({{ page.img_path }}/c8.png){: .shadow}

이제 우리는 듀얼부팅 환경에서 각 운영체제에 매번 수동으로 페어링을 시도할 필요가 없어졌습니다!!

## **(참고)Ubuntu가 자동으로 블루투스 스피커를 인식 못 하는 경우 해결방법**
Ubuntu를 켜기 전에 블루투스 스피커가 켜져 있었다면, Ubuntu가 자동으로 블루투스 스피커를 인식하지 못 할 수도 있습니다.

이 문제를 해결하기 위해서는 `.desktop` 파일을 만들어서 `Ubuntu`를 켤 때, 자동으로 연결될 수 있도록 설정합니다.

1. **`.desktop` 파일이 작동할때, 실행시키는 스크립트를 만들어줍니다.**
    ```bash
    nano ~/.local/bin/connect-emberton.sh
    ```
    아래의 내용을 스크립트에 그대로 넣습니다.

    ``` bash
    #!/bin/bash

    # 페어링되어 있는 모든 Bluetooth 장치의 MAC 주소 가져오기
    mapfile -t DEVICES < <(
        bluetoothctl paired-devices 2>/dev/null | awk '{print $2}'
    )

    # 페어링된 장치가 없으면 종료
    if [ ${#DEVICES[@]} -eq 0 ]; then
        exit 0
    fi

    # 최대 20회 재시도
    for i in {1..20}
    do
        # Bluetooth 어댑터가 준비되지 않았다면 1초 대기
        if ! bluetoothctl show 2>/dev/null | grep -q "Powered: yes"; then
            sleep 1
            continue
        fi

        ALL_CONNECTED=true

        # 페어링된 모든 장치에 대해 연결 시도
        for MAC in "${DEVICES[@]}"
        do
            # 이미 연결되어 있으면 건너뜀
            if bluetoothctl info "$MAC" 2>/dev/null | grep -q "Connected: yes"; then
                continue
            fi

            ALL_CONNECTED=false

            # 연결되지 않은 장치에 연결 시도
            bluetoothctl connect "$MAC" >/dev/null 2>&1
        done

        # 모든 장치가 연결되었다면 종료
        if [ "$ALL_CONNECTED" = true ]; then
            exit 0
        fi

        sleep 1
    done

    exit 0
    ```
3. **`connect-emberton.sh`에 실행권한을 줍니다.**
    ```bash
    chmod +x ~/.local/bin/connect-emberton.sh
    ```

4. **`.desktop` 파일을 만들어줍니다.**
    ```bash
    mkdir -p ~/.config/autostart
    nano ~/.config/autostart/connect-emberton.desktop
    ```
    아래의 내용을 입력합니다.

    ```bash
    [Desktop Entry]
    Type=Application
    Name=Connect Bluetooth Devices
    Comment=Automatically connect paired Bluetooth devices
    Exec=/home/hyojun/.local/bin/connect-bluetooth.sh
    Terminal=false
    X-GNOME-Autostart-enabled=true
    NoDisplay=true
    ```

이제 우분투를 켜기 전에 스피커가 켜져있더라도 스피커를 잘 찾을 겁니다.

## **마치며**
AI 개발자한테는 우분투와 윈도우 둘다 필요해서 듀얼부팅을 많이 사용하는 것 같습니다. 원래는 "에이 그냥 수동으로 페어링 하면 되지"라고 생각했는데, 생각보다 너무 귀찮았습니다. 이 포스팅에 나온대로 세팅을 하니까 확실이 엄청 편해졌습니다. 그리고 이번 기회에 어떻게 블루투스 통신이 작동하는지 더 명확히 알게 되어서 좋은 기회였다고 생각합니다.