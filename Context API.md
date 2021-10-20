# Context API
## O que vamos aprender?
 
No React, algumas informações são passadas de pai para filho como props, no `"contexto"` de aplicações menores não se vê muitos problemas, mas quando sua aplicação ganha escala, e você precisa passar informações vários neveis a baixo, e grande parte das vezes precisamos que a informação "suba" de volta, para o elemento pai, avô, ou até mesmo que essa informação chegue a um elemento que esta muito distante nessa árvore de pai e filho que o `React` cria.
 
Então como resolver esse problema? Usando `LocalStorage`? Bom, como no bloco onde estudamos `Redux` não é interessante utilizar LocalStorage compartilhar informações entre componentes, e como no Redux, `Context` disponibiliza uma forma simples de compartilhar informações na árvore de componentes.
 
---
 
## Você sera capaz de:
 
* Utilizar Context API para gerenciar estado do React.
 
---
 
## Por que isso é importante?
 
`Context` é importante quando precisamos compartilhar informações que consideramos que possamos usar em vários lugares da árvore de componente do React, ou seja, tornar esse dado global, para que não seja necessário passar essas informações multiníveis com props, callbacks ou até mesmo no localStorage 😖.
 
---
 
# Conteúdo
 
## O problema
 
Pensaremos em um cenário onde você faz o ‘login’ na página de um jogo, e você joga varias e varias vezes, então você decide ver como esta seu ranking... *Penso que esse cenário está fresquinho na memória 🤭*, e nessa tela de ranking você precisa do e-mail de usuário capturado la no início. Como você compartilharia essa informação?
 
Veremos o seguinte código. Em primeiro momento vamos nos atentar apenas as informações do `input email`.
```
// Tela de login
 
class Login extends React.Component {
 constructor() {
   super();
 
   this.state = {
     email: '',
   };
 
   this.handleChange = this.handleChange.bind(this);
 }
 
 handleChange(event) {
   this.setState({ email: event.target.value });
 }
 
 render() {
   return (
     <form>
       <label htmlFor="">
         E-Mail:
         <input
           type="email"
           onChange={ this.handleChange }
           value={ this.state.email }
         />
       </label>
       <label htmlFor="">
         Senha:
         <input type="password" />
       </label>
       <button type="button">Login</button>
     </form>
   );
 }
}
 
 
```
 
Agora vamos observar o componente `Ranking`.
 
```
class Ranking extends React.Component {
 constructor(props) {
   super(props);
   this.state = {
     actualEmail: props.email,
   };
 }
 
 render() {
   return (
     <>
       <header>
         email: {this.state.email}
       </header>
       <h1>Ranking</h1>
       <section>
         <ol>
           <li>Pessoa 1</li>
           <li>Pessoa 2</li>
           <li>Pessoa 3</li>
           <li>Pessoa 4</li>
         </ol>
       </section>
     </>
   );
 }
}
```
 
Agora pensaremos que forma mandaremos essa `props` para o componente `Ranking`... Seria com uma função que declaramos no componente `App` que altera o estado do mesmo, e passamos como props para o componente `Login`, que recupera o estado do login, recuperando o estado do login passamos até... 🤯. Eu já me perdi aqui.
 
 
---
 
## A solução
 
 
Para evitarmos esse problema simples, e não termos que importar uma biblioteca externa como `Redux` Podemos usar o `Context(Contexto)` do React.
 
Primeiro criaremos o nosso `Context`.
 
```
import React, { createContext } from 'react';
 
// defaultValue Como próprio nome diz, podemos passar um valor padrão, geralmente deixamos ele vazio.
const MyFirstContext = createContext(defaultValue);
 
export default MyFirstContext
```
 
`createContext ` irá retornar um objeto que possui dois componentes como propriedade: `Consumer` e `Provider`.
 
O `Provider` tem a responsabilidade de enviar as informações, `prover` para os componentes filhos.
 
O `Consumer` tem a responsabilidade de receber as informações passadas pelo `Provider`, ou seja, consumi-las.
 
 
Implementaremos o `Provider`. O `Provider` Asseita uma prop chamada `value` que pode ser passada e consumida posteriormente.
 
> É Importante ressaltar, que cada vez que a `prop "value"`  for alterada, o componente que estiver consumindo-a vai ser renderizado novamente.
```
<MyFirstContext.provider value={'World'}>
 <Component2>
   <Componente2>
     {/* etc*/}
   </Componente2>
 </Component2>
</MyFirstContext.Provider>
```
Vamos implementar o `Consumer`.
 
```
function ComponentExample() {
 return(
   <MyFirstContext.consumer>
     {(value) => {
       <h1>Hello {value}!</h1>
     }}
   </MyFirstContext.consumer>
 )
}
```
 
Com a função a cima, será renderizado um titulo como o texto `Hello World!`.
 
> O argumento `value` passado na função a cima, recebe a informação passada como prop pelo `value` do `Provider`.
 
---
## Um pouco mais sobre Context
 
É Importante ressaltar que o context é usado apenas em casos que você precise utilizar uma determinada informação em varios componentes diferentes...
 
Se precisa evitar apenas passar props muitos niveis abaixo, da uma espiada nesse artigo sobre [composição de componente.](https://pt-br.reactjs.org/docs/composition-vs-inheritance.html)
 
### Class.contextType
 
Outra manteira de consumir as informações passadas pelo context, é pela propriedade contextType, uma maneira mais pratica e menos verbosa de se consumir o mesmo.
 
```
class MyComponente extends React.Component {
 componentDidMount() {
   const value = this.context;
   // cria um efeito colateral antes do componente ser montado
 }
 
 render() {
   const value = this.context;
   return(
     <h1>
     {value}
     /*Renderiza algo com base na informação passada pelo context*/
     </h1>
   );
 }
}
MyComponent.contextType = MyContext;
```
# Exercicios
 
Agora que você já esta familiarizado com o `context`, já leu o conteúdo do dia e a aula ao vivo, colocaremos a mão na massa!
 
Nessa etapa guiaremos você para criar seu primeiro `contexto`!
 
>- Primeiramente crie seu ambiente de desenvolvimento `npx create-react-app seu-repositorio`
>- Nesse exercicio vamos criar uma pequena aplicação que tem darkMode e lightMode.
 
1. Na pasta `src` crie uma pasta para armazenar seu `context`.
2. Na pasta `context` crie o seu contexto em um arquivo separado e o exporte.
3. Crie um componente para o `Provider` ser facilmente utilizado no seu código.
4. O estado inicial do contexto deve ser as propriedades para o lightMode.
5. Você terá que criar ao menos 3 paginas que consomem esse contexto, que altere de forma dinâmica o tema das mesmas com as informações passadas pelo estado do `context`.
 
>> Nesse exercicio o foco é você praticar e entender como utilizar o `context` em uma aplicação, então você terá liberdade para criar um tema para essa aplicação, então abuse da criatividade para criar um app *TOP*
 
# Recursos adicionais
 
Documentação na página do [React](https://pt-br.reactjs.org/docs/context.html).
 
Uma breve leitura para reforçar o conteúdo [como usar de forma simples e fácil](https://www.dtidigital.com.br/blog/context-api-como-usar-de-forma-simples-e-facil/).
 
Conteúdo visual em [video](https://www.youtube.com/watch?v=ch8kiuRJc7I&ab_channel=NoobCoder).
 
# Gabarito
 
1. Na pasta `src` crie uma pasta para armazenar seu `context`.
2. Na pasta `context` crie o seu contexto em um arquivo separado e o exporte.
 
```
// MyApp/src/context/MyContext.js
 
import { createContext } from 'react';
 
const Context = createContext();
 
export default Context;
```
3. Crie um componente para o `Provider` ser facilmente utilizado no seu código.
4. O estado inicial do contexto deve ser as propriedades para o lightMode.
```
// MyApp/src/context/MyProvider.jsx
 
import React from "react";
import PropTypes from 'prop-types';
import MyContext from './MyContext'
 
class MyProvider extends React.Component {
 constructor(props) {
   super(props);
   this.state = {
     theme: 'light'
   }
 
   this.handleTheme = this.handleTheme.bind(this);
 }
 
 handleTheme() {
   this.setState((prevProps) => ({
     theme: prevProps.theme === 'light' ? 'dark': 'light'
   }));
 }
 
 render() {
   const context = {
     ...this.state
   };
   const { children } = this.props;
   return(
     <MyContext.Provider value={context} >
       {children}
     </MyContext.Provider>
   );
 }
}
 
MyProvider.propTypes = {
 children: PropTypes.node.isRequired,
};
 
export default MyProvider;
 
```
 
5. Você terá que criar ao menos 3 paginas que consomem esse contexto, que altere de forma dinamica o tema das mesmas com as informações passadas pelo estado do `context`.
 
```
// MyApp/src/App.js
 
import React from 'react';
import { Route, Switch } from 'react-router';
import { BrowserRouter } from 'react-router-dom';
import InitialPage from './pages/InitialPage';
import SecondPage from './pages/SecondPage';
import ThirdPage from './pages/ThirdPage';
 
class App extends React.Component {
 render() {
   return (
     <BrowserRouter>
       <Switch>
         <Route exact path="/" component={ InitialPage } />
         <Route path="/second-page" component={ SecondPage } />
         <Route path="/Third-page" component={ ThirdPage } />
       </Switch>
     </BrowserRouter>
   );
 }
}
 
export default App;
```
 
```
// MyApp/src/pages/InitialPage.jsx
 
import React from 'react';
import { Link } from 'react-router-dom';
import MyContext from '../context/MyContext';
import '../Theme.css';
 
export default class InitialPage extends React.Component {
 render() {
   const { handleTheme, theme } = this.context;
   return (
     <div className={ theme }>
       <h1>Tela inicial</h1>
       <h2>Hello World com Context</h2>
       <button type="button" onClick={ handleTheme }>
         {theme === 'light' ? 'dark' : 'light'}
         Mode
       </button>
       <Link to="/second-page">
         <button type="button">
           Proxima Pagina
         </button>
       </Link>
     </div>
   );
 }
}
 
InitialPage.contextType = MyContext;
 
```
 
 
```
// MyApp/src/pages/SecondPage.jsx
 
import React from 'react';
import { Link } from 'react-router-dom';
import MyContext from '../context/MyContext';
import '../Theme.css';
 
export default class SecondPage extends React.Component {
 render() {
   const { handleTheme, theme } = this.context;
   return (
     <div className={ theme }>
       <h1>Segunda pagina</h1>
       <button type="button" onClick={ handleTheme }>
         {theme === 'light' ? 'dark' : 'light'}
         Mode
       </button>
       <Link to="/third-page">
         <button type="button">Proxima pagina</button>
       </Link>
     </div>
   );
 }
}
 
SecondPage.contextType = MyContext;
 
```
 
 
```
// MyApp/src/pages/ThirdPage.jsx
 
import React from 'react';
import { Link } from 'react-router-dom';
import MyContext from '../context/MyContext';
import '../Theme.css';
 
export default class ThirdPage extends React.Component {
 render() {
   const { handleTheme, theme } = this.context;
   return (
     <div className={ theme }>
       <h1>Terceira pagina</h1>
       <button type="button" onClick={ handleTheme }>
         {theme === 'light' ? 'dark' : 'light'}
         Mode
       </button>
       <Link to="/">
         <button type="button">Inicio</button>
       </Link>
     </div>
   );
 }
}
 
ThirdPage.contextType = MyContext;
 
```
 
 
```
// MyApp/src/Theme.css
 
.light {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  background-color: #ffffff;
  color: #222222;
  text-align: center;
  align-items: center;
}
 
.light button {
  width: 50vw;
  height: 20px;
}
 
.dark {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  color: #ffffff;
  background-color: #222222;
  text-align: center;
  align-items: center;
}
 
.dark button {
  width: 50vw;
  height: 20px;
}
```


