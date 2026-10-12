# Sobel ve Prewitt Operatörleri Karşılaştırma Raporu

Görüntü işleme dersi kapsamında gerçekleştirilen bu çalışmada, dijital görüntüler üzerinde kenar tespiti (edge detection) işlemleri için yaygın olarak kullanılan iki temel gradyan tabanlı filtre incelenmiştir: Prewitt ve Sobel operatörleri. Bu operatörler, görüntüdeki piksellerin parlaklık (yoğunluk) değişimlerini matematiksel türev yöntemleriyle hesaplayarak nesnelerin sınırlarını (kenarlarını) belirginleştirmeyi amaçlar.

## 1. Operatörlerin Temel Yapısı ve Çalışma Mantığı

Her iki operatör de yatay (X ekseni) ve dikey (Y ekseni) gradyanları hesaplamak için 3x3 boyutunda konvolüsyon matrisleri (çekirdek/kernel) kullanır. 

*   **Prewitt Operatörü:** Piksellerin değişim oranını hesaplarken komşu piksellere eşit ağırlık verir. Örneğin, X ekseni matrisinde sütun değerleri `-1, 0, 1` şeklinde dağılırken, Y ekseninde satır değerleri bu şekildedir. Ortalama alma işlemi homojendir.
*   **Sobel Operatörü:** Prewitt'e çok benzer bir yapıda olmakla birlikte, merkez piksele daha fazla ağırlık (2 katsayısı) verir. Bu durum, Sobel filtresinin doğası gereği gradyan hesaplarken aynı zamanda hafif bir Gauss yumuşatması (smoothing) yapmasını sağlar.

## 2. Uygulama Sonuçlarının Kıyaslanması

Tuğla duvar görseli üzerinde yapılan pratik uygulamalar sonucunda iki filtre arasında belirgin farklar gözlemlenmiştir:

*   **Gürültüye (Noise) Karşı Duyarlılık:** Görüntüler doğası gereği sensör gürültüsü veya ince dokusal pürüzler içerir. Prewitt filtresi tüm komşu piksellere eşit ağırlık verdiği için bu küçük pürüzleri de birer "kenar" olarak algılama eğilimindedir. Sobel operatörü ise merkez ağırlıklı yapısı sayesinde bu gürültüleri filtrelemede daha başarılıdır ve sahte kenar (false edge) oluşumunu minimize eder.
*   **Kenar Keskinliği ve Kalınlığı:** Sobel filtresinin uygulandığı sonuçlarda, tuğla derzleri ve sınır hatları Prewitt'e göre daha kalın, parlak ve belirgin çıkmıştır. Prewitt sonuçları ise daha keskin ancak görsel olarak daha zayıf gradyan tepkileri vermiştir.
*   **Gauss Filtresi ile Entegrasyon:** Çalışmanın son aşamasında Sobel filtresi uygulanmadan önce görüntüye 3x3 Gauss filtresi uygulanmıştır. Bu ön işlem (blurring), yüksek frekanslı gürültüleri tamamen baskılamış ve Sobel operatörünün yalnızca gerçek yapısal kenarlara (tuğlaların ana yatay ve dikey hatlarına) odaklanmasını sağlayarak en temiz sonucu üretmiştir.

## Sonuç

Her iki operatör de hesaplama maliyeti düşük ve temel kenar tespiti işlemleri için oldukça etkilidir. Ancak, gerçek dünya görüntülerinde genellikle gürültü bulunduğu için, merkez piksele ağırlık vererek dahili bir yumuşatma sunan **Sobel operatörü**, Prewitt operatörüne kıyasla daha pürüzsüz, güvenilir ve başarılı sonuçlar vermiştir. Daha temiz bir kenar haritası elde etmek için operatör öncesi Gauss gibi yumuşatma filtrelerinin kullanılması işlemin kalitesini doğrudan artırmaktadır.
