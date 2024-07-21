---
description: mysqld 프로세스를 삭제하다 마주한 에러입니다.
---

# macOS 업데이트 후 mysql 실행 에러

macOS 업데이트 후 이전에는 보이지 않았던 mysqld 프로세스가 활성 상태에서 메모리를 잡아 먹는게 꼴보기 싫었습니다. 300mb 정도 사용하는데 16G 램에는 너무 크게 느껴집니다. 심지어 강제 종료도 되질 않네요.

열심히 구글링을 해서 mysql 을 삭제하고 다시 다운로드 받았습니다. 전 homebrew 를 사용하기 때문에 아래와 같이 진행했습니다.

```bash
brew uninstall mysql

brew install mysql

brew services start mysql
-> Error: Failure while executing; `/bin/launchctl bootstrap gui/501 /Users/devlee/Library/LaunchAgents/homebrew.mxcl.mysql.plist` exited with 5.
```

다시 설치한 mysql 은 실행이 되지 않고\
`Failure while executing; /bin/launchctl bootstrap gui/501 /Users/username/Library/LaunchAgents/homebrew.mxcl.mysql.plist exited with 5` 에러를 발생시킵니다.\
`homebrew.mxcl.mysql.plist` 이 파일이 있는 디렉토리가 뭔가 문제 같습니다. 그리고 실행할 때마다 mysql-safe 라는 프로세스가 백그라운드로 실행된다고 맥북 알림이 뜹니다.\
mysql 은 실행되지 않았기 때문에 mysql-safe 라는 프로세스는 실행이 되면 안되는데 뭔가 mysql 이 제대로 삭제 되지 않았고 캐시가 남아 mysql-safe 가 실행되어 있는 것으로 추측이 됩니다.

아래 명령어를 통해 제대로 삭제하고 다시 설치하니 정상적으로 실행됩니다.

```bash
brew remove mysql
brew cleanup
launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
sudo rm -rf /usr/local/var/mysql

brew install mysql
```

`LaunchAgents` 폴더는 맥에서 시작 프로그램을 관리합니다.\
`plist` 파일은 어플리케이션에서 필요한 설정 정보를 저장하는 XML 형식의 파일입니다.\
따라서 위 명령어는 mysql 을 삭제하고 brew 캐시를 삭제하고 `launchctl` 명령을 통해 mysql 관련 시작 프로그램을 unload 시키고 삭제합니다. (unload 안시키고 삭제를 안해봐서 어떻게 되는지 궁금하네요. 삭제가 안될까요?)\
그리고 `/usr/local/var/mysql` 디렉토리를 강제 삭제 시키는데 어떤 역할의 폴더인지 모르겠습니다. var 디렉토리는 시스템에서 사용되는 가변 파일들이 저장되는 곳이라는데 mysql 관련 파일들이 있던 것으로 추측된다.\
다음으로 mysql 을 설치하고 실행하면 정상 실행이 됩니다.\
리눅스 공부를 해야겠습니다.

출처: [https://lhy.kr/brew-mysql-start-error](https://lhy.kr/brew-mysql-start-error)
