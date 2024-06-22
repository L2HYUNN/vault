```js
  document.querySelector("#app").innerHTML = `
	  <input />
	  <button>Click</button>
`;

  document.querySelector("button").addEventListener("click", () => {
    const currentValue = document.querySelector("input").value;

    document.querySelector("input").value = currentValue + "*";
  });

  let count = 0;
  setInterval(() => {
    count += 1;
    document.querySelector("#app").innerHTML = `
    <input />
    <button>Click</button>
    <p>count: ${count}</p>
  `;
  }, 5000);
```

위의 예시에서 innerHTML을 사용해서 태그 전체를 대체하고 있기 때문에 기존에 가지고 있던 state를 잃어버리는 문제가 있다. React에서는 변경된 부분만 반영해주기 때문에 이와 같은 문제가 발생하지 않는다.