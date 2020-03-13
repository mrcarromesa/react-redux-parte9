<h1>Iniciando projeto RocketShoes</h1>

- Basicamente foi iniciado o projeto conforme:

  - [Projeto com ReactJS Parte 1 (Inicio)](https://github.com/mrcarromesa/react-parte1)
  - [Configurando o ESLint, Prettier, Editor Config](https://github.com/mrcarromesa/react-parte3)

---

<h2>Detalhes sobre as rotas</h2>

- No caso das rotas dessa vez não foi adicionado o componente `BrowserRoute` diretamente
nas rotas, mas sim no `src/App.js`, motivo: como precisaremos ter outros componentes que
precisará acessar as rotas como no caso um Header de navegação, então esse componente precisará
está dentro do `BrowserRoute`, e preciso do componente `Routes` para exibir a rota acessada,
dessa forma o código no arquivo `src/App.js` ficará dessa forma:

```js
function App() {
  return (
    <BrowserRouter>
      <Header />
      <Routes />
    </BrowserRouter>
  );
}
```

- Nesse caso a ideia do `Header` é uma navegação para acesso as rotas.


---

<h2> Estilizar componentes que não são do html e sim nativo do react</h2>

- No arquivo `src/components/Header/style.js` temos o component `Link` importado:

```js
import { Link } from 'react-router-dom';
```

- Para estilizar esse componente com `styled-component` é necessário realizar o seguinte:

```js
export const Cart = styled(Link)``;
```

- Nesse caso colocamos o component dentro dos "(", ")"

- E no arquivo que importar esse style poderá chamar esse compoente `Cart` que no caso é um `Link` já estilizado:

```js
<Cart>...</Cart>
```

---

<h2>Trabalhando com colunas CSS</h2>

- Veja o exemplo a seguir:

```css
ul {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}
```

- Definimos a propriedade `display: grid;` e propriedade `grid-template-columns` bem como a propriedade `grid-gap` para o container, o qual tornará seus elementos em colunas seguindo template em `grid-template-columns`

- Mais detalhes sobre tamanhos e outras propriedades em: [grid-template-columns](https://www.origamid.com/projetos/css-grid-layout-guia-completo/)


---

<h2>Elementos relativos CSS</h2>

- Veja o seguinte exemplo:

```html
<div>
  <span>Nome</span>
  <button><span>OK</span></button>
```
- Se desejarmos manipular apenas o primeiro `span` que está diretamente na `div`, e não mexer no `span` dentro
do `button` utilizamos o seguinte css:

```css
div > span {
  ...
}
```

- Dessa forma o `span` dentro do `button` não sofrerá alteração.


---

<h2>Forçando o margi-top: auto;</h2>

- O container deverá ser `display: flex`;

- O item dentro do container pode inserir o `margin-top: auto` para mante-lo próximo do bottom do container

---

<h2>align-self</h2>

- Essa é outra propriedade de css que alinha um elemento dentro de um container
flex, podendo ser para ocupar toda area: `stretch`, centralizar verticalmente `center`,
alinhar ao topo vertical `start`, ou a base vertical `end`.


---
<h2>Para facilitar a manipulação de cores dentro do js podemos utilzar a lib polished</h2>

- Se precisamos manipular uma dada cor seja para um hover por exemplo podemos utilizar o polished

- Para instalar execute o comando:

```bash
yarn add polished
```

- Importar no arquivo:

```js
import { darken } from 'polished';
```

- Utilizar no css do `styled-component`:

```js
&:hover {
  background: ${darken(0.1, '#7159c1')};
}
```

- Parametros:

  - o primeiro é a força
  - o segundo é a cor que queremos manipular
---

<h2>Propriedade de alinhamento utilizada junto com display flex</h2>

- Podemos utilizar a propriedade `align-items: baseline;` para alinhar os componetes a linha base dele.
