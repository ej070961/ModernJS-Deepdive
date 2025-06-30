# ⏱️ 타이머와 호출 스케줄링

> 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 실행을 예약하는 메커니즘을 **호출 스케줄링**이라 부른다.

## 호출 스케줄링

JavaScript는 `setTimeout`, `setInterval`, `clearTimeout`, `clearInterval` 네 가지 함수를 통해 호출 스케줄링을 제공한다.

### 1. `setTimeout(callback, delay)`

- **동작**: `delay`(밀리초)만큼 대기한 뒤 `callback`을 **한 번만** 실행
- **반환값**: 타이머 ID (숫자)

```js
const id = setTimeout(() => {
  console.log("1초 후 한 번만 실행");
}, 1000);
```

### 2. `setInterval(callback, interval)`

- **동작**: `interval`(밀리초)마다 `callback`을 **반복 실행**
- **반환값**: 타이머 ID (숫자)

```js
const id = setInterval(() => {
  console.log("0.5초마다 반복 실행");
}, 500);
```

### 3. `clearTimeout(id)` / `clearInterval(id)`

- **동작**: 각각 `setTimeout`/`setInterval`으로 생성된 타이머를 취소

```js
clearTimeout(id); // setTimeout 취소
clearInterval(id); // setInterval 취소
```

> 💡 브라우저와 Node.js 환경에서 모두 전역 객체 메서드로 제공하는 “호스트 함수”다.

<br/>

## 🚀 디바운스(Debounce)

- **정의**: 연속해서 발생하는 이벤트를 “마지막 이벤트 이후 일정 시간 경과” 시점에 한 번만 처리
- **용도**: 사용자 입력(input), resize, keyup 등 잦은 이벤트 처리 최적화

### 유틸 함수 예시

```js
function debounce(fn, delay) {
  let timerId;
  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

### 사용 예: 검색어 자동완성

```html
<input id="search" placeholder="검색어 입력…" />
<script>
  const input = document.getElementById("search");
  input.addEventListener(
    "input",
    debounce((e) => {
      fetch(`/api/search?q=${e.target.value}`)
        .then((res) => res.json())
        .then((results) => console.log(results));
    }, 300)
  );
</script>
```

- 사용자가 입력을 멈추고 **300ms**가 지나면 한 번만 API 호출
- 불필요한 네트워크 요청 대폭 감소

<br/>

## ⚡️ 스로틀(Throttle)

- **정의**: 연속해서 발생하는 이벤트를 “일정 간격마다” 한 번만 처리
- **용도**: scroll, mousemove 같은 고빈도 이벤트 처리

### 유틸 함수 예시

```js
function throttle(fn, interval) {
  let lastTime = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      fn.apply(this, args);
    }
  };
}
```

### 사용 예: 스크롤 애니메이션 최적화

```js
window.addEventListener(
  "scroll",
  throttle(() => {
    const scrollY = window.scrollY;
    document.body.style.setProperty("--scroll-pos", scrollY);
    // heavy DOM 작업 최소화
  }, 200)
);
```

- 스크롤 이벤트가 계속 발생해도 **200ms 단위**로만 핸들러 실행
- 레이아웃·페인트 비용 절감

## ✔️ 요약

- **`setTimeout` / `setInterval`**: 콜백 실행을 예약하고, **clear** 계열로 취소 가능
- **디바운스**: “마지막” 이벤트 뒤 일정 시간이 지나면 한 번만 실행
- **스로틀**: “일정 간격”마다 최대 한 번만 실행
- 이 기법들을 활용하면 고빈도 이벤트 처리로 인한 성능 이슈를 효과적으로 해결할 수 있다.
