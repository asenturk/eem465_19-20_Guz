**Ödev 1:**

Butonun basılı tutulması süresine göre LEDler yakılacaktır. Süre 2 tabanıda basamaklara karşılık gelen LEDler yakılarak gösterilecektir.

Yeşil LED $2^0$ basamağı
Turuncu LED $2^1$ basamağı
Kırmızı LED $2^2$ basamağı
Mavi LED $2^3$ basamağı olarak düşünülecektir. Dolayısıyla örneğin 12 saniye ve fazla 13 saniyeden az basılması durumunda 

12 saniye => 1100 dan dolayı Mavi ve Kırmızı LED yanacaktır. 


Butona basılma ve butondan elin çekilmesi interrupt ile algılanacak ve her zaman geçtikçe LEDler saniyeyi gösterecek şekilde yanacaktır. 

Zamanı ölçmek için SysTick timer kullanılacaktır.

Butona kaç saniye basılı tutulduğu bir değişkende tutulacak ve bu değişkenin bitleri taranarak hangi LEDlerin yanması gerektiği belirlenecektir. çok fazla if-else yapısı kullanmayınız.

---

Ödev  en son **24.11.2019** tarihinde Youtube'a yüklenip video linki aşağıdaki bağlantı adresinde bulunan forma eklenecektir.
Forma 1 adet cevap ekleyebilirsiniz. Bundan dolayı dikkatli olmanız gerekmektedir.
https://forms.gle/ipGVur4CWFbo588r7

Videonun görüntülenme iznini unlisted olarak ayarlayabilirsiniz.
Private olarak ayarlamayınız.

Video kendinizi tanıtmanızla ve bu videonun ne videosu olduğunu ifade etmenizle başlamalıdır.
Videonuzda, derste anlatıldığı gibi algoritmayı nasıl oluşturduğunuz, hangi yazmaçları nasıl ve ne için kullandığınız anlatılmalıdır. Debug edilerek de yapılan ayarlar gösterilebilir. Programın kart üzerinde çalıştığı gösterilmelidir. Videonun bir köşesinde sizin görüntünü olmalıdır. Video 5 dakika civarı olmalıdır. 8 dakikayı kesinlikle geçmesin. Video en az 720 en fazla 1080 boyutunda olmalıdır.





