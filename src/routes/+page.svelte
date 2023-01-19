<script>
	let cart = [];

	let products = fetch('https://fakestoreapi.com/products')
		.then((res) => res.json())
		.then((json) => {
			console.log(json);
			return json;
		});

	const addToCart = (product) => {
		console.log(product);
		for (let item of cart) {
			if (item.id === product.id) {
				//product.quantity += 1
				cart = cart;
				return;
			}
		}
		cart = [...cart, product];
	};

	const minusItem = (product) => {
		for (let item of cart) {
			if (item.id === product.id) {
				if (product.quantity > 1) {
					//product.quantity -= 1
					cart = cart;
				} else {
					cart = cart.filter((cartItem) => cartItem != product);
				}
				return;
			}
		}
	};

	const plusItem = (product) => {
		for (let item of cart) {
			if (item.id === product.id) {
				//item.quantity += 1
				cart = cart;
				return;
			}
		}
	};

	$: total = cart.reduce((sum, item) => sum + item.price, 0);
</script>

<p>There are {cart.length} items in your cart</p>
<div class="product-list">
	{#await products}
		<!-- promise is pending -->
		<p>waiting for the promise to resolve...</p>
	{:then values}
		<!-- promise was fulfilled -->

		{#each values as product}
			<div>
				<div class="image" style="background-image: url({product.image})" />
				<h4>{product.title.slice(0, 10)}</h4>
				<p>${product.price}</p>
				<button on:click={() => addToCart(product)}>Add to cart</button>
			</div>
		{/each}
	{:catch error}
		<!-- promise was rejected -->
		<p>Something went wrong: {error.message}</p>
	{/await}
</div>

<div class="cart-list">
	{#each cart as item}
		{#if item.length > 0}
			<div class="cart-item">
				<img width="50" src={item.image} alt={item.name} />
				<div>
					{cart.title.slice(0, 10)}
					<button on:click={() => plusItem(item)}>+</button>
					<button on:click={() => minusItem(item)}>-</button>
				</div>
				<p>$ {item.price * item.quantity}</p>
			</div>
		{/if}
	{/each}
	<div class="total">
		<h4>Total: $ {total}</h4>
	</div>
</div>
<p>
	<a
		href="https://www.youtube.com/watch?v=0NtugwTVW5Q&list=PLm_Qt4aKpfKgN-CKxJaLcfcTkpU_vjqQO&index=1"
		>Watch the tutorial</a
	>
</p>

<style>
	.product-list,
	.cart-item {
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		column-gap: 50px;
		row-gap: 50px;
		justify-content: center;
	}

	.image {
		height: 150px;
		width: 150px;
		background-size: contain;
		background-position: center;
		background-repeat: no-repeat;
	}
	.total {
		text-align: right;
	}

	.cart-list {
		border: 2px solid;
		padding: 10px;
	}
	button {
		border: none;
		outline: 0;
		padding: 0.5rem;
		color: white;
		background-color: #000;
		text-align: center;
		cursor: pointer;
		width: 30%;
		font-size: 1.1rem;
		border: 2px solid gold;
	}
</style>
