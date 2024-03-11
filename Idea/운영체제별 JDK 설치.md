자바 프로그램을 실행하기 위해서는 먼저 **Java SE(Standard Edition)** 의 구현체인 **JDK(Java Development Kit)** 을 설치해야 한다.

JDK는 크게 다음과 같은 두 가지가 존재한다.
- **Open JDK**
- **Oracle JDK**

| 구분          | Open JDK          | Oracle JDK                        |
| ----------- | ----------------- | --------------------------------- |
| 라이선스        | GNU GPL version 2 | Oracle Technology Network License |
| 사용료         | 무료                | 개발 및 학습용, 상업용: 유료                 |
| 개발 소스 공개 의무 | 없음                | 없음                                |
> [!info]
> Oracle JDK는 Open JDK 보다 응답성과 JVM 성능이 상대적으로 뛰어나다.

JDK LTS(Long Term Support)
- JDK 8
- JDK 11
- **JDK 17** (we use)

> [!note]
> JDK 설치 및 환경 변수 설정 방법은 책을 참고한다.

아래의 명령어를 통해 환경 변수 설정이 잘 되어있는지 확인하자.

```shell
java -version
```

- Output
```shell
java version "17.0.2" 2022-01-18 LTS
Java(TM) SE Runtime Environment (build 17.0.2+8-LTS-86)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.2+8-LTS-86, mixed mode, sharing)
```



