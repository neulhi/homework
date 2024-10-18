# apple 제품 카드

## 목차

1. [코드 설명](#코드-설명)
1. [회고](#회고)
1. [Apple layout(grid) 반응형 구현 배포 링크](https://neulhi.github.io/homework/login/apple.html "Apple layout(grid) 반응형 구현")

<br />

## 코드 설명

> 1. 카드 컴포넌트를 먼저 작업후 세부적으로 클래스를 추가하는 방식으로 작업했다.  
> 1. `car-container-1 , 1fr`, `car-container-2 , 2fr` 으로 반응형 작업용  
> 1. `bg-dark / 어두운 배경 이미지` , `bg-white / 밝은 배경 이미지` 글자색, 버튼 스타일 변경용  
> 1. `card-*` 배경 이미지용  
> 1. `<span>부제목</span>` 텍스트 스타일 변경용

### `HTML`

```html
<main class="main">
  <div class="card-container-1">
    <div class="card-content">
      <div class="card__item card-1 bg-dark">
        <h2 class="card__headline">제목</h2>
        <p class="card__subhead">부제목</p>
        <p class="card__callout">출시일 추후 공개</p>
        <div class="card__link">
          <a href="" target="_blank" rel="noopener noreferrer" aria-label="제목 제품 더 알아보기">더 알아보기</a>
          <a href="" target="_blank" rel="noopener noreferrer" aria-label="제목 제품 가격 보기">가격 보기</a>
        </div>
      </div>
    </div>
  </div>

  <div class="card-container-2">
    <div class="card-content">
      <div class="card__item card-2 bg-white">
        <h2 class="card__headline">제목</h2>
        <p class="card__subhead">
          <span>부제목</span>
        </p>
        <div class="card__link">
          <a href="" target="_blank" rel="noopener noreferrer" aria-label="제목 제품 더 알아보기">더 알아보기</a>
          <a href="" target="_blank" rel="noopener noreferrer" aria-label="제목 제품 가격 보기">가격 보기</a>
        </div>
      </div>
    </div>
  </div>
</main>
```

<br>

## `CSS`
>	1. theme.css 파일을 @import 했지만 .main{}에 지역변수로 다시 지정하여 사용했다. (같은 페이지에 코드가 없어 자동완성이 안되는 불편함도 있었음)  
>	1. 반응형은 grid로 컴포넌트 안의 내용은 flex를 사용하여 스타일링  
>	1. image-set()을 사용해 픽셀밀도 별로 배경이미지 변경  

```css
background: image-set(url(./../../assets/products/ipad_pro.jpeg) 1x, url(./../../assets/products/ipad_pro_2x.jpeg) 2x) no-repeat center;
background-size: cover;
```

<br>

## `회고`
컴포넌트 단위로 생각하고 세분화해서 작업하니 작업이 편했지만 css 스타일링을 하다가 마크업을 수정하는 경우가 종종 생겼다.  
(요구사항이나 문제를 세분화해서 보려고 시도를 계속 해봐야 겠다.)    
