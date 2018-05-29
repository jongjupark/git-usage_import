# git-usage
자주 사용하는 깃 명령어 모음 


## 구조 

코드는 아래 세 단계에 걸쳐 저장된다.

스테이징 -> 커밋 -> 원격저장소

1. git add {파일명} 으로 파일을 스테이징 상태에 넣는다.
2. git commit 으로 스테이징 상태에 있는 모든 변경사항을 커밋한다. 여기까지가 로컬에서의 작업
3. git push 로 커밋된 저장소를 원격 저장소로 밀어넣는다.


## 기본 명령어

저장소 생성

```
git init
```

원격 저장소로부터 복제 

```
git clone {url}
```

변경 사항 체크

```
git status // 
```

특정 파일 스테이징

```
git add {파일명} 
```

변경된 모든 파일 스테이징

```
git add * 
```

커밋

```
git commit -m “{변경 내용}” 
```

원격으로 보내기

```
git push origin master 
```

원격저장소 추가

```
git remote add origin {원격서버주소} 
```

참고 페이지

- download(osx): http://code.google.com/p/git-osx-installer/downloads/list
- download(windows): http://git-scm.com/download/win
- 설치 메뉴얼: http://blog.outsider.ne.kr/389
- 사용 메뉴얼:http://dogfeet.github.io/articles/2012/how-to-github.html
- git 간편 안내서: http://rogerdudler.github.com/git-guide/index.ko.html
- 한장으로 핵심 기능만: http://rogerdudler.github.com/git-guide/files/git_cheat_sheet.pdf


## Commit

커밋 합치기

```
git rebase -i HEAD~4 // 최신 4개의 커밋을 하나로 합치기
```

커밋 메세지 수성

```
$ git commit --amend // 마지막 커밋메세지 수정(ref)
```

간단한 commit방법

```
$ git add {변경한 파일병}
$ git commit -m “{변경 내용}"
```

커밋 이력 확인

```
$ git log // 모든 커밋로그 확인
$ git log -3 // 최근 3개 커밋로그 확인
$ git log --pretty=oneline // 각 커밋을 한 줄로 표시
```

커밋 취소

```
$ git reset HEAD^ // 마지막 커밋 삭제
$ git reset --hard HEAD // 마지막 커밋 상태로 되돌림
$ git reset HEAD * // 스테이징을 언스테이징으로 변경, ref
```


## Branch

master 브랜치를 특정 커밋으로 옮기기

```
git checkout better_branch
git merge --strategy=ours master    # keep the content of this branch, but record a merge
git checkout master
git merge better_branch            # fast-forward master up to the merge
```

브랜치 목록

```
$ git branch // 로컬
$ git branch -r // 리모트 
$ git branch -a // 로컬, 리모트 포함된 모든 브랜치 보기
```

브랜치 생성

```
git branch new master // master -> new 브랜치 생성
git push origin new // new 브랜치를 리모트로 보내기
```

브랜치 삭제

```
git branch -D {삭제할 브랜치 명} // local
git push origin :{the_remote_branch} // remote
```

빈 브랜치 생성

```
$ git checkout --orphan {새로운 브랜치 명}
$ git commit -a // 커밋해야 새로운 브랜치 생성됨
$ git checkout -b new-branch // 브랜치 생성과 동시에 체크아웃
```

리모트 브랜치 가져오기

```
$ git checkout -t origin/{가져올 브랜치명} // ref
```

브랜치 이름 변경

```
$ git branch -m {new name} // ref
```


## Tag


태그 생성

```
git tag -a {tag name} -m {tag message} {commit hash}
git tag {tag name} {tag name} -f -m "{new message}" // Edit tag message
```

태그 삭제

```
git tag -d {tag name}
git push origin :tags/{tag name} // remote
```

태그 푸시

```
git push origin --tags
git push origin {tag name}
git push --tags
```


## 기타 

파일 삭제

```
git rm --cached --ignore-unmatch [삭제할 파일명]
```

히스토리 삭제

- 목적: 패스워드, 아이디 같은 비공개 정보가 담긴 파일을 실수로 올렸을 때 삭제하는 방법이다. (history에서도 해당 파일만 삭제)

```
$ git clone [url] # 소스 다운로드
$ cd [foler_name] # 해당 폴더 이동
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch [삭제할 파일명]' --prune-empty -- --all # 모든 히스토리에서 해당 파일 삭제
$ git push origin master --force # 서버로 전송
```

히스토리에서 폴더 삭제:

```
git filter-branch --tree-filter 'rm -rf vendor/gems' HEAD
```

리모트 주소 추가하여 로컬에 싱크하기

```
$ git remote add upstream {리모트 주소}
$ git pull upstream {브랜치명}
```

최적화

```
$ git gc
$ git gc --aggressive
```

## 서버 설정

강제 푸시 설정

```
git config receive.denynonfastforwards false
```

## Alias

~/.gitconfig 파일을 설정하여 깃 명령어의 앨리어스를 지정할 수 있다.

~/.gitconfig > alias 부분:

```

[alias]
  br = branch
  co = checkout
  rb = rebase
  st = status
  cm = commit
  pl = pull
  ps = push
  lg = log --graph --abbrev-commit --decorate --format=format:'%C(cyan)%h%C(reset) - %C(green)(%ar)%C(reset)  %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(yellow)%d%C(reset)' --all
  ad = add
  tg = tag
  df = diff 
```

작업 실행취소
GIT에서 수행한 작업은 작업 History에 저장되고, 이 History를 토대로 작업 실행취소(Ctrl+Z)를 할 수 있습니다.



작업 내역 확인하기
가장 최근에 수행한 작업부터 순서대로 작업 History를 볼 수 있습니다.

?
git reflog


작업 실행취소하기
다음과 같이 입력하면 작업 History의 가장 최상단에 있는 1개 작업을 실행취소 합니다.

?
git reset HEAD@{1}


파일 수정 내용 초기화하기
작업을 하다가 문득 수정한 특정 파일을 초기화 하고 싶은 경우가 있을 것입니다. 이 경우 다음과 같이 입력하면 해당 파일을 HEAD 상태로 되돌립니다.

?
git checkout -- {되돌릴 파일 이름}


Commit 삭제하기 - Hard reset
이미 수행한 Commt을 삭제하기 위해서는 Reset 명령어에 --hard 옵션을 붙이면 됩니다. 예를 들어, 다음과 같이 입력하면 HEAD로부터 3개의 Commit을 삭제합니다.

?
git reset --hard HEAD~3
혹은, 다음과 같이 입력해도 됩니다. '^'의 갯수가 HEAD로부터 몇 개의 Commit을 삭제할 지 나타냅니다.

?
git reset --hard HEAD^^^


샌드위치 되어 있는 특정 Commit 무력화하기 - revert
위에서 설명한 Hard reset은 HEAD부터 특정 Commit까지를 삭제를 하는 경우에만 사용할 수 있습니다. 즉, 반드시 HEAD부터만 삭제할 수 있으며, 작업 타임라인의 중간에 있는 Commit만 쏙 뽑아내어 없앨 수는 없습니다.

작업 타임라인의 중간에 끼어 있는 Commit만 뽑아서 삭제하고 싶다면 Rebase 명령을 수행해서 삭제하려는 Commit을 제외하고 이후에 발생한 Commit들을 하나씩 다시 쌓는 작업을 해 줘야 하며, 이 때 Commit ID가 바뀌는 문제를 감수해야 합니다. 따라서, 이미 Push된 Commit에 대해서는 Rebase작업을 해서는 안 되며, 불가피하게 해야 할 경우에는 PM및 다른 개발자들에게 이 사실을 알린 뒤 신중하게 작업해야 합니다.

물론, Rebase이후 Forced Push를 하면 Repository의 Commit들을 모두 갈아 엎으면서 Commit을 삭제하는 소기의 목적을 달성할 수 있지만, 이는 개인 프로젝트와 같이 Commit ID가 바뀌어도 그닥 문제가 없는 제한적인 상황에서만 사용할 수 있습니다.

대개 특정 Commit의 작업을 취소하려는 경우, Rebase작업을 통해 Commit을 삭제하려고 시도하기 보다는, 취소하려는 Commit의 작업 내용과 정 반대되는 새로운 Commit을 만들어 붙이는 방법(Revert)으로 Commit을 삭제한 것과 동일한 효과를 만들어 주는 방법을 사용합니다. (e.g. (+1) + (-1) = 0)

이렇게 하면 소스코드 타임라인이 다소 지저분해지기는 하지만, Rebase작업을 통해 Commit ID가 바뀌면서 발생하는 혼란에 비하면 아주 작은 문제라고 할 수 있습니다.

?
git revert {무력화할 Commit ID}
이 명령을 실행하면 Commit 명령을 실행했을 때와 마찬가지로 로그를 입력하는 에디터가 나타납니다.

Revert작업은 결과적으로 보면 대상 Commit의 작업내용과 정 반대되는 작업의 새로운 Commit을 작성하는 것과 같으며, 개발자가 삭제할 Commit의 수정 사항을 일일이 반대로 편집하여 다시 Commit하는 수고를 덜어주기 위한 일종의 Commit매크로와 같습니다.

각각의 Commit은 소스코드의 상태를 저장하고 있는 것이 아니라, 이전 Commit과 비교한 변경 사항에 대한 정보를 담고 있습니다. Commit에 대한 Patch를 떠서 그 파일 내부를 열어 보면 이게 무슨 말인지 잘 이해할 수 있습니다.

이와 같은 Commit의 특성이 Revert명령을 가능하게 하는 것입니다.



