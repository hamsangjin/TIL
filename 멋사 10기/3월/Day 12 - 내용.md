# nvm & node 설치(MAC)

- 개발 환경
    - node version: 20.11.1
    - npm version: 10.2.4

```php
// brew 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install node
brew install nvm

mkdir ~/.nvm
cd ~/

// .zshrc 열기
vi ~/.zshrc
// 해당 내용 추가
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && . "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && . "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

// 터미널 재시작

nvm install 15.12.0
nvm use 15.12.0

// 15.12.0
node -v

npm install -g npm@7.6.3
```

<br>

---

<br>

# VS code - Live Server
먼저 VS Code의 확장에서 다운로드 받아준다.
<img width="678" alt="스크린샷 2024-03-27 11 15 39" src="https://github.com/hamsangjin/TIL/assets/103736614/3843902c-a8a1-4a5a-bd11-2df66d3fe509">

<br>

그 후 실행시킬 파일에서 마우스 우클릭 후, Open With Live Server를 눌러 실행시켜준다.
<img width="629" alt="스크린샷 2024-03-27 11 16 12" src="https://github.com/hamsangjin/TIL/assets/103736614/587625b6-2dc2-45bf-a2ab-2f061f152abd">

<br>

그러면 아래와 같이 실행되며, 수정해도 동적으로 바로 변경된다.
<img width="534" alt="스크린샷 2024-03-27 11 17 09" src="https://github.com/hamsangjin/TIL/assets/103736614/efbfb99e-dfe0-4e2d-b112-7c2466495c8e">

<br>

---

<br>

# HTML5 

## 시작태그, 끝태그, 내용
```html
<h1> Hello </h1>
```

- 시작태그: `<h1>`
- 끝태그: `</h1>`
- 내용: `Hello`
- 요소: `시작태그` + `내용` + `끝태그`

<br>

## 속성
```html
<a href="http://www.naver.com">naver</a>
```
- 속성이름: `href`
- 속성값: `http://www.naver.com`
- 속성블록: `href="http://www.naver.com"`

<br>

## 내용이 없을 경우
```html
<br>

<br/>
```
- `<br>` or `<br/>` : 줄 바꿈
