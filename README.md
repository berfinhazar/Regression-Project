# Academic Text Complexity Regression: Hibrit Özellik Mühendisliği

Bu proje, akademik tez özetlerinin yapısal ve anlamsal özelliklerini analiz ederek metnin **"Karmaşıklık İndeksi" (Complexity Index)** değerini tahmin etmeyi amaçlar. Sadece kelime sayısına değil, metnin akademik derinliğine odaklanan bir regresyon çalışmasıdır.

---

### Özellik Mühendisliği (Feature Engineering)
Bu projenin kalbi, metinlerden türetilen **TextFeatureExtractor** sınıfıdır. Ham metinler şu 6 ana başlıkta sayısallaştırılmıştır:
* **Okunabilirlik Skoru:** Flesch Reading Ease uyarlaması.
* **Cümle Karmaşıklığı:** Ortalama cümle uzunluğu ve yapısal analiz.
* **Leksikal Çeşitlilik:** Unique kelime oranı (TTR).
* **Teknik Yoğunluk:** Uzun ve akademik terimlerin kullanım sıklığı.
* **Edilgen Yapı Oranı:** Akademik dildeki pasif cümle kullanımı.
* **Bağlaç Yoğunluğu:** 'Dolayısıyla', 'nitekim' gibi akademik bağlaçların analizi.

---

### Model Performans Tablosu

TF-IDF vektörleri ile yukarıdaki yapısal özelliklerin birleştirildiği (stacking) hibrit veri seti üzerinde alınan sonuçlar:

| Model | R² Score | MAE (Hata Payı) | RMSE |
| :--- | :---: | :---: | :---: |
| **CatBoost** | **0.6071** | **0.9569** | 1.4687 |
| **Voting Ensemble** | **0.5918** | **0.9863** | 1.4970 |
| **SVR** | **0.5593** | **1.0499** | 1.5554 |
| **XGBoost (Optimized)** | **0.4214** | **1.1373** | 1.7822 |

---

### Öne Çıkan Teknikler
* **Optuna:** XGBoost modeli üzerinde meta-sezgisel hiperparametre optimizasyonu yapılmıştır.
* **Hybrid Vectorization:** Standart TF-IDF vektörleri, elle türetilen (manual) yapısal özelliklerle `hstack` yöntemiyle birleştirilmiştir.
* **CatBoost Dominance:** Karmaşık ve gürültülü metin özelliklerinde CatBoost'un en yüksek genelleme kapasitesine sahip olduğu kanıtlanmıştır.

---

### Çıkarımlar
* **Fizik vs. Diğerleri:** Yapılan analizde Fizik alanındaki tezlerin, teknik yoğunluk ve cümle uzunluğu bakımından en yüksek karmaşıklık indeksine sahip olduğu saptanmıştır.
* **Anlamsal Tahmin:** Modelin 0.95 MAE değeri ile hata payı 1 puanın altına indirilmiş, akademik dildeki "ağırlık" sayısal olarak doğrulanmıştır.
