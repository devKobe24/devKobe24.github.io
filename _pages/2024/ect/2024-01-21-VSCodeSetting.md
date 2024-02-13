---
title: 🧑‍💻 [환경설정] Zsh에서 `code .` 커맨드가 동작하지 않을 때
date: 2024-01-21 03:39:00
modified: 2024-01-21 03:51:00
tags: [setting]
description: 환경설정.
---

# 🧑‍💻 [환경설정] Zsh에서 `code .` 커맨드가 동작하지 않을 때

터미널 또는 zsh에서 `command not found: code`와 같은 `code` 커맨드를 인식하지 못할 때, VSCode의 CLI(명령줄 인터페이스)가 시스템의 PATH에 추가 되지 않았음을 나타냅니다.<br>
<br>
해결 방법은 다음과 같습니다:<br>
<br>
1. VSCode 설치 확인: 우선 VSCode가 시스템에 설치되어 있는지 확인합니다.
2. VSCode 명령줄 도구 설치/확인:
    - VSCode를 열고, 명령 팔레트를 엽니다(`Cmd+ Shift + P`)
    - `Shell Command Install: 'code' command in PATH` 명령을 검색하고 실행합니다.
        - 위 과정은 `code` 명령어를 시스템 PATH에 추가하는 과정입니다.
3. 쉘 재시작:
    - 변경 사항을 적용하기 위해 터미널이나 zsh 세션을 재시작합니다.
4. PATH 설정 확인:
    - zsh 설정 파일(보통 `~/.zshrc`)을 열고 `code` 명령어의 경로가 PATH에 추가되었는지 확인합니다.
    - 추가되지 않았다면, 수동으로 PATH에 VSCode의 실행 파일 경로를 추가해야 할 수 있습니다.
5. 터미널에서 VSCode 실행:
    - 변경 사항을 적용한 후, `code .` 명령어를 다시 실행하여 VSCode가 정상적으로 열리는지 확인합니다.
