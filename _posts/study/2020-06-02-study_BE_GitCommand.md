---
layout: post
author: study
title:  "Git"
description: "그동안 공부하며 알아온 Git Command"
categories: [ study ]
tags: [ git ]
---

# Linux Command

## 개요
 알아두면 좋을 Linux 명령어와 git 명령어를 사용해보며 잊기 전에 작성해두자.

## GIT
 - git diff 주소1..주소2 : 주소 1과 주소 2간 정보차이를 본다.(branch이름도 가능)
 - git 명령어 --help : 도움말 
 - git tag 이름 branch이름 : tag를 달아준다.(Light weight tag)
   - -a : git tag -a 이름 -m "annotation" branch이름 으로 anntated tag를 달아준다. (git tag -a 1.0 -m "first release" master)
 - git push --tags : 태그를 원격저장소에 올려준다.(Release)
  

### Reset
 - git reset --hard 돌아가려는커밋주소 : 커밋을 뒤로 돌려준다.(복원지점을 건드리는 거라 잘 생각하고 하자.)
   - --soft : repository에 있는 값을 지우고 해당 커밋 주소의 repository 값으로 바꿈
   - --mixed : repository, index(add)에 있는 값을 지우고 해당 커밋 주소의 repository 값으로 둘다 바꿈
   - --hard : repository, index(add), work directory에 있는 값을 지우고 해당 커밋 주소의 repository 값으로 셋다 바꿈

### Log
 - git log : git 의 log기록을 알려준다.
   - -p : git의 commit 로그기록과 차이를 알려준다.
   - --branches : 현재 저장소에 있는 모든 branch들을 보여준다.
   - --decorate : git의 log에서 branch정보를 알려주는데, 최근버전부터 default값이 된거 같다. 사용 안해도 됨.
   - --graph : git의 branch의 구도를 graph로 보여준다.
   - --oneline : graph와 함께 쓰이면서 더 간결하게 graph를 보여준다.
   - branch이름1..branch이름2 : branch 간 로그차이를 보여준다.
   - --reverse : 역순으로 보여준다.

### Merge
 - git merge 들어가려는 대상(Starting Point) : 받아들이려는 branch로(Destination) checkout하여 Starting Point를 입력하여 병합한다. 
    - Fast-forward : 만일 병합하려는 두 대상이 하나의 대상 목적지에 종속적이라면 merge하는 것 자체가 사실상 단순히 commit을 대상 목적지로 이동시키는 것이기 때문에 별도의 commit을 발생하지 아니하는데 이를 Fast-forward라고 한다.

    - Merge made by the 'recursive' strategy. : 병합하려는 두 대상이 하나의 뿌리를 둔 다른 가지 형태로 구성되어 있다면 해당 가지의 마지막 지점을 병합하여 새로운 commit을(merge commit) 만든다. 해당 commit은 Destination이 가지고 있다.

    - CONFLICT (content) : merge하려는 대상 중 서로 같은 내용이 동시에 수정될 경우 해당 문구를 발생하며 해당branch status 에서 해당branch|MERGING status로 변하게 된다.(당연히 어떤 branch가 진짜인지는 기계가 판단할수 없기 때문) 이때 명령어 status를 입력하여 Unmerged 된 공간을 확인후 수정하도록 한뒤 commit을 하면 자동으로 merge commit이 진행되게 된다.

### Branch
 - git branch 이름 : 새로운 branch를 만듬 
 - git checkout 목적지 : 목적지로 이동(commit의 주소로도 갈 수 있다.)
   - -b : 새로운 branch를 만들고 그 branch로 이동
   - -d : branch를 삭제한다. (삭제하고자 하는 branch에서 사용불가하다.)
   - -D : 해당 branch가 어떤 branch에도 종속적이지 않는 자료를 가지고 있다면, -d로 명령할 경우, 정말 삭제할 것이냐고 묻는다. 이때 강제로 지워준다.

### Stash
 - git stash : git stash save와 동일한 작업이며, work space에 지장이 있을 파일을 commit하지 않고 어딘가에 잠시 두게 해준다. 해당 명령은 여러번 하게 되고 stack에 저장된다. 단, 해당파일은 추적되고 있어야 한다. 다시 말해 적어도 한번 git에 add가 되어있어야 한다. 
 - git stash apply : 어딘가에 잠시 둔 파일을 팝업해준다.
 - git stash list : stash로 저장을 해두면 해당 저장 log는 명시적으로 지우지 않는 이상 reset --hard 를 하더라도 남아있는데 이를 보는 명령어이다.
 - git stash drop : 저장된 로그 중 가장 최근 로그 1개를 지워준다.
   - 동시에 하려면 git stash apply; git stash drop;
 - git stash pop : git stash apply; git stash drop;와 같은 작업을 해주는 명령어.
  


## Linux
 - cd : 디렉토리 이동
   - cd / : root directory 이동
   - cd ~ : home directory 이동
 - Ctrl + insert : 복사
 - Shift + insert : 붙여넣기
 
## MAN
 - p : 나가기
 - h : man 사용법 확인 (help)
 - 방향키 & 엔터 : 한줄씩 넘기기
 - page Up & Down, Space Bar : 한페이지 씩 넘기기
 - /검색어 (enter) : 검색
   - n : 다음문자
   - N : 이전문자
 
