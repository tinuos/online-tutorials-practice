# 드롭다운 메뉴 예제

## 요소 구조 및 스타일 처리 내용 파악하기

```
- navigation
  - userbx
    - imgBx
      - img
  - menuToggle
  - menu
```

- 전체 네비게이션 박스 안에 프로필, 메뉴 토글 버튼, 메뉴가 있는 형태
  - 프로필에선 사진 이미지만 보여주다 토글 동작이 발생하면 사용자 이름이 같이 보여진다.
  - 메뉴는 보이지 않다가 토글 동작이 발생하면 높이가 생기면서 내부 콘텐츠가 보여진다.
  - 2가지 모두 navigation의 너비나 높이에 영향을 받는다.
  - toggle 동작의 스타일 처리는 자바스크립트를 이용하여 `.active` 클래스를 navigation에 부여

## 스타일 속성 이해하기

### calc(expression)

- 표현식의 결과값을 css 속성 값으로 사용할 수 있는 css 함수
- 단순 계산식은 무엇이든 가능. 표준 연산자 우선순위를 따른다.
- 위 예제에선 토글이 동작하여 전체 컨테이너 크기가 변할 때 내부 요소의 너비를 계산하기 위해 사용되었다. 예제 코드를 살펴보며 이해해보자.

```css
/* User box */
.navigation .userbx {
  position: relative;
  width: 60px;
  height: 60px;
  background-color: #fff;
  display: flex;
  align-items: center;
  overflow: hidden;
  transition: 0.5s;
  transition-delay: 0.5s;
}
.navigation.active .userbx {
  width: calc(100% - 60px);
}
```

- 전체 컨테이너인 `.navigation`은 토글 동작(`.navigation.active`) 시  너비가 120px 에서 300px 로 늘어난다.
- 단순히 너비가 늘어날 때 토글 버튼 너비를 뺀 값을 지정할 수도 있지만 그렇게 하지 않고 위 예제와 같이 늘어나는 전체 너비에서 토글 너비만 뺀 결과값을 속성으로 지정한다.
- 이렇게 작성하는 이유는 유지보수를 쉽게 하기 위함인 것 같다. 상대적인 속성값(여기선 100%)을 이용하여 자동으로 계산이 되기 때문에, 추후에 토글 시 컨테이너의 너비값이 바뀐다 하더라도 토글 메뉴버튼 너비를 제외한 전체 너비를 가져갈 수 있게 되는 것.
- 이러한 방식은 드롭다운 메뉴의 높이 계산에도 똑같이 사용되었다.

```css
.menu {
  position: absolute;
  width: 100%;
  height: calc(100% - 60px);
  margin-top: 60px;
  padding: 20px;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}
```

### object-fit

- img나 video 같은 [대체 요소](https://developer.mozilla.org/ko/docs/Web/CSS/Replaced_element)가 갖는 콘텐츠의 크기를 조절하기 위해 사용하는 속성
- 처음보는 속성이라 많이 당황했는데, cover라는 값이 쓰이는 것을 보고 background의 size 속성에서 사용하는 것과 유사한 것임을 알게 되었다.
- 예제에서는 프로필 이미지의 원본 크기의 가로 세로비를 유지하기 위해 사용

```css
.navigation .userbx .imgBx img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

- object-fit과는 관련없는 내용이지만, imgBx 크기(min-width: 60px / height: 60px)에 맞추기 위해 position: absolute와 top, left를 0으로 준 것을 확인할 수 있다. (img 요소는 기본적으로 인라인 요소. 텍스트처럼 배치되는 것(baseline)을 회피하기 위해)