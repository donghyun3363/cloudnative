# GIT

## github.com 계정 생성

GIT 은 소스의 버젼을 관리해주는 툴이다.  
github.com 은 git을 위한 저장소로 소스를 개방한다면 무료로 사용가능하다. 

github.com을 사용하기 위해서는 계정이 필요하다.  
http://github.com 에 접속하여 계정을 만들도록 한다.

계정을 만들면 다음과 같은 화면이 보인다.

![](images/git1.png)


## Repository 생성

Repository는 프로젝트를 관리하는 단위이다.  
새로운 프로젝트를 만들기 위해서 화면에서 `Start a project` 를 선택한다.

![](images/git2.png)

위의 화면에서 **Repository name** 에 "project1"을 입력하고 하위의 `Create repository`를 클릭한다.

![](images/git3.png)

최종적으로 위와 같은 화면이 나오는데, git 명령을 이용하여 로컬 디스크에 github.com의 소스를 가져오는 방법을 설명한 것이다.

아래에서 git 을 설치하고 명령을 통하여 방금 만든 Repository에 소스를 업데이트 해 보도록 한다.

## git 설치

git 은 git repository에 접근하여 소스를 관리하는 도구이다.   
https://git-scm.com/ 에서 다운로드 받을 수 있으며 윈도우즈에서는 설치파일을 다운로드 받아 설치를 하면된다.

![](images/git4.png)

>VM에서는 이미 설치가 되어있다.

## git clone {repository address}

맨 처음 git repository에서 소스를 로컬 디스크로 복제하는 명령이다. 실행하면 다음과 같다

~~~
$ git clone https://github.com/jonggyou/project1

Cloning into 'project1'...
warning: You appear to have cloned an empty repository.
~~~

기존에 만들었던 repository에 아무런 파일도 포함되지 않아서 현재 디렉토리에서 파일 리스트를 보면 project1 이라는 빈 디렉토리가 생성된걸 확인할 수 있다.

## git add

파일을 repository에 넣기 위해서 새로운 파일을 만들어보자.

README.md 파일은 기본적으로 repository에서 해당 파일의 내용이 보여지는 파일이름이다.

자신이 익숙한 에디터를 이용하여 README.md 파일을 만들어 보도록 한다.

md 파일은 Mark Down 의 약자로 html 보다 간편하게 작성하는 로직을 가진 언어이다. 자세한 것은 https://www.markdownguide.org/ 에서 확인하면 된다.

README.md 파일에 다음과 같은 내용을 쓰고 저장 한다.

~~~
the first project
~~~

git 은 상태에 따라 관리를 한다.

![](images/git5.png)

git add 명령어를 통하여 staged 상태 만든다.

~~~
$ git add README.md
~~~

현재 디렉토리의 모든 파일을 staged 상태로 만들기 위해서는 다음과 같이 명령한다.

~~~
$ git add .
~~~

## git status

현재의 상태를 보기 위해서 git status 명령을 한다.

~~~
$ git status

On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.md

~~~

아직 commit 된 파일은 없고 commit 될 새로운 파일 README.md 가 존재함을 알 수 있다.

짤막하게 확인 하려면 다음과 같이 옵션을 준다.

~~~
$ git status -s

A  README.md
~~~

## git commit

로컬 디스크에서 수정하거나 만들어진 파일을 커밋하는 명령이다. git add 를 하지 않은 상태인 Unstaged 상태의 파일들은 커밋되지 않는다.

~~~
$ git commit
~~~

처음 명령을 실행하면 다음과 같은 메시지가 보인다.

~~~
$ git commit

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'user1@user1-VirtualBox.(none)')
user1@user1-VirtualBox:~/project1$ 
~~~

git repository는 일반적으로 한 사람만 업데이트 하는 저장소가 아니라 수십, 수백명이 업데이트 하는 저장소이다.  

그래서 각 커밋에 대한 오너쉽을 가지기 때문에 커밋하는 주체의 email 과 name 이 필요하다.

자신만의 email과 name을 설정해 준다.

~~~
$ git config --global user.email "kimjonggyou@gmail.com"
$ git config --global user.name "Jonggyou Kim"
~~~

그 후 "git commit" 명령을 내리면 nano 에디터가 뜨면서 커밋에 대한 코멘트를 넣는 화면이 나타난다.

커밋 할 때마다 이러한 에디터가 나와서 코멧트를 넣는 수고를 덜기 위해서 옵션을 사용한다.

먼저 Ctrl-x 를 눌러 현재 nano 에디터를 빠져나온다. 아무런 코멘트를 달지 않았기 때문에 다음과 같은 메시지를 내면서 git commit 명령어는 무시된다.

~~~
Aborting commit due to empty commit message.
~~~

다음과 같이 커밋을 한다.

~~~
$ git commit -m 'initial'

[master (root-commit) 7dff111] initial
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
~~~

커밋이 완료됨을 알 수 있다.

이 상태는 로컬의 git 에서 commit 이 됨을 의미한다. 여러사람이 같이 공유하는 git repository에는 아직 해당 파일이 존재하지 않고 있다.


## git push

push 명령어는 git repository에 로컬 디스크에서 커밋된 파일들을 반영하기 위한 명령어이다.

~~~
$ git push

Username for 'https://github.com': 
~~~

처음 "git push" 명령을 내리면 github.com 의 계정을 묻는 화면이 나타난다.

이 화면에서 계정의 username 과 password를 입력해도 되고, "git login" 명령을 미리 써서 로그인을 해 두어도 된다.

한번 로그인 되면, 두번은 물어보지 않는다.

~~~
$ git push

Username for 'https://github.com': kimjonggyou@gmail.com
Password for 'https://kimjonggyou@gmail.com@github.com': 
Counting objects: 3, done.
Writing objects: 100% (3/3), 231 bytes | 231.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/jonggyou/project1
 * [new branch]      master -> master
~~~

다음과 같이 github repository에 푸시가 성공했음을 알 수 있다.

이제 웹브라우저를 통해서 github repository에 접속을 해 보자.

![](images/git6.png)

화면에서 보는 바와 같이 README.md 파일이 존재하고 initial 이라는 코멘트로 갱신됨을 알 수 있다.

그리고 README.md 파일의 내용은 아래에서 볼 수 있다.


감사합니다.
