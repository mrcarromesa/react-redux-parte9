<h1>React com Redux - parte 3</h1>

- Trabalhando com internacionalização:

- Mais detalhes nesse link [Internacionalização De Forma Nativa em Javascript](https://blog.matheuscastiglioni.com.br/internacionalizacao-de-forma-nativa-javascript/)

- Um exemplo está no arquivo `src/util/format.js`

```js
export const { format: formatPrice } = new Intl.NumberFormat('pt-BR', {
  style: 'currency',
  currency: 'BRL'
});

```

- Nesse caso o foi utlizado uma desestruturação da função, pois comumente a função é escrita da seguinte forma:

```js
const valorFormatado = new Intl.NumberFormat('pt-BR', {
  style: 'currency',
  currency: 'BRL'
}).format('VALOR_AQUI');
```

- No outro caso exportamos a desestruturação o seja o format e apelidamos de `formatPrice`, no caso para ficar melhor entendivel:

```js
export const formatPrice = new Intl.NumberFormat('pt-BR', {
  style: 'currency',
  currency: 'BRL'
}).format;
```

---

<h2>Pensando na performance</h2>

- podemos adicionar functions diretamente no render:

```js
// ...
<span>{formatPrice(product.price)}</span>
// ..
```

- O problema é que toda vez que o state sofrer uma alteração essa função será chamada sempre

- Nesse caso podemos utilizar dentro da função de incialização:

```js
async componentDidMount() {
  const response = await api.get('/products');

  const data = response.data.map(product => ({
    ...product,
    priceFormatted: formatPrice(product.price)
  }));

  this.setState({ products: data });
}
```

- Então algo que tem que cuidar muito no react é essa parte de recursos, de chamar funções que serão chamadas novamente sempre que houver alteração no estado.
