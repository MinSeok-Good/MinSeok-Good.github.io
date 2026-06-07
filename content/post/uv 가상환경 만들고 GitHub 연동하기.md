---
title: "UV 가상환경과 github 연동"
date: "2026-06-07"
draft: false
categories:
  - Study
tags:
  - UV
  - 가상환경
  - github
---

# uv 가상환경 만들고 GitHub 연동하기

### 1단계: GitHub Repository 생성 (웹 페이지)

1. GitHub 프로필 ➔ **Repositories** 클릭 ➔ **New** 클릭

![image.png](/img/image.png)

1. **Repository name** 설정 (프로젝트 이름)
2. **Public**(전체공개) 또는 **Private**(비공개) 설정
3. **Add a README file** 체크 (프로젝트 설명서 자동 생성)
4. **Choose a license** 설정 (통상 오픈소스인 **MIT License** 선택)

![image 1.png](/img/image%201.png)

- 맨 아래 **Create repository** 버튼 클릭 후, 초록색 **[<> Code]** 버튼을 눌러 HTTPS 주소 복사하기

![image 2.png](/img/image%202.png)

### 2단계: 로컬 PC로 가져오기 (Git Clone)

1. 코딩 작업을 진행할 로컬 디렉터리(폴더)로 이동합니다.
2. 폴더 빈 곳 마우스 우클릭 ➔ **추가 옵션 표시** ➔ **Git Bash Here** 클릭
3. GitHub에서 복사한 주소로 다운로드:Bash
    
    ```
    git clone <복사한_주소>
    ```
    
4. 다운로드가 완료되면 생성된 프로젝트 폴더 안으로 이동:Bash
    
    ```
    cd <생성된_레포지토리_폴더명>
    ```
    

*(※ 만약 내 PC에서 Git을 처음 쓰는 경우에만 아래 로그인 명령어를 최초 1회 실행합니다.)*

Bash

```
git config --global user.email "내_이메일@주소.com"
git config --global user.name "깃허브_닉네임"
```

### 3단계: VS Code 연동 및 가상환경 구동

1. 현재 Git Bash 위치에서 그대로 VS Code 실행:Bash
    
    ```
    code .
    ```
    
2. **[중요]** 기존에 작업하던 외부 소스 코드나 데이터 파일(목적 파일)이 있다면, 이 시점에 VS Code 창에 보이는 폴더 안으로 복사하여 붙여넣습니다. (단, `pyproject.toml` 파일이 반드시 포함되어 있어야 합니다.)
3. VS Code 내에서 터미널(`Ctrl + ``)을 열고 아래 명령어 순서대로 입력:Bash
    
    ```markdown
    # 1. pyproject.toml 기반으로 독립된 가상환경(.venv) 자동 구축
    uv sync
    
    # 2. 라이브러리 환경 정상 작동 여부 검증 (선택)
    uv run python check_env.py
    
    # 3. 주피터 랩 구동 및 분석 시작
    uv run jupyter lab
    ```
    

### 4단계: 작업 내용 GitHub에 반영하기 (Git Push)

주피터 랩에서 작업을 마치고 코드를 저장한 후, VS Code 터미널에서 아래 명령어를 순서대로 입력하여 원격 창고로 업로드합니다.

Bash

```
git add .
git commit -m "feat: 주피터 노트북 분석 환경 세팅 및 코드 추가"
git push origin main
```

### 💡 실무 트러블슈팅 팁

만약 패키지 충돌이나 꼬임 현상이 의심되어 **환경을 완전히 깨끗하게 초기화**하고 싶다면?

1. 프로젝트 폴더 내에 생성된 **`.venv` 폴더를 통째로 삭제**합니다.
2. 필요시 uv 캐시를 비워줍니다: `uv cache clean`
3. 다시 터미널에 `uv sync`를 입력하면 처음처럼 깔끔하게 가상환경이 재구축됩니다.