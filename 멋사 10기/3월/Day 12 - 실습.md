# 실습문제 1: 자기소개 페이지 만들기

## 문제 설명
1. HTML5 문서 기본 구조를 사용하여 페이지를 생성하세요.
2. `<header>`, `<footer>`, `<main>`, `<section>` 태그를 사용하여 문서를 구조화하세요.
3. 자기소개를 위한 `<section>`을 최소 2개 만드세요. 예를 들어, 하나는 교육 및 경력, 다른 하나는 취미 및 관심사에 관한 것이 될 수 있습니다.
4. 각 섹션에는 적절한 제목`(<h1> , <h2> , ...)`을 사용하세요.
5. 자기소개에 사용할 사진 한 장을 `<img>` 태그를 사용하여 삽입하세요. `alt` 속성을 잊지 마세요.
6. 좋아하는 인용구를 `<blockquote>` 태그를 사용하여 추가하세요. 인용구의 출처는 `<cite>` 태그를 사용하여 명시하세요.
7. 연락처 정보를 `<footer>` 에 `<address>` 태그를 사용하여 포함시키세요.

## 구현 코드
```html
<!DOCTYPE html>
<html lang="ko">
    <head>
            <meta charset="UTF-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title>개인 포트폴리오</title>
    </head>
    <body>
        <header>
            <h1>함상진</h1>
            <img src="images/me.png" alt="블로그 포스트 관련 이미지" width="200" />
            <p>웹 개발자 취준생</p>
        </header>

        <hr>

        <section>
            <h2>소개</h2>
            <p>
                안녕하세요, 저는 웹개발자를 위해 준비중인 함상진입니다.
            </p>
        </section>
        
        <hr>

        <section>
            <h2> 좋아하는 인용구 </h2>
            <blockquote> 일단 하자 ! </blockquote>
            <cite> 출처: 나의 뇌</cite>
        </section>
        

        <hr>

        <section>
            <h2>경력</h2>
            <ul>
                <li>한경국립대학교 컴퓨터공학과 졸업 2018.03 ~ 2024.02</li>
                <li>멋쟁이사자처럼 백엔드스쿨 10기 2024.03.11 ~</li>
            </ul>
        </section>

        <hr>

        <section>
            <h2>프로젝트</h2>
            <ul>
                    <li>
                        2023 BRIGHT MAKERS EXPO 제15회 캡스톤디자인 경진대회 - 다인원 포즈 렌더링 프로그램(대상)<br>
                        객체 탐지 및 포즈 추정 부분을 진행
                    </li>
            </ul>
        </section>

        <hr>

        <footer>
            <h2>연락처 정보</h2>
            <address> Address: 사랑시 고백구 행복동 </address>
            <p>Email: rhkstlash@naver.com</p>
            <p> My Github : <a href="https://github.com/hamsangjin"> Github</a> </p>
            <p>My Velog : <a href="https://velog.io/@hamsangjin/posts"> Velog</a> </p>
        </footer>
    </body>
</html>
```

## 구현 결과
<img width="735" alt="스크린샷 2024-03-27 16 23 36" src="https://github.com/hamsangjin/TIL/assets/103736614/53663895-e682-455e-b998-d489ec6e3de0">


<br>

---

<br>

# 실습문제 2: 간단한 블로그 포스트 페이지 만들기

## 문제 설명
1. HTML5 문서의 기본 구조로 시작하세요.
2. 포스트의 제목은 `<h1>` 태그를 사용하여 페이지 상단에 배치하세요.
3. 게시 날짜와 작성자 이름을 `<p>` 태그에 `<small>` 태그를 사용하여 표시하세요. 
4. 본문 내용은 여러 개의 `<p>` 태그를 사용하여 입력하세요.
5. 중요한 키워드 또는 문장은 `<strong>` 또는 `<em>` 태그를 사용하여 강조하세요.
6. 포스트 내에서 참조하는 외부 사이트는 `<a>` 태그를 사용하여 링크하세요. `target="_blank"` 속성을 사용하여 새 탭에서 링크가 열리도록 하세요.
7. 포스트에 관련 이미지를 최소 하나 포함시키세요. `<img>` 태그를 사용하고, `alt` 속성을 추가하세요.
8. 포스트의 마지막에는 댓글 섹션을 상상하고, 사용자의 댓글 3개를 `<ul>` 또는 `<ol>`과 `<li>` 태그를 사용하여 목록으로 만드세요.

## 구현 코드
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title> 함상진님의 미니홈피 </title>
    </head>
    <body>
        <header>
            <h1> 함상진님의 미니홈피 </h1>
            <p> +:+:+:+:★ⓗⓐⓥⓔ ⓐ ⓝⓘⓒⓔ ⓓⓐⓨ★+:+:+:+:  </p>
        </header>

        <hr>

        <section>
            <p>
                <small>Today: 63</small>
                <small>|</small>
                <small>Total: 2029392</small>
            </p>
        </section>

        <section>
            <img src="images/blog-profile.jpeg" width="200" alt="싸이월드 짤"/>
            <p>
                학생○lㄹΓ는 죄로                <br>
                학교ㄹΓ는 교도소øłlnㅓ          <br>
                교실○lㄹΓ는 감옥○l 갇혀         <br>
                출석부ㄹΓ는 죄수명단øłl 올ㅇr   <br>
                교복○l란 죄수복을 입ヱ          <br>
                공부란 벌을 받ヱ                <br>
                졸업○l란 석방을 ブl⊂ト린⊂ト.    <br>

                <br>
                <br>

                <a href="https://m.blog.naver.com/38qudehd/222695432873" target="_blank">짤 출처</a>
            </p>
        </section>
        <hr>
        <section>
            <h2> 댓글 </h2>
            <ol>
                <li> 홍길동 : 퍼가요~♡ </li>
                <li> 아무개 : 일촌 고고 </li>
            </ol>
        </section>
    </body>
</html>
```

## 구현 결과
<img width="299" alt="스크린샷 2024-03-27 16 24 10" src="https://github.com/hamsangjin/TIL/assets/103736614/d823bc57-b462-4c9b-9fd6-2f90a142b6e0">


<br>

---

<br>

# 실습문제 3: 제품 소개 페이지 만들기

## 문제 설명
1. HTML5 기본 구조로 페이지를 시작하세요.
2. 제품 이름은 `<h1>` 태그를 사용하여 크게 표시하세요.
3. 제품 설명은 `<p>` 태그를 사용하여 작성하세요.
4. 제품의 주요 특징을 `<ul>` 또는 `<ol>` 태그를 사용하여 목록으로 만드세요.
5. 제품 이미지를 최소 두 개 이상 포함시키세요. 각 이미지에는 `alt` 속성을 추가하세요.
6. 제품에 대한 사용자 리뷰를 `<section>` 태그를 사용하여 추가하세요. 각 리뷰는 `<article>` 태그 안에 있어야 합니다.
7. 각 리뷰에는 리뷰어의 이름을 `<h3>` 태그로, 리뷰 내용을 `<p>` 태그로 포함시키세요.
8. 제품 구매 페이지나 연락처 페이지로의 링크를 `<a>` 태그를 사용하여 추가하세요.

## 구현 코드
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <div>
            <section>
                <h1> 상진 바버샵 </h1>
                <img src="images/hairshop.jpeg" alt="상진 바버샵"/>
            </section> 

            <hr>

            <section>
            <h2> 컷트 + 다운펌 50,000원 </h2>
            <p>
                사이드파트 또는 아이비리그 등 윗머리 디자인, 머리를 감아도 고정되는 손쉬운 스타일링을 도와드립니다. <br>
            </p>
            </section>

            <section>
            <h2> 주의사항 </h2>
            <ul>
                <li> 1시간 남았을 때 예약 가능합니다.  </li>
                <li> 당일 취소 불가합니다. </li>
            </ul>
            </section>

            <hr>

            <section>
                <article>
                    <h2> ★☆☆☆☆ 1.0</h2>
                    <small>2024. 03. 27 | 1번째 방문 | 영수증</small>
                    <br>
                    
                    <img src="images/hairReview.png" width="200" height="200" alt="미용실 리뷰"/>
                    <p> 
                        측두엽손상, 배고픈사람에게 자기머리떼어준 호빵맨, 태어나자마자 인큐베이터에서 떨어졌냐느니 별소리를 다 들었습니다. <br>
                        원장님께서 as는 안된다고하네요 번창하세요 </p>
                </article>
            </section>
            
            <section>
                <a href="www.sangjin@Barber.shop"> 홈페이지 </a>
            </section>
        </div>
    </body>
</html>
```

## 구현 결과
<img width="840" alt="스크린샷 2024-03-27 16 25 22" src="https://github.com/hamsangjin/TIL/assets/103736614/6129610e-1647-4dea-986f-6c04b8af2de5">
