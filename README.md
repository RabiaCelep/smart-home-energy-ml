#  Smart Home Energy – Machine Learning Projesi

Bu projede **Smart Home Dataset** veri seti kullanarak,
bir akıllı ev ortamındaki enerji tüketim davranışları analiz ettim.

Projenin temel amacı;
saat ve hava koşullarına bağlı olarak **normal (baseline) enerji tüketimini**
öğrenebilen bir makine öğrenmesi modeli geliştirmekti.

---

##  1. Veri Seti ve İlk Temizlik

Projede kullanılan veri seti oldukça büyük ve çok sayıda ölçüm içermekteydi.
İlk aşamada veri setini inceledim ve eksik (NaN) değerlere sahip satırları
tamamen temizledim.

Veri setinin hacmi büyük olduğu için,
eksik verilerin silinmesi modelin başarısını olumsuz etkilemedi.
Aksine, bu işlem sayesinde:

- Model daha temiz ve tutarlı verilerle eğitilmiş oldu. 
  
<img width="776" height="66" alt="Ekran görüntüsü 2026-01-19 220801" src="https://github.com/user-attachments/assets/dc8ad3aa-bc40-4fa7-acb8-e40f94700a44" />

---

##  2. Zaman Bilgisinin Dönüştürülmesi

Veri setindeki zaman bilgisi **saniye cinsinde** tutulmaktaydı.
Bu haliyle model için anlamlı ve kullanşlı bir bilgi değildi.

Bu nedenle zaman verisini:

- Okunabilir tarih formatına çevirip yalnızca saat bilgisini kullandım.


Bu işlem sayesinde model,
**günün hangi saatlerinde enerji tüketiminin arttığını veya azaldığını**
öğrenebilir hale gelmiş oldu.

Saat bilgisi, enerji tüketimi açısından
en önemli göstergelerden biri.

<img width="675" height="98" alt="Ekran görüntüsü 2026-01-19 220212" src="https://github.com/user-attachments/assets/411fd3e9-510d-4f87-953c-d96bfcd92c89" />


---

##  3. Hava Durumu Verilerinin Hazırlanması

Enerji tüketimini etkileyebileceğini düşündüğüm hava durumu değişkenleri:

- Sıcaklık  
- Nem  
- Rüzgar hızı  
- Basınç  
- Bulutluluk vb.  

Bu sütunları kullanılabilecek şekilde sayısal formata dönüştürdüm.
Böylece model, hava koşullarını doğrudan kullanabilir hale geldi.

<img width="584" height="202" alt="Ekran görüntüsü 2026-01-19 220224" src="https://github.com/user-attachments/assets/5013148a-753c-4c6d-9a93-15693e0f2a4f" />


---

##  4. Pivot Tablolar ile Baseline Oluşturma

Bu proje için **en kritik adım**, pivot tablolar kullanarak
**baseline (referans) enerji tüketiminin** oluşturulmasıydı.

###  Pivot Tablo Nedir?

Pivot tablo;
veri setindeki çok sayıda ölçümü
belirli bir kritere göre **gruplandırarak özetleyen**
bir veri analiz aracıdır.

Bu projede pivot tablolar:
- Saatlere göre
- Hava koşullarına göre  

**ortalama enerji tüketimini** hesaplamak için kullanıldı.

---

###  Pivot Tablolar Neden Kullanıldı?

Hocanın da özellikle vurguladığı gibi,
modelin her saat veya hava koşulu için
**normal tüketimi öğrenebilmesi** gerekmekteydi.

Pivot tablolar sayesinde:

- Her saat için ortalama tüketim belirlendi
- Her sıcaklık, nem ve rüzgar seviyesi için normal tüketim hesaplandı
- Bu değerler **sayısal bir referans noktası (baseline)** haline getirildi

Bu sayede model,
anlık tüketimi bu normal değerlerle karşılaştırabilir hale gelmiş oldu.

<img width="516" height="519" alt="Ekran görüntüsü 2026-01-19 220247" src="https://github.com/user-attachments/assets/503cede0-659e-4889-a4a8-28e330c83523" />


---

##  5. Baseline Değerlerin Modele Eklenmesi

Oluşturulan pivot tablolardan elde edilen baseline değerlerini,
ana veri setiyle birleştirdim.

Bu işlem sonucunda model:

- Sadece anlık veriye değil
- Aynı koşullardaki **normal tüketim bilgisine** de sahip olmuş oldu. 

Bu durum, özellikle tüketimi
**düşük / yüksek** olarak sınıflandırmada
çok işimize yaradı.

<img width="586" height="126" alt="Ekran görüntüsü 2026-01-19 220259" src="https://github.com/user-attachments/assets/a0895473-a960-4ca0-b297-daf5144bcd63" />


---

##  6. Hedef Değişkenin Oluşturulması

Enerji tüketimi:

- Ortalama değerin üzerindeyse **yüksek tüketim**
- Altındaysa **düşük tüketim**

olarak etiketlenmiş oldu.


---

##  7. Model Eğitimi

Veri seti:

- %80 eğitim
- %20 test  

olacak şekilde ayırdım.

Farklı makine öğrenmesi modelleri denedim ama 
en başarılı sonucu **Random Forest** modeli verdi.

<img width="526" height="265" alt="Ekran görüntüsü 2026-01-19 220319" src="https://github.com/user-attachments/assets/db215f1d-c88a-41dd-b68f-2e13b0353ed4" />

---

##  8. Sonuçlar ve Değerlendirme

- Random Forest modeli **%83’ün üzerinde doğruluk** elde etti
- En başarılı model olarak öne çıkmış oldu<

###  Özellik Önem Analizi

Modelin verdiği sonuçlara göre:

- Saat bilgisi
- Sıcaklık
- Nem
- Pivot tablolarla oluşturulan baseline değerleri  

enerji tüketimini **ciddi şekilde etkileyen faktörlerdir**.

Bu da pivot tablolarla oluşturulan baseline yaklaşımının
model performansına doğrudan katkı sağladığını göstermektedir.

---

##  Genel Değerlendirme

- Pivot tablolar, karmaşık veri setini sadeleştirmede çok etkilidir    
- Saat ve hava koşulları enerji tüketiminde belirleyici rol oynamaktadır  
- Random Forest, bu problem için en uygun model olmuştur  

---

##  Sonuç

Bu projede:

- Veri temizleme  
- Zaman ve hava durumu analizi  
- Pivot tablo kullanımı  
- Baseline oluşturma  
- Makine öğrenmesi modeli eğitimi  

adımları eksiksiz ve mantıklı bir sırayla uygulanmıştır.

![Sertifika](Makine_Öğrenmesi_Sertifika.pdf)



