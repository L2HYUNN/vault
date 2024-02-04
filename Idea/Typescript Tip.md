> [!note] Remember
> **Typescript**를 사용하는 이유는 정적 타입 시스템을 이용하여 런타임에 발생할 수 있는 에러를 사전에 방지하기 위함이다. 따라서 항상 **Type Safe**한 코드를 짜기 위해서 노력해야한다. 가능한 **Type**은 좁게 선언하는 것이 좋다. 

## Tips

1. **`enum` 보다 `union type` 사용하기**
	enum을 사용하고 컴파일하면 객체가 생성됨 -> 런타임에 영향을 준다.
	union을 이용하면 enum과 동일한 타입 자동 완성 기능을 사용할 수 있다.
	const enum을 사용하면 컴파일 후 객체가 생성되지 않는다.
	enum은 값 입력을 위해 Import 코드를 작성해야 하므로 union을 이용하는 것이 좋다.

2. **`index signature` 보다 `mapped type` 사용하기**
![[Pasted image 20240201212935.png]]

```ts
// index signature
const PRICE_MAP: { [fruit: string]: number } = {
  apple: 1000,
  banana: 1500,
  orange: 2000,
};

// mapped type
const PRICE_MAP: { [fruit in Fruit]: number } = {
  apple: 1000,
  banana: 1500,
  orange: 2000,
};
```
> [!tip] **mapped type**를 사용하면 가격 정보를 추가하지 않을 경우 실수를 방지할 수 없다.

3. **외부 패키지의 타입 치환하기**
	고차 함수 형태를 이용하여 조건을 만족하는 타입 함수를 만들 수 있다.
```ts
// useTypedSelector.ts
export default function useTypedSelector<R>(selector: (state: MyState) => R): R {
  return useSelector(selector);
}
```

4. **타입 가드**

**Tagged Union Types**
```ts
// Tagged Union Types

interface Webtoon {
  type: "webtoon";  title: string;
  episode: number;
  isFinish: boolean;
}

interface WebNovel {
  type: "webNovel";  title: string;
  episode: number;
  age: "all" | 12 | 15 | 19;
}

interface SF {
  type: "sf";  title: string;
  episode: number;
  price: 3000;
}

function printInfo(content: Webtoon | WebNovel | SF) {
  switch (content.type) {    
	case "webtoon":
      return content.isFinish; // ✅ OK - content: Webtoon
    case "webNovel":
      return content.age; // ✅ OK - content: WebNovel
    case "sf":
      return content.price; // ✅ OK - content: SF
  }
}
```

**Template Literal Types**
```ts
// Template Literal Types

interface Webtoon {
  type: `${string}Webtoon`; // Template Literal Types  title: string;
  isFinish: boolean;
}

interface WebNovel {
  type: `${string}Novel`; // Template Literal Types  title: string;
  age: "all" | 12 | 15 | 19;
}

function printInfo(content: Webtoon | WebNovel) {
  if (content.type === "KakaoWebtoon") {
    console.log(content.isFinish); // ✅ OK - content: Webtoon
  }
}
```


further more about `Template Literal Types`
```ts
type ContentType = "webtoon" | "novel" | "book";
type Status = "series" | "completed" | "rest";
type Content = `${Status}-${ContentType}`; // 👍
// "series-webtoon" | "completed-webtoon" | "rest-webtoon" | "series-novel" ...
```

```ts
type Content = `${Status}${Capitalize<ContentType>}`;
// "completedWebtoon" | "seriesNovel" | "restBook" ...
```

```ts
type Content<S1 extends string, S2 extends string> = `${S1}${Capitalize<S2>}`;

type C1 = Content<"season", "vod">; // "seasonVod"
```

## Reference
[타입스크립트 꿀팁](https://fe-developers.kakaoent.com/2021/211012-typescript-tip/#4-%ED%83%80%EC%9E%85-%EA%B0%80%EB%93%9C-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)
[Typescript - Union Type, Intersection Type, Etc.](https://fe-developers.kakaoent.com/2022/221124-typescript-tip/)