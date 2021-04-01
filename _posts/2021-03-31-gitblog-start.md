---
title:  "[Blog]깃 블로그 시작하기[1] - (Jekyll)지킬 테마 사용하기"

categories:
  - Blog
tags:
  - [Blog, Jekyll, Github, Git, GitBlog]

toc: true
toc_sticky: true
 
date: 2021-03-31
last_modified_at: 2020-01-31
---

기존에 깃 블로그를 했었는데 관리를 너무 안하고 운영하는 방법을 잊어버려서 처음부터 다시 시작하기로 했습니다.<br>
저는 맥북을 사용하고 있다보니 맥os환경 기준으로 포스팅을 해보려고 합니다.<br>
git을 처음 접하시는 분들은 git bash를 다운받아서 사용해야 하는데 [여기](https://hyeonjiwon.github.io/etc/git_install)를 참고하셔서 다운받으면 됩니다. <br><br>

# 1. Jekyll 지킬 테마 다운받기
 
 저는 minimal mistakes 테마를 사용하려고 합니다. 
 다른 테마를 원하시는 분들은 아래의 링크를 통해서 테마를 고르시면 될 것 같습니다.<br>
( 테마링크 : [http://jekyllthemes.org/](http://jekyllthemes.org/) )

저는 [minimal mistakes](https://github.com/mmistakes/minimal-mistakes) 홈페이지에서 테마파일을 다운로드 하였습니다. <br>
![blog1-1](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-1.png?raw=true)



# 2. Github 원격 저장소 생성하기

테마를 다운받으신 후에 자신의 github계정의 새로운 Repository를 생성하여 줍니다.<br>

![blog1-2](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-2.png?raw=true)

repository name은 자신의 github 닉네임. github.io로 하시면 됩니다.<br>
그러고 나서 아무것도 체크 안 하시고 생성을 누르시면 reposiroty가 생성된 것을 확인할 수 있습니다.

![blog1-3](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-3.png?raw=true)


# 3. 로컬 저장소 생성 

로컬 저장소를 생성하는 방법은 두가지의 방법이 있습니다.

1. [git clone 명령어를 통해서 저장소폴더 복제하기](#1-git-clone-명령어를-통해서-원격저장소-복제하기)
   
2. [저장소와 같은 이름의 폴더를 직접 생성하기](#2-원격저장소와-같은-폴더를-직접-생성하기)

저는 직접 폴더를 생성해서 진행할 것이기 때문에 git명령어를 통해서 생성하는 방법을 먼저 설명하겠습니다.ㅎ<br>
진행하기전에 몇가지 git명령어들을 알고 가시면 좋습니다.<br><br>
 `git 명령어`

- `git init`<br>
    - 깃 초기화 : 해당 폴더에서 git init을 사용하면 해당폴더에서 깃명령어를 사용하겠다.


- `git clone (원격 저장소 github URL)`<br>
    - 깃 복제 : 해당 폴더에서 git clone을 실행하면 원격저장소 폴더를 로컬저장소 폴더로 복제해온다.

- `git status`
    - 깃 상태확인 : 해당 폴더의 추가,삭제,수정등 변경사항의 상태를 확인할 수 있다. 

- `git add . `
    - stage에 폴더 추가 : 폴더내의 변경사항들을 모두 stage area에 올린다. -> 파일들을 commit에 포함시키기

- `git commit -m "message"`
    - 원격서버에 올린준비 : stage area에 올라온 모든 파일들을 메세지와 함께 원격 저장소에 올릴 준비를 한다. 

- `git remote add (원격 저장소 github URL)`
    - 원격 연결 : 현재 위치의 로컬저장소와 원격저장소를 연결시킵니다.

- `git push -u origin master`
    - 원격에 변경사항 적용 : push 명령어를 통해서 stage area에 올라온 파일들을 깃허브 원격저장소에 반영을 한다. 

### 1. git clone 명령어를 통해서 원격저장소 복제하기
  깃 원격저장소를 생성하고 싶은 폴더위치에서 `git init` 명령어를 사용하여 git명령어활성화를 해줍니다.<br>

  `git clone https://github.com/edw216/edw216.github.io.git`<br><br>
  ![blog1-10](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-10.png?raw=true)
  자신의 깃주소는 저장소에서 주소복사하기 하시면 됩니다.<br>
  git clone을 통해서 해당 폴더위치에 원격저장소 폴더를 복제해줍니다.<br>
  복제한 폴더에 다운로드 받은 파일을 압축해제 해줍니다.<br><br>
  이 후로는 [여기]()로 이동하셔서 진행하시면 됩니다.

### 2. 원격저장소와 같은 폴더를 직접 생성하기

자신이 원하는 위치에 닉네임.github.io 이름의 폴더를 직접 생성합니다.<br>
생성한 폴더에 다운로드 받아놓은 테마파일을 압축해제하여 줍니다.<br>

그리고 깃명령어를 실행하기위해 `git init` 명령어를 사용해줍니다.

# 4. 원격 저장소 연결

일단 폴더의 저장된 내용중에 불필요한 파일들을 삭제 해줍니다.<br>
- `.editorconfig`
- `.gitattributes`
- `.github`
- `/docs`
- `/test`
- `CHANGELOG.md`
- `minimal-mistakes-jekyll.gemspec`
- `README.md`
- `screenshot-layouts.png`
- `screenshot.png`
  
<br>그리고 나서 블로그에 게시물파일을 저장하는 폴더 `_posts` 파일을 생성해줍니다.<br>
자세한 내용은 [mistakes 홈페이지](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remote-theme-method)를 참고 하세요.<br><br>
`git status`를 통해서 폴더 상태를 확인하면 빨간색의 파일들이 보일겁니다.<br>
아직 stage에 등록이 되어있지 않은 폴더들이에요.<br> 

![blog1-4](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-4.png?raw=true)


`git add .` 명령어를 통해서 모든 파일을 stage에 등록합니다.<br><br>

![blog1-5](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-5.png?raw=true)

add . 을 한 후에 status를 통해 상태를 확인하면 초록색으로 파일들이 stage에 올라간 것이 보일겁니다.<br>

`git commit -m "first commit"`을 진행 하시고 status를 한번더 확인해보면<br>

![blog1-6](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-6.png?raw=true)

흰색 으로 파일들이 원격에 올라갈 준비가 됩니다.<br>

이제 마지막으로 원격과의 연결을 해줍니다.<br>

`git remote add https://github.com/edw216/edw216.github.io.git` <br>
원격저장소와 로컬 저장소를 연결하고 `git push -u origin master` 원격저장소에 push 해주면 끝 -!<br>


![blog1-8](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-8.png?raw=true)


이제 `https://edw216.github.io` 자신의 블로그 주소를 통해서 들어가서 확인하기<br>

![blog1-9](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog1/blog1-9.png?raw=true)



블로그 생성을 마치고 다음 포스팅에는 블로그 게시물 등록과 커스터마이징을 다뤄보겠습니다. ㅎㅎ
<br><br><br><br><br>


> 참고 블로그 : [https://ohjinjin.github.io/blog/blog/](https://ohjinjin.github.io/blog/blog/)
