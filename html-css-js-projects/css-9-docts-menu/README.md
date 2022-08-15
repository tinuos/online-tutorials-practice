# 9개의 점을 이용한 아이콘 메뉴 예제

- [예제 영상 링크](https://www.youtube.com/watch?v=5OLDpdqdyWE&list=PL5e68lK9hEzc8P9BJCSX1k9C8uKAV5Oa5&index=2)

---

## UI 개요

- 영상 초기에 개발자(??? 아래부턴 그냥 제작자라 하겠다)는 축소된 메뉴 아이콘에 있는 점 형태의 도형이 커지며 개별 아이콘으로 변하는 몇몇 UI에 영감을 받았다고 소개하며 시작한다.
- 영상에서 다룰 UI는 메뉴 형태의 요소 클릭 시 9개의 점이 아이콘으로 변하는 형태이다.

---

## 핵심 내용

- 해당 UI 에서는 메뉴 구현용 요소(div.navigation) 클릭 시 크기 변화를 위한 스크립트와 아이콘용 스크립트(ionicons)를 제외하고 자바스크립트가 따로 사용되지 않았다.

### CSS의 사용자 지정 속성, var(), calc() 활용

```html
~
  <div class="navigation">
    <span style="--i:0;--x:-1;--y:0"><ion-icon name="camera-outline"></ion-icon></span>
    <span style="--i:1;--x:1;--y:0"><ion-icon name="diamond-outline"></ion-icon></span>
    <span style="--i:2;--x:0;--y:-1"><ion-icon name="chatbubble-outline"></ion-icon></span>
    <span style="--i:3;--x:0;--y:1"><ion-icon name="alarm-outline"></ion-icon></span>
    <span style="--i:4;--x:1;--y:1"><ion-icon name="game-controller-outline"></ion-icon></span>
    <span style="--i:5;--x:-1;--y:-1"><ion-icon name="moon-outline"></ion-icon></span>
    <span style="--i:6;--x:0;--y:0"><ion-icon name="notifications-outline"></ion-icon></span>
    <span style="--i:7;--x:-1;--y:1"><ion-icon name="water-outline"></ion-icon></span>
    <span style="--i:8;--x:1;--y:-1"><ion-icon name="time-outline"></ion-icon></span>
  </div>
~
```

- navigation 안의 span은 클릭되기 전 하얀색 점을 나타내기 위한 요소로, 총 9개를 사용하여 CSS를 통해 정사각형 컨테이너에 균형있게 배치된다.
  - 클릭 시엔 너비와 높이가 커지며 내부의 ion-icon 아이콘이 나타난다.
- 여기서 span의 인라인 스타일에 주목할 것.
  - 각각의 span 마다 `--i:n;--x:~;--y:~;`처럼 사용자 지정 속성이 부여된 것을 알 수 있다.
    - CSS에선 `--속성명`을 이용하여 사용자 지정 속성을 만들 수 있으며 속성값을 부여할 수 있다.

```css
~
.navigation span {
  position: absolute;
  width: 7px;
  height: 7px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #fff;
  border-radius: 50%;
  transform: translate(calc(12px * var(--x)), calc(12px * var(--y)));
  transition: transform 0.5s, width 0.5s, height 0.5s, background 0.5s;
  transition-delay: calc(0.1s * var(--i));
}
.navigation.active span {
  width: 45px;
  height: 45px;
  background: #333849;
  transform: translate(calc(60px * var(--x)), calc(60px * var(--y)));
}
~
```

- 위 스타일에서 알 수 있듯이 x와 y 속성은 컨테이너 안에서의 위치를 조정하기 위해 사용된다. translate를 이용하여 인라인 스타일 내에 이미 부여되어 있는 x값과 y값을 통해 calc() 함수를 이용하여 위치를 조정하며, 이 때 해당 값을 가져오기 위해 var()를 사용한다. (인라인 스타일이 외부 스타일 시트보다 우선 적용되기 때문에 요소에 이미 부여된 x나 y를 가져올 수 있는 것으로 이해)
  - 여기서 x, y 에 부여된 값들은 제작자가 구현하고자 하는 transition 효과 순서에 따라 부여된 것 (수직, 수평, 대각순으로 커지며 작아진다)

```css
~
.navigation {
  position: relative;
  width: 70px;
  height: 70px;
  background: #212532;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 0.5s;
  transition-delay: 0.8s;
}
.navigation.active {
  width: 200px;
  height: 200px;
  transition-delay: 0s;
}
~
```

- i 속성의 값은 transition-delay를 주기 위해 사용
  - 0부터 ~ 8까지 (총 9개) 값이 부여되어 있는데, 각자 자신의 i값에 따라 0부터 0.8초까지 지연되어 transition이 일어난다. (.navigation span 규칙에서)
- 클릭 시 총 0.8 초가 지연되기 때문에 다시 축소된 아이콘으로 돌아갈 때를 위하여 `.navigation` 에서도 transition-delay가 0.8s로 설정된 것을 확인할 수 있다. (0.8초를 기다렸다가 transition이 실행된다)

---

## 이 외 내용

### filter 속성과 drop-shadow

```
/* SVG 필터를 가리키는 URL */
filter: url("filters.svg#filter-id");

/* <filter-function> 값 예제 */
filter: blur(5px);
filter: brightness(0.4);
filter: contrast(200%);
filter: drop-shadow(16px 16px 20px blue);
filter: grayscale(50%);
filter: hue-rotate(90deg);
filter: invert(75%);
filter: opacity(25%);
filter: saturate(30%);
filter: sepia(60%);

/* 다중 값 */
filter: contrast(175%) brightness(3%);

/* 필터 없음 */
filter: none;

/* 전역 값 */
filter: inherit;
filter: initial;
filter: unset;
```

- 여기선 아이콘 호버 시 연두색으로 빛나게 하기 위한 효과를 주기 위해 사용. filter는 흐림 효과나 색상 변형 등을 요소에 적용하기 위한 속성이라 한다.
- 위 MDN 예제 구문처럼 SVG 필터를 참조하거나 filter 관련 함수를 사용한다. 여기선 drop-shdow란 필터 함수가 사용되었는데, 요소에 그림자 효과를 부여할 수 있다.

---

**참고 자료**

- [MDN - CSS filter](https://developer.mozilla.org/ko/docs/Web/CSS/filter)