Linux Commands
==========================================
[Bash manual](http://www.gnu.org/software/bash/manual/bashref.html#Shell-Parameter-Expansion)  
[Blog ref](http://itscom.org/archives/2450)  

## 리눅스 OS버전 확인
- lsb_release -ca
- cat /etc/*issue* 
- cat /etc/issue
- cat /etc/redhat-release

## uname -a
    * 커널 정보 확인

## chmod
    * 파일의 권한 변경
    * description
        ls -l로 디렉토리를 출력해보면 아래와 같이 출력된다.
        [ drwxr-xr-x  2  root  root  4096 Apr 22 16:59 conory ]
        위의 의미는 무엇일까?
        
    * options
        -R, 서브 디렉토리 파일 모두 변경
    * sample
        chmod 744 /scripts/ko.sh
        chmod 400 : 해당 파일의 소유자만 읽기 권한만을 갖음. 다른 유저에게는 보이지도 않게 됨.

## apt-get
  * linux software 설치 및 삭제 또는 목록 업데이트 커맨드.
  * super user 권한이 필요하기 때문에 sudo apt-get {명령} 으로 사용한다
  * sudo apt-get update : 리눅스 소프트웨어 목록을 최신으로 업데이트
  * sudo apt-get upgrade: 현재의 리눅스 소프트웨어 최신의 목록을 기준으로 소프트웨어 업그레이드
  * sudo apt-get install apache2: 아파치2 설치

## apt
  * Demian계 16.04부터 apt-get을 대신할 새로운 패키지 매니저
  * [APT_LINK](https://www.maketecheasier.com/apt-vs-apt-get-ubuntu/)

## cp
파일과 디렉토리 복사
-f, 파일 존재시 강제로 override
-R, 반복되는 디렉토리에 대한 적용

cp ko.txt ~/

## crontab
스케줄러

crontab -l
crontab -e

## cut
필드 골라보기 명령어로 awk '{print $N}'과 비슷하지만 더 간단하게 사용 가능
-c, 문자 선택
-d, 기본 구분값 TAB 대신 구분자
-f, 필드 선택
-fN-M, N부터 M까지 출력
-f-M, 처음부터 끝까지 출력

* -c 옵션으로 문자 1번째에서 3번째 까지 출력
echo /etc/redhat-release | cut -c1-3 # /et

* -d 구분자 옵션으로 : 을 기준 2번째 필드 출력
echo "/home:[backup]" | cut -d: -f2 #output: [backup]

* uname -a 결과값의 첫번째와 세번재 필드 출력
uname -a | cut -d' ' -f1,3 #output: Linux 4.4.0-17134-Microsoft

## date
날짜 출력 또는 설정
echo date #output: Sat Aug 25 17:55:38 DST 2018

%d, 일자
%H 시간
%m 월
%M 분
%Y 년도
%u, date of week(1..7) 1이 월요일

date -s '2013-04-17 22:50:00'
date --date[or just -d]="1 day ago" '+%Y/%m/%d' => 커맨드 실행일 기준 yesterday 값을 포맷에 맞게 출력
date -d "25 JUN 2018" +%Y%m%%d => 입력 날짜를 지정된 형식으로 변환

## df
파일 시스템 디스크 사용량 출력
-h 사람이 읽기 좋은 단위로 출력
-i inode 정보 출력
-k 1k 단위로 출력

df -h
df -Th

## echo
인수로 지정된 문자열을 출력
-e 문자열에서 역슬래쉬와 조합이되는 이스케이프 문자 함께 사용(\n: 개행문자, \t: 탭)
-n 마지막에 따라오는 개행문자를 출력하지 않는다

* echo $LANG;echo $HOME
output:
en_US.UTF-8
/home/ko

* -e 옵션으로 개행문자를 인식 두 줄 출력
echo -e "Hello\nWorld"
output:
Hello
World

## env
환경변수 보기

## expr
수식연산 
expr 3 + 5

## find
파일 및 디렉토리 검색
* cgi 파일 중 "문자"를 포함하는 파일 출력
find ./ -type f -name *.cgi -exec grep "문자" {} \; -print
-exec는 find의 ACTIONS 커맨드로서 find한 파일에 대하여 다음 명령을 실행하는 것. {}은 발견한 파일 이름으로 변환이 되어 실행됨.
자세한 사항은 man find 참조

* find 검색 개행없이 결과값 넘겨주어 권한을 775 변경
find . -type d -print0 | xargs -0 chmod 775

find /home/abc -type f -mmin +3 -print | grep 'doc\|pdf'

* 2개 이상 디렉토리에서 7z파일 검색
find /back/192.*.10 /back/192.*.20 -name "*_1803*.7z"

## grep
문자 패턴 매치
-c 검색결과 총 행수를 출력
-i ignore case-sensitive
-o 매칭된 패턴만 출력, 즉 라인 출력이 아님
-v 패턴이 일치하지 않는 내용 출력
-w 워드 단위로 일치하는 결과만 출력

\| or 조건 ex. 'ABC\|DF'
.* and 조건 ex. 'ABC.*DF'

* 파티션명에 data 또는 back 있는경우 출력
df -h | grep 'data\|back'

* ip a 정보 중 inet 와 brd 있는경우 출력
ip a | grep 'inet.*brd'

* 문자열 중 / 의 개수를 출력
echo '/data/bk/10.1.1.2' | grep -o '/' | wc -w

* 기타 사용법
ps -ef | grep "ph[p]"
w | grep -v 'tty1'

## free
메모리 사용량 출력
free -lm

## head
파일의 앞 부분부터 원하는 행을 출력
csv로그와 같이 데이터 사이즈가 너무 큰 경우. 윈도우즈에서 파일 오픈시 시간이 많이 걸린다. 특히나 네트워크 환경에서는.
이런 경우 head를 이용하면 간편하게 원하는 부분만 빠르게 확인 가능

head -3 smb.conf

## history
유저가 사용한 명령어 히스토리를 보여준다.
이벤트 지시자 !를 함께 사용하여 히스토리 검색 실행 가능

!! 직전명령 실행
!1 히스토리 첫번째 실행
!-3 히스토리 최근 3번째 실행

history | more
history 10

## ifconfig
네트워크 인터페이스 설정

ifconfig eth0

## ip
네트워크 인터페이스 설정 명령어

ip addr
ip link set eth0 up
ip addr add 192.168.11.11 dev eth0

## kill
프로세스 종료
-9 강제 종료

kill -9 1167

## killall
프로세스 이름으로 종료

killall httpd

## ln
파일간 링크 생성

ln -s /usr/local/mysql/bin/mysql /usr/bin/

## ls
파일 리스트 출력
-a 모든파일(.으로 시작하는 파일 포함) 출력
-C 한 줄에 여러 정보 출력
-F 파일의 유형을 파일 끝에 표시
-l 파일의 상세 정보 출력
-R 서브 디렉토리까지 출력
-t 수정시간으로 정렬
--time-style=long-iso 년도 표기

## mkdir
디렉토리 생성
-P 상위 디렉토리 없어도 생성

mkdir -P /home/ko/wwww

## mount
파일시스템 마운트

mount dev/mapper/sdisk/data

## mv
파일 이동하기
-b 백업
-u 목적지 파일 보다 최신 파일만 복사
-v 상세하게 보여주기

mv -uv ./* /home/ab/

## netstat
네트워크 상태 출력
-r 커널 라우팅 테이블 출력
-s 프로토콜별 상태 출력
-c 지속적인 상태 출력

netstat -an | grep "LISTEN"

## nice
프로그램의 우선순위 지정
범위: -20(우선순위 높음) ~ 19(우선순위 낮음)

nice -n 19 /scripts/backup.sh

## nohup
백그라운드로 쉘 실행
쉘 커맨드 끝에 & 를 넣은 것과 같음

## pgrep
실행중인 프로세스 찾기
-l PID와 함께 일치하는 프로세스의 이름 출력
-f -l 옵션과 함께 사용하면 명령어의 경로도 출력
-n 패턴과 일치하는 프로세스의 가장 최근 PID 출력
-x 패턴과 정확하게 일치되는 프로세스만 출력

## pkill
특정 프로세스 kill
-n 패턴과 일치하는 프로세스의 가장 최근에 실행된 프로세스 하나만 종료
-x 패턴과 정확하게 일치하는 프로세스만 종료

## ping
네트워크 핑 점검

ping -c 3 192.168.11.11

## ps
프로새스 모니터링

ps -U root -u root u

## wc
라인 또는 단어 수 출력

-c 문자 수 보여줌
-w 단어 수 보여줌
-l 라인 수 보여줌

ls | wc -l
cat ./httpd.conf | grep 'apache' | wc -w

## which 
명령어 경로 출력

## w
로그인된 사용자 보기
w -i
 19:47:23 up  3:09,  0 users,  load average: 0.52, 0.58, 0.59
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT

## scp
    * 원격 보안복사
    * example: 192.168.1.2서버로 이동하여 /home/kd 디렉토리를 현재 local의 위치로 recursive copy
    scp -r root@192.168.1.2:/home/kd ./

## rsync
    * 원격 서버와 디렉토리를 sync 시켜줌
    * 사용법은 scp와 비슷
    * rsync -chavzP --stats {location1} {location2}
    * location1 => sync 시키고자 하는 데이터가 있는 디렉토리 또는 파일
    * location2 => sync를 받는 디렉토리 또는 파일

## tr
    * 문자열의 특정 문자를 삭제하거나 변환
    * options
        -d "SET1" SET1과 일치하는 패턴 삭제
    * example
    whoami | tr "a-z" "A-Z" => 문자열의 소문자를 대문자로 변환
    date | tr -d "a-z" => 소문자 a-z 삭제

## tail
    * 파일의 마지막 부분 출력.
    * 로그를 보고 싶을 때 보통 사용. 실시간 보기가 가능한 옵션 때문.
    * options
        -f: 파일의 마지막 10라인을 실시간 출력
        -F: 파일의 실시간 변동을 보여주되 로그 파일처럼 특정 시간이 지난 후 파일이 변하게 되면 새로운 파일을 오픈하여 보여줌. 
            다음 파일을 열기 위해 또 다른 커맨드가 필요 없음
        -n: n만큼의 라인을 출력
        -n+n: 마지막 줄이 아니라 첫번째 줄부터 시작해 n 번째 라인 이후부터 출력
        --byte={number value}: 입력 숫자 값 사이즈 만큼의 내용을 출력
    * example
    tail -n 20 sample.log => 파일 끝의 마지막 20줄 출력
    tail -n +20 sample.log => 20번째 줄 이후부터 출력
    tail -f sample.log => 실시간 출력(모니터링)

## source
    * 지정한 소스파일 또는 함수를 실행시킴
    * 수정된 코드를 바로 적용하여 실행시키고자 할 때 사용됨

## export
* 현재 실행 중인 스크립트나 쉘의 모든 자식 프로세스가 변수를 사용할 수 있게 해준다. 역방향은 X.
* bash는 변수 선언과 함께 export 명령을 수행할 수 있으나 sh는 변수 선언과 export 적용을 두 단계로 나눠서 실행해야 한다.
* sh
```sh
PATH=$PATH:/usr/local/bin
export PATH
```
* bash
```bash
export PATH=$PATH:/usr/local/bin
```

## dirname
* 입력된 경로(또는 경로 + 파일)로부터 디렉토리를 추출한다
* 해당 위치에 실제 파일 또는 폴더 유무와 상관 없다.
* 상대 경로로 입력하면 상대경로가, 절대경로로 입력하면 절대경로가 출력
* 단, ~는 실제 홈폴더로 변경된다.
```bash
$ dirname /var/www/index.html
# /var/www/html
$ dirname ../a/../b/hello.sh
# ../a/../b
$ dirname /etc/ 또는 dirname /etc
# /
$ dirname ~abc/../sample.txt
# root/abc/..
```

## basename
* 입력된 path에서 가장 마지막 디렉토리 또는 파일의 명을 추출
* dirname과 비슷해보이지만 dirname의 경우 최종 디렉토리만을 타겟으로 한다.
```bash
$ basename /var/www/sample.log
# output -> sample.log
$ dirname /var/www/sample.log
# output -> www
```
* options  
```bash
$ basename /var/www/sample.log .log
# output -> sample
$ basename -s .log /var/www/sample.log .log
# output -> sample
$ basename -m /var/www/sample.log /usr/www/test.log
# output -> sample test
```

## xargs
* Xargs is a useful tool to run a particular command for every item in a list.
* 복수의 stdin에 각각에 대하여 지정한 command를 모두 각각 적용하도록 한다.
```bash
$ ls
# image1.JPG image2.JPG image3.JPG image4.jpg
$ basename -s .JPG -a *.JPG | xargs -n1 -i mv {}.JPG {}.jpg
$ ls
# image1.jpg image2.jpg image3.jpg image4.jpg
$ ps -ef | grep php | awk '{print $2}' | xargs kill -9 
$ find /etc/ -name '*.conf' | xargs >> /root/123.txt
$ find . -type d -print0 | xargs -0 chmod 
```
1. basename으로 현재 디렉토리의 모든 .JPG파일을 읽어 extention을 제외한 파일명만 모두 stdout으로 pipeline을 통해 xargs로 보낸다.
1. xargs는 -n 옵션을 통해 --max-args를 1로 지정한다. 즉, xargs를 통해 반복적으로 실행할 커맨드의 arguments의 수는 1개씩!
1. -i 옵션은 --replace[=R]으로서 stdin으로 읽어들인 initial-args의 이름을 R로 replace한다는 의미. 만약 R값이 특정되어있지 않다면 {}으로 가정한다.

## tree
디렉토리의 구성을 한 눈에 보여주는 커맨드.  
linux default built-in command(x)
```
$ tree Docker
../Docker/
├── bind_mount.md
├── docker-clean.sh
├── identidock
│   ├── Dockerfile
│   ├── app
│   │   └── identidock.py
│   ├── cmd.sh
│   └── docker-compose.yml
└── setup_docker_windows_wsl.md
```

## tee

