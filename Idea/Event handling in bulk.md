```js
document.querySelector("#app").innerHTML = `
    <button class="btn-add-card" type="button">Add card</button>
  
    <div class="cards"></div>
  `;

  let cardCount = 0;
  // const cards = [

  // ]

  document.querySelector(".btn-add-card").addEventListener("click", () => {
    cardCount += 1;
    const card = document.createElement("div");
    card.className = "card";
    // card.setAttribute("data-number", cardCount)
    card.innerHTML = `
      <p>Card #${cardCount}</p>
      <button class="btn-hello" type="button" data-number="${cardCount}">hello</button>
    `;
    const myCardCount = cardCount;
    // card.querySelector(".btn-hello").addEventListener("click", () => {
    //   console.log(`💡 hello! ${myCardCount}`);
    // });
    document.querySelector(".cards").appendChild(card);
  });

  document.querySelector(".cards").addEventListener("click", (event) => {
    // console.log("click from .cards", event);
    const maybeButton = event.target;
    if (maybeButton.matches(".btn-hello")) {
      // const cardName = maybeButton.parentElement.children[0].innerText;
      // const cardNumber = parseInt(cardName.split(" ")[1].slice(1), 10);
      // console.log("button is clicked!", cardNumber);
      console.log(
        "button is clicked!",
        maybeButton.getAttribute("data-number")
      );
    } else {
      console.log("something else. let's ignore this.");
    }
  });
```

처음 시도한 방법은 컴포넌트를 생성하는 동시에 이벤트 리스터를 부착하였다.

두 번쨰 방법은 `event.target.match()`를 이용하여 원하는 요소를 찾아 함수가 실행될 수 있게 하였다.

> Chrome 콘솔에서 태그를 클릭하고 `$0`을 입력하면 임시 변수에 태그를 담을 수 있다.

제대로된 `state`를 관리하는 것이 아닌 `innerHTML`을 이용하는 것은 안정적인 방법이 아니다.

제대로 된 접근 방법은 `data attribute`를 이용하는 것이다.

`data attribute`를 통해 상태를 입력하고 그것을 `getAttribute`를 통해 얻어 사용할 수 있다.