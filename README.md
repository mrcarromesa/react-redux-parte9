<h1>React com redux parte 9 - Atualizar quantidade e totais</h1>

- Adicionar a nova action em `src/store/modules/cart/actions.js`

```js
export function updateAmount(id, amount) {
  return { type: '@cart/UPDATE_AMOUNT', id, amount };
}
```

- No arquivo `src/store/modules/cart/reducer.js`

```js
case '@cart/UPDATE_AMOUNT':
  if (action.amount <= 0) {
    return state;
  }
  return produce(state, draft => {
    const productIndex = draft.findIndex(p => p.id === action.id);

    if (productIndex >= 0) {
      draft[productIndex].amount = Number(action.amount);
    }
  });
```

- Lembrando que a responsabilidade de validar os dados é do redux, nesse caso para verificar se a quantidade é menor que zero.


- Alterar a page `Cart` :

```js
const mapDispatchToProps = dispatch =>
  bindActionCreators(CartActions, dispatch);

export default connect(mapStateToProps, mapDispatchToProps)(Cart);
```

- Adicionar a nova prop:

```js
function Cart({ cart, removeFromCart, updateAmount }) {
```


----

<h2>Calcular Subtotal</h2>


- Poderiamos realizar o calculo do subtotal no redux, ou criar uma function para no render para calcular, porém entramos no problema de performace então para realizar esse calculo faremos o seguinte:

- Na página `Cart` adicionar o import:

```js
import { formatPrice } from '../../util/format';
```

- Ajustar a function `mapStateToProps`

- Não esquecer os parenteses em volta das chaves.

```js
const mapStateToProps = state => ({
  // Não esquecer os parenteses em volta das chaves.
  cart: state.cart.map(product => ({
    ...product,
    subtotal: formatPrice(product.amount * product.price)
  }))
});
```

- Não esquecer os parenteses em volta das chaves.


---

<h2> O total </h2>

- Ajustar o `mapStateToProps` da página `Cart`:

```js
const mapStateToProps = state => ({
  // Não esquecer os parenteses em volta das chaves.
  cart: state.cart.map(product => ({
    ...product,
    subtotal: formatPrice(product.amount * product.price)
  })),
  total: formatPrice(state.cart.reduce((total, product) => {
    return total + product.price * product.amount;
  }, 0))
});
```
- Realizamos o reduce para reduzir o array para um só valor,
nesse caso somamos a variavel total ao preço vezes a quantidade do produto, iniciando o total em zero.

- Não se esquecer de adicionar a propriedade `total` na function `Cart`:

```js
function Cart({ cart, total, removeFromCart, updateAmount }) {
```


---
<h2>Resumindo...</h2>

- para calcular o total utilizando `redux` por questão de desempenho, utilizamos o `mapStateToProps`


---

<h2>Exibindo quantidades na Home</h2>

- Na página `Home` vamos adicionar o `mapStateToProps`:

```js
const mapStateToProps = state => ({
  amount: state.cart.reduce((amount, product) => {
    amount[product.id] = product.amount;
    return amount;
  }, {})
});
```

- Passamos o objeto

- A primeira variavel nesse caso `amount` o qual é inicializada com `{}`

- E a variavel de controle que pertence ao objeto `state.cart` que chamamos de `product`;

- e geramos um array, com as quantidades.

- Exemplo de reduce [Javascript reduce on array of objects](https://stackoverflow.com/questions/5732043/javascript-reduce-on-array-of-objects)



```js
var arr = [{x:1}, {x:2}, {x:4}];
arr.reduce(function (acc, obj) { return acc + obj.x; }, 0); // 7
console.log(arr);
```

- Como a page `Home` é uma class utilizamos o `this.props.amount` para obter o objeto gerado no `mapStateToProps`

- E na quantidade colocamos:

```js
 {amount[product.id] || 0}
```

- O zero é no caso de ser `undefined` ou `vazio`
