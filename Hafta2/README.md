## 2. Hafta Konular

- Async Javascript (callbacks, promise, async/await)
- Array methods (filter, map, reduce…)
- API istekleri (fetch api / axios)
- CORS (backend/frontend ortak konu)

## CallBacks

Callback fonsiyonlar başka bir fonksiyona parametre olarak atanan fonksiyonlardır.
Örnekle açıklamak gerekirse:

```javascript
function sayHi(name){
alert(‘hello’+name);
}
function getName(callback){
let name=promt(‘lütfen adınızı girin’);
callback(name);
}
getName(sayHİ);
```

## Promise

Gerçek hayattaki gibi söz vermeye benzer.Eğer sözümü tutarsam başarılı olurum(resolve) eğer tutmazsam hatalı olurum(reject).
Promise Nesne asenkron işlemlerde işlemin sonuncu başarılı ya da başarısız olarak tamamlanmasında gelecek olan sonucu döndürür.

```javascript
let promise = new Promise((resolve, reject) => {
	if(condition){
resolve(‘success’);
}
else{
reject(‘error’);
}

});
```

Şeklinde tanımlanır ve yapacağımız işleme göre `resolve` ve `reject` fonksiyonlarını çalıştırırız.

### Promise 3 farklı duruma sahiptir.

- Pending : Sonuc tanımlanmamış ise
- Fulfilled : Sonuc başarılı ise
- Rejected : Sonuc hatalı ise bu değerleri döndürür.

```javascript
promise
  .then((res) => {
    console.log("then: " + res);
  })
  .catch((err) => {
    console.log("catch: " + err);
  });
```

Burada `.then() `eğer promiseimiz `success` olursa `resolve()` fonknsiyonunu çağıracak.
`.catch()` ise promise `failed` olursa `reject() `fonksiyonunu çağıracak.

### Promise callback karşılşatırma

```javascript
function aboneYap(callback,errorCallback){
    if(oturumdaMı){
        errorCallback({
            name: "oturumda değil",
            mesaj: ":("
                   }
        );
    }
    else if(videoİzliyorMU){
        errorCallback({
            name: "video izliyor",
            mesaj: "<3"
                   }
        );

    else{
        callback("şimdi abone olma zamanı");
    }
}

aboneYap((message)=>{
    console.log("abone yapıldı: "+message);
}, (error)=>{
    console.log("hata: "+error.name+" "+error.mesaj);
});
```

`Callback` ile yukarıda yaptığımız yapıyı `Promise` ile de aşağıda yapıyoruz.

```javascript
let oturumdaMı = false;
let videoİzliyorMU = false;

function aboneYap() {
  return new Promise((resolve, reject) => {
    if (oturumdaMı) {
      reject({
        name: "oturumda değil",
        mesaj: ":(",
      });
    } else if (videoİzliyorMU) {
      reject({
        name: "video izliyor",
        mesaj: "<3",
      });
    } else {
      resolve("şimdi abone olma zamanı");
    }
  });
}

aboneYap()
  .then((res) => {
    console.log(res);
  })
  .then(() => {
    console.log("Teşekkürler");
  })
  .catch((err) => {
    console.log(err.name + " " + err.mesaj);
  });
```

Görüldüğü gibi resolve işleminde .then() i defalarca kullanabilirim.

## Promise.all()

Birden fazla promise alıp bir adet promise sonucu döndürür. Eğer hepsi resolve edlilirse hepsinin resolvunu bir dizi şeklinde return eder. Ama bir tanesi bile reject ise sadece reject olan değeri return eder.

```javascript
const görev1 = new Promise((resolve, reject) => {
  resolve("görev 1 tamamlandı");
});

const görev2 = new Promise((resolve, reject) => {
  resolve("görev 2 tamamlandı");
});

const görev3 = new Promise((resolve, reject) => {
  resolve("görev 3tamamlandı");
});
```

3 adet promisimizin olduğunu görüyoruz.

Bunları aynı anda resolve etmek istediğimzde:

```javascript
Promise.all([görev1, görev2, görev3]).then((messages) => {
  console.log(messages);
});
```

## Promise.allSettled()

Bize eklediğimiz promilserin durmunu ve valuesini gösteren bir array döndürür.

## Promise.any()

Eklediğimiz promislerden ilk resolve olanlanı döndürür reject olanaları döndürmez. Eğer hiçbiri resolve değilse o zaman catch blogu içindeki hatayı döndürür.

## Promise.race()

Bu komutu kullanarak ise hangi promise daha hızlı resolve veya reject edilirse onu çağırır.

## async-await

Async ve await promiseleri daha kolay yazmamızı sağlar.
+ Async, fonksiyonun bir promise döndürmesini sağlar.
+ Await, fonksiyonun promise i beklmesini sağlar.

```javascript
async function myFunction() {
  return "Hello";
}
yukardaki yapı aşağıdaki ile aynıdır. Ikisi d promise return eder.
function myFunction() {
  return Promise.resolve("Hello");
}



async function myDisplay() {
    let myPromise = new Promise(function(resolve, reject) {
      resolve("I love You !!");
    });

    let a = await myPromise;
    console.log(a);
  }

  myDisplay() ;
```

Aşağıdaki kodda async-await in promise ile kullanılmasında dair bir örnek gösterilmiştir.

```javascript
function makeRequest(location) {
  return new Promise(function (resolve, reject) {
    console.log("making request to " + location);
    if (location === "Google") {
      resolve("Google says hi");
    } else {
      reject("call me with google pls");
    }
  });
}

function proccessRequest(response) {
  return new Promise(function (resolve, reject) {
    console.log("proccesing response");
    resolve("extra info " + response);
  });
}

async function doWork() {
  try {
    const response = await makeRequest("facebook");
    console.log("response received");
    const proccesResponse = await proccessRequest(response);
    console.log(proccesResponse);
  } catch (err) {
    console.log(err);
  }
}

doWork();
```

---

## Array Methods

Arrayler birden fazla ve farklı türde değerleri barındıran bir veri türüdür.
Örnek vermek gerekirse:

```javascript
const myArray = ["elma", 1, 2, "armut"];
```

Arrayler index ve valuelere sahiptir. Ve index numarası 0 dan başlar. Yukarıdaki örnekte 0. indexteki elemanın valuesi 'elma' dır.

## concat()

Birden fazla arrayı birleştirmeye yarar.

```javascript
const arr1 = ["Dwight", "Angela"];
const arr2 = ["Jim", "Pam"];
const employee = arr1.concat(arr2);
// epmloyee  değişkenini yazdırmak istediğimizde ["Dwight" ,"Angela","Jim","Pam"] şeklinde bir dizi döndürecektir.
```

## entries()

Arraydeki her değeri itere edilibilir key,value çiftlerinden bir dizi haline getirir.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const f = fruits.entries();
```

## every()

Parametre olarak bir fonksiyon alır.Eğer arraydaki değerler paramtere olarak verdiğimiz fonksiyon içindeki ```her``` değişken için şartı sağlıyorsa `true` , eğer bir tanesi bile sağlamıyorsa `false` değerini döndürür.

```javascript
const yas = [20, 15, 18, 22, 17];
const sonuc = yas.every(chechkAge);

function chechkAge(age) {
  return age < 18;
}
console.log(sonuc); // sonuc değişkenini yazdırdığımızda fasle dönecektir.
```

## fill()
Verdiğimiz değeri alıp dizi değerlerin yerine geçecektir.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.fill("Kiwi"); // tüm dizi  ["Kiwi", "Kiwi", "Kiwi", "Kiwi"] şeklini alacaktır.
fruits.fill("Kiwi"2,4); // 2. indexten başla 4. indexe kadar (4. index dahil değil) Kiwi ile yer değiştir  ["Banana", "Orange", "Kiwi", "Kiwi"] şeklini alacaktır.

```

## find()

Verdiğimiz fonksyion içindeki şartı sağlayan ilk elemanı döndürür.

```javascript
const yas = [100, 11, 15, 1, 17];
const sonuc = yas.find(chechkAge);

function chechkAge(age) {
  return age < 18;
}

console.log(sonuc); //sonuc 11 olacaktır.
```

## filter()

Parametre olarak fonksiyon alır. Ve eğer dizimizdeki değerler o fonksiyondaki şartı sağlıyorsa o değerleri bir dizi şeklinde döndürür. yani adı üzere filtreleme yapar.

```javascript
const yas = [20, 15, 18, 12, 17];
const sonuc = yas.filter(chechkAge);

function chechkAge(age) {
  return age < 18;
}
console.log(sonuc); // sonuc değişkenini yazdırdığımızda [15,12,17] değerlerini döneceketir
```

## findIndex()
find() gibi burda şartı sağlayan ilk değeri bulur ama değer yerine onun indexini döndürür.

```javascript
const yas = [100, 11, 15, 1, 17];
const sonuc = yas.find(chechkAge);

function chechkAge(age) {
  return age < 18;
}

console.log(sonuc); //sonuc 2 olacaktır.
```

## forEach()
Arrayimizi itere edilebilir hale getirir ve array elemanlarına kolay bir şekilde ulaşmamızı sağlar.Parametre olarak bir fonksiyon alır.
Aşağıdaki örnek ile daha iyi anlamlandıracağız.

```javascript
let sum = 0;
const numbers = [65, 44, 12, 4];
 numbers.forEach(myFunction);

function myFunction(item) {
  sum += item;
}

console.log(sum) // 125 değerini yazar.

```
## includes();
Verdiğimiz parametredeki değeri dizinin içerip içermedğie dair bir boolean değer döner.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.includes("Mango"); // true döner.

const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.includes("Banana",3); // 3 burda başlangıç indexidir. False dönecektir.

```
## indexOf();
Verdiğimiz parametredeki değeri dizinin içindeki eşleştiği ilk indexi döner.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.includes("Mango"); // 3 döner.

const fruits = ["Banana", "Orange", "Apple", "Mango", "Apple"];
let index = fruits.indexOf("Apple", 3);; // 3 burda başlangıç indexidir. 4 değerini dönecektir.
```

## isArray()
Değikenin Array objesi olup olmadığna dair boolean değer döner.

## join()
Arrayı strign olarak döndürür. 
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];

let text = fruits.join();
console.log(text); // Banana,Orange,Apple,Mango

let text = fruits.join(" and "); // bir ayrıcı ekleyebiliriz 
console.log(text); // Banana and Orange and Apple and Mango

```

## keys()
Array iteratör objesi haline getirir ve arrayimizin indexlerini tutar.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const keys = Object.keys(fruits);

let text = "";
for (let x of keys) {
  text += x + ",";

}
console.log(text); // 0,1,2,3 değerini döner.
```
## values()
Array iteratör objesi haline getirir ve arrayimizin valuelerini tutar.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const values = Object.values(fruits);

let text = "";
for (let x of values) {
  text += x + ",";

}
console.log(text); // Banana,Orange,Apple,Mango değerini döner.
```

## length
Arrayin uznuluğunu verir ama saymaya bu kez 1 den başlar.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
let uzunluk=fruits.length;
console.log(uzunluk); // 4 değerini döner.
```
## map()

Parametre olarak bir fonksiyon alır. Array üzerinde her elementi gezer ve o fonksiyondaki işlemi gerçekleştirir ve bize güncellenemiş bir array döndürür.
```javascript
const numbers = [65, 44, 12, 4];
const newArr = numbers.map(myFunction);


function myFunction(num) {
  return num * 10;
}
console.log(newArr); //[650, 440, 120, 40] değerini döner.
```
## push()

Arraya yeni bir elaman eklemek istediğimizde kullanılır. Arrayin sonuna ekler.
```javascript

const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi");  // ["Banana", "Orange", "Apple", "Mango","Kiwi"];
```

## pop()

Arraya yeni bir elaman çıkarkamak istediğimizde kullanılır. Arrayin sonunadan başlar çıkarmaya.
```javascript

const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.pop();  // ["Banana", "Orange", "Apple"];
```

## shift()

Arrayaden yeni bir elaman çıkaramak istediğimizde kullanılır. Arrayin başından başlar çıkarmaya.
```javascript

const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift();  // ["Orange", "Apple",Mango];
```
## unshift()

Arraya yeni bir elaman eklemek istediğimizde kullanılır. Arrayin başına ekler.
```javascript

const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift('Kiwi');  // ["Kiwi","Banana", "Orange", "Apple", "Mango"];
```

## some()
Parametre olarak bir fonksiyon alır.Eğer arraydaki değerler paramtere olarak verdiğimiz fonksiyon içindeki ```bazı``` değişken için şartı sağlıyorsa `true` eğer hiçbiri sağlamıyorsa `false` değerini döndürür.

```javascript
const yas = [20, 15, 18, 22, 17];
const sonuc = yas.some(chechkAge);

function chechkAge(age) {
  return age < 18;
}
console.log(sonuc); // sonuc değişkenini yazdırdığımızda true dönecektir.
```

## reverse()
Arrayi ters çevirir.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.reverse(); //["Mango","Apple","Orange","Banana"]
```

## splice()
Diziden eleman eklme ve silme işlemini yapar.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi"); // 2.indexten başla "Lemon" ve "Kiwi" yi ekle ve 0 elaman sil.  ["Banana", "Orange", "Lemon","Kiwi", Apple", "Mango"]

const fruits = ["Banana", "Orange", "Apple", "Mango","Kiwi"];
fruits.splice(2, 2); // 2.indexten başla 2 elaman sil.  ["Banana", "Orange", "Kiwi"]

```

## toString()
Arrayi String türüne dönüştürür.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
let text = fruits.toString(); // Banana,Orange,Apple,Mango değerini döner.
```

## sort()
Diziyi alfabetik olarak sıralar.

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
let text = fruits.toString(); // Apple,Banana,Mango,Orange değerini döner.
```
Aşağıdaki yap ile sayıları nasıl sıralayabiliriz.

```javascript
const points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a-b}); // 1,5,10,25,40,100
```
## slice()
Diziden belirili elamanları almak istediğimizde kullanırız.

```javascript
const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
const citrus = fruits.slice(1, 3);  // 1. indexten aşla 3.indexe kadar olanları al. Sonuç ["Orange","Lemon"] şeklinde olur.

```

## reduce()
Sadece tek bir değer döner. Diziyi kümülatif bir yapıda dolaşır.

```javascript
const numbers = [15, 2, 1, 4];

let result= numbers.reduce(getSum, 0);

function getSum(total, num) {
  return total + num;
}
console.log(result); // 22 değerini yazacaktır. 

```
Kümülatifi biraz daha açarsak su şekilde olur.
ilk değer ```a``` ikinci değere ```b``` dersek değer 3.değer ```a=a+b ```şeklinde ve böyle devam eder. Örnek üzerinde açıklarsak:
a=15, b=2, a=15+2 yani ```a=17 ``` . b miz de ```b=1 ```olur. a=17+1, a=18, b=4, a=18+4 yani ```a=22 ``` olur.

---
## Fetch Api
Apilere istek atmakta kullanılır çoğunlukla.

```javascript
fetch('https://jsonplaceholder.typicode.com/todos/1') 
```
Eğer bunu console edersek bize bir promise döndürecektir. Promislerle nasıl başa çıkacağımızı yukarıda öğrenmiştik.
```javascript
fetch('https://jsonplaceholder.typicode.com/todos/1').then(res =>
    res.json()
).then(data => 
    console.log(data)
)
```
İlk then() içindeki ```.json()``` fonksiyonu bu response nesnesi içindeki dataları json formatına çevirmeye yarar.
İkinci then() de ise responsedan promise içindeki datayı console eder.
Console bakarsak çıktı aşağıdaki gibi olacaktır.

```javcascript
{userId: 1, id: 1, title: 'delectus aut autem', completed: false}
```
Eğer hata alırsak diye res nesnesini döndürmeden önce if içinde kontrol etmemiz gerek.
```javascript
   fetch('https://jsonplaceholder.typicode.com/tod/1').then(res => {
        if (res.status >= 400) {
          res.json().then(err => {
            const error = new Error('somethig went wrong');
            error.data = err;
            throw error;
          })
        }
        return res.json();
      }).then(data =>
        console.log(data)
      )
```

Şuana kadar yaptıklarımız fetch() için GET metodunu kullanmaktı.Artık fetch(url) içinde 2. parametre olarak request infolarını girerek diğer metodları kullanabiliriz.

```javascript
fetch('https://jsonplaceholder.typicode.com/posts',{
    method:'POST',
    headers:{ 
        'Content-Type':'application/json'
    },
    body:JSON.stringify({
      name:'user 1',
    })

  }).then(res =>
      res.json()
  ).then(data => 
      console.log(data)
  ).catch(err=>{
    console.log(err);
  })
```

Görüldüğü gibi method bölümüne POST, body bölümünede yeni ekleyeceğimiz datayı yazdık. Bunu JSON.stringify() fonksiyonu içine yazdık çünkü datamız Web'e string formatında göndermemiz gerekiyor.

## Axios
Fetch in aksine axios, default olarak gelen bir fonksiyon değildir. 3. parti bir pakettir Ve axiosun en güzel yanı çoğu şeyi bizim yerimize yapması.

```javascript
      axios.get('https://jsonplaceholder.typicode.com/todos/1').then(res => console.log(res.data));
```
Fetch() de olduğu gibi .json() metotuna gerek yok. axios.get() metodu ile verilerimizi çekebiliriz.

```javascript
        axios.post('https://jsonplaceholder.typicode.com/posts', {
        name: 'user 1',
        surName: 'surname 1'
      }, {

        // headers: {
        //   'Content-Type': 'application/json'
        // }
      }).then(res => console.log(res.data)).catch(err=>{
        console.log(err);
      })
```
Burada da ```axios.post()``` ile veri gönderebiliyoruz. Fetch olduğu gibi metod,headers ya da body yazmamıza gerek yok axios bunu bizim içi yapıyor. Sadece verileri yazsak yeterli. 
Olurda kendi headerımızı yazmak istiyoruz 3. arguman olarak post metoduna yazabiliriz.
Catch bloğu içinde hatalarımızı yakalayabiliriz.

---

## CORS

CORS (Kökler Arası Kaynak Paylaşımı), bir web sayfası veya web uygulaması üzerinden sunulan kaynaklara (medya, css ve js dosyaları, fontlar, vb.) kaynağın sunulduğu alan adının  dışından gerçekleştirilen isteklerin yönetimini sağlayan bir işlemdir.

Örnek vermek gerekirse bizim web sayfamızda bulunan bir görsele farklı bir web sayfasında sergileneceğini düşünelim. Bu durumda o sayfa görüntülendiğinde bizim sunucumuzdaki kaynağı çağıracaktır.Dolayısıyla bir kaynak paylaşımı olacaktır.Bu tarz konularda CORS ile kaynağın farklı domainlerden çağrılmasını kısıtlama girişiminde bulunabiliriz.

Kısaca CORS'un amacı güvensiz originlerden gelen istekleri engelleyerek kaynağın sadece güvenli originler arasında paylaşılmasını sağlar. 

