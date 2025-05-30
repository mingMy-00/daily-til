# 코딩 가이드

개인 프로젝트에서 사용하는 코딩 표준과 컨벤션 정리

## JavaScript 스타일 가이드

### 변수 선언
- `const`를 우선 사용
- 재할당이 필요한 경우만 `let` 사용
- `var` 사용 금지

```javascript
// Good
const userName = 'John';
let userAge = 25;

// Bad
var userName = 'John';
```

### 함수 작성
- Arrow function 우선 사용
- 명확한 함수명 사용

```javascript
// Good
const calculateTotal = (price, tax) => price + (price * tax);

// Acceptable
function calculateTotal(price, tax) {
  return price + (price * tax);
}
```

## CSS 스타일 가이드

### 클래스 네이밍
- BEM 방법론 사용
- kebab-case 사용

```css
/* Good */
.button--primary {}
.card__header {}

/* Bad */
.buttonPrimary {}
.cardHeader {}
``` 