# CSS

## CSS의 기본 구성
CSS의 기본 구성은 크게 선택자와 선언부로 이루어진다. <br>

```css
selector{
  property: value;
  property: value;
}
```

선언부는 다시 속성과 속성 값으로 이루어지며, `;`으로 속성과 속성값을 구분하여 여러 개의 선언을 지정할 수 있다.

<br>

---

<br>

# 선택자

## 선택자 - *
```css
*  {
  margin: 0;
  padding: 0;
}
```
- `*`: 페이지에 있는 전체 요소를 대상으로 적용
- 브라우저에 과부하가 걸리기 때문에 실전에서 사용하기엔 적절하지 않다.

<br>

```css
#container * {
  border: 1px solid black;
}
```
- `#container div`의 자식 요소 전체를 대상으로 적용

<br>

## 선택자 - #id
```css
#container  {
  width: 960px;
  margin: auto;
}
```
- id 선택자로, 선택자 앞에 `#` 기호를 붙여 id를 대상으로 삼는다.
- 찾을 가망성이 거의 없는 요소에 id를 사용하고 그 유일한 요소에만 스타일을 적용한다.
- 하지만 id 선택자는 유연성이 없고 재활용할 수 없으니, 가능한 처음에 태그명이나 새로운 HTML요소 중 하나, 아니면 가상 클래스라도 사용하는 것이 좋다.

<br>

## 선택자 - .class
```css
.error {
  color: red;
}
```
- class 선택자로, class는 id와 다르게 여러 개의 요소를 대상으로 정할 수 있다.
- 스타일을 한 그룹의 요소에 적용할 때는 class를 사용한다.


<br>

## 선택자 - X Y
```css
li a {
  text-decoration: none;
}
```
- 선택자를 이용해 더 상세히 작업할 때 이 선택자를 사용한다.
- 전체 `a태그`를 대상으로 하기보다 `li태그`에 있는 `a태그`를 대상으로만 한다면, 더욱 스타일 적용이 상세해진다.

<br>

## 선택자 - 태그
```css
a {
  color: red;
}
ul {
  margin-left: 0;
}
```
- id나 class가 아닌 type에 따라 한 페이지에 있는 모든 요소를 대상으로 삼고 싶다면 type선택자를 이용한다.

<br>

## 선택자 - X:가상클래스
```css
a:link {
  color: red;
}
a:visited {
  color: purple;
}
```
- 클릭하기 전 상태의 앵커 태그 전체를 대상으로 할 경우 `:link` 가상 클래스를 사용
- 클릭했었거나 방문했던 페이지에 있는 앵커 태그 전체를 대상으로 할 경우 `:visited` 가상 클래스를 사용

<br>

## 선택자 - X + Y
```css
ul + p {
  color: red;
}
```
- 인접 선택자로 부르는 선택자로, 앞의 요소 바로 뒤에 있는 요소만 선택한다.
- 위 코드는 각 `ul태그`뒤에 오는 첫 번째 `p태그`만 빨간색이 된다.

<br>

## 선택자 - X > Y
```css
div#container > ul {
  border: 1px solid black;
}
```
- 위에서 본 `태그 태그`는 자식뿐만 아니라 자손까지 적용이 됐지만 `태그 > 태그`는 자식만을 선택한다.

<br>

## 선택자 - X ~ Y
```css
ul ~ p {
  color: red;
}
```
- `X + Y`와 유사하지만 바로 뒤에 있는 요소만 선택하는 것이 아닌, `ul` 아래 있는 모든 `p`요소들을 선택하게 된다.

<br>

## 선택자 - X[title]
```css
a[title] {
  color: green;
}
```
- 속성 선택자라고 하며, `title` 속성이 있는 `a태그`만을 선택한다.

<br>

## 선택자 - X[href=""]
```css
a[href="http://net.tutsplus.com"] {
  color: #00ff00;
}
```
- 위 코드는 `http://net.tutsplus.com`로 연결된 전체 `a태그`에 스타일을 적용한다는 뜻이다.

<br>

## 선택자 - X[href*=""]
```css
a[href*="naver"] {
  color: #00ff00;
}
```
- 별표는 입력값이 속성값 안 어딘가 보여야한다는 것을 표시한다.
- 위 코드는 `naver.com`, `blog.naver.com`, `naverplus.com`까지도 적용하며 문자열의 앞과 뒤에 `^`와 `$`를 붙일 수도 있다.

<br>

## 선택자 - X[href^=""]
```css
a[href^="http"] {
   background: url(path/to/external/icon.png) no-repeat;
   padding-left: 10px;
}
```
- 속성 안의 값이 특정 값으로 시작하는 태그를 선택

<br>

## 선택자 - 태그[href$=""]
```css
a[href$=".jpg"] {
   color: red;
}
```
- 속성 안의 값이 특정 값으로 끝나는 태그를 선택

<br>

## 선택자 - X[data-*=""]
```css
a[data-filetype="image"] {
   color: red;
}
```
- 속성 안의 값이 특정 값을 포함하는 태그를 선택

<br>

## 선택자 - X[foo~="bar"]
```css
a[data-info~="external"] {
   color: red;
}
a[data-info~="image"] {
   border: 1px solid black;
}
```
- 속성 안의 값이 특정 값을 단어로 포함하는 태그를 선택

<br>

## 선택자 - X:checked
```css
input[type=radio]:checked {
   border: 1px solid black;
}
```
- 이 가상 클래스는 라디오 버튼이나 체크박스처럼 체크되는 사용자 인터페이스 요소만을 대상으로 한다.

<br>

## 선택자 - X:after / before
```css
.exciting-text::after {
  content: " <- 흥미진진!";
}
```
- `before`와 `after` 가상 클래스는 선택된 요소 주변에 콘텐츠를 생성해준다.

<br>

## 선택자 - X:hover
```css
div:hover {
  background: #e3e3e3;
}
```
- 사용자 동작 가상 클래스이며, 사용자가 요소 위에 커서를 올릴 때 특정한 스타일을 적용하고 싶을 때 사용

<br>

## 선택자 - X:not(선택자)
```css
div:not(#container) {
   color: blue;
}
```
- `negetion 가상 클래스`는 특히 유용하며, 모든 div를 선택하고 싶은데, 그중에서 `id`가 `container`인 것만 빼고 싶을 경우 위와 같이 사용한다.

<br>

## 선택자 - X::가상 요소
```css
p::first-line {
   font-weight: bold;
   font-size: 1.2em;
}
```
- 가상 요소는 `::`로 표시하며, 일부분에 스타일을 적용하는데 가상 요소를 사용할 수 있고, 블록 레벨 요소에 적용해야 효과를 볼 수 있다.

<br>

## 선택자 - X:nth-child(n)
```css
li:nth-child(3) {
   color: red;
}
```
- `nth-child`는 변수값을 정수로 받으며, 0부터 시작하지는 않고, 어떤 부모의 n번째 특정 태그에만 해당된다.
- 두 번째 항목을 대상으로 하고 싶다면 `li:nth-child(2)`로 작성한다.

<br>

## 선택자 - X:nth-last-child(n)
```css
li:nth-last-child(2) {
   color: red;
}
```
- 만약 ul에 항목이 엄청 많고, 세 번째 항목만 필요하다고 한다면 어떻게 한 경우 사용하면 된다.

<br>

## 선택자 - X:nth-of-type(n)
```css
ul:nth-of-type(3) {
  border: 1px solid black;
}
```
- `child`를 선택하지 않고 요소의 `type`을 선택해야 하는 경우가 있다.
- `ul`이 5개 있을 때, 세 번째 `ul`에만 스타일을 지정하고 싶은데 그것을 지정할 `id`가 없을 경우 사용하면 된다.

<br>

## 선택자 - X:nth-last-of-type(n)
```css
ul:nth-last-of-type(3) {
  border: 1px solid black;
}
```
- `X:nth-of-type(n)`은 첫 번째 요소부터 찾았다면 `X:nth-last-of-type(n)`은 마지막 요소부터 찾는 것이다.

<br>

## 선택자 - X:first-child
```css
ul li:first-child {
  border-top: none;
}
```
- `ul태그`에 있는 `li태그`의 첫 번째 자식만 대상으로 선택

<br>

## 선택자 - X:last-child
```css
ul > li:last-child {
  color: green;
}
```
- `ul태그`에 있는 `li태그`들의 마지막 자식만 대상으로 선택

<br>

## 선택자 - X:only-child
```css
div p:only-child {
  color: red;
}
```
- 이 선택자는 부모의 자식 중 형제 자식이 없는 요소를 선택

<br>

## 선택자 - X:only-type
```css
li:only-of-type {
  font-weight: bold;
}
```
- 이 선택자는 부모의 자식 중 형제가 없는 자식이 없는 요소를 선택

<br>

## 선택자 - X:first-of-type
```css
<div>
  <p> My paragraph here. </p>
  <ul>
    <li> List Item 1 </li>
    <li> List Item 2 </li>
  </ul>
  <ul>
    <li> List Item 3 </li>
    <li> List Item 4 </li>
  </ul>
</div>
```
- 이 선택자는 부모의 자식 중 형제 자식 중 같은 타입이 없는 자식 요소를 선택

<br>

---

<br>

# CSS3 단위

## 크기 단위
- `%`: 백분율 단위
- `em`: 배수 단위
  - `em`: 배수의 기준이 상위 부모(크기 값이 설정되어 있어야함 아무 부모도 설정되어 있지 않다면 기본 16px을 기준으로 함)
  - `rem`: 배수의 기준이 root
- `px`: 픽셀 단위

<br>

## 색상 단위
- `#000000`: HEX 코드 단위
- `rgb(red, green, blue)`: RGB 색상단위
- `rgba(red, green, blue, alpha)`: RGBA 색상단위
- `hsl(hue, saturation, lightness)`: HSL 색상단위
- `hsla(hue, saturation, lightness, alpha)`: HSLA 색생단위

<br>

## URL 단위
이미지나 파일을 불러올 때는 URL 단위를 사용한다.

<br>

---

<br>

# 가시 속성

## display 속성
- `none`: 태그를 화면에서 보이지 않게 만든다.
- `block`: 태그를 block 형식으로 지정한다.
- `inline`: 태그를 inline 형식으로 지정한다.
- `inline-block`: 태그를 inline-block형식으로 지정한다.

<br>

## opacity 속성
- `opacity` 속성은 태그의 투명도를 조절하는 스타일 속성으로 `0.0`부터 `1.0`사이의 숫자로 투명도를 조절한다.

<br>

## 박스 속성
<img width="449" alt="스크린샷 2024-03-28 15 36 08" src="https://github.com/hamsangjin/TIL/assets/103736614/c7bd4916-d6f6-4b18-9d94-bb11f9de7318">

- `width`: 박스의 너비
- `height`: 박스의 높이
- `margin`: `margin` 영역의 크기를 의미하며, `border` 바깥쪽에 다른 요소들과의 공간을 말한다.
- `padding`: `padding` 영역의 크기를 의미하며, 실제 박스 크기`(width*height)`와 `border` 사이의 공간을 말한다.
- `border-width`: 테두리의 너비를 지정하는 스타일 속성 / 크기 단위의 키워드 사용
- `border-style`: 테두리의 형태를 지정하는 속성
