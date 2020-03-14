<h1>React com redux parte 4 - Instalando e Configurando o Redux</h1>

- Inicialmente instalamos o redux, e a integração do react com o redux

- Na linha de comando digitar o seguinte:

```bash
yarn add redux react-redux
```

- Criar a pasta `src/store`, que será onde todos os arquivos redux irão ficar;

- Criar um arquivo `src/store/index.js` onde será realizada a configuração inicial do redux;

- No arquivo `src/App.js` importar `import { Provider } from 'react-redux'`, esse `Provider`
deixará disponível o store da aplicação que foi criado em `src/store/index.js` de forma global
disponível para todos os componentes, então deve inserir por volta de todos os componentes da aplicação:


```js
  <Provider>
    <BrowserRouter>
      <Header />
      <Routes />
      <GlobalStyle />
    </BrowserRouter>
  </Provider>
```

- E ainda no `src/App.js` informar ao provider o store da aplicação:

- Primeiro importar o store:

```js
import store from './store'
```

- e adicionar propriedade ao componente `Provider`

```js
<Provier store={store}>
```

- Para poder utilizar o store do redux é necessário criar uma função de reducer dentro do store `src/store/index.js`:

```js
function cart() {
  return [];
}

const store = createStore(cart);
```

```js
  <Provider store={store}>
```

---

<h2>Criar módulos reducer</h2>

- Criar o arquivo `src/store/modules/cart/reducer.js` e dentro do arquivo adicionar a function `cart` ao invés de manter no arquivo `src/store/index`:

```js
export default function cart() {
  return [];
}
```

- Caso tenha mais de um reducer na aplicação o correto é criar um `src/store/modules/rootReducer.js` com o conteúdo:

```js
import { combineReducers } from 'redux';

import cart from './cart/reducer';

export default cobineReducers({
  cart,
});
```

- E quando precisar adicionar mais apenas adicionar dentro da function `cobineReducers()`

- e no arquivo `src/store/index`, importatmos o `./modules/rootReducer` e alteramos na função `createStore`:

```js
import rootReducer from './modules/rootReducer';

const store = createStore(rootReducer);
```
