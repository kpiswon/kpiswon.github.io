---
layout: post
title:  "기본 리눅스(Linux) 명령행(Command line)"
date:   2020-09-02 10:30:00
categories: Algorithm
tags : [알고리즘, 윈도우, 리눅스, windows, linux, 명령행, 명령어, 기본명령어,]
comments: true
---

리눅스에서 사용되는 기본적인 명령어들을 정리해보자.

---

## 기본 터미널 사용법  

 |기능|명령어|비고|
 |:---:|:---:|:---:|
 |문자삭제|백스페이스 <br> Delete키|
 |단어삭제|^w|ctrl+w를 의미하며 공백을 기준으로 단어를 삭제한다.
 |라인삭제|^u|해당 라인을 모두 지운다
 |입력취소|^c|
 |자동완성|tab|입력 도중에 나머지 명령을 자동으로 완성하는 기능
 |도스키|상/하 화살표키| 이전에 입력한 명령을 보여주는 기능

---

## 리눅스 명령어  

#### 명령어 문법  
  `$ 명령어(command) [옵션(option)] [인자(argument)]`  
  * 명령어 : 작업을 지시하는 프로그램  
  * 옵션 : 명령어의 세부 기능에 대한 선택사항  
  * 인자 : 명령어에 전달되는 값 (파일명, 디렉토리 등)  

#### 매뉴얼 보기  
  `$ man 명령어` 
  궁금한 명령어의 매뉴얼을 보여주는 명령어  
  
#### 도움말 보기
  `$ 명령어 --help`  
  궁금한 명령어의 도움말을 보여주는 명령어  

---

### 리눅스 기본 명령어 

 |명령어|설명|예시|
 |:---|:---|:---|
 ls|디렉토리에 있는 파일 나열|$ ls <br> $ ls -l (자세히 표현) <br> $ls -al (더 자세히 표현)|
 cd|디렉토리 이동|$ cd test/ <br> $ cd .. (parent 디렉토리로 이동)|
 pwd|현재 작업 디렉토리의 이름(경로)출력|$ pwd|
 mkdir|디렉토리 생성|$ mkdir test|
 rmdir|디렉토리 삭제 (비어있는 경우만)|$ rmdir test
 touch|(기존 파일의) 시간 변경 <br> (파일이 없는 경우) 빈 파일 생성|$ touch aaa|
 cp|파일 복사(copy)|$ cp aaa aaa2|
 mv|파일명 변경 / 파일 위치 이동|$ mv aaa2 aaa_clone (이름 변경) <br> $ mv aaa2 /User (파일 이동)
 rm|파일/디렉토리 삭제|$ rm aaa_clone <br> $ rm -r test|
 chmod|파일 모드 변경(change mode)|$ chmod +rw aaa <br> (+ : 추가, - : 제거, <br> r : 읽기, w : 쓰기, x : 실행)|
 clear|화면을 깨끗하게 지움|$ clear|

---

### 텍스트 출력 명령어

  |명령어|설명|예시|
  |:---|:---|:---
  cat|텍스트 파일의 내용을 화면에 출력|$ cat sample
  tac|텍스트 파일의 내용을 화면에 역순으로 출력|$ tac sample
  more|텍스트 파일 내용을 화면 단위로 출력|$ more sample
  less|텍스트 파일 내용을 화면 단위로 출력|$ less sample
  head|파일의 시작 부분을 출력 (기본값10행)|$ head sample <br> $head -5 sample
  tail|파일의 끝 부분 출력 (기본값10행)|$ tail sample <br> $ tail -15 sample

  `more`와 `less`는 기능이 거의 비슷하니 손에 익은 걸 쓰면 된다. `cat`과는 달리 화면 단위로 끊어서 살펴보는 데 유용하다.

  > `man`, `more`, `less` 명령어 사용팁
  * 화면 스크롤은 **`화살표`/`SPACE`/`F`/`B` 키** 등을 이용
  * 종료는 **`Q 키`**


#### cat
 * 파일을 화면에 출력  
 `$ cat sample`  

 * 여러 파일을 화면에 출력  
 `$ cat sample sample2 sample3`

 * 파일을 파일에 출력(sample2에 sample1의 내용을 합침)  
 `$ cat sample > sample2`  
 sample2 파일이 없는 경우 파일을 새로 생성해서 저장해준다.
 
 * 여러 파일을 파일에 출력  
 `$ cat sample sample2 sample3 > sample_all`

---

### 텍스트 관련 명령어  

명령어|설명
:---|:---
wc|파일의 라인(newline),단어,바이트 수 출력
grep|문자열 검색
tr|특정 문자 교체/삭제
sort|파일 내용 정렬
uniq|파일에서 중복된 줄 제거
cut|입력에서 문자나 필드 선택
paste|파일들에서 대응되는 행을 통합
cmp|파일비교
diff|파일비교
comm|파일비교
iconv|문자 인토딩 방식을 변경

상세한 설명은 아래 정리한다.

---

#### wc  
 파일의 라인(newline), 단어, 바이트 수를 출력해주는 명령어이다.  
 `wc [옵션]... [파일]...`

###### 옵션  

 옵션|기능
 :---|:---
 -c|바이트 수
 -m|문자 수
 -w|단어 수
 -l|라인 수

 > `$ wc sample` : 라인, 단어, 바이트 수  
 > `$ wc -l sample` : 라인 수

 ---

#### grep  
 문자열 검색 (패턴에 해당하는 라인 찾기)  
 `$ grep [옵션] 패턴 [파일]`  

 * 패턴 : 문자, 문자열, 정규표현  

###### 옵션  

 옵션|기능
 -i|대소문자를 무시하고 검색
 -n|라인 번호도 함께 출력
 -v|패턴과 일치하지 않는 라인을 출력
 -c|패턴과 일치하는 라인 수 출력

###### 특수문자

 문자|의미|예시|결과
 :---:|:---|:---|:---
 ^|라인의 시작|^a|a로 시작하는 모든 행
 $|라인의 끝|a$|a로 끝나는 모든 행
 .|임의의 한 문자|a..o|a로 시작해서 o로 끝나는 4글자를 포함하는 모든 행
 *|앞 문자가 없거나 <br> 여러 번 반복|abc* <br> cont.*|ab, abc, abcc, abccc ... <br> "cont"를 포함하는 모든 행

 > `$ grep -v '^$' sample > sample2` : 빈 라인 제거  
 > 라인이 시작하자마자 끝나는 데, 그런 패턴이 아닌 라인들을 출력하는 것을 출력하므로 결국 빈 라인을 제거하는 것이다.

---

#### tr  
 특정 문자를 교체하거나 삭제하는 명령어이다.  
 
 > *`$ tr [옵션]... 집합1 [집합2]`*
 
 `집합1`에 속한 문자를 `집합2`에 속한 문자로 교체  
 (`집합2`가 있는 경우는 교체하는 명령어가 되고, 없는 경우는 `집합1`을 삭제하라는 의미이다.)

###### 옵션  

 옵션|기능
 :---:|:---
 -c|집합1에 명시되지 않은 모든 문자 (complement)
 -d|집합1에 명시된 문자를 삭제
 -s|집합1에 명시된 문자가 연속으로 나타나면 하나로 축약  

 > `$ tr 'A-Z' 'a-z' < sample`       : 모든 대문자를 소문자로 교체  
 > `$ tr ' ' '\n' < sample`          : 공백을 NEWLINE(개행)으로 교체  
 > `$ tr -sc 'A-Za-z' '\n' < sample` : 알파벳을 제외한 모든 문자가 연속되어 나타나더라도 NEWLINE으로 교체  
 > `$ tr -s ' ' < sample2`           : 연속된 공백문자를 하나로 합침

---

#### sort  
 텍스트 파일을 정렬  
 `$ sort [옵션]... [파일]...`

###### 옵션  

 옵션|기능
 :---:|:---
 -n|수치 정렬
 -r|역순 정렬
 -k start[,stop]|필드를 기준으로 정렬(start에서 stop까지)

 > `$ sort 파일` : 문자열 정렬  
 > `$ sort -n 파일` : 숫자 오름차순 정렬  
 > `$ sort -nr 파일` : 숫자 내림차순 정렬  
 > `$ sort -k1,1 -k2,2n` : key1(1번째 필드부터 1번째 필드까지)는 문자열(default)로, key2(2번째 필드부터 2번째 필드까지)는 숫자(n)로 정렬

###### 주의사항  

 <span style="color:red"> ******* WARNING ******* The locale specified by the environment affects sort order. Set LC_ALL=C to get the traditional sort order that uses native byte values.</span>
 
 -  sort 명령어는 locale의 영향을 받으므로 locale 명령어로 확인을 해보자. Locale에 따라 정렬의 결과가 다르게 나올 수 있으니 다음과 같이 사용하면 된다.  
 - 코드값에 의한 정렬 결과를 원할 때  
 `$ LC_ALL=C sort [옵션]... [파일]...`  
 - 환경변수  
 `$ export LC_ALL=C` : 터미널의 재부팅 전까지 locale을 유지해준다.

 ---

#### uniq

 연속된 줄을 하나만 남기고 삭제  
 `$ uniq [옵션]... [입력 [출력]]`

###### 옵션

 옵션|기능
 -c|출현 횟수를 함께 출력
 -d|연속해서 중복된 라인만 출력
 -u|중복되지 않은 라인만 출력

###### 주의사항
 정렬된 파일에서만 실행하여야 의미가 있다. 정렬을 하지 않으면 unique한 line만 남지 않게 된다.

--- 

#### cut  
 파일의 각 라인에서 선택된 필드를 잘라낸다.  
 `$ cut 옵션... [파일]...`  

###### 옵션  

 옵션|기능
 :---:|:---
 -c|선택할 문자 위치 지정
 -f|필드를 지정
 -d|필드 구분자 (default: TAB)

 > `$ cut -c 1-7 sample4` : 1~7번째 문자 선택  
 > `$ cut -c 9-` : 9번째 이후 문자 선택  
 > `$ cut -f1` : TAB으로 구분된 첫번째 필드를 선택  
 > `$ cut -f2` : TAB으로 구분된 두번째 필드를 선택  
 > `$ cut -d` ` -f1 : 공백으로 구분된 첫번째 필드를 선택  

---

#### paste  
 둘 이상의 파일의 같은 라인을 TAB으로 구분하여 결합해준다.
 `$ paste [옵션]... [파일]...`  

###### 옵션  

 옵션|기능
 :---:|:---
 -d|필드 구분자 (default : TAB)
 -s|병렬 대신 직렬(serial)로 결합(한 파일씩 처리)

###### 예제  
 * abc 파일  
 ```
 a
 b
 c
 ```

 * 123 파일  
 ```
 1
 2
 3
 ```

 > `$ paste abc 123 > result`
  * result 파일  
  ```
  a		1
  b		2
  c		3
  ```  

> `$ paste -s abc 123 > result2`
  * result2 파일  
  ```
  a	b	c
  1	2	3
  ```


---

#### cmp  
 두 파일 비교 (바이트 단위)  
 `$ cmp 파일1 파일2` 
 > `$ cmp sample sample` : 동일한 파일인 경우, 내용이 같은 두 파일을 비교하면 화면에 아무 것도 출력되지 않음  
 > `$ cmp sample sample2` 

---

#### diff  
 파일 비교 (줄 단위)  
 `$ diff 파일1 파일2`  
 > `$ diff sample sample` : 동일한 파일인 경우, 내용이 같은 두 파일을 비교하면 화면에 아무것도 출력되지 않음  
 > `$ diff sample sample2` 

---

#### comm  
 정렬된 두 파일 비교 (줄 단위)  

 `$ comm [옵션] 파일1 파일2`  
 > **3열의 출력**을 해준다.  
  > 1열 : 파일1에만 있는 줄 출력  
  > 2열 : 파일2에만 있는 줄 출력  
  > 3열 : 파일1,2 모두 있는 줄 출력  

###### 옵션  

 옵션|기능
 :---:|:---
 -1|1열을 제외
 -2|2열을 제외
 -3|3열을 제외

 > `$ comm sample1 sample2`  
 > `$ comm -3 sample1 sample2` : 중복되지 않은 줄만 표시  
 > `$ comm -12 sample1 sample2` : 중복된 줄만 표시  

---

#### iconv  
 파일의 문자 인코딩 방식을 변경  
 `$ iconv -f 변경전인코딩 -t 변경후인코딩 [파일]`  

 > `$ iconv -f cp949 -t utf8 infile > outfile` : 완성형을 UTF-8로 변환  
 > `$ iconv -l` : 지원하는 인코딩 목록을 확인  

---

#### 파이프라인(Pipeline)  
 명령어의 표준 출력을 다른 명령어의 표준 입력으로 넘겨주는 역할을 한다.  
 '|'로 표현한다.

 > `$ grep -c of sample3` : of를 포함하는 라인 수  
 > `$ grep of sample3 | wc -l` : 위와 동일  
 > `$ sort -i sample3 | uniq -c | sort -nr | less` : 1. 대소문자 구분 없이 정렬한 결과 -> 2. 출현 횟수를 세면서 하나만 남기고 삭제 -> 3. 출현 횟수 숫자 내림차순 정렬 -> 4. 텍스트 파일 내용을 화면 단위로 출력


---


최종수정일 2020.09.01



> ###### References
> 고려대학교 202R 알고리즘, 이도길

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon

