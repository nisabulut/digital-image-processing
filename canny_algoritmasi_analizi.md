# Canny Kenar Tespit Algoritması: Adımların Gerekliliği ve Analizi

Canny algoritması, basit bir kenar bulma filtresinden ziyade birbirine sıkı sıkıya bağlı 4 aşamalı bir süreçtir. Bu algoritmayı sıfırdan yazarken her bir adımın aslında bir sonraki adımın çökmesini engellemek için tasarlandığını görebiliyoruz. Aşağıda bu adımların neden zorunlu olduğunu özetledim:

### 1. Gürültü Bastırma (5x5 Gauss Filtresi)
**Neden gerekli?** 
Kenar tespiti temelde bir türev (matematiksel değişim hızı) işlemidir. Türev alma işlemi doğası gereği görüntüdeki yüksek frekanslı sinyallere, yani gürültülere (noise) karşı inanılmaz derecede hassastır. Eğer bu adımı atlayıp doğrudan kenar bulmaya çalışırsak, kamera sensöründeki ufak bir kirlilik veya kumlanma bile devasa bir kenar olarak algılanır. Gauss filtresi ile resmi hafifçe bulanıklaştırarak bu sahte detayları eziyoruz ve sadece gerçek nesne sınırlarının kalmasını sağlıyoruz.

### 2. Gradyan Hesaplama (Sobel Filtresi)
**Neden gerekli?**
Kenarların nerede olduğunu bulmak için resimdeki parlaklık değişiminin şiddetini (magnitude) bulmamız şart. Ancak Canny algoritması için sadece şiddet yetmez; değişimin **hangi yöne** doğru olduğunu (açısını) da bulmamız gerekir. Sobel operatörü ile hem X hem de Y eksenindeki değişimleri hesaplayarak piksellerin eğim açısını (0-180 derece arası) elde ediyoruz. Bu açı bilgisi olmasaydı, bir sonraki adımda çizgileri inceltmemiz imkansız olurdu.

### 3. Non-Maximum Suppression (Maksimum Olmayanların Bastırılması)
**Neden gerekli?**
Sobel filtresinden çıkan kenarlar çok kalın ve yayvandır. Gerçek dünyada bir nesnenin kenarı kalın bir şerit olamaz. Bizim tek piksellik, ince ve kesin çizgilere ihtiyacımız var. Bu adımda, bir önceki aşamada bulduğumuz açı bilgisini kullanarak her pikseli sadece kendi yönündeki komşularıyla (örneğin kenar dikeyse, sağındaki ve solundaki piksellerle) kıyaslıyoruz. Eğer merkezdeki piksel bulunduğu bölgedeki en parlak (en güçlü) piksel değilse onu siyah (0) yapıp eliyoruz. Böylece o kalın şeritler tıraşlanıp ipince ve net çizgiler haline geliyor.

### 4. Hysteresis Thresholding (Çift Eşikleme)
**Neden gerekli?**
Kenarları bulduk ve incelttik, ancak tek bir sabit eşik (threshold) değeri kullansaydık işimiz şansa kalırdı. Işık değişimlerinden veya gölgelerden dolayı bazı gerçek kenarların değeri düşük çıkabilir. Tek bir yüksek eşik belirlersek çizgiler kopuk kopuk olur (kesik çizgiler). Tek bir düşük eşik belirlersek de her yer sahte kenarla dolar.
Canny bunun yerine iki eşik (alt ve üst) kullanır:
- Üst eşiği geçenleri "kesin kenar" kabul ederiz.
- Alt ve üst eşik arasında kalanları "zayıf kenar" kabul ederiz. 
Eğer bu zayıf kenarlar, kesin bir kenara fiziksel olarak temas ediyorsa (8-yönlü komşuluk), onları da kurtarıp çizgiye dahil ederiz. Bağlantısız, havada asılı kalan zayıf pikselleri ise gürültü sayıp sileriz. Bu sayede kopmayan, sürekli kenar hatları elde etmiş oluruz.
