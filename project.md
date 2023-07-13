1. İhtiyacımız olan tüm değerleri script.js üzerinde tanımlıyoruz.
    
    ```jsx
    const hourEl = document.querySelector('.hout')
    const minuteEl = document.querySelector('.minute')
    const secondEl = document.querySelector('.second')
    const timeEl = document.querySelector('.time')
    const dateEl = document.querySelector('.date')
    const toggle = document.querySelector('.toggle')
    ```
    
2. Arından date kısmında tanımlayacağımız gün ve ayları bir array içerisine alıyoruz.
    
    ```jsx
    const days = ["Pazar","Pazartesi","Salı","Çarşamba","Perşembe","Cuma","Cumartesi"];
    const months = ["Ocak","Şubat","Mart","Nisan","Mayıs","Haziran","Temmuz","Ağustos","Eylül","Ekim","Kasım","Aralık"]
    ```
    
3. İlk önce Dark Mode üzerinde çalışacağız.
4. HTML üzerine **“dark”** sınıfını ekleyeceğiz, anasayfada görünen butonun içindeki *“Dark Mode”* yazısını *“Light Mode”* olarak dönüştüreceğiz.
5. Butona event işlemi atıyoruz. `toggle.addEventListener(’click’, ()⇒{})`
6. Fonksiyon parametre olarak bir event alıyor. Event işlemi içerisinde html’i tanımlıyoruz.  `toggle.addEventListener(’click’, ()⇒{ const html= document.querySelector('html')})`
7. Ardından bir koşul işlemi uyguluyoruz. Html’in classında **‘dark’** var mı? Eğer varsa class içindeki **‘dark’** sil.
    
    ```jsx
    toggle.addEventListener(’click’, (e) ⇒ {
    	const html = document.querySelector('html')
    	if(html.classList.contains('dark')){
    		html.classList.remove('dark')
    	}
    })
    ```
    
8. Koşul içerisinde butona **‘e’** parametresi üzerinde ulaşarak butonun içerisindeki yazıyı **‘Dark Mode’** olarak değiştiriyoruz. `e.target.innerHTML = ‘Dark Mode’`
9. Else kısmında yukarıda yaptıklarımızın tam tersini yapıyoruz.
    
    ```jsx
    toggle.addEventListener(’click’, (e) ⇒ {
    	const html = document.querySelector('html')
    	if(html.classList.contains('dark'){
    		html.classList.remove('dark')
    		e.target.innerHTML = ‘Dark Mode’
    	} else {
    		html.classList.add('dark')
    		e.target.innerHTML = ‘Light Mode’
    	}
    })
    ```
    
10. Mode kısmı bu kadardı. Sırada saat işlemini gerçekleştirmek var.
11. setTime adında bir fonksiyon yaratıyoruz. Fonksiyon içerisinde ilk yapacağımız şey **“time”** adında bir değişken yaratıp bunu Tarih ile eşitlemek. `const time = new Date()` Fonksiyonu Global Scope da çağırmayı unutmayalım 🙂
12. Değişkeni tanımladıktan sonra tarih içerisindeki diğer değişkenleri tanımlamak kalıyor. **“month, day, hours, minutes, seconds”** değişkenlerini tanımlıyoruz bunları sırası ile `“time.getMonth(), time.getDay(), time.getHours(), time.getMinutes(), time.getSeconds()”` eşitliyoruz. 
13. **“hours”** tanımlandıktan sonra saati 24 saatlik versiyona değil 12 saatlik biçimde kullanmak için `const hoursForClick = hours % 12` adında bir değişken tanımlıyoruz.
14. İlk başta tanımladığımız **“hourEl”** burada style-transform işlemi uyguluyoruz. Rotate işlemini dinamik hale çeviriyoruz. Ancak bunun için yardımcı bir ölçeklendirme işlemi uyguluyoruz. Bu ölçeklendirme bir sayı aralığını başka bir sayı aralığına eşitleme işlemidir.
    
    ```jsx
    function setTime(){
    	const time = new Date()
    	const month = time.getMonth()
    	const day = time.getDay() 
    	const date = time.getDate() 
    	const hours = time.getHours()
    	const hoursForClick = hours % 12
    	const minutes = time.getMinutes()
    	const seconds = time.getSeconds()
    
    	hourEl.style.transform = `translate(-50%, -100%) rotate(${scale(hoursForClock, 0, 11, 0, 360)}deg)`
    }
    
    const scale = (num, in_min, in_max, out_min, out_max) => {
    	return (num - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
    }
    ```
    
15. Yukarıda yaptığımız style işlemini, dakika ve saniye olaylarına da yapmak istiyorum.
    
    ```jsx
    function setTime(){
    	const time = new Date()
    	const month = time.getMonth()
    	const day = time.getDay() 
    	const hours = time.getHours()
    	const hoursForClick = hours % 12
    	const minutes = time.getMinutes()
    	const seconds = time.getSeconds()
    
    	hourEl.style.transform = `translate(-50%, -100%) rotate(${scale(hoursForClock, 0, 11, 0, 360)}deg)`
    	minuteEl.style.transform = `translate(-50%, -100%) rotate(${scale(minutes, 0, 59, 0, 360)}deg)`
    	secondEl.style.transform = `translate(-50%, -100%) rotate(${scale(seconds, 0, 59, 0, 360)}deg)`
    }
    
    const scale = (num, in_min, in_max, out_min, out_max) => {
    	return (num - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
    }
    ```
    
16. Her saniye çağırmak için kullanılan `setInterval()` fonksiyonunu Global Scope da çağırdığımız `setTime()` fonksiyonundan sonra kullanıyoruz. Ve kaç milisaniyede bir çağırılacağını belirtiyoruz. `setInterval (setTime, 1000)`
17. Bir sonraki Zaman ve Tarih yazılarını ayarlamaya geçmeden önce AM & PM ayarlamak için en yukarıda ampm adında bir değişken yaratalım. Bu durumu kontrol etmek için saati kontrol etmeliyiz saat eğer 12’den büyük veya eşit ise PM küçük ise AM yazdırılacak `const ampm = hours ≥ 12 ? ‘PM’ : ‘AM’`
18. setTime fonksiyonu içerisinde şimdi yukarıda tanımladığımız time içerisine güncel saati tanımlıyoruz. `timeEl.innerHTML = `${hoursForClock}:${minutes < 10 ? `0${minutes}` : minutes} ${ampm}`` 
19. Ve aynı şekilde dateEl için bir tanımlama yapıyoruz. İlk olarak günleri ve ardından ay ve kaçıncı tarihte olduğumuzu bildiriyoruz. `dateEl.innerHTML = `${days[day]}, ${months[month]} <span class="circle"> ${date} </span>``