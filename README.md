<h1>React com redux parte 7 - Cart</h1>

- Na pagina cart adicionar o `connect` do redux:

```js
import { connect } from 'react-redux';

// ...

function Cart({ cart }) {
  //..
}

const mapStateToProps = state => ({
  cart: state.cart
});

export default connect(mapStateToProps)(Cart);
```

- No `src/store/modules/cart/reducer.js` alterar para o seguinte:

```js
export default function cart(state = [], action) {
  switch (action.type) {
    case 'ADD_TO_CART':
      return [...state, { ...action.product, amount: 1 }];
    default:
      return state;
  }
}

```

- A invés de utilizar:

```js
return [...state, action.product];
```

- Estamos utilizando:

```js
return [...state, {...action.product, amount: 1}];
```

- para já adicionar a quantidade padrão `1` no item adicionado ao carrinho.

- E na página `cart`  adicionar o `cart.map`, para retornar o itens presente no `state.cart`

---

<h2>Immer</h2>

Como o state no React é imutavel, precisamos sempre criar um novo estado ao invés de alterar o estado anterior.

O immer nos permite ter um meio termo entre os dois, escrever o código como se fosse mutável e o immer com esse nosso código cria um novo estado.

- [Repositório do Immer](https://github.com/immerjs/immer)
- [Docs do Immer](https://immerjs.github.io/immer/docs/introduction)

- Instalar o Immer:

```bash
yarn add immer
```

- No arquivo `src/store/modules/cart/reducer.js`, alterar para:

```js
import produce from 'immer';

export default function cart(state = [], action) {
  switch (action.type) {
    case 'ADD_TO_CART':
      return produce(state, draft => {

        const productIndex = draft.findIndex(p => p.id === action.product.id);

        if (productIndex >= 0) {
          draft[productIndex].amount++;
        } else {
          draft.push({...action.product, amount: 1});
        }

      });
    default:
      return state;
  }
}

```

- O elemento draft é do tipo Proxy, o qual não dá para pegar os elementos diretamente no console,
Para sabermos os elementos que há nele realizamos o seguinte:

```js
// Primeiro encontramos esse
console.log(Object.keys(draft));

// Depois de encontrar o primeiro
// Vamos para o próximo até encontrar uma propriedade que possamos acessar, como no caso id, ou title

if (draft.length > 0) {
  // Executar esse apenas após encontrar o primeiro
  console.log(Object.keys(draft[0]));
  // Executar esse apenas após encontrar o anterior
  console.log(draft[0].title);
}
```

- Dessa forma da para entender como e que propriedade obter de draft para pesquisar qual elemento alterar.

- E após essa alteração o carrinho soma quando é adicionado o mesmo produto

---

<h2>Remover Item do carrinho</h2>

- Lembrando que no `App.js` há o componente `<Provider store={store}>` o qual adiciona o `props.dispatch` aos `props`

- Com isso em mente altere a function `Cart`:

```js
function Cart({ cart, dispatch }) {
```

- No botão de remover item na function `Cart` adicione o evento `onClick`:

```js
<button
    type="button"
    onClick={() =>
      dispatch({ type: 'REMOVE_FROM_CART', id: product.id })
    }
  >
```

- Ao clicar podemos ver no app `Reactotron` que foi chamada a action `REMOVE_FROM_CART`