Ama CORS bir güvenlik çözümü değildir. CORS pek çok tarayıcı üzerinden kolayca devre dışı bırakılabilmektedir. Genel amacı içerik sahipliğinin korunmak ve sunucu kaynaklarının daha efektif yönetilmesini sağlamaktır.

Originler arasındaki istekeler ``` XMLHtttpRequest(XHR)``` olarak gitmektedir.

CORS configrasyonları ```same-origin policy (SOP)``` gözetilerek yapılır. XHR ve fetch api isteklerinde bu duruma uyacak şekilde istekler atılır.Aslında CORS hataları almamız SOP ile yapılmış engellemelerden dolayıdır. Biz bunu CORS ile esnetebiliriz.

### Hangi requestler CORS kullanır?
+ XHR ve Fetch API kısıtılamaları CORS ile çözümlenmesi gereken requestlerdir.
+ Web fonts(css ile isteilen @font-face)
+ WebGL 
+ Canvas da drawImage() ile image/video framleri çizme isteği


### Origin nedir? 
Origin aslında bizim URLlerimizdir.İki originin birbirleri ile aynı olması için  protokol, host ve port bilgilerinin aynı olması gereklidir. Yani eğer iki adres bilgisi aynı olsa bile, farklı port değerlerine sahip ise same-origin olma durumundan çıkarlar.
Örnek Vermek gerekirse:

URL: http://myawsomesite.com

| url | origin status | 
| --------------------------------- | ------------------| 
| http://myawsomesite.com/login     | same origin       | 
| http://myawsomesite.com/user/1    | same origin       |   
| https://myawsomesite.com          | not same origin   |   
| http://en.myawsomesite.com        | not same origin   |   

### CORS nasıl çalışır?
Client'dan  Servere'a bir request attığımızda SOP'a göre önce ```Preflight Requesti``` atılır. Bu aslında bir HTTP Options requestidir. Ve bizim tarayıcı ile SOP kısıtlamalrına uyup uymadığımızı söyler.

![](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/preflight_correct.png)

### Prefligth requesti neler tetiklemez?
Bu istek türü preflight isteğini tetiklemez ve tek bir HTTP isteği halinde gönderilir.Bu tür istekelere simple(basit) isteklerde denilmektedir.
Aşağıdaki özelliklerden herhangi birine sahip olması yeterlidir: 

**Şu HTTP metodları:**

+ GET
+ HEAD 
+ POST 

**Fetch Spec’inde yasaklı başlıklar bölümünün haricindeki HTTP başlıklarına sahiptirler:**
+ Accept
+ Accept-Language
+ Content-Language
+ Content-Type (bazı değerler haricinde)
+ DPR
+ Downlink
+ Save-Data
+ Viewport-Width
+ Width

**Content-Type başlığı için izin verilen değerler:**

+ application/x-www-form-urlencoded
+ multipart/form-data
+ text/plain


### Prefligth requesti neler tetikler?

Basit isteklerin aksine preflight gerektiren isteklerde, orijinal isteğin gönderilmesinden önce tarayıcı otomatik olarak OPTIONS metodu ile bir preflight isteği oluşturur ve bunu sunucuya gönderir. Bu istek sayesinde, origin’in ilgili isteği gerçekleştirmesi için izni olup olmadığı kontrol edilir. Bir isteğin preflight’ı tetikleyebilmesi için aşağıdaki özelliklerden herhangi birine sahip olması yeterlidir:

**Şu HTTP metodları:**

+ PUT
+ DELETE 
+ CONNECT
+ OPTİNOS
+ PATCH

**Fetch Spec’inde yasaklı başlıklar bölümünde yer alan HTTP başlıklardan birine sahip olması:**
+ Accept
+ Accept-Language
+ Content-Language
+ Content-Type (bazı değerler haricinde)
+ DPR
+ Downlink
+ Save-Data
+ Viewport-Width
+ Width

**Content-Type başlığı için izin verilen değerler:**

+ application/x-www-form-urlencoded
+ multipart/form-data
+ text/plain
