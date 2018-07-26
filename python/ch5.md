# [스터디] 파이썬 :: 1. 파이썬 소개 및 개발환경 구축
> 작성일: 2018.07.27 , 작성자: 이대형

이번 장은 파이썬의 파일 입출력 및 데이터베이스에 대해 알아보겠습니다.

### 목차
1. [파일입출력](#m1)
2. [파일 관리](#m2)
3. [Quiz!](#m3)

---

<a id="m1"></a> 
## 1. 파일입출력
### 파일 쓰기
- 파일 경로란 입출력 대상 파일의 이름이다. 디렉토리 경로를 포함할 수 있고, 파일명만 있으면 혀재 디렉토리에서 찾는다.
- 모드는 읽기, 쓰기, 추가 등 파일로 무엇을 할 것인가를 지정하며, 읽을 파일이 없거나 생성할 파일이 이미 있을 때의 처리방식을 결정한다.

`open("sample.txt", "r")` <=sample.txt 파일을 읽기 모드로 열기

모드
| 모드    | 설명      |
|--------|:-------------------:|
| r   | 파일을 읽는다. 파일이 없으면 예외 발생        |
| w   | 파일에 기록한다. 파일이 이미 있으면 덮어쓰기        |
| a   | 파일에 데이터를 추가한다.  |
| x   | 파일에 기록하되 파일이 이미 있으면 실패한다.        |
**모드 뒤에는 파일의 종류를 지정하는 문자를 쓴다.**

**t는 텍스트 파일, b는 이진 파일.      ex) open("sample.txt","rt")**


#### 예제: ch5Ex1.py - 파일 쓰기 
```python
f=open("sample.txt","wt") #wt 텍스트 파일 쓰기모드로 열기
f.write("""안녕하세요
파이썬 프로젝트 파이팅입니다!
파이팅!!""")
f.close()
```
![샘플 이미지](https://imgur.com/TELbo3u.jpg")


### 파일 읽기

#### 예제: ch5Ex2.py - 파일읽기1
```python
try:
  f=open("sample.txt","rt")
  text=f.read()
  print(text)
  except FileNoFoundError:
    print("파일이 없습니다.")
  finally:
    f.close()
```


#### 예제: ch5Ex3.py - 파일읽기2
```python
f=open("sample.txt","rt")
while True:         #무한루프를 돌며
  text=f.read(10)   #한번에 10문자씩 읽어 출력하되
  if len(text)==0:  #읽은 길이가 0이면 
    break           #파일 끝에 도달했다는 뜻이므로 루프를 탈출한다
  print(text, end='')
f.close()
```
#### 예제: ch5Ex4.py - 파일읽기3(readline함수 사용)
```python
f=open("sample.txt","rt")
text=""
line=1
while True:
  row=f.readline()
  if not row: break
  text += str(line) + " : " + row
  line += 1
f.close()
print(text)
```
![샘플이미지](https://imgur.com/B0ozv3o.jpg")

#### 예제: ch5Ex5.py - 파일읽기4(readlines함수 사용->row를 for문으로 돌리기)
```python
f=open("sample.txt","rt")
rows=f.readlines()  #readlines()대신 readline()을 쓰면 한줄만 출력함.
for row in rows:
  print(row,end = '')
f.close()
```

#### 예제: ch5Ex6.py - 파일읽기5(줄단위로 읽을 때 가장 간단한 방법->파일 자체에서 for문 돌리기)
```python
f=open("sample.txt","rt")
for line in f:
  print(line,end='')
f.close()
```

### 입출력 위치

파일 객체는 다음 입출력 위치를 정확하게 기억하기 때문에 조금씩 반복적으로 읽어도 전체 파일을 다 읽을 수 있다. 

현재 입출력 위치를 조사할 때는 **tell** 함수를 호출, 위치를 변경할 때는 **seek**함수를 사용한다.

#### 예제: ch5Ex7.py - seek함수
```python
f=open("sample.txt","rt")
f.seek(12,0)      #파일을 열자마자 12바이트 건너 뛰고 이 위치에서부터 읽기 시작한다.
text=f.read()     #한글은 한글자에 2바이트 차지
f.close()
print(text)
```
![샘플이미지](https://imgur.com/3tj87I5.jpg")


### 내용 추가
"a"모드는 파일에 내용을 추가한다.
#### 예제: ch5Ex8.py - 내용추가

```python
f=open("sample.txt","at")
f.write("\n\n 다들 고생하셨어요.")      
f.close()
```
![샘플이미지](https://imgur.com/l3EMdhb.jpg")

<a id="m2"></a> 
## 2. 파일 관리

#### 파일 관리 함수
| 함수     |  설명      |
|--------|:-------------------:|
| shutil.copy(a,b)    |     파일을 복사한다     |
| shutil.move(a,b)    |       파일을 이동한다   |
| shutil.rmtree(path)    |      디렉토리를 삭제한다    |
| os.rename(a,b)    |     파일 이름을 변경한다.     |
| os.remove(f)    |   파일을 삭제한다       |
| os.chmod(f,m)    |      파일의 퍼미션을 변경한다.     |
| shutil.chown(f,u,g)    |     파일의 소유권을 변경한다.     |
| os.link(a,b)    |    하드 링크를 생성한다.      |
| os.symlink(a,b)    |     심볼릭링크를 생성한다.     |


#### 디렉토리 관리 함수

| 함수     |  설명      |
|--------|:-------------------:|
| os.chdir(d)    |     현재 디렉토리를 변경한다     |
| os.mkdir(d)    |     디렉토리를 생성한다     |
| os.rmdir(d)    |     디렉토리를 제거한다     |
| os.getcwd()    |     현재 디렉토리를 조사한다     |
| os.listdir(d)    |     디렉토리의 내용을 나열한다.     |
| glob.glob(p)    |     패턴과 일치하는 파일의 목록을 나열한다.     |


##### 디렉토리 경로 조사, 조작하는 함수

| 함수     |  설명      |
|--------|:-------------------:|
| os.path.isabs(f)    |     절대 경로인지 조사한다.     |
| os.path.abspath(f)    |     파일의 절대경로를 구한다.     |
| os.path.realpath(f)    |     원본 파일의 경로를 구한다.     |
| os.path.exists(f)    |     파일의 존재 여부를 조사한다     |
| os.path.isfile(f)    |     파일인지 조사한다.    |
| os.path.isdir(f)    |     디렉토리인지 조사한다.     |

#### 예제: ch5Ex9.py - 디렉토리 리스트

```python
import os

files =os.listdir("C:\\Users\Lee DaeHyung\Desktop\대형\수업내용\DHrepo\hitlab_edu\python\src")
for f in files:
  print(f)
```
![샘플이미지](https://imgur.com/Ha0KcVS.jpg")

#### 파일 관리 유틸리티

#### 예제: ch5Ex10.py - 파일명 바꾸기

```python
import os

path="C:\\Users\Lee DaeHyung\Desktop\대형\수업내용\DHrepo\hitlab_edu\python"
files=os.listdir(path)

for f in files:
  if (f.find("-")and f.endswith(".PNG")):
    name= f[0:-4]           #.PNG 라는 문자는 뒤에서부터 4번째자리에 있다(-4).
    ext= f[-4:]             #ext=.PNG임을 뜻한다(-4번째자리부터 끝까지)
    part= name.split("-")   #'-' 를 기준으로 part를 나눈다 part[0],part[1]
    newname=part[1].strip()+"-"+part[0].strip()+ext
    print(newname)
    os.rename(path+"\\"+f,path+"\\"+newname)
```
![샘플이미지](https://imgur.com/QiCTVv8.jpg")

코드 실행 후 디렉토리

![샘플이미지](https://imgur.com/ZcjJt1v.jpg")


<a id="m3"></a>
## 3. Quiz!

#### Q. 파일명이 "애국가_1절.txt, 애국가_2절.txt, 애국가_3절.txt, 애국가_4절.txt 파일을 만들어 보세요(내용은 아무것도 없어도 됩니다)"
<details>
  <summary>정답보기</summary>
  
  ```python
  for i in range(4):
  k=str(i+1)
  f=open("애국가_"+k+"절.txt","wt")
  f.write("""무궁화 삼천리""")
  f.close
 ```
</details>



#### Q. 파일명이 "애국가_1절.txt, 애국가_2절.txt, 애국가_3절.txt, 애국가_4절.txt 파일을 '_'기준으로 애국가와 n절의 이름을 바꿔(switch)보세요"
힌트
```python
import os

path="C:\\Users\Lee DaeHyung\Desktop\대형\수업내용\DHrepo\hitlab_edu\python"
files=os.listdir(path)

for f in files:
  if (f.find("-")and f.endswith(".PNG")):
    name= f[0:-4]           #.PNG 라는 문자는 뒤에서부터 4번째자리에 있다(-4).
    ext= f[-4:]             #ext=.PNG임을 뜻한다(-4번째자리부터 끝까지)
    part= name.split("-")   #'-' 를 기준으로 part를 나눈다 part[0],part[1]
    newname=part[1].strip()+"-"+part[0].strip()+ext
    print(newname)
    os.rename(path+"\\"+f,path+"\\"+newname)
```
<details>
  <summary>정답보기</summary>
  
  ```python
import os
path="C:\\Users\Lee DaeHyung\Desktop\대형\수업내용\DHrepo\hitlab_edu\python\애국가예제"
files=os.listdir(path)
for f in files:
  if (f.find("_") and f.endswith(".txt")):
    name= f[0:-4]          
    ext= f[-4:]            
    part= name.split("_")  
    newname=part[1].strip()+"_"+part[0].strip() + ext
    print(newname)
    os.rename(path+"\\"+f,path+"\\"+newname)
 ```
</details>