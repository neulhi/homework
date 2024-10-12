# Login-form layout 구현 과제

## 목차

1. [중요 조건](#중요-조건)
1. [코드 설명](#코드-설명)
1. [회고](#회고)
1. [login-form layout 구현 배포 링크](https://neulhi.github.io/homework/login/login.html "login-form layout 구현")

<br />

## 중요 조건

접근성, 성능은 기본이지만 수업에서 다루지 않았던 부분은 어떻게 구현해야 할까  
어려운 문제로 다가왔기 때문에 해결해 보고 싶었다.

다음은 중요하게 생각한 조건들이다.

- `마크업`

  > 1.  **웹접근성**을 고려하여 로그인 폼 서식을 마크업 할 것
  >     (레이블 제공의 경우 WAI-ARIA가 아닌 **HTML 네이티브 방식**으로 구현)
  > 1.  아이디와 비밀번호는 **필수 입력 서식**임을 알 수 있도록 구현할 것
  > 1.  로그인 상태 유지와 IP 보안 ON/OFF UI는 **마우스 이외에 키보드로도 조작 가능**하도록 구현할 것

- `스타일링`
  > 1. 미디어 쿼리를 사용하여 **반응형**으로 구현할 것
  >    (768px 미만 모바일 / 768px 이상 데스크탑)
  > 1. **모바일 퍼스트**로 스타일링 할 것  
  >    (공통 스타일과 모바일 스타일을 먼저 구현한 후 데스크탑 스타일을 재정의)

<br />

## 코드 설명

> 화면 중앙 배치를 위해 전체를 한번 감싸고 로고(h1), 폼(form)으로 나누었다.

```html
<div class="login-wrapper">
  <h1></h1>
  <form></form>
</div>
```

<br>

### `heading 로고` 부분은 `h1` 안에

---

> `<svg>`요소에 속성을 추가하여 사용했다. 1. `role="img"` 2. `aria-labelledby="logo"` 3. `<title id="logo">네이버</title>`

```html
<h1 class="brand">
  <a href="https://www.naver.com/" class="logo">
    <svg role="img" width="230" height="44" viewBox="0 0 230 44" fill="none" xmlns="http://www.w3.org/2000/svg" aria-labelledby="logo">
      <title id="logo">네이버</title>
      <path d="" fill="#" />
    </svg>
  </a>
</h1>
```

<br>

## `form`

> - 3개의 컴포넌트로 보았다.
>
> 1.  아이디, 비밀번호 입력 칸
> 1.  로그인 버튼
> 1.  체크 박스 요소

<br>

**1. 아이디, 비빌번호 입력 칸**

조건 만족을 위해 input 속성 `required(필수), minlength="10"(최소 10자리 이상)`  
아래쪽에 추가 에러메세지를 구현하기 위해 `span요소`를 사용했다.  
검색을 하다 `input:invalid:not(:placeholder-shown)`을 알게되서  
기본 상태를 css로 `disply: none;` 조건에 유효하지 않았을 때 `display: inline;` 처리해서
추가 에러메세지를 구현!!!!!  
로고부터 차례대로 구현했는데 시작부터 엄청난 삽질과... 가장 많은 시간을 투자했다😵‍💫

```html
<label for="login-email" class="login__label sr-only">아이디</label>
<input type="email" class="login__input" placeholder="아이디" required name="email" id="login-email" />
<span class="error-message">아이디는 이메일을 입력해 주세요</span>

<label for="login-password" class="password__label sr-only">비빌번호</label>
<input type="password" class="password__input" placeholder="비밀번호" required name="password" id="login-password" minlength="10" />
<span class="error-message">비밀번호는 10자리 이상 입력해 주세요</span>
```

```css
/* 에러 메시지 스타일 */
.error-message {
  display: none;
  color: red;
  font-size: 0.9em;
}

/* 입력 값이 유효하지 않고, 값이 입력된 상태일 때만 에러 메시지 표시 */
input:invalid:not(:placeholder-shown) + .error-message {
  display: inline;
}
```

<br>

**2. 로그인 버튼**

> `type="submit"` 사용  
>  css에서 버튼에 `all: unset;` 적용하니 키보드로 이동했을 때 테두리까지 보이지 않아서  
>  `&:focus { border: 2px solid var(--color-black);}` 테두리를 추가 했다.

```html
<button type="submit" class="button-login">로그인</button>
```

```css
.button-login {
  all: unset;
  font-size: var(--font-size-basic);
  text-align: center;
  block-size: 45px;
  color: var(--color-white);
  background-color: var(--color-green);
  margin-block-start: 1.25rem;
  cursor: pointer;

  &:focus {
    border: 2px solid var(--color-black);
  }
}
```

<br>

**3. 체크 박스 요소**

> 배치를 위해 `<div class="keep-wrapper">`로 체크 박스 요소 전체를 한번 감싸고  
> `<div class="keep-check">`로그인 상태유지  
> `<div class="ip-input">`IP 보안 각각 배치를 위해 사용했다.
> css에서 체크박스 기본 브라우저 스타일링을  
> `[type="checkbox"] {appearance: none;}`로 없애고  
> `:checked` , `::after` , `::before`를 사용해 스타일링 했다.

```html
<div class="keep-wrapper">
  <div class="keep-check">
    <input type="checkbox" name="keep-input" id="keep-input" />
    <label for="keep-input" class="keep-text"><span>로그인 상태 유지</span></label>
  </div>
  <div class="ip-input">
    <a href="./pages/ip_security.html" target="_blank">IP 보안</a>
    <input type="checkbox" name="ip-input" id="ip-input" />
    <label for="ip-input" class="ip-text" aria-label="IP 보안 온오프 체크"></label>
  </div>
</div>
```

```css
/* 체크 박스 요소 */
.keep-wrapper {
  input {
    margin: 0;
  }

  .keep-check {
    position: relative;
    display: flex;
    justify-content: flex-end;
    align-items: center;

    #keep-input,
    .keep-text {
      cursor: pointer;
      vertical-align: middle;
    }

    [type="checkbox"] {
      appearance: none;
      inline-size: 24px;
      block-size: 24px;
      margin-inline-end: 0.3125rem;
      background-image: url(../../assets/login/unchecked.svg);
    }

    [type="checkbox"]:checked {
      background-image: url(../../assets/login/checked.svg);
    }
  }

  .ip-input {
    display: none;
  }

  @media (min-width: 768px) {
    & {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .keep-check {
      display: unset;
    }

    .ip-input {
      display: flex;
      align-items: center;

      a {
        padding-inline-end: 0.5rem;
      }

      [type="checkbox"] {
        appearance: none;
      }

      /* 과제 IP보안 UI(텍스트) - 직접 구현 */
      /* [type="checkbox"]::before {
        content: "OFF";
		font-weight: 700;
		color: var(--color-dark-gray);
      }

      [type="checkbox"]:checked::after {
        content: "ON";
        color: var(--color-green);
		font-weight: 700;
      }

	  [type="checkbox"]:checked::before {
		content: none;
	  } */

      /* 현재 네이버 IP보안 UI(토글) - 검색해서 단위만 수정하여 구현 */
      label {
        display: block;
        position: relative;
        inline-size: 48px;
        block-size: 20px;
        background: var(--color-gray);
        border-radius: 60px;
        transition: background 0.4s;
        cursor: pointer;
      }

      label::after {
        content: "";
        position: absolute;
        inset-inline-start: 4px;
        inset-block-start: 50%;
        inline-size: 14px;
        block-size: 14px;
        border-radius: 100%;
        background-color: var(--color-white);
        transform: translateY(-50%);
        box-shadow: 1px 3px 4px rgba(0, 0, 0, 0.1);
        transition: all 0.4s;
      }

      label::before {
        content: "OFF";
        font-size: 11px;
        font-weight: bold;
        position: absolute;
        inset-inline-start: 20px;
        inset-block-start: 50%;
        transform: translateY(-50%);
        transition: all 0.4s;
      }

      #ip-input:checked + label {
        background: var(--color-green);
      }

      #ip-input:checked + label::after {
        inset-inline-start: 30px;
      }

      #ip-input:checked + label::before {
        content: "ON";
        color: var(--color-white);
        inset-inline-start: 8px;
      }

      #ip-input:checked + label:focus {
        border: 2px solid var(--color-black);
      }
    }
  }
}
```

<br />

## `회고`

1. 모바일 퍼스트의 장점은 모바일에서 먼저 스타일을 정의하기 때문에 작은 기기에서는 불필요한 스타일을 로드하지 않아 성능이 향상된다는 것을 새로 알게 되었다.

1. IP보안 온오프 UI가 사용자가 클릭해 보지 않으면 알기 힘들어서 현재 네이버는 토글 형태로 바꾼 것 같아 검색해서 비슷하게 구현했지만 네이버처럼 키보드로 이동했을 때 작동은 하지만 테두리가 보이지 않았다. 이유는 검색을 해도 찾지 못했다.🤔

1. css를 잘 쓰면 반응형을 구현할 때 @media를 많이 쓰지 않아도 될 것 같다.

1. 네이밍은 길지 않고 직관적인 게 가장 좋은 것 같다.  
   (그래서 생각보다 시간이 많이 투자된다)

1. 구현하고 마크다운 작성하면서 회고하고 완성하는데 하루종일 걸렸다..............................😂
