Jenkins는 CI/CD 툴이며 홈페이지는 https://jenkins.io 이다.

# Jenkins 설치

https://jenkins.io/download 에 접속하면 각 OS 별 jenkins 설치방법에 대한 설명 및 다운로드가 가능하다.
ubuntu/debian의 경우 버젼에 맞는 java 가 필요하다. jenkins는 Java 9 에서는 수행되지 않는다.

- 2.54 (2017-04) and newer: Java 8
- 1.612 (2015-05) and newer: Java 7

해당 버젼에 알맞은 JDK를 설치하도록 한다.
설치가 완료되면 http://localhost:8080 에 접속한다.

>VM에는 jenkins와 OpenJDK Java 8 이 이미 설치되어 있어서 설정하는 화면은 나타나지 않고 초기화면이 나올 것이다.

접속하면 다음과 같은 화면을 볼 수 있다.

![](images/jenkins1.png)

표시되어 있는 **/var/lib/jenkins/secrets/initialAdminPassword** 파일에서 패스워드를 복사해서 입력하고 다음을 눌러준다.  
파일을 읽기 위해서는 root 권한으로 읽어야 한다.

~~~
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
~~~

다음과 같은 화면이 나오면 **Install suggested plugins**을 눌러준다.

![](images/jenkins2.png)

plugin이 설치될 것이다.

![](images/jenkins3.png)

설치가 완료되고 username/password/email 등을 넣고 시작하면 다음과 같은 화면이 보일 것이다.

![](images/jenkins4.png)

# 파이프라인 생성

Jenkins Pipeline (또는 단순히 "Pipeline")은 Jenkins에 지속적인 전송 파이프 라인 을 구현하고 통합하는 것을 지원하는 플러그이다.

continuous delivery pipeline은 사용자와 고객에 이르기까지 버전 제어에서 소프트웨어를 가져 오는 프로세스의 자동화 된 표현이다.

Jenkins Pipeline은 복잡한 전달 파이프 라인을 "코드"로 모델링 할 수있는 확장 가능한 도구 세트를 제공한다.


1. Jenkins 메뉴중 **New Item** 을 클릭하여 새로운 job을 만든다.

    ![](images/jenkins5.png)

1. 아이템 이름을 넣어주고(예:version print) 하위의 **Multibranch Pipeline**를 선택한다. 이 항목은 git repository가 변경되면 수행하는 것을 의미한다.

    ![](images/jenkins6.png)

    OK를 누른다.

1. Branch Sources 항목의 **Add source**를 선택하면 Git 을 선택할 수 있다. 

    ![](images/jenkins7.png)

    Git을 선택하면 Git Repository의 주소를 넣는 부분에 기존에 만들었던 git repository의 주소를 넣는다.

    Credentials 에는 git repository에 접근하기 위한 username/password를 넣어준다.

    ![](images/jenkins8.png)

    OK를 누르면 다음과 같이 jenkins가 git reposotory를 살펴보는 로그가 나오게 된다.

    ![](images/jenkins8.png)

    성공적으로 repository를 체크하고 Jenkinsfile 을 살펴보고 있음을 알 수 있다.

    ![](images/jenkins9.png)

    아직은 Jenkinsfile 이 없기 때문에 not found가 나온다.

    ![](images/jenkins10.png)

    위의 "version print" job을 선택하면 다음과 같이 나온다.

    ![](images/jenkins11.png)


# Jenkinsfile 생성



1. 아래의 내용으로 project1 디렉토리 내에 Jenkinsfile 이라는 이름으로 저장한다.

    ~~~jenkinsfile
    pipeline {
        agent { docker { image 'node:6.3' } }
        stages {
            stage('build') {
                steps {
                    sh 'npm --version'
                }
            }
        }
    }
    ~~~

    의미는 node:6.3 이라는 이름의 도커 이미지를 바탕으로 npm --version 명령을 실행하는 것이다.

1. 이제 Jenkinsfile 을 github repository에 push 하도록 한다.
    ~~~
    $ git add .
    $ git commit -m 'add jenkinsfile'
    $ git push
    ~~~

1. "Scan Multibranch Pipeline Now" 를 누른다.

    ![](images/jenkins12.png)

    그러면 Jeninksfile 이 변화되었음을 감지하고 해당 파이프라인을 수행한다.
    수행된 job번호 (#2)를 클릭하고 "Console Output"을 클릭하면 전체 로그를 볼 수 있다.

    ![](images/jenkins13.png)

1. 로그의 내용을 면 Jenkinsfile 에서 정의한 "npm --version"이 수행되었음을 알 수 있다.

    ~~~
    
    ...
    + docker inspect -f . node:6.3
    .
    [Pipeline] withDockerContainer
    Jenkins does not seem to be running inside a container
    $ docker run -t -d -u 122:127 -w /var/lib/jenkins/workspace/version_print_master -v /var/lib/jenkins/workspace/version_print_master:/var/lib/jenkins/workspace/version_print_master:rw,z -v /var/lib/jenkins/workspace/version_print_master@tmp:/var/lib/jenkins/workspace/version_print_master@tmp:rw,z -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** node:6.3 cat
    $ docker top 7ab2fb86abae8089e1e4ac6a6f44aac84c3824dafe9d99b3e5ee511a7dfd6b33 -eo pid,comm
    [Pipeline] {
    [Pipeline] stage
    [Pipeline] { (build)
    [Pipeline] sh
    + npm --version
    3.10.3
    [Pipeline] }
    [Pipeline] // stage
    [Pipeline] }
    $ docker stop --time=1 7ab2fb86abae8089e1e4ac6a6f44aac84c3824dafe9d99b3e5ee511a7dfd6b33
    $ docker rm -f 7ab2fb86abae8089e1e4ac6a6f44aac84c3824dafe9d99b3e5ee511a7dfd6b33
    [Pipeline] // withDockerContainer
    [Pipeline] }
    [Pipeline] // withEnv
    [Pipeline] }
    [Pipeline] // node
    [Pipeline] End of Pipeline
    Finished: SUCCESS

    ~~~


    


