# Project Guideline

- [Git](#git)
  - [Git 규칙](#some-git-rules)
  - [Git 워크플로우](#git-workflow)
  - [좋은 커밋 메시지 작성하기](#writing-good-commit-messages)
- [문서화](#documentation)

### 1.1 Git 규칙
Git에는 명심해야할 규칙들이 있습니다.
* feature 브랜치(branch)에서 작업하세요.

  _이유:_
  > 이 방법을 사용하면 모든 작업은 메인 브랜치 대신에 격리된 별도의 브랜치에서 하게 됩니다. 이렇게 하면 혼란 없이 여러개의 풀 리퀘스트(Pull Request)를 제출할 수 있습니다. 또한 잠재적으로 불안정한, 완료되지 않은 코드로 마스터 브랜치를 오염시키지 않고, 작업을 반복할 수 있습니다. [더 알아보기](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* `develop`에서 브랜치를 만드세요.

  _이유:_
  >이 방법을 사용하면, 마스터 브랜치의 코드를 항상 거의 문제없이 빌드할 수 있고, 릴리즈를 위해서 직접 사용할 수도 있습니다 (일부 프로젝트의 경우 과할 수도 있음).
* `develop`과 `master`에 직접 푸시하지 않고, 풀 리퀘스트를 만드세요.

  _이유:_
  > 풀 리퀘스트는 기능 구현을 완료한 것을 다른 팀 멤버들에게 알립니다. 또한 쉬운 코드 리뷰를 가능케 하며, 제안된 기능에 대해 토론할 수 있는 포럼을 제공합니다.
* 개발한 기능을 푸시하고 풀 리퀘스트를 만들기 전에, 로컬 `develop` 브랜치를 업데이트하고 인터랙티브한 리베이스(rebase)를 진행하세요.

  _이유:_
  > 리베이스는 요청한 브랜치(`master` 혹은 `develop`)을 병합(merge)합니다. 또한 병합 커밋을 만들지 않으면서 로컬에서 만든 커밋들을 적용합니다 (충돌이 없다고 가정한다면). 결국 깨끗한 히스토리를 남기게 됩니다. [더 알아보기](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
* 풀 리퀘스트를 만들기 전에 리베이스하는 동안 잠재적인 충돌을 제거하세요.
* 병합 후, 로컬과 원격에 있는 feature 브랜치를 삭제하세요.

  _이유:_
  > 이 방법은 더 이상 사용하지 않는 브랜치들로부터 브랜치 리스트를 정리할 것입니다. 또한, 브랜치가 `master` 또는 `develop`으로 병합되는 것을 단 한 번으로 보장합니다. feature 브랜치는 작업이 진행되고 있는 도중에만 존재해야 합니다.
* 풀 리퀘스트를 생성하기 전에, feature 브랜치는 잘 빌드되는지, 코드 스타일 체크를 포함한 모든 테스트를 통과하는 지 검증하세요.

  _이유:_
  > 안정적인 브랜치에 코드를 새로 푸시하려 할 때, 만약 feature 브랜치의 테스트가 실패한다면, 목표한 브랜치의 빌드도 실패할 가능성이 높습니다. 또한 풀 리퀘스트를 만들기 전에 코드 스타일 검사를 적용해야합니다. 이렇게 하면 가독성을 높이고, 코드에 실제 변경사항을 작성할 때 포맷을 수정하는 변경사항이 섞일 가능성을 낮춥니다.
* 이 [.gitignore file](./.gitignore)을 사용하세요.

  _이유:_
  > 이 파일에는 이미 원격 저장소에 코드와 함께 보내면 안되는 시스템 파일 목록이 있습니다. 또한 이 파일은 가장 많이 사용되는 에디터와 대부분의 공통 의존성 폴더에 대한 폴더 및 파일 설정을 포함하고 있습니다.
* `develop`과 `master` 브랜치를 보호하세요.

  _이유:_
  > 이 방법은 예측하지 못한, 돌이킬 수 없는 변경으로부터 production-ready 브랜치들을 보호합니다. 더 알아보기: [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html)
  <a name="git-workflow"></a>
### 1.2 Git 워크플로우
상기한 이유들 때문에, 우리는 [인터랙티브 리베이스](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing), 그리고 [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow)의 몇가지 요소(브랜치 네이밍과 develop 브랜치의 보유)와 함께 [Feature 브랜치 워크플로우](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)를 사용해야 합니다. 주요 단계는 다음과 같습니다.

* 새로운 프로젝트의 경우, 프로젝트 디렉토리에 Git 레포지토리를 초기화하세요. __유지보수 작업의 경우 이 단계는 무시하세요__.
   ```sh
   cd <project directory>
   git init
   ```

* 새로운 feature/bug-fix 브랜치를 체크아웃하세요.
    ```sh
    git checkout -b <branchname>
    ```
* 변경사항을 작성하세요.
    ```sh
    git add
    git commit -a
    ```
  _이유:_
  > `git commit -a`는 제목과 본문을 분리시킨 상태로 에디터를 엽니다. *섹션 1.3*에서 자세히 알아보세요.
* 놓친 변경사항을 받기 위해 원격 저장소와 동기화하세요.
    ```sh
    git checkout develop
    git pull
    ```

  _이유:_
  > 이렇게 하면 충돌(conflict)을 포함하는 풀 리퀘스트를 만드는 대신에, 당신의 컴퓨터에서 리베이스함으로써 충돌을 처리할 수 있습니다.
* 인터랙티브한 리베이스를 통해 develop 브랜치의 마지막 변경사항을 feature 브랜치로 업데이트 하세요.
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```

  _이유:_
  > --autosquash를 사용해서 모든 커밋을 하나의 커밋으로 밀어 넣을 수도 있습니다. develop 브랜치에서 하나의 기능을 위한 많은 커밋들은 아무도 원하지 않기 때문이죠. [더 알아보기](https://robots.thoughtbot.com/autosquashing-git-commits)
* 만약 충돌이 발생하지 않았다면 이 단계를 건너뛰어도 좋습니다. 충돌이 발생했다면, [그것을 해결(resolve)하고](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) 리베이스를 계속하세요.
    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```
* 브랜치를 푸시하세요. 리베이스는 이력을 변경시킵니다. 따라서 당신은 `-f`를 사용해서 원격 브랜치로 강제 변경해야합니다. 만약 다른 누군가가 당신의 브랜치에서 작업하고 있다면, 조금 덜 파괴적인 `--force-with-lease`를 사용하세요.
    ```sh
    git push -f
    ```

  _이유:_
  > 리베이스 할 때, 당신은 feature 브랜치의 이력을 변경하고 있는 겁니다. 그 결과, Git은 일반적인 `git push`를 거부합니다. 대신, 당신은 -f 혹은 --force 플래그를 사용할 필요가 있습니다. [더 알아보기](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
* 풀 리퀘스트를 만드세요.
* 풀 리퀘스트는 리뷰어에 의해 수용되고, 병합되고 종료될 것 입니다.
* 모든 작업이 끝났다면 당신의 로컬 feature 브랜치는 지우세요.

  ```sh
  git branch -d <branchname>
  ```
  원격 저장소에 존재하지 않는 모든 브랜치를 제거하기 위해서는 다음과 같이 하면 됩니다.
  ```sh
  git fetch -p && for branch in `git branch -vv | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>
### 1.3 좋은 커밋 메시지 작성하기

커밋을 작성하는 좋은 가이드라인을 가지고 있으면 Git으로 작업하거나 다른 사람들과 협업하는 것이 상당히 쉬워집니다. 다음은 그 규칙들입니다. ([출처](https://chris.beams.io/posts/git-commit/#seven-rules))

* 줄 바꿈을 통해서 제목과 본문을 구분하세요.

  _이유:_
  > Git은 당신의 커밋 메시지의 첫번째 줄을 요약으로 분간할만큼 똑똑합니다. 사실, git log 대신에 git shortlog를 사용하면 커밋 ID와 요약정보만이 표시된 커밋 메시지의 긴 리스트를 볼 수 있습니다.
* 제목을 50자로, 본문은 72자로 제한하세요.

  _이유:_
  > 커밋은 가능한 보기 좋고 집중되어야하며, 장황하게 설명해서는 안됩니다. [더 알아보기](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)
* 제목에 대문자를 사용하세요.
* 제목을 마침표로 끝내지마세요.
* 제목에 [명령법(imperative mood)](https://en.wikipedia.org/wiki/Imperative_mood)을 사용하세요.

  _이유:_
  > 커미터가 완료한 일을 표현하는 메시지를 작성하는 것이 아닙니다. 이런 메시지들은 커밋이 레포지토리에 적용된 뒤에 어떻게 되는지를 설명하는 것으로 간주하는 것이 낫습니다. [더 읽기](https://news.ycombinator.com/item?id=2079612)
* 본문은 **어떻게** 대신 **무엇을**과 **왜**를 설명하는데 사용하세요.

## 2. 문서화

### 2.1 Github 문서
* `README.md`를 위해서 이 [템플릿](./README.sample.md)을 사용하세요. 필요한 섹션은 자유롭게 추가하세요.
* 한 개 이상의 레포지토리가 있는 프로젝트는 각각의 `README.md` 파일에 링크를 추가해주세요.
* 프로젝트가 발전함에 따라 `README.md`를 최신으로 유지하세요.
* 코드에 주석을 달아주세요. 가능하다면, 각 섹션에서 무엇을 표현하려고 하는지 명확하게 만드세요.
* GitHub 혹은 StackOverflow에 당신이 사용한 접근법이나 코드에 대한 토론이 있다면, 주석에 그 링크를 첨부하세요.
* 주석을 나쁜 코드에 대한 변명으로 사용하지 마세요. 코드를 깔끔하게 유지하세요.
* 클린 코드는 주석을 전혀 달지 않는 것에 대한 변명이 아닙니다.
* 코드가 발전함에 따라 주석도 적절하게 바꿔주세요.

### 2.2 Spring 문서
* Swagger을 적용하세요.
    _이유:_
    > 포스트맨으로 테스트보다 더 효율적입니다. 어떤 parameter을 넣어야 할지 알 수 있습니다.
* Test code를 작성하고 MockMvc를 통해 Rest Doc을 작성하세요.
    _이유:_
    > Swagger처럼 테스트는 안되지만 문서는 더 직관적입니다. 테스트 코드를 통해 안정성이 올라갑니다.