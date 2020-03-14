<h1>React com redux parte 5 - Salvando itens no carrinho (Reducer)</h1>

- No arquivo `src/pages/Home/index.js` importar o seguinte:

```js
import { connect } from 'react-redux';
```

- O `connect` irá conectar o componente com o estado do redux

- Então no componente tiramos o `export default` e inserimos no final do arquivo:

```js
export default connect()(Home);
```

- Pois o `connect` executa uma function que retorna outra function

- Ainda no componente `Home` adicionamos o `onClick` no `button`:

```js
onClick={() => this.handleAddProduct(product)}
```

- E então implementamos a function:

```js
handleAddProduct = product => {
    const { dispatch } = this.props;


    dispatch({
      type: 'ADD_TO_CART', //Nome da action
      product // um "valor" ou restante do conteudo da action
    });
};
```
- o redux irá realizar a chamada para todas as funções dentro do modules, que estão definidas dentro do `rootReducer`

- Agora dentro o `src/store/modules/cart/reducer.js` realizar a alteração:

```js
export default function cart(state = [], action) {

  console.log(action);

  return [];
}

```

- Dando atenção a function:

```js
function cart(state, action)
```

- O `state` é o estado anterior e é imutável

- O `action` é a ação que está sendo disparada para todos os reducers

- Para adicionar/alterar informações dentro do `state`, primeiro realizamos um `switch case`, para decidir quando realizar uma ação ou não:

```js
switch (action.type) {
  case 'ADD_TO_CART':
    return [...state, action.product]; // retorna o estado modificado

  default :
    return state; // retorna o próprio state
}
```

- Então como, ao realizar o dispach de uma action é chamado todos os reducers, é necessário que em todos eles só escutem a action que seja relativo a ele.


----


<h2>Acessando o reducer através de outro componente</h2>

- Acessar o reducer `cart` através do componente `Header`:

- Alterar o `export default`:

```js
function Header({ cart, cartSize }) {
  //...
}

export default connect(state => {
  cart: state.cart
  cartSize: state.cart.length
})(Header);
```


- a propriedade `cart` pode ser outro nome que definirmos apenas lembrando que em `Header({ cart })` ali no lugar de `cart` também precisará ser o mesmo nome que definirmos.

- e no `state.cart`, a propriedade `cart` é o nome do reducer que criamos em `src/store/modules/cart/reducer.js`

