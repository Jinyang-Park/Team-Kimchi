# Team Kimchi

![](/public/images/common/mainpage.PNG)

다양한 김치를 편리하게 구매할 수 있는 서비스를 만들었습니다.
사용자들에게 다양한 김치 카테고리를 손 쉽게 김치를 살 수 있게 서비스를 제공 하였고 상품설명, 레시피 그리고 후기를 남길 수 있는 페이지들을 각각 제공해서 각 제품의 특성을 파악 할 수 있고 후기를 통해 정확한 정보를 얻을 수 있게 해주는 사이트입니다.

[배포링크](https://team-kimchi.vercel.app/)

[시연영상](https://www.youtube.com/watch?v=D8fl06j4UJI)
<br />

# 👥팀원 구성

| 이름   | 깃허브 주소                     |
| ------ | ------------------------------- |
| 이학경 | https://github.com/suwoncityBoy |
| 성효진 | https://github.com/su-no        |
| 박진양 | https://github.com/Jinyang-Park |
| 김우상 | https://github.com/freesian12   |
| 유승민 | https://github.com/dnjfht       |

<br />

# 🤝우리팀 규칙 (코드컨벤션)

### git commit Message Convention

| feat     | 새로운 기능과 관련된 것을 의미한다.                                                              |
| -------- | ------------------------------------------------------------------------------------------------ |
| fix      | 오류와 같은 것을 수정했을 때 사용한다.                                                           |
| docs     | 문서와 관련하여 수정한 부분이 있을 때 사용한다.                                                  |
| style    | 코드의 변화와 관련없는 포맷이나 세미콜론을 놓친 것과 같은 부분들을 의미한다.                     |
| refactor | 코드의 리팩토링을 의미한다.                                                                      |
| test     | test를 추가하거나 수정했을 때를 의미한다.                                                        |
| chore    | build와 관련된 부분, 패키지 매니저 설정 등 여러가지 production code와 무관한 부분 들을 의미한다. |
| design   | css 등 스타일에 관련된 것을 의미한다.                                                            |

<br />

### github flow

하나의 메인 브랜치인 `main` 중점으로 운용하며 PR을 활용한다.
`main` 브랜치는 항상 최신 버전을 유지하며 안정적이어야함.

- 기능 구현을 완료하면 main 브랜치로 `Pull Request` 요청
- `Vercel`의 Countinuous Deploy를 활용해 Preview 사이트 테스트
- 팀원 1명 이상 리뷰 후 승인, Merge
- 각자의 브랜치에서 Pull
- 총 Commit 220개, PR 67개

### naming Convention

- camelcase로 변수 ,함수 사용

### css 초기화

- 에릭마이어의 reset.css를 통해 크로스브라우징작업

### prettier 포멧팅 설정

```jsx
{
  "arrowParens": "always",
  "bracketSpacing": true,
  "endOfLine": "auto",
  "htmlWhitespaceSensitivity": "css",
  "jsxBracketSameLine": false,
  "jsxSingleQuote": false,
  "printWidth": 80,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false
}
```

<br />

# 🔧기술스택

### Client

`react` , `react-router` , `redux` , `redux-toolkit` , `axios` , `styled-component`

### Server

`json-server`

### deploy

`glitch`, `vercel`

<br />

# 📁폴더구조

![](/public/images/common/directory.png.png)

<br />

# 👨‍💻구현기능

- Mainpage
  - 카테고리별 김치 목록 슬라이더 형태로 구성
  - 메인페이지 제품 노출
  - nav바에 라우터 구성
- category
  - 카테고리 별 제품 목록 노출
- product
  - 해당 제품의 정보 노출
  - 버튼형태로 3개의 라우터 구성
    - detail
      - 해당 제품의 자세한 설명 노출
    - recipe
      - 해당 제품의 레시피 노출
    - review
      - 리뷰 입력, 수정 , 삭제
  - 추천 제품 목록 슬라이더 형태로 구성
- cart
  - 장바구니로 담은 제품목록 구성
  - 합계 노출

<br />

# 🤷트러블슈팅

### 장바구니 페이지로 이동 시 스크롤 최상단으로 이동할때

**기존 방식**

⇒ window.location.reaload()로 인해서 전체 페이지가 새로고침 되면서 재렌더링 됨.

```jsx
// components/Kimchi.jsx

<Icon
  onClick={() => {
    navigate('/cart');
    window.location.reload();
  }}
>
```

**변경 방식**

특정 페이지 component 내부에서 scrollTo 메소드 사용

⇒ 새로고침을 하지 않고 스크롤을 이동할 수 있음.

```jsx
// pages/Cart.jsx

export default function Cart() {
  useEffect(() => {
    // 페이지 이동 시 최상단으로 스크롤
    window.scrollTo(0, 0);
  }, []);

  return (...)
}
```

<br />

### 상품 추천 (참조 타입과 setState)

**기존 방식**

➡ 받아온 데이터 랜덤으로 뽑아내는 과정에서 상품의 name, price, image가 빈 배열로 출력되었다.

```jsx
// 상품 추천 랜덤 함수
const Randomkimchi = [];

let allKimchis = data;
for (let i = 0; i < 4; i++) {
  let randomNum = Math.floor(Math.random() * allKimchis.length);
  Randomkimchi.push(allKimchis[randomNum]);
  allKimchis.splice(randomNum, 1);
}
console.log(Randomkimchi);
```

**문제점**

➡ 리액트는 배열이나 객체 데이터의 변화를 감지하여 정상 작동하도록 한다.

`push` 참조 자료형의 가변 내장 함수를 사용하면 내부의 데이터는 변하더라도 그 주소값은 그대로 이다. 이것은 `데이터 무결성 제약 조건 위배` 에 해당하여 문제를 발생시킬 여지를 남긴다.

무결성을 유지하려면 기존 데이터를 수정하는 가변 내장 함수(push, unshift, pop, splice)를 사용하면 안된다.

**해결 방법**

➡ 새로운 값을 기존 값에 push를 해주고 리액트가 변화를 감지할 수 있도록 새로운 배열을 만들어 state가 변경되었음을 인지하도록 해주었다.

```jsx
// 상품 추천 랜덤 함수
    const Randomkimchi = [];

    let allKimchis = data;
    for (let i = 0; i < 4; i++) {
      let randomNum = Math.floor(Math.random() * allKimchis.length);
      Randomkimchi.push(allKimchis[randomNum]);
      console.log(Randomkimchi);
      allKimchis.splice(randomNum, 1);
      console.log(allKimchis);
    }
    setRecommendedProduct(Randomkimchi);
  };
```

<br />

### 📄프로젝트 노션

[프로젝트 노션 정리](https://flashy-rhubarb-f0d.notion.site/B-4-Kimchi-0cbe7b4544114a9f892c5c60a76742a6)
