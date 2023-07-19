merge 테스트 - master 브랜치 수정 사항
# Git Workflow 정리

## 목차

1. [Git Workflow]
2. [Workflow 관련 Git 명령어 소개]

---

## 1. [Git Workflow]
### Git Workflow 란?
* 2010년 Vincent Driessen이 아래 Reference에 있는 A successful Git branching model이라는 글을 기고 하면서 널리 알려진 Git으로 개발하는 방법론
* 5개의 브랜치 형태를 통해 코드를 관리하는 방법론
  * master
    * 브랜치의 시작이며 고객에게 배포하기 위한 브랜치
  * develop   
    * 개발 브랜치로 master 브랜치에서 분기되며, feature 브랜치에서 개발된 코드를 merge 하거나, 기능 개발이 완료된 코드를 반영하는 브랜치
    * 모든 기능이 개발되고, QA (품질검사) 완료 후 master 브랜치에 merge 됨
  * feature
    * 단위 기능을 개발하는 브랜치로 develop 브랜치에서 분기되며, 개발 완료 후 develop 브랜치로 merge 를 진행한다. 
  * release
    * 배포 전 QA (품질검사) 를 하기위한 브랜치로 develop 브랜치에서 분기됨
    * QA 과정에서 나온 버그는 release 브랜치에서 수정 및 반영을 진행하며, QA 가 완료되면 master 브랜치와, develop 브랜치에 merge 를 진행한다.
  * hotfix
    * master 브랜치로 배포 후 버그가 발생하였을 떄 긴급하게 수정을 진행하는 브랜치
      
<img src="git_flow.png" width="1000px" height="1000px"></img><br/>

## 2. [Workflow 관련 Git 명령어 소개]

* git 에서 형상 관리를 진행함에 있어 유용한 키워드들을 정리하였다.
* branch
  * 브랜치를 생성하는 명령어
  * git branch <<브랜치 명>>
  * 예시
    * 생성 전
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git branch
    * main
    ```
    * 생성 후     
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git branch master
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git branch
    * main
      master
    ```
  * 리모트 반영 시 git push origin <<브랜치명>> 을 통해 반영
* checkout
  * 브랜치를 전환하거나, 파일의 수정 사항을 복구할 때 사용하는 명령어
  * git checkout <<브랜치 명>> or git checkout <<파일명>>
  * 예시
    * 브랜치 전환
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git branch
    * main
      master
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git checkout master
    Switched to branch 'master'
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git branch
      main
    * master
    ```
    * 변경 사항 복구
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git diff --color
    diff --git a/git_workflow.md b/git_workflow.md
    index 4dc308c..fcdb166 100644
    --- a/git_workflow.md
    +++ b/git_workflow.md
    @@ -1,3 +1,5 @@
    +checkout 테스트
    +
     # Git Workflow 정리

     ## 목차
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git checkout git_workflow.md
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git diff --color
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$
    ```
* tag
  * 태깅을 달기 위한 명령어
  * 예시
    * 태깅 전
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git log
    commit d39725cf99b9a111cf66cdb14a7bb15f9d7de68f (HEAD -> master, origin/master, origin/main, origin/HEAD, main)
    ```
    * 태깅 후
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git tag V4.0.8
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git log
    commit d39725cf99b9a111cf66cdb14a7bb15f9d7de68f (HEAD -> master, tag: V4.0.8, origin/master, origin/main, origin/HEAD, main)
    ```
  * 리모트 반영 시 git push origin <<태그명>> 을 통해 반영
* stash
  * 수정 사항을 임시 저장하는 명령어
  * git stash 로 임시 저장 -> git stash pop 으로 복구
  * 예시
    * stash 전
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git stash list
    
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git diff --color
    diff --git a/git_workflow.md b/git_workflow.md
    index 4dc308c..10b4789 100644
    --- a/git_workflow.md
    +++ b/git_workflow.md
    @@ -1,3 +1,5 @@
    +Stash 테스트
    +
    ```
    * stash
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git stash
    Saved working directory and index state WIP on master: d39725c Add files via upload
    
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git diff --color
    
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git stash list
    stash@{0}: WIP on master: d39725c Add files via upload
    ```
    * stash pop
    ```
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git stash pop
    On branch master
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

	    modified:   git_workflow.md

    no changes added to commit (use "git add" and/or "git commit -a")
    Dropped refs/stash@{0} (2483bd2b0a59e21a2a2850a073e97f51f7dd282b)
    
    SeonilKimui-MacBook-Pro-2:git_workflow seonil$ git diff --color
    diff --git a/git_workflow.md b/git_workflow.md
    index 4dc308c..10b4789 100644
    --- a/git_workflow.md
    +++ b/git_workflow.md
    @@ -1,3 +1,5 @@
    +Stash 테스트
    +
    # Git Workflow 정리
    ```
* merge
  * 다른 브랜치의 수정 사항을 병합하는 키워드
  * 병합할 브랜치로 checkout 한 후 병합 대상 브랜치로 merge 명령어 수행
    * git merge main master (main 브랜치로 master 의 수정 사항이 병합)
    * 
* fetch
  
* rebase
  
* revert
  * 반영된 코드를 이전으로 원복 시키는 키워드
  * 

레퍼런스 
 * https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html
