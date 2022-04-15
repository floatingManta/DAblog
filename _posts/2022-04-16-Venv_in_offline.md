# Python 가상환경 설정
조건:  
- 인터넷 연결이 없는 오프라인
- Windows 10환경
- Visual Studio Code사용
- Jupyter Notebook을 가상환경 커널에 연결

## 목적
- AS-IS: 기존 파이썬 환경은 콘다 기반이고 3.7버전임
  - 콘다 기반이어서 콘다 유료화 이슈 이후 직장에서 가상환경을 운영하기 불리함
  - 매 업데이트마다 콘다 오프라인 설치 파일을 내부망으로 옮겨줘야 함
- 가상환경을 운영하는 이유
	- 글로벌 환경과 겹치면서 환경이 깨지는 상황을 방지함
- TO-DO: 2022년 4월 15일 기준 파이썬 3.10.4 버전으로 콘다가 아닌 가상환경을 생성하고 패키지를 설치하여 운영함

## 작업
	1. 파이썬을 설치한다.
	2. 가상환경을 생성한다.
	3. 인터넷 연결 환경에서 패키지를 설치하고 requirements.txt를 생성하고 패키지를 다운로드한다.
	4. 내부망으로 패키지를 옮겨 설치한다.
	
### 1. 파이썬을 설치한다.
고민사항: 
- 여러 버전의 Python을 설치하면 정상적으로 작동할까?
- 기존 콘다 배포판을 지우지 않은 상태에서 바닐라 Python을 설치하면 정상적으로 작동할까?
  
1. https://www.python.org 에 접속하여 Windows용 원하는 버전 설치파일을 다운로드 받는다.
2. 내부망과 외부망 PC에 각각 설치한다.
기존에 있던 버전이라면 설치파일 실행 시 업데이트를 선택할 수 있고, 다른 버전이라면 설치를 선택할 수 있다.  
설치 시 PATH에 추가하는 옵션을 선택하고 설치 위치를 최대한 사용자 바로 아래에 간단하게 설치할 수 있도록 한다.
3. cmd를 열고 다음 코드로 가상환경 설치를 원하는 디렉토리에 실행파일을 복사한다.
`copy python310\python.exe python310\python310.exe`

### 2. 가상환경을 만든다.
고민사항:
- 콘다 가상환경과 파이썬 가상환경이 같이 운영될까?
- VSCode에 어떻게 인식시킬까?
  
1. VSCode를 켜고 터미널을 연다. VSCode로 가상환경을 연결하려면 반드시 VSCode에서 실행해야 한다.
2. 가상환경을 만들고 싶은 위치로 이동한다.
`cd 가상환경을 만들 위치`
3. 다음 코드를 실행한다.
```
py -3 -m venv (가상환경 이름)
(가상환경 이름)\scripts\activate
```
만일 "Activate.ps1 is not digitally signed. You cannot run this script on the current system."라고 뜨면서 실행이 안되는 경우,  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`을 실행한다.
그 후 다시 위의 activate 명령을 실행한다.  

**파이썬 설치와 가상환경 생성은 내부망과 외부망 모두에서 진행한다.**

### 3. 패키지를 설치한다.
고민사항: 
- 어떻게 빠르게 각종 필요한 패키지를 설치할 수 있을까?
- 파이썬 버전에 따라 패키지 설치 파일이 다른데 어떻게 내 외부망에서 일치시킬 수 있을까?: 이 문제는 위의 1,2 단계에서 내외부에 전부 같은 단계를 실행하여 방지한다.  
이 과정을 무시하여 발생할 수 있는 문제는 다음 페이지 참고:  
https://cryptosalamander.tistory.com/148 판다스 에러난 경우

1. 기존 가상환경을 activate한 터미널에서 다음 명령을 실행하여 requirments.txt를 만든다.
`pip freeze > requirements.txt`
2. 외부망으로 requirements.txt를 옮기고 최신 Python 버전 가상환경 터미널에서 다음 명령을 실행하여 패키지를 다운로드 받는다.
`pip download $PATH -r .\requirements.txt`  
requirements파일의 위치를 지정해 줄 수도 있고, 파일을 가상환경이 활성화된 위치로 이동하여 바로 명령이 실행되게 할 수도 있다.
웬만하면 다운로드 위치를 지정해서 폴더 통째로 압축이 편하도록 해 준다.
3. 다운받은 패키지를 압축해서 내부망으로 옮긴다.
4. VSCode 터미널에서 다음 명령어를 실행하여 패키지를 설치한다.
`pip install --no-index --find-links="./" -r .\requirements.txt`

### 4. Jupyter Notebook 커널을 설정한다.
고민사항:
- 가상환경은 생겼는데 커널을 선택할 때는 왜 가상환경이 안 보일까?

1. 최소 ipykernel 또는 jupyter 패키지가 설치되어야 한다.
2. 터미널에서 가상환경을 활성화한다.
3. 다음 명령을 실행하여 커널을 추가한다.
`ipython kernel install --user --name=.venv --display-name (선택 창에서 보일 이름)`
4. VSCode를 끄고 다시 켠다.

# 추가 고민 사항
Microsoft C++ Build Tools 내부망에 설치(pyobdc 패키지 설치를 위해)  
https://velog.io/@flasharrow/pip-install-%EC%97%90%EB%9F%AC-Microsoft-Visual-C-14.0-is-required.-%ED%95%B4%EA%B2%B0  
분할 압축 시도 https://kr.bandisoft.com/bandizip/help/how-to-split-a-large-file-into-smaller-files-with-bandizip/  
용량 제한으로 인해 실패  

https://levelup.gitconnected.com/install-multiple-python-versions-on-windows-10-15a8685ec99d  
주피터노트북 가상환경?

# Reference
https://levelup.gitconnected.com/install-multiple-python-versions-on-windows-10-15a8685ec99d  
여러 버전의 파이썬을 설치하는 것, 상세한 설치방법과 설치파일 복사하는 방법, 가상환경 생성 방법 설명  
https://code.visualstudio.com/docs/python/python-tutorial#_install-and-use-packages  
VSCode에서 터미널로 가상환경을 생성하고 설치 권한을 조정하는 방법  
https://korea1782.tistory.com/5  
requirements.txt를 통한 빠른 패키지 설치  
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=c_ist82&logNo=220788764088  
패키지 설치 안내
https://srinivas1996kumar.medium.com/adding-custom-kernels-to-a-jupyter-notebook-in-visual-studio-53e4d595208c
Jupyter Notebook에 커널 연결하기
