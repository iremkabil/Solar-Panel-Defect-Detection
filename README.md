# 🌞 Solar Panel Defect Detection with CNN & Transfer Learning

Bu proje, güneş paneli görüntülerindeki gizli kusurları derin öğrenme yöntemleriyle sınıflandırmak amacıyla hazırlanmıştır. Projede **SPHERE — Solar Panel Hidden Defect Evaluation for Renewable Energy** veri seti kullanılarak panel görüntüleri `broken`, `clean` ve `dirty` olmak üzere 3 sınıfa ayrılmıştır.

Proje kapsamında sıfırdan tasarlanmış bir CNN modeli ve transfer learning tabanlı modern CNN mimarileri karşılaştırılmıştır.

---

## 📌 Proje Amacı

Güneş panellerinde oluşabilecek kırık, kirlenme ve normal/temiz durumların görüntü tabanlı olarak otomatik tespit edilmesi hedeflenmiştir.

Bu problem, yenilenebilir enerji sistemlerinde bakım süreçlerinin daha hızlı, düşük maliyetli ve otomatik hale getirilmesi açısından önemlidir. Modelin doğru sınıflandırma yapması sayesinde:

- Kırık veya hasarlı paneller erken tespit edilebilir.
- Kirli paneller için bakım/temizlik planlaması yapılabilir.
- Temiz paneller gereksiz bakım süreçlerinden ayrıştırılabilir.
- Endüstriyel güneş paneli denetimlerinde karar destek sistemi geliştirilebilir.

---

## 🗂️ Veri Seti

**Veri Seti:** SPHERE — Solar Panel Hidden Defect Evaluation for Renewable Energy  
**Kaggle:** [SPHERE Dataset](https://www.kaggle.com/datasets/kubilayayturan/sphere-solar-panel-hidden-defect-evaluation-for-re/data)  
**Referans Makale:** Ayturan et al. (2025), *Applied Sciences*, 15(9), 4880

### Sınıflar

| Sınıf | Açıklama | Label |
|---|---|---|
| `broken` | Kırık / hasarlı panel görüntüleri | 0 |
| `clean` | Temiz / normal panel görüntüleri | 1 |
| `dirty` | Kirli panel görüntüleri | 2 |

Notebook içinde veri seti Kaggle ortamında otomatik olarak şu yapıda aranır:

```text
/kaggle/input/
└── .../
    └── CNN_Data_Arranged/
        └── 320p/
            ├── broken/
            ├── clean/
            └── dirty/
```

---

## 🧠 Kullanılan Modeller

Projede 4 farklı derin öğrenme mimarisi denenmiştir:

| Model | Yaklaşım | Açıklama |
|---|---|---|
| Özel CNN | Sıfırdan eğitim | Conv2D, BatchNorm, MaxPooling, Global Average Pooling, Dropout ve L2 regularization kullanıldı. |
| EfficientNetB0 | Transfer Learning | ImageNet ağırlıklarıyla başlatıldı, iki aşamalı eğitim ve fine-tuning uygulandı. |
| MobileNetV3Large | Transfer Learning | Mobil/edge cihazlara uygun hafif mimari olarak değerlendirildi. |
| ConvNeXt-Tiny | Transfer Learning | Modern CNN mimarisi olarak projeye dahil edildi. |

---

## ⚙️ Kullanılan Teknolojiler

- Python
- TensorFlow / Keras
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- OpenCV
- SciPy
- Kaggle Notebook

---

## 🔄 Proje Akışı

Notebook genel olarak aşağıdaki adımlardan oluşmaktadır:

1. **Kurulum ve kütüphane importları**
2. **Veri setinin Kaggle ortamında otomatik bulunması**
3. **Görüntü yollarının ve sınıf etiketlerinin hazırlanması**
4. **Keşifsel veri analizi**
   - Sınıf dağılımı
   - Örnek görüntüler
   - Piksel yoğunluk dağılımları
5. **Veri ön işleme**
   - Görüntü boyutlandırma
   - Train / validation / test ayrımı
   - Stratified split
6. **Data augmentation**
   - Rotation
   - Shift
   - Zoom
   - Geometrik dönüşümler
7. **Model eğitimi**
   - Özel CNN
   - EfficientNetB0
   - MobileNetV3Large
   - ConvNeXt-Tiny
8. **Model değerlendirme**
   - Classification report
   - Confusion matrix
   - ROC-AUC
   - Accuracy, precision, recall, F1-score
9. **Model karşılaştırması**
10. **Grad-CAM / açıklanabilir yapay zeka görselleştirmesi**

---

## 🧪 Veri Bölme

Veri seti stratified split yaklaşımıyla ayrılmıştır. Böylece her alt veri kümesinde sınıf oranları korunmuştur.

| Veri Kümesi | Oran |
|---|---:|
| Train | %70 |
| Validation | %15 |
| Test | %15 |

Notebook içinde örnek dağılım:

| Split | Görüntü Sayısı |
|---|---:|
| Train | 4255 |
| Validation | 912 |
| Test | 912 |

---

## 📊 Model Sonuçları

Aşağıdaki tablo notebook içerisinde raporlanan test sonuçlarına göre hazırlanmıştır.

| Model | Accuracy | Weighted Precision | Weighted Recall | Weighted F1-Score | Weighted ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Özel CNN | 0.81 | 0.85 | 0.81 | 0.81 | 0.9699 |
| EfficientNetB0 | 0.88 | 0.88 | 0.88 | 0.88 | 0.9702 |
| MobileNetV3Large | 0.87 | 0.87 | 0.87 | 0.87 | 0.9689 |
| ConvNeXt-Tiny | Notebook içinde eğitim ve değerlendirme akışı mevcut | - | - | - | - |

> Not: ConvNeXt-Tiny için eğitim ve değerlendirme hücreleri notebook içinde yer almaktadır. Ancak yüklenen notebook dosyasında bu modelin nihai test çıktısı kayıtlı olmadığı için tabloya kesin metrik değeri yazılmamıştır. Notebook yeniden çalıştırıldığında karşılaştırmalı sonuç tablosuna eklenebilir.

---

## 🔍 Önemli Bulgular

- Veri setindeki sınıflar tamamen dengeli değildir ancak ciddi bir dengesizlik de bulunmamaktadır.
- `broken` sınıfı görsel olarak daha belirgin lokal hasar desenleri içerdiği için modeller tarafından genellikle güçlü şekilde öğrenilmiştir.
- `clean` ve `dirty` sınıfları arasında karışıklık oluşabilmektedir.
- Bu karışıklığın temel nedeni, panel yüzeyindeki kirliliğin keskin sınırlara sahip olmaması ve bazı görüntülerde temiz/kirli ayrımının görsel olarak belirsizleşmesidir.
- Transfer learning modelleri, sıfırdan eğitilen özel CNN modeline göre daha kararlı ve yüksek performans üretmiştir.
- EfficientNetB0, genel metrikler açısından en güçlü sonuçlardan birini vermiştir.
- MobileNetV3Large daha hafif bir model olmasına rağmen rekabetçi sonuçlar üretmiştir.
- Grad-CAM görselleştirmeleri, modelin karar verirken görüntünün hangi bölgelerine odaklandığını yorumlamaya yardımcı olmuştur.

---

## 🧩 Grad-CAM ile Açıklanabilirlik

Projede modellerin kararlarını yorumlamak için Grad-CAM benzeri ısı haritası görselleştirmeleri kullanılmıştır.

Bu sayede:

- Modelin hangi bölgelere dikkat ettiği incelenmiştir.
- Yanlış sınıflandırmalarda modelin hangi görsel desenlerden etkilendiği analiz edilebilmiştir.
- Model kararları daha açıklanabilir hale getirilmiştir.

Grad-CAM çıktılarında sıcak renkler modelin daha fazla odaklandığı bölgeleri, soğuk renkler ise karar sürecinde daha az etkili bölgeleri temsil eder.

---

## 🚀 Çalıştırma Adımları

### 1. Repository'yi klonla

```bash
git clone https://github.com/kullanici-adi/repository-adi.git
cd repository-adi
```

### 2. Kaggle Notebook ortamını kullan

Bu proje Kaggle ortamına göre düzenlenmiştir. Notebook'u Kaggle üzerinde açıp veri setini notebook'a eklemek en pratik yöntemdir.

Kaggle'da:

1. Notebook'u aç.
2. Sağ panelden **Add Data** seçeneğine tıkla.
3. SPHERE veri setini ekle.
4. Notebook hücrelerini sırayla çalıştır.

### 3. Gerekli kütüphaneler

Notebook içinde bazı kurulumlar otomatik yapılmaktadır. Yerel ortamda çalıştırmak için temel paketler:

```bash
pip install tensorflow numpy pandas matplotlib seaborn scikit-learn opencv-python scipy tf-keras-vis
```

---

## 📁 Önerilen Proje Yapısı

```text
solar-panel-defect-detection/
│
├── solar-panel-defect-detection-cnn-convnext-tiny.ipynb
├── README.md
│
├── saved_models/          # Eğitim sonrası oluşan model dosyaları
├── outputs/               # Grafikler ve değerlendirme çıktıları
└── requirements.txt       # İsteğe bağlı bağımlılık dosyası
```

> `saved_models/` ve büyük veri dosyaları GitHub'a yüklenmeyebilir. Veri seti Kaggle üzerinden projeye eklenmelidir.

---

## 📌 Değerlendirme Metrikleri

Projede model performansını değerlendirmek için aşağıdaki metrikler kullanılmıştır:

- **Accuracy:** Toplam doğru tahmin oranı
- **Precision:** Modelin pozitif dediği tahminlerdeki doğruluk oranı
- **Recall:** Gerçek pozitif örnekleri yakalama oranı
- **F1-Score:** Precision ve recall değerlerinin dengeli ortalaması
- **ROC-AUC:** Sınıflar arasındaki ayırt etme gücü
- **Confusion Matrix:** Hangi sınıfların birbirine karıştığını gösteren tablo

Sadece accuracy metriğine bakmak yeterli değildir. Özellikle `broken` ve `dirty` sınıfları için recall değeri bakım ve güvenlik açısından önemlidir.

---

## ✅ Sonuç

Bu projede güneş paneli kusur tespiti problemi için farklı CNN tabanlı derin öğrenme modelleri karşılaştırılmıştır. Transfer learning yaklaşımı, sınırlı veriyle daha güçlü temsil öğrenimi sağladığı için sıfırdan eğitilen CNN modeline kıyasla daha başarılı sonuçlar üretmiştir.

En dikkat çekici sonuçlardan biri, `clean` ve `dirty` sınıfları arasındaki görsel benzerliğin modeller için zorlayıcı olmasıdır. Bu durum, gerçek endüstriyel uygulamalarda yalnızca genel doğruluğun değil, sınıf bazlı hata analizinin de önemli olduğunu göstermektedir.

---

## 👤 Hazırlayan

**İrem Kabil**  
Bilgisayar Mühendisliği  
Derin Öğrenme Projesi
