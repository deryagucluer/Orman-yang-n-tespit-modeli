# Orman-Yangın-Tespit-Modeli
Wildfire Image Dataset üzerinde CNN + Keras Tuner (HPO) ile orman yangını tespiti.

## Giriş

Bu proje, **Wildfire Image Dataset** üzerinde görüntülerden **yangın / normal** ayrımı yapmayı amaçlar.  
Basit bir **CNN** modeli kurulmuş, **Keras Tuner** ile en iyi ayarlar (HPO) bulunmuş ve model testte doğrulandı.

- Görseller 224×224’e ölçeklendi, eğitimde hafif artırma (rotation/shift/zoom/flip) uygulandı.
- Doğrulama ve test için sadece ölçekleme kullanıldı.
- HPO ile filtre sayıları, dense üniteleri, dropout ve öğrenme oranı denendi.
- En iyi ayarlarla model tekrar eğitildi ve test sonuçları raporlandı.


## Metrikler
**Test (nihai):**
- PR-AUC: **0.988**  
- ROC-AUC: **0.973**     
- Accuracy: **91.18%**
- Loss: **0.2235**

**Karışıklık Matrisi (özet):**
- TP: **43** · TN: **19** · FP: **3** · FN: **3**

- **Ayrım gücü yüksek:** PR-AUC **0.988** ve ROC-AUC **0.973**, modelin pozitif sınıfı (yangın) güçlü biçimde ayırt ettiğini gösterir.  
- **Doğruluk ve hata dengesi:** Accuracy **%91.18**; FP ve FN yalnızca **3’er adet**. Bu dağılım, modelin hem yanlış alarmı hem de kaçan yangınları düşük seviyede tuttuğunu gösterir.  
- **Pozitif sınıf kalitesi:** CM’den **Precision ≈ Recall ≈ %93.5** (43/(43+3)), yani model yangın gördüğünde çoğunlukla doğru, yangınları da büyük oranda yakalıyor.  
- **Negatif sınıf:** **Specificity ≈ %86.4** (19/(19+3)); normal görüntüleri ayırt etme başarımı da yüksek.

**Özet:** Model, yangın tespitinde **yüksek güvenilirlikte** sonuç üretmektedir; hem ayrım gücü (AUC’ler) hem de sınıf bazlı hatalar pratik kullanım için **tatmin edici** düzeydedir.


## Sonuç ve Gelecek Çalışmalar
### Sonuç
- Model, Wildfire Image Dataset üzerinde **PR-AUC = 0.988**, **ROC-AUC = 0.973** ve **Accuracy = %91.18** elde etti.
- Karışıklık matrisi: **TP=43 · TN=19 · FP=3 · FN=3** — yanlış alarm ve kaçan yangın sayıları düşüktür.
- Doğrulama artırmadan ayrılarak (yalnızca ölçekleme) **temiz değerlendirme** yapıldı; sonuçlar modelin **güçlü ayrım gücüne** ve **tatmin edici genellemesine** işaret ediyor.
- Eğitim sürecinde **EarlyStopping / ReduceLROnPlateau / Checkpoint** kullanımı, aşırı uyumu sınırlayıp en iyi ağırlıkları korudu.

### Gelecek Çalışmalar
- **Eşik ayarı:** Amaç fonksiyonuna göre (erken tespit vs.) **0.4–0.6** bandında eşik optimizasyonu ile **FN/FP** dengesini hassaslaştırmak.
- **Transfer learning:** EfficientNet/MobileNet gibi önceden eğitilmiş ağlarla ince ayar (fine-tune) yaparak performansı artırmak.
- **Artırma & veri çeşitliliği:** Duman, farklı aydınlatma/gece, uzak alev gibi durumlar için senaryoya özel augmentasyon veya ek veri ile dayanıklılığı artırmak.
- **Hata analizi & açıklanabilirlik:** Yanlış sınıflanan örnekleri sistematik incelemek; **Grad-CAM** ile modelin odaklandığı bölgeleri görselleştirmek.
- **Sınıf ağırlıkları / örnekleme:** Dengesizlik daha belirgin olduğunda `class_weight` veya dengeli örnekleme stratejileri denemek.
- **Kalibrasyon:** Olasılık çıktılarının güvenilirliğini artırmak için Platt scaling / isotonic kalibrasyon değerlendirmek.
- **Değerlendirme genişletme:** (Vakit varsa) çapraz doğrulama veya farklı veri bölmeleriyle sağlamlık kontrolü; ROC/PR eğrilerini rapora eklemek.
- **Dağıtıma hazırlık:** Basit bir inference betiği/servisi (örn. Streamlit/Flask) ve model boyutu–gecikme ölçümleriyle kullanım senaryosuna uyarlama.
## Linkler:
  - **Veri Seti**: <https://www.kaggle.com/datasets/brsdincer/wildfire-detection-image-data>
  - <https://www.kaggle.com/code/deryagucluer/cnn-ile-orman-yang-n-tespiti>
