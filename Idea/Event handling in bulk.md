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