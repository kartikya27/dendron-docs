---
id: tjdvsar7o3huru9pjyn3u1t
title: Scripts For Daily Use
desc: ""
updated: 1681133013340
created: 1681121816465
---

> Quantity Box for increase and decrease value

```html
<div
  class="cat-col product-card"
  title="click on image to see full details"
  data-product-id="{{ $product->srno }}"
>
  <div class="quantity-box">
    <label class="qty-width" style="width:15%">Qty.</label>
    <button class="decrease-product-count-btn">-</button>
    <span id="product-count">1</span>
    <button class="increase-product-count-btn">+</button>
  </div>
</div>
```

### Javascript

```js
const productCards = document.querySelectorAll(".product-card");
productCards.forEach((productCard) => {
  const increaseProductCountBtn = productCard.querySelector(
    ".increase-product-count-btn"
  );
  const decreaseProductCountBtn = productCard.querySelector(
    ".decrease-product-count-btn"
  );
  const productCountSpan = productCard.querySelector("#product-count");

  increaseProductCountBtn.addEventListener("click", () => {
    const currentCount = parseInt(productCountSpan.textContent);
    const newCount = currentCount + 1;
    if (newCount <= 25) {
      productCountSpan.textContent = newCount;
    }
  });

  decreaseProductCountBtn.addEventListener("click", () => {
    const currentCount = parseInt(productCountSpan.textContent);
    const newCount = Math.max(currentCount - 1, 1);
    productCountSpan.textContent = newCount;
  });
});
```

---
