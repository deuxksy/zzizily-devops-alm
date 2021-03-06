# devops-alm

## 목차

1. ci
2. db
3. d
4. issue
5. ldap
6. proxy
7. repository
8. version
9. end

## 1. ci

## 2. db

## 3. dns

BIND9 & Webmin 을 이용한 DNS 관리 하기

Create Master Zone 할때 Master server 항목을 localhost를 주자

docker 가 실행 될때마다 hostname 변경됨에 따른 고정값을 주기 위해서

/etc/bind/named.conf.options

```txt
options {
  directory "/var/cache/bind";

  // If there is a firewall between you and nameservers you want
  // to talk to, you may need to fix the firewall to allow multiple
  // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

  // If your ISP provided one or more IP addresses for stable 
  // nameservers, you probably want to use them as forwarders.  
  // Uncomment the following block, and insert the addresses replacing 
  // the all-0's placeholder.

  // forwarders {
  // 	0.0.0.0;
  // };

  //========================================================================
  // If BIND logs error messages about the root key being expired,
  // you will need to update your keys.  See https://www.isc.org/bind-keys
  //========================================================================
  dnssec-validation auto;
  listen-on port 53	{ any; };
  listen-on-v6      { any; };
  allow-query       { any; };
  forwarders {
    1.1.1.1;
    8.8.8.8;
    };
};
```

[Webmin 설정](1)

## 4. issue

Windows 기반의 docker 에서 하면 아래와 같은 문제가 발생 할수 있음 처음에 clone 을 Windows 에서 하면 안됨 아나...

```bash
standard_init_linux.go:219: exec user process caused: no such file or directory
```

CRLF 문제라고 하는되 어느 파일에서 발생 한건지 모르겠다 ㅠ.ㅠ;

### 실행

```bash
$ docker-compose up
Creating network "docker-yona_default" with the default driver
Creating docker-yona_mariadb_1 ... done
Creating docker-yona_yona_1    ... done
Creating docker-yona_adminer_1 ... done
Attaching to docker-yona_mariadb_1, docker-yona_adminer_1, docker-yona_yona_1

...
더 이상의 자세한 설명은 생략한다.
...

Caused by: org.mariadb.jdbc.internal.util.dao.QueryException: Could not connect to address=(host=127.0.0.1)(port=3306)(type=master) : Connection refuse
```

위와 같은 DB 접속 오류 발생함

./yona/conf/application.conf 파일 131 라인을 보면 MariaDB 설정 부분이 있음

```yml
# MariaDB
db.default.driver=org.mariadb.jdbc.Driver
db.default.url="jdbc:mariadb://127.0.0.1:3306/yona?useServerPrepStmts=true"
db.default.user=yona
db.default.password=password
```

IP, PASSWORD 를 확인 해서 변경 해주자 다른 설정이 궁금하면 [참조](https://github.com/yona-projects/yona/blob/next/docs/ko/application-conf-desc.md) 해서 변경 하자

## 5. ldap

## 6. proxy

## 7. repository

## 8. version

## 9. refernce

[1]: (https://doxfer.webmin.com/Webmin/BIND_DNS_Server)