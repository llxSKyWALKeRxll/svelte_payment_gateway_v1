<script>
  import { writable } from "svelte/store";

  let cart = [
    { id: 1, name: "Watch", price: 450, addedToCart: false },
    { id: 2, name: "Laptop", price: 67000, addedToCart: false },
    { id: 3, name: "Mobile", price: 16750, addedToCart: false },
    { id: 4, name: "Bike", price: 275000, addedToCart: false },
    { id: 5, name: "Car", price: 1800000, addedToCart: false },
    { id: 6, name: "Meal", price: 250, addedToCart: false },
    { id: 7, name: "Shoes", price: 1500, addedToCart: false },
    { id: 8, name: "Movie Ticket", price: 350, addedToCart: false },
  ];

  const currentCart = writable([]);
  const currentPrice = writable(0);

  function addToCart(id) {
    cart = cart.map((cartItem) => {
      if (cartItem.id === id) {
        cartItem.addedToCart = true;
        currentPrice.update((oldPrice) => oldPrice + cartItem.price );
        currentCart.update((items) => {
          if (!items.some((item) => item.id === id)) {
            return [...items, cartItem];
          }
          return items;
        });
      }
      return cartItem;
    });
  }

  function removeFromCart(id) {
    cart = cart.map((cartItem) => {
      if (cartItem.id === id) {
        cartItem.addedToCart = false;
        currentPrice.update((oldPrice) => oldPrice - cartItem.price );
        currentCart.update((items) =>
          items.filter((item) => item.id !== id)
        );
      }
      return cartItem;
    });
  }
</script>

<h2>Cart</h2>

<ul class="cart-container">
  {#each cart as cartItem}
    <li class="cart-item">
      <div>{cartItem.name}</div>
      <div>{cartItem.price} INR</div>
      {#if !cartItem.addedToCart}
        <button on:click={() => addToCart(cartItem.id)}>Add to cart</button>
      {:else}
        <button on:click={() => removeFromCart(cartItem.id)}>Remove from cart</button>
      {/if}
    </li>
  {/each}
</ul>

<div class="price-display">Current cart price: {$currentPrice} INR</div>

<button class="checkout-button">Checkout</button>

<style>
  h2 {
    display: flex;
    justify-content: center;
    padding: 0.5rem;
  }

  .price-display {
    display: flex;
    justify-content: center;
    padding: 0.5rem;
  }

  .cart-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* X items per row */
    gap: 40px; /* Space between the boxes */
    list-style-type: none;
    justify-content: center;
    padding: 2%;
    border: 2px solid black;
    outline-style: solid;
    outline-color: red;
  }

  .cart-item {
    border: 1px solid #ddd;
    padding: 15px;
    background-color: #f9f9f9;
    text-align: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    border: 2px solid black;
    outline-style: outset;
    outline-color: grey;
  }

  .cart-item div {
    display: block;
    margin-bottom: 10px;
  }

  .cart-item button {
    background-color: #ff2f00;
    color: white;
    border: none;
    padding: 8px 16px;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.5s ease;
  }

  .cart-item button:hover {
    background-color: #7a0303;
  }

  .checkout-button {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 8px 16px;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s ease;
    margin: 0 auto;
    display: block;
  }

  .checkout-button:hover {
    background-color: #0056b3;
  }
</style>
