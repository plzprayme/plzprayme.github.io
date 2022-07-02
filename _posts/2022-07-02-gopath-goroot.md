---
title: GOROOT와 GOPATH
date: 2022-07-03 00:14:00 +0800
categories: [Go lang, 개발환경]
tags: [go, golang, gopath, goroot, ldflags]
---

# 시작하기 전
입사 후에 다루는 프로젝트들의 대부분이 Go lang이다. 항상 그렇듯, 프로젝트 초기 셋팅을 하는데에 아주 애를 먹었다. Java 프로젝트를 다루면서 gradle, maven을 사용할 떄가 생각이나는구만  
GO 에서는 makefile을 사용해서 프로젝트를 컴파일하기도 하더라. 그리고 GO 를 컴파일 할 떄 ldflags라는 옵션을 줄 수 있는데 여기서 많이 애를 먹었다.  
그래서 이번 포스팅에서는 GOROOT, GOPATH, ldflags를 다루고자 한다. 알고쓰자.  

# GOROOT
go sdk를 설치한 디렉토리를 나타낸다. 자바에 비유를 들자면 JAVA_HOME 이다.  
go cli를 사용하기 위해서 go sdk를 설치하고 shell profile에 아래와 같이 설정하자.  
```
export GOROOT={YOUR_SDK_DIRECTIORY}
export PATH=$PATH:$GOROOT/bin
```
그리고 터미널에서 `go version` 을 입력해서 테스트해보자. (터미널을 재부팅해야한다.)

# GOPATH
그렇다면 GOPATH는 뭘까? GOPATH는 GO 프로젝트의 import 위치를 잡아준다. 소스코드를 찾는 위치 결정뿐만 아니라 모듈을 다운로드 하는 장소도 결정해준다.  
또, GOPATH는 아래와 같은 구조를 가져야 한다.  

```
GOPATH=/home/user/go

/home/user/go/
    src/
        foo/
            bar/               (go code in package bar)
                x.go
            quux/              (go code in package main)
                y.go
    bin/
        quux                   (installed command)
    pkg/
        linux_amd64/
            foo/
                bar.a          (installed package object)
```

src, bin, pkg 가 있어야한다. 참고로 알아두자. 그런데 이런걸 직접 셋팅하기가 귀찮으니 그냥 GoLand를 쓰자..  

# ldflags
뜬금없이 ldflags는 뭘까? 이건 `go build` 의 이야기다. 빌드 시점에 go의 변수를 바꿔치기할 수 있다.  
무슨 말인지 모르곘다면 아래 코드를 보자.  

```go
// version.go
package main

var (
	Version = "0.2.3"
)

func main() {
    fmt.Prinln(Version) // 0.2.3
}
```

위와 같은 코드를 `go build -i -o bin/mybin` 으로 빌드하고 `bin/mybin`을 실행해보자. 그러면 `0.2.3` 이 출력된다.  
반면에 `go build -i "-X main.Version=1.0.0" -o bin/mybin` 으로 빌드하고 또 실행을해보자. 그러면 `1.0.0` 이 출력된다.  
여기서 주목할 점은 `main` 이라는 패키지의 위치는 GOPATH를 기반으로 탐색한다는 점이다. 그래서 GOPATH가 정확하게 설정이 되어 있어야한다.  


# 끝으로
GOPATH와 GOROOT 정말 별 것도 아닌데, 마음이 급해서. 빨리 회사에서 퍼포먼스를 내고 싶은 마음에 잘 알아보지 않고 마구잡이로 개발을 시작했다.  
뿌리가 깊지 않은 나무는 항상 흔들리기 마련이다. 아니나 다를까 쓸데없이 많은 시간을 소모했고, 동료의 시간까지 소모했다.  
여유를 가지자.s