# 네비게이션 아이콘 표시 메뉴 예제

- [예제 영상 링크](https://www.youtube.com/watch?v=ArTVfdHOB-M&list=PL5e68lK9hEzc8P9BJCSX1k9C8uKAV5Oa5&index=3&t=1s)

---

## UI 개요

- 모바일에서 흔히 볼 수 있는 하단 네비게이션 메뉴를 구현
  - 단 현재 선택된 메뉴 아이콘 원형 컨테이너에 떠 있는 식이며 메뉴 이동이 발생할 때마다 자연스럽게 이동하며 아이콘이 바뀐다.
- 선택된 아이콘 바로 아래 메뉴바는 곡선 처리가 되어야 한다.

---

## 핵심 내용

**네비게이션 메뉴 HTML 코드 내용**

```html
~
  <div class="navigation">
    <ul>
      <li class="list active">
        <a href="#">
          <span class="icon"><ion-icon name="home-outline"></ion-icon></span>
          <span class="text">Home</span>
        </a>
      </li>
      <li class="list">
        <a href="#">
          <span class="icon"><ion-icon name="person-outline"></ion-icon></span>
          <span class="text">Profile</span>
        </a>
      </li>
      <li class="list">
        <a href="#">
          <span class="icon"><ion-icon name="chatbubble-outline"></ion-icon></span>
          <span class="text">Message</span>
        </a>
      </li>
      <li class="list">
        <a href="#">
          <span class="icon"><ion-icon name="camera-outline"></ion-icon></span>
          <span class="text">Photos</span>
        </a>
      </li>
      <li class="list">
        <a href="#">
          <span class="icon"><ion-icon name="settings-outline"></ion-icon></span>
          <span class="text">Settings</span>
        </a>
      </li>
      <div class="indicator"></div>
    </ul>
  </div>
~
```

### 아이콘 표현용 요소 (indicator) 배치

- 메뉴 요소인 ul의 맨 아래 indicator 클래스를 가지는 요소가 이번 UI 예제의 핵심

```css
~
.indicator {
  position: absolute;
  top: -50%;
  width: 70px;
  height: 70px;
  background: #29fd53;
  border-radius: 50%;
  border: 6px solid var(--clr);
  transition: 0.5s;
}
.indicator::before {
  content: '';
  position: absolute;
  top: 50%;
  left: -22px;
  width: 20px;
  height: 20px;
  background: transparent;
  border-top-right-radius: 20px;
  box-shadow: 1px -10px 0 0 var(--clr);
}
.indicator::after {
  content: '';
  position: absolute;
  top: 50%;
  right: -22px;
  width: 20px;
  height: 20px;
  background: transparent;
  border-top-left-radius: 20px;
  box-shadow: -1px -10px 0 0 var(--clr);
}

.navigation ul li:nth-child(1).active ~ .indicator {
  transform: translateX(calc(70px * 0));
}
.navigation ul li:nth-child(2).active ~ .indicator {
  transform: translateX(calc(70px * 1));
}
.navigation ul li:nth-child(3).active ~ .indicator {
  transform: translateX(calc(70px * 2));
}
.navigation ul li:nth-child(4).active ~ .indicator {
  transform: translateX(calc(70px * 3));
}
.navigation ul li:nth-child(5).active ~ .indicator {
  transform: translateX(calc(70px * 4));
}
```

- 해당 요소의 border와 가상 요소 before와 after의 box-shdow 속성을 이용하여 곡선 형태를 표현한다. 배경색과 동일한 색을 부여하여 자연스러운 곡선이 되는 것처럼 보이는 것. 여기서 사용된 외곽선의 색이나 그림자 색은 모두 root 에 정의한 사용자 속성값이 사용되도록 var로 설정되었고, 이에 따라 배경색의 변화가 있더라도 해당 곡선 모양은 똑같이 유지될 수 있다. (참고로 해당 색상은 아이콘 및 메뉴 텍스트에도 같이 사용된다) root에 대한 내용은 아래에 따로 정리한다.
- UI 기능은 자바스크립트를 이용하여 active라는 클래스를 부여하거나 제거함으로써 동작하며, 자연스러운 처리를 위해 웬만한 스타일 규칙에는 transtion과 transform 이 설정되어 있다. 또한 active가 부여된 항목으로 원형 아이콘의 이동을 제어하기 위하여 일반 형제(~) 선택자가 사용된 것을 볼 수 있다.
  - 일반 형제 선택자의 특징은 첫 번째 요소를 뒤따르면서 같은 부모를 공유하는 두 번째 요소를 선택하는 것.
  - `.navigation ul li:nth-child(2).active ~ .indicator` 선택자를 예를 들어 이해해보자면 active가 부여된 2번째 항목부터 같은 부모(ul)을 공유하는 `.indicator` 요소를 선택하는 것(indicator 요소는 ul안 맨 마지막에 배치되어 있다)
  - 이로 인해 특정 항목이 클릭되어 active가 부여되면 위에 정의한 5가지 이동 처리용 스타일 규칙이 적용되어 각각 calc 함수로 계산된 위치만큼 이동하게 된다. (여기선 항목 너비가 70px 이기 때문에 70px * 0 부터 70px * 4까지 각각 계산한다)

---

## 이 외 내용

### `:root`

- 의사 클래스 중 하나
  - 의사 클래스란 요소가 특별한 상태를 가질 때 스타일을 제어할 수 있도록 선택자에 추가할 수 있는 키워드를 말한다.
  - 대표적인 예로 `:hover`를 들 수 있다.
- root 의사클래스는 `:root` 형태로 사용하며, 문서 트리에서 루트 요소를 선택한다. HTML에서 루트 요소는 `<html>` 요소.
- 그럼 그냥 html 태그 선택자를 사용하면 되지 않나???
  - mdn에서는 root 의사 클래스가 명시도가 더 높다고만 설명
  - 구글링을 해보니 css는 html에만 사용되는 것이 아니라 xml, svg 등에도 사용될 수 있다.
  - xml, svg 등에 대한 자세한 내용은 모르지만, root 의사 클래스로 특정 문서의 루트 요소에 스타일 제어가 가능하기 때문에 사용하는 것으로 이해했다.
  - 즉, `:root`를 통해 문서의 루트 요소(최상위 요소)에 미리 스타일 부여가 가능한 것. 이러한 점 때문에 전체 스타일에서 공통으로 사용할 전역 속성 등을 설정하기 위해 많이 활용하는 것 같다.
  - 위 예제에서도 배경 및 텍스트 컬러로 사용되는 `--cls` 라는 속성을 `:root`에 부여해놓고 각각의 스타일 규칙에서 var를 이용하여 적용시킨 것을 볼 수 있다. 이렇게 작성하면 나중에 색 변경을 할 때 `:root` 에 있는 값만 변경하면 전체가 바뀌기 때문에 유지보수가 쉬워진다.

### line-height를 이용한 요소 크기제어

![image](https://user-images.githubusercontent.com/104971437/184835901-2f5b7265-e6d9-4746-8f90-7bc6d07d3f38.png)

- line-height는 의미 그대로 콘텐츠의 줄높이를 설정할 때 사용하는 속성
- 라인을 가지는 박스에서 줄높이가 설정된다.
- 이 예제에선 대부분 flex를 이용하여 중앙 정렬을 한다. 아이콘 용 span(.icon)도 block으로 바꿔서 배치한다.
- span(.icon)의 line-height를 설정하여 내부의 아이콘이 메뉴 높이와 거의 같게 높이를 가질 수 있도록 하여 중앙에 배치되는 것처럼 보여진다.
- ionicon 사용 시 아이콘이 약간 위로 표시되는 부분이 있어 제작자가 전체 높이(70px)보다 5px을 더 부여한 것 같다.
- 아이콘 아래 텍스트의 경우 position이 absolute이기 때문에 line-height와는 별개로 동작한다. a의 flex 정렬 영향을 받은 초기 위치에서 translateY와 opacity 변화를 이용하여 나타났다 사라지는 효과를 보여준다.

---

**참고 자료**

- [MDN - :root](https://developer.mozilla.org/ko/docs/Web/CSS/:root)

