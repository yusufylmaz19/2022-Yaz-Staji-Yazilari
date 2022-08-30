## `HAFTA 5 Konuları`
+ Redux
+ Context API


## `Installing Redux`
#### `npm`
```bash
npm install redux
```
#### `yarn`
```bash
yarn add redux
```

## `Redux`
Redux'ın kendi sitesinde **_"Redux JavaScript uygulamaları için öngörülebilir bir state kapsayıcısıdır."
Uygulamalarımızı tutarlı davranışlar sergileyecek şekilde yazmamızı hem de `Redux-DevTools` sayesinde `time-travel` debug yapmamızı sağlar._**  Şeklinde bir tanımlama vardır. Yani kısace Redux bir `state` yönetim aracıdır.

## `Store` 
Tüm data ve statelerimizi tutan global bir state diyebiliriz.Store oluşturuken redux'ın `createStore()` hook'unu kullanmamız ve `reducer`'ı parametre olarak göndermemiz gerekir.
## `Action`
Tam olarak ne yapmak istediğimizi tanımlar. Bunu bir **event** olarak düşünebiliriz. Object return eden fonksiyonlardır. 2 parametre return eder. Birincisi action'ını ne olduğunu belirten bir `type` diğeri ise state'imizi update etmek için değer taşıyan `payload` değişkenidir.

## `Reducer`
Actionlarımıza göre statelerimizde yapacağımız değikleri gerçekleştirir.Buna da **event handler** diyebiliriz.Bu da object return eder ve 2 paramtere alır. Biri inital state, diğeri ise actiondır. Action type'larına göre state'i update eder bunu yaparken action'ının payload'ından dönen değerleri de kullanabilir.

## `Dispatch`
Actionlarımızı execute edebilmek için kullanırız.Oluşturduğumuz store altında bir fonksiyon olup, arguman olarak gönderdiğimiz actionları çalıştırır.

**Küçük bir demo ile code üzerinde kavramları biraz irdeliyelim.**

### `store.js`
Burada createStore ile store'umuzu oluşturuyoruz. createStore 'a parametre olarak reducer'ımızı veriyoruz.
```js
import {legacy_createStore as createStore} from 'redux';  
import counter from './reducer';

const store=createStore(counter)

export default store
```

### `actions.js`
Action fonksiyonlarımızı oluşturacağız.
```js
export const increment = value => ({
    type: 'INCREMENT',
    payload: {
        value: value
    }
})

export const decrement = value => ({
    type: 'DECREMENT',
    payload: {
        value: value
    }
})

```
### `reducer.js`
Action type'ına göre reducer'larımızı oluşturacağız.
```js
const counter=(state=0,action)=>{
    switch(action.type){
        case 'INCREMENT':
            return state + action.payload.value
        case 'DECREMENT':
            return state - action.payload.value
        default:
            return state
        }
}

export default counter;
```
### `App.js`
Burada action'larımızı ve store'umuzu import edip `dispatch()` fonksiyonu ile execute ediyouruz.
```js
import {increment,decrement} from './actions'
import store from './store';

store.dispatch(increment(5))  //0 değerine 5 ekleyip state'i 5 yapar.
store.dispatch(increment(6))  //5 değerine 6 ekleyip state'i 11 yapar.
store.dispatch(decrement(3))  //11 değerineden 3 çıkartıp state'i  8 yapar.
console.log(store.getState()); // state'i console'ladığımızda bu değerleri görüntüleyebiliriz.

function App() {
  return (
    <div className="App">
    </div>
  );
}

export default App;s
```
---
### ***Yukarıda Sadece bir tane reducer ile çalışmıştık ya birden fazla olursa?***


`createStore()` sadece bir tane reducer parametresi alır.Bizim birden fazla reducer'ımız var ise burada `combineReducers()` hook'una ihtiyacımız olacaktır. Bunu da küçük bir örnekle inceleyelim.

_Reducer kalsörü içinede 2 tane reducer oluşturalım._
### `reducers/counter.js`
```js
const counter=(state=0,action)=>{
    switch(action.type){
        case 'INCREMENT':
            return state + action.payload.value
        case 'DECREMENT':
            return state - action.payload.value
        default:
            return state
        }
}

export default counter;
```

### `reducers/isLog.js`
```js
const isLog=(state=false,action)=>{
    switch(action.type){
        case 'SIGN_IN':
            return !state 
        default:
            return state
    }
}

export default isLog
```
_Bu iki reducer'i `index.js` dosyası içinde combine edelim_

### `reducers/index.js`

`combineReducer() `hook'u ile 2 reducer'ı combine ettik.
```js
import { combineReducers } from "redux";
import counter from "./counter";
import isLog from "./isLog";

const allReducers=combineReducers(
    {
        counter,
        isLog
    }
)

export default allReducers;
```

### `store.js`
```js
import {legacy_createStore as createStore} from 'redux';  
import allReducer from './reducers/index';

const store=createStore(
    allReducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__() //bu kod yazının başında bahsettiğim time-travel debug için Redux Devtool aracınıda reducer'larımızı görüntülemeye yarar.
);

export default store
```

### `actions.js`
```js
export const increment = value => ({
    type: 'INCREMENT',
    payload: {
        value: value
    }
})

export const decrement = value => ({
    type: 'DECREMENT',
    payload: {
        value: value
    }
})
```

### `index.js`
Burada bir değikilik yapmamız gerekiyor.Görüldüğü üzere react-redux dan `Provider` componentini ekledik. Provideri redux yapsını kullanmak istediğimiz compontlerde parent component şeklinde kullanmamız gerekir ve store'umuzu props olarak geçmemiz gerekiyor.
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { Provider } from 'react-redux';
import store from './store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <Provider store={store}>
        <App />
    </Provider>
);
```
### `App.js`
Burada da karşımıza 2 tane hook çıkıyor.  `useSelector()`, state'lerimize erişebilmek için kullandığımız bir hook. `useDispatch()`, ise actions'ları execute edebilmek için kullandığımız bir hook.

```js
import React from "react";
import {useSelector,useDispatch} from "react-redux";
import {increment,decrement} from "./actions";

function App() {
  const dispatch=useDispatch();
  const counter = useSelector(state => state.counter);
  const isLogged = useSelector(state => state.isLogged);

  return (
    <div className="App">
      Counter: {counter}
      <button onClick={() => dispatch(increment(1))}>+</button>
      <button onClick={() => dispatch(decrement(1))}>-</button>

      {isLogged ? <p>Logged in</p> : <p>Logged out</p>}
    </div>
  );
}

export default App;
```

Redux'ın resmi sitesinde artık `Redux-Core` yerine `Redux Toolkit` kullanmamız gerektiğini söyler. Toolkit Redux taki karmaşık yapıyı daha basit hale getirmeye yarar. Detaylı bilgi için resmi [site](https://redux.js.org/introduction/getting-started)'yi inceleyin

---

## `Context API`
Tıpkı Redux gibi bu da bir state yönetim aracıdır. Redux'ın aksine bu React içinde gömülü şekilde gelir. 3th party bir tool değildir.

###  `useContext()`
React Context'ler component ağacında istediğimiz veriyi prop'lar üzerinden taşımadan componentler arasında taşımayı sağlar.Component saymız arttıkça state'i yönetmek karmaşık hale gelir. Örnekle açıklayalım:

```javascript
import React, { useState } from 'react'
import { render } from 'react-dom'

const Navbar({theme})=>{
		<>
		<h1>{theme==='dark'?'dark mode':'light':'light mode'}</h1>
		</>
}

const Heade({theme}){
	<>
		<header>
			<Nabar theme={theme}>
		</header>
	</>
}

const App = () => {
  const [theme, setTheme] = useState('dark')

  return (
    <div>
      <Header theme={theme} />
    </div>
  )
}

render(<App />, document.getElementById('root'))
```
Yukarıda görldğü gibi `theme` state ini  2 alt seviyedeki `Nabar` componentine propslar yardımı ile taşıdık.
Ama bu seviye sayısı fazla olsaydı durum nasıl olacaktı peki? İşte burada Context API'yi kullanmamız gerekiyor.Kısaca Statelerimizi global hale getirmek istiyorsak kullanırız.

***Yine küçük bir demo ile kavramları pekiştirelim.***

### `movieContext.js`
Burada state'i global yapmak, yani birden fazla componentin erişebilmesi için createContext() ile bir context tanımladık.

```js
import React,{useState,createContext} from 'react'

export const MovieContext = createContext()

export const  MovieProvider=props=>{
  
    const [movies, setMovies] = useState([
        {
            id: 1,
            name: 'Paths of Glory',
            price: '$10.99',
        },
        {
            id: 2,
            name: 'The Godfather',
            price: '$10.99',
        },
        {
            id: 3,
            name: 'Leon: The Professional',
            price: '$10.99',
        }
    ])
      
    return 
    <MovieContext.Provider value={[movies,setMovies]}>{props.children}</MovieContext.Provider>
}

```
fonksiyonumuz return değerinin içinde MovieContext.Provider şeklinde bir yapı gönderiyor bunun anlamı bu context `value`
prop'ında aldığı veriyi `children` elementlerine gönderebilecek

### `App.js`
movieContext.js den `<MovieProvider>`'ı import edip hangi componentine sağlayıcı olması gerektiğini belirttik. Burada `<Nav>` ve `<MovieList>` componentleri artık movieContext daki stateleri kullanbilecek.

```js
import MovieList from "./movieList";
import Nav from "./nav";
import { MovieProvider } from "./movieContext";

function App() {
  return (
    <MovieProvider>
      <div className="App">
        <Nav/> 
        <MovieList/>
      </div>
    </MovieProvider>
  );
}

export default App;

```
### `movieList.js`
Burada state'leri kullanmak için  `useContext`'e  
movieContext.js den aldığımız MovieContext'i parametre olarak gönderip bir değişken oluşturuyoruz.Artık bu değişkeni rahatlıkla kullanabiliririz

```js
import React,{useContext} from 'react'
import Movie from './movie'
import {MovieContext} from './movieContext'

export default function MovieList() {   
      const [movies,setMovies]=useContext(MovieContext)

        return (
            <div>
            {
                 movies.map(movie=>(
                     <Movie name={movie.name} price={movie.price} key={movie.id}/>
                 ))

            }
            </div>
        )
}
```

### `nav.js`
```js
import React,{useContext} from 'react'
import {MovieContext} from './movieContext'

export default function Nav() {
  const [movies,setMovies]=useContext(MovieContext)
  return (
    <div>
        <h1>Movie number</h1>
        <h3>{movies.length}</h3>
    </div>
  )
}
```
***Demo'nun çıktısına bakalım***
![](./demo.png)
Görüldüğü gibi verilerimi hem Movielist componentinde hem de Nav componentinde kullanabildim.