<h1>React com redux parte 8 - Ajustando as Actions</h1>

- Criar o arquivo `src/store/modules/cart/actions.js` e adicionar o conteudo:

```js
export function addToCart(product) {
  return {
    type: 'ADD_TO_CART',
    product
  };
}

export function removeFromCart(id) {
  return { type: 'REMOVE_FROM_CART', id };
}

```

- Na página `Home` adicionar o import:

```js
import { bindActionCreators } from 'redux';

import * as CartActions from '../../store/modules/cart/actions';
```

- E agora com esse `bindActionCreators` podemos adicionar a seguinte function ao final do arquivo:

```js
const mapDispatchToProps = dispatch =>
  bindActionCreators(CartActions, dispatch);
```

- Essa function converte pedaços do state em propriedades, converte actions do redux em propriedades do componente

- Como não temos o `mapStateToProps` que é primeiro parametro para inserir no `connect()`, inserimos o primeiro parametro como `null`:

```js
export default connect(null, mapDispatchToProps)(Home);
```

- Dessa forma podemos alterar a function `handleAddProduct`:

```js
handleAddProduct = product => {
  const { addToCart } = this.props;

  addToCart(product);
};
```

- Então o que acontece..., a `const mapDispatchToProps`, envia para o `props`, o `dispatch({CartActions})`, dessa forma só é necessário chamar a function do `CartActions` que nesse caso é `addToCart`.

- Dessa forma não precisamos mais chamar tudo isso:

```js
dispatch({type: 'ADD_TO_CART', product});
```

- O código fica mais "verboso"

---

<h2>Refatorar os nomes das actions</h2>

- Para ficar fácil de identificar os nomes de que modulo está sendo disparado a action, uma boa prática é utilizar esse formato `@nome_do_modulo/AÇÃO`

- no arquivo `src/store/modules/cart/actions.js` alterar para:

```js
export function addToCart(product) {
  return {
    type: '@cart/ADD',
    product
  };
}

export function removeFromCart(id) {
  return { type: '@cart/REMOVE', id };
}

```

- no arquivo `src/store/modules/cart/reducer.js` alterar para:

```js
import produce from 'immer';

export default function cart(state = [], action) {
  switch (action.type) {
    case '@cart/ADD':
      return produce(state, draft => {
        // console.log(draft.length);
        // console.log(Object.keys(draft));

        /* if (draft.length > 0) {
          console.log(Object.keys(draft[0]));
          console.log(draft[0].title);
        } */

        const productIndex = draft.findIndex(p => p.id === action.product.id);

        if (productIndex >= 0) {
          draft[productIndex].amount += 1;
        } else {
          draft.push({ ...action.product, amount: 1 });
        }
      });
    case '@cart/REMOVE':
      return produce(state, draft => {
        const productIndex = draft.findIndex(p => p.id === action.id);

        if (productIndex >= 0) {
          draft.splice(productIndex, 1);
        }
      });
    default:
      return state;
  }
}

```