# 멀티플 스텝 수행

파이프 라인은 여러 단계로 구성되어있어 응용 프로그램을 작성, 테스트 및 배포 할 수 있다. Jenkins Pipeline을 사용하면 자동화 과정을 모델링하는 데 도움이되는 여러 단계를 손쉽게 구성 할 수 있다.

단일 조치를 수행하는 단일 명령과 같은 "Step"은 단계가 성공하면 다음 단계로 넘어갑니다. 단계가 올바르게 실행되지 않으면 파이프 라인이 실패한다. 파이프 라인의 모든 단계가 성공적으로 완료되면 파이프 라인이 성공적으로 실행 된 것으로 간주됩니다.

1. 기존의 Jenkinsfie 을 다음과 같이 수정해 보자
    ~~~
    pipeline {
        agent any
        stages {
            stage('Build') {
                steps {
                    sh 'echo "Hello World"'
                    sh '''
                        echo "Multiline shell steps works too"
                        ls -lah
                    '''
                }
            }
        }
    }
    ~~~

1. 다시 github에 push 하도록 한다.
    ~~~
    $ git add .
    $ git commit -m 'add jenkinsfile'
    $ git push
    ~~~

1. Console Log를 살펴보면 다음과 같이 명령한 모든 스텝들이 수행되었음을 알 수 있다.

    ~~~

    + echo Hello World
    Hello World
    [Pipeline] sh
    + echo Multiline shell steps works too
    Multiline shell steps works too
    + ls -lah
    total 20K
    drwxr-xr-x 3 jenkins jenkins 4.0K  5월 11 16:07 .
    drwxr-xr-x 4 jenkins jenkins 4.0K  5월 11 15:31 ..
    drwxr-xr-x 8 jenkins jenkins 4.0K  5월 11 16:07 .git
    -rw-r--r-- 1 jenkins jenkins  927  5월 11 16:07 Jenkinsfile
    -rw-r--r-- 1 jenkins jenkins   18  5월 11 15:31 README.md
    [Pipeline] }
    [Pipeline] // stage
    [Pipeline] stage
    [Pipeline] { (Test)
    [Pipeline] sh
    + echo test
    test
    [Pipeline] }
    [Pipeline] // stage
    [Pipeline] stage
    [Pipeline] { (Declarative: Post Actions)
    [Pipeline] echo
    This will always run
    [Pipeline] echo
    This will run only if successful
    [Pipeline] }
    [Pipeline] // stage
    [Pipeline] }
    [Pipeline] // withEnv
    [Pipeline] }
    [Pipeline] // node
    [Pipeline] End of Pipeline
    Finished: SUCCESS
    
    ~~~

1. Pipeline의 각 step은 다음과 같이 보여진다.

    ![](images/jenkins14.png)

# 실행 환경 설정

각 예제에서 **agent** 지시문을 발견했을 수 있다. **agent** 지시문은 Jenkins에게 파이프 라인 또는 파이프 라인의 하위 집합을 실행하는 방법과 방법을 알려준다. 

파이프 라인은 Docker 이미지와 컨테이너를 쉽게 사용할 수 있도록 설계되었다. 이를 통해 파이프 라인은 다양한 시스템 도구와 에이전트에 대한 종속성을 수동으로 구성 할 필요없이 필요한 환경과 도구를 정의 할 수 있다. 이 방법을 사용하면 Docker 컨테이너에 패키징 할 수 있는 거의 모든 도구를 사용할 수 있다.

~~~
pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
~~~

# 실행 변수 설정

환경 변수는 아래 예제처럼 전역으로 설정할 수 있다. 환경 변수를 단계별로 설정하면 환경 변수는 정의 된 단계에만 적용된다.
~~~
pipeline {
    agent any

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }

    stages {
        stage('Build') {
            steps {
                sh 'printenv'
            }
        }
    }
}
~~~

# 테스트 및 배포

CI툴 파이프 라인의 핵심 부분은 테스트이지만, 대부분의 사람들은 실패한 테스트에 대한 정보를 찾기 위해 수천 줄의 콘솔 출력을 살펴보기를 원하지 않는다. 이를 쉽게 수행하기 위해 Jenkins는 테스트 한 결과를 기록하고 집계 하고 출력 할 수 있는 수 있다. Jenkins에는 일반적으로 junit 단계가 제공되어 JUnit 스타일 XML 보고서를 출력 할 수 있고 일반 테스트 보고서 형식을 처리하는 추가 플러그인이 추가할 수도 있다.

배포하기 위한 jar도 만들 수 있으며 archiveArtifacts 를 이용한다.

~~~
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew check'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
            junit 'build/reports/**/*.xml'
        }
    }
}
~~~

archiveArtifacts 단계에서 이슈 경로 및 파일 이름과 지문의 아티팩트를 명시해야한다. 이슈의 경로 및 파일 이름만 지정해야하는 경우 매개 변수 이름 아티팩트를 생략 할 수 있다.  
(예 :archiveArtifacts 'build/libs/**/*. jar')

---
참고: https://jenkins.io/doc