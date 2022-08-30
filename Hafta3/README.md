## `3. Hafta Konular`

- Hooks
- Components

## `Components`
Componentler, bağımsız ve tekrar kullanılabilir kod parçalarıdır. JavaScript fonksiyonları ile aynı amaca hizmet ederler.İzole biçimde çalışır ve HTML elementleri return ederler.

Google'ın searchbarı bir component olabilir, navbar yeri başka bir component olabilir,sidebarı ayrı bir component olabilir, içeriklerin paylaşılıdğı article bölümü ayrı bir component olabilir. Ve son olarak tüm bu componentleri tek bir homepage componenti altında toplarsak ki bu da apayrı bir component olacaktır.

Componentler HTML return eder demişitk ama bu HTML ler JSX adını verdiğimiz bir formatta yazılmalı.


#### `2 tip component vardır:`
+ Class Component
+ Functional  Components

### ```Functional Components```
Basitçe Javascript  fonksiyonlarıdır.Javascript fonsiyonları yazarak functional componentler oluşturabiliriz.Bu fonksiyonlar parametre olarak veri alabilir veya almayabilirler.

```javascript
const functionalComponent=()=>
{
    return <h1>omg it is so hard</h1>;
}
```

### ```Class Components```
Class componentler functional componentlerden biraz daha karmaşıktır.functional componentler, programınızdaki diğer componentlerin farkında değildir, oysa class componentleri birbirleriyle çalışabilir.Bir class componentten diğerine veri aktarabiliriz.

```javascript
class classComponent extends React.Component
{
    render(){
          return <h1>that's what she said</h1>;
    }
}
```
Yukarıda oluşturduğumuz 2 component birbilerine eşdeğerdir. Ancak Class ve Functional componentler arasında tabiki farklar vardır. Gelin onları inceleyelim.

| Functional Components | Class Components    | 
| --------------------------------- | ------------------| 
|Functional component sadece bir React elementi(JSX) geri döndüren bir JavaScript pure fonksiyonudur.  |Class component React sınıfıdan kalıtım alınarak oluşturulur.Aynı şekilde o da React elementi döndürür.  | 
| Render metodu yokur.    | JSX elemntini return ederken render() metodu olmak zorundadır.      |   
| Durumsuz(stateless) component olarak bilinir.Basitçe datayı kabul edip display ederler.         | Durumlu(stateful) component olarak bilinir.Çünkü logic ve state implement ederler.    |   
| React  lifecycle metodları(ör: componentDidMount) burada kullanılmazlar.   | React lifecycle metodları burada kullanırlar.  |  
| Hooklar burada kolayca kullanırlar ve bu durum bunu stateful hale getirir.Örnek: `const [name,SetName]= React.useState(‘ ‘) `  | Bir hooku implement edebilmek için farklı bir yapı kullanırlar.Örnek: ` constructor(props) {super(props);  this.state = {name: ‘ ‘}}`|  
|Constructorlar kullanılmaz. |Constructorlar stateleri saklamak için kullanılması gerekmektedir. |

## `Rendering elements in React`
Elementimizi browser DOM unda render etmeden önce bizim bir kapsayıcyıa veya root DOM elementine ihtiyacımız var. index.html dosaymıızın `<div id="root"></div>` durumnda sahip olduğunu düşünelim.

`App.js`: React elementini root node'unda render etmeden önce App.js dosyamızın içine aşağıdaki kodları yazmamız gerkiyor.
```javascript
import React,{ Component } from 'react';

class App extends Component {

render() {
	return (	
	<div>
		<h1>Welcome my son,</h1>
	</div>

	);
}
}	
export default App;
```

## `Updating elements in React`
React elementleri değiştirilemezlerdir.Element bir kere oluşturuldumu onun childern ve attributelerini değiştirmek imkansızdır.Böylece elementleri Update etme işleminde, valuelerimizi zaman içinde birden fazla render edebilmek için `render()` metodunu kullanmamız gerekir.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

function showTime()
{
const myElement = (
				<div>
						<h1>Welcome to the machine</h1>
						<h2>{new Date().toLocaleTimeString()}</h2>
				</div>
				);

ReactDOM.render(
	myElement,
	document.getElementById("root")
);				
}

setInterval(showTime, 1000);

```

## `Rendering Components`
React de bir componenti render etmek için biz bir elementi user-defined component ile başlatabiliriz. Ve bunu ReactDOM.render()'ın ilk parameteresi olarak gönderebiliriz.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

const Welcome=()=>
{
		return <h1>Dwight you ignorant slut</h1>
}

ReactDOM.render(
	<Welcome />,
	document.getElementById("root")
);
```
**Not:**  Component isimleri her zaman ilk harfi büyük olmalıdır.

`const elementName = <ComponentName />;`
Yukarıdaki syntaxda ComponentName user-defined componentidir.

`Hook a geçmeden önce state ve props kavramlarına biraz göz atalım`

## `State`
State,Component hakkında bilgi ve veriyi tutan ve zaman içinde değişeblien bir yapıdır.Kullanıcıya ve sisteme cevap vererek State i değiştirebiliriz.State reactin kalbi diyebiliriz çünkü componentin davranışını ve nasıl render edileceği belirler.Sadece component içinde veya copmonent tarafından erişilmeli ya da değiştirilmelidir.

## `Props`
Propslar HTML attributleri ile benzer çalışır. Propslar bir tag in attriubute inin value si için data store eden nesnelerdir.
Bir componentten diğerine data geçmemizi sağlarlar. Bir fonksiyonun parametersini ekler gibi componentlerin içinde datamızı parametre olarak aktarabiliriz.Propslar değiştirilemezlerdir bu yüzden onları componentin içinde değiştiremeyiz.

## `Difference between State and Props`

|Props|State|
| --------------------------------- | ------------------| 
|Read-only dir.|Asenkron yapıda olabilir.|
|Değiştirilemezlerdir.| Değiştirilebilirler.|
|Bir componente arguman olarak veya bir componentden başka bir componente data aktarmak için kullanırlar.|State, component hakkında bilgi tutar.|
|Child component tarfından erişilebilir.|Child component tarafından erişilemez.|
|Componentleri tekrar kullanılabilir yapar.|Componentleri tekrar kullanlabilir yapmaz.|
|Stateless componentlerin propslatı olabilir.|Stateless componentler state sahip değillerdir.|
 
## `Hooks`

Hooks React 16.8 versiyonu ile gelen yeni bir özelliktir.React hook ile state ve diğer React özelliklerini fonksiyon compenentlerinde kullanabiliriz. 16.8 den önce bunları sadece class compenentlerinde yazabiliyorduk.

## `Stateful vs. Stateless Components`
Reacte componentler stateful ve stateless olabilirler.

+ Bir stateful component kendi içinde local state i tanımlar ve yönetir.
+ Bir stateless component local state i ve side-effecti olmayn JavaScript pure fonksiyonlarıdır.

`pure fonksiyon` side-effecte sahip olmayan fonksiyonlardır.Yani her zaman aynı input için aynı output'u üretirler.

Eğer biz stateful ve side-effect mantığını componentten çıkarırsak stateless componenti elde ederiz.Stateful ve side-effect mantığı uygulmamızın her yerinde terkar kullanılabilir.

# `React Hooks and Stateful Logic`

React hook ile functional componentlerde stateful logici ve side-effecti izole edebiliriz. 
Hooklar, state in davranışını ve side-effecti componentlerimizden izole etmemize yarayan javascript fonksiyonlarıdır. 
Stateful logic, olduğunda local olarak tanımlanan ve yönetilen state değişkenleri olabilir.

# `What Exactly Are React Hooks?`
React hooks, tekar kulllanılabilir yapıları functional componentlerden izole eden JavaScript fonksiyonlarıdır. 
Hooklar stateful olabilir, ve side-effectleri yönetebilirler.

React bir çok standart Hook sağlar:

### `useState:` 
Stateleri yönetmek için kullanılır. Stateful değer ve onu güncelleyecek bir Updater fonksiyon(setter) gönderir.
```javascript
import { useState } from "react"; //tabi useState kullanabilmek için react sınıfıdan import etmemiz gerekir.

const [state,setState]=useState({theme:"dark",value:5}) // burada görüldiğü gibi bir ilk değer olarak bir object almış
function change(){
	setState({theme:"light",value:0}); // change fonksiyonu çağrıldığında setState ile state i güncelliyoruz
}

// stateimizi JSX içinde aşağıdaki şekilde erişim sağlarız.
<h1>{state}</h1>
<button onClick={change}>state</button>
``` 

### `useEffect`
Bu hook 2 arguman alır. İkincisi opsiyoneldir, bunu side effectleri yönetmek için kullanırız.

`1- arguman olmadan `

```javascript
	useEffect(() => {
	doSomething();            // Bu her renderde çalışacağı anlamına gelir.
	});
```
`2- arguman olarak boş bir array `

```javascript
	useEffect(() => {
	doSomething();         // Bu sadece ilk rendere çalışacağı anlamına gelir
	},[]);
```
`3- arguman olarak bir değiken`

```javascript
	useEffect(() => {
	doSomething();          // Bu da verdiğimiz value ye bağlı olarak value her değiştiğinde çalışır 
	},[value]);
```

###  `useContext`
React Context'ler component ağacında istediğimiz veriyi prop'lar üzerinden taşımadan componentlar arasında taşımayı sağlar.Component saymız arttıkça state'i yönetmek karmaşık hale gelir. Örnekle açıklayalım:

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

`useContext şunları içerir:`

+ 1-Created context - data
+ 2-Context Provider - provide context to all children
+ 3-Passed value - data you want to share
+ 4-Hook - to read shared data

```javascript
const user = {
  name: "Jim",
  lastName: "Halpert",
};

export const UserContext = createContext(user); 

<UserContext.Provider value={user}>
{children}
</UserContext.Provider>;
```

Child elementte contexti import etmemiz ve onu useContext'e arguman olarak eklememiz gerekiyor.
```javascript
import { UserContext } from "./App";

const { name } = useContext(UserContext);

return <h1>Hi {name}<>
```

## `useRef`
Dom elementlerine erişmek için kullanırız.

```javascript
	import {useState, useRef} from 'react';
	const inputRef= useRef()

	function focus(){
		inputRef.current.focus()
		inputRef.current.value='some values'
	}

	//
	<>
   		<input ref={inputRef} value={name}/>

	</>
```
## `useReducer`
useState'e benzer. Karmaşık state yapılarını yönetmek için kullanılır. Örnek ile daha iyi açıklayalım.

```javascript
import {useReducer,useState} from 'react'; // import işlemelri

const ACTIONS={
   INCREMENT:'increment' ,   // hangi işlemi yapacağımız bilgisini bir object içinde saklıyoruz ve oradan çağıracağız.
   DECREMENT:'decrement' 
} 

function reducer(state,action){    // reducer fonksiyonumuz 2 paramtere alıyor. state ve action. action tipine göre state'ideğitirecek
    switch(action.type){
        case ACTIONS.INCREMENT:
        return state+1
		case ACTIONS.DECREMENT:
        return state-1
        default:return todos
    }
}

export functions Todo=()=>{

	const [count,dispatch]=useReducer(reducer,0)       
	//Burada useReducer() 2 parametre alıyor, ilki reducer fonksiyonunu ikincisi intial değer. 
	//Burada disptach(type) metodu reducerda tanımladığımız işlemleri gerçkeleştirecektir ve bu değişikliği count üzerinede uygulayacaktır.
	//Aşağıda kullanımı gösterildi. Ayrıca dispatch() type argumanı yanında 2. paramtere olarak payload değeri alır.
	//Ki bu bizim dispatch ile göndermek istedğimiz değiken ve değeri içerir. dispatch({type:Action,payload={name:'something'}})

	return(
			<>
					<h1>{count}</h1>
					<button onClikc={()=>{dispatch(ACTIONS.INCREMENT)}}>+</button>
					<button onClikc={()=>{dispatch(ACTIONS.DECREMENT)}}>-</button>
					
			</>
	)
}

```	

## `useCallback`

useCallback parametre olarak bir fonksiyon ve dependency array alıyor ve dependency array içerisindeki değerler değişmediği sürece parametre olarak aldığı fonksiyonu return ediyor. 

```javascript
    import {useCalback} from 'react';
	const memoizedFunc = useCallback(() => {
  	someFunc(a,b)
	},[a,b])
```
## `useMemo`
Eğer yaptığımız işlem uzun süre alıyorsa useMemo kullanarak bunu her seferinde render etmek yerine sadece value değiştiğinde render edilmesini sağlarız. useMemo geriye fonksiyonun döndürdüğü değeri döndürür.

```javascript
	import {useState, useMemo} from 'react';
	function slowFunction(num){
	console.log('slow function started')
	
	for(let i=0;i<100000;i++){}
	return num*2
	}

	const [sayi,setSayi]=useState(0)
	const [dark,setDark]=useState(false)
	const doubleNumber=useMemo(()=>slowFunction(sayi),[sayi])
```
---
## `Demo`
Küçük bir meme generator app i yapalım.

Uygulamamızın çıktısı aşağıdaki gibi olacaktır.
![](./gif.gif)

#### `meme.js` 
```javascript
import React from "react";
import TrollFace from "../TrollFace.svg";
import { useEffect,useState } from "react";

export default function Meme() {

  const myUrl = "https://api.imgflip.com/get_memes";

  const [meme, setMeme] = useState({
    upText: "",
    downText: "",
    imgUrl: TrollFace,
  });

  const [allMeme, setAllMeme] = useState([]);

  // burada sayfa ilk yüklendiğinde API den datalarımızı çekiyoruz ve onu allMeme stateine aktarıyouruz
  useEffect(() => {
    fetch(myUrl)
      .then((res) => res.json())
      .then((data) => setAllMeme(data.data.memes));
  }, []);


  // burada datamızı içinde random bir görseli meme stateine aktarıyoruz
  const getMeme = () => {
    const memeDataArray = allMeme;
    const randomNumber = Math.floor(Math.random() * memeDataArray.length);
    const randomUrl = memeDataArray[randomNumber].url;
    setMeme((preMeme) => ({
      ...preMeme,
      imgUrl: randomUrl,
    }));
  };

  // burada ise input alanlarımızın değerlerini meme stateinin aktarıyoruz
  const handleChage = (event) => {
    const { name, value } = event.target;
    setMeme((preMeme) => ({
      ...preMeme,
      [name]: value,
    }));
  };

  
  return (
    <main className="memeContainer">
      <header className="memeHeader">
        <img className="memeLogo" src={TrollFace} />
        <p className="memeTitle">Meme Generator</p>
      </header>
      <section className="memeInputSection">
        <input
          className="memeUpInput memeInput"
          type="text"
          placeholder="up text here"
          name="upText"
          value={meme.upText}
          onChange={handleChage}
        />
        <input
          className="memeDownInput memeInput"
          type="text"
          placeholder="down text here"
          name="downText"
          value={meme.downText}
          onChange={handleChage}
        />
      </section>
      <button className="memeGenButton" type="button" onClick={getMeme}>
        Get a new meme image
      </button>
      <section className="memeImageSection">
        <img className="memeImg" src={meme.imgUrl} />
        <h2 className="upText overText">{meme.upText}</h2>
        <h2 className="downText overText">{meme.downText}</h2>
      </section>
    </main>
  );
}

```

#### `app.js` 
```javascript
import React from "react";
import Meme from "./meme";
import "./meme.css"

export default function(){
    return(
        <Meme/>
    )
}
```

#### `index.js` 
```javascript
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './componentes/App';

const container=document.getElementById('root');
const root = createRoot(container);

root.render(
    <App/>
);

```