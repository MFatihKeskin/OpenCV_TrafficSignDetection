# OpenCV_TrafficSignDetection

## ABSTRACT (ÖZET)
Bu projede “Python” programlama dili ile görüntü işleme dersi kapsamında görüntü üzerindeki trafik işaretlerinin/levhalarının tespit edilmesi konusu üzerinde çalışılacaktır. Seçilen 8 adet görüntüye eşikleme[1], ayrıt saptama[2], morfolojik operatörler[3], çizgi/elips/daire tespiti[4],filtreleme[5] gibi yöntemler uygulanarak incelenecektir. Son olarak öznitelik çıkartımı[6] yöntemi sayesinde trafik işaretlerinin tespiti yapılacaktır.

## 1. Giriş
Projede, birbirinden farklı 8 ayrı görüntü üzerinde tek bir algoritma uygulanarak trafik işaretlerinin tespit edilmesi amaçlanmıştır. Görüntüler, birbirlerinden farklı oldukları için farklı koordinatlarda, farklı piksel değerlerinde ve farklı trafik işaretlerine sahip olacaklardır. İlgili parametrelerden ve yöntemlerden faydalanılarak, görüntülerin tek bir algoritma sayesinde analizlerinin yapılması amaçlanmıştır. Bu analizler sayesinde, çeşitli görüntü işleme yöntemleri uygulanacaktır. Analizler sonucu uygun sıralama ile görüntü işleme yöntemleri kullanılarak, hedefe uygun şekilde trafik işaretlerinin tespiti sağlanmış olacaktır.

## 2. Kullanılan Yöntemler
Bilgisayar ortamında görüntü işleme için, Python Programlama Dili’nin bir aracı olan Jupyter Notebook kullanılacaktır.
Trafik işaretlerinin çeşitlerinden kaynaklı olarak birçok farklı şekil içeren trafik işareti bulunmaktadır. Sadece Türkiye’de bulunan trafik işaretlerinin örnek alınması durumunda ters üçgen, üçgen, eşkenar dörtgen, kare, dikdörtgen, sekizgen ve daire şeklinde bulunabilirler.
Bu durumda tek algoritma sayesinde bütün trafik işaret levhalarının algılanması için bütün olabilecek şekiller kontrol edilmelidir ve bulunan her benzer şekli üzerinde analizler yapılmalıdır.

## 2.1. Giriş Görüntülerinin Okunması
Analizlerin yapılabilmesi ve yöntemlerin uygulanabilmesi için, giriş görüntüleri .jpg formatında Jupyter Notebook ortamına eklenmiştir.
Eklenen görüntüler renkli olduğu için gri seviyeye indirgenmiştir.
Son olarak gri seviyeye indirgenmiş görüntünün genişliği 600 piksel olarak ayarlanmıştır. Bunun sebebi daire şekline sahip trafik işareti bulurken kullanılan çap boyutunu bir standarda eşitlemek içindir. Bu işlev sayesinde, seçilen görüntünün genişliği ve uzunluğu giriş görüntüsüyle orantılı olarak ayarlanır. Örneğin; giriş görüntü genişliği (1200,600) ise resize fonksiyonu için (width=600) seçildiği durumda yeni oluşturulan görüntü boyutları (600,300) olacaktır.

## 2.2. Top-Hat Dönüşümü
Gri seviyeye indirgenen görüntüler üzerinde ön ve arka planın ayırt edilebilmesi için Top-Hat Dönüşümü uygulanmıştır. Top-Hat Dönüşümü ile trafik işaretinin koyu renkteki arka planlarının ve içerisindeki açık bölgeleri veya tam tersi durum özelliği kullanılarak, trafik işareti olamayacak bölgeleri bastıran, trafik işareti olabilecek bölgeleri ön plana çıkartılmıştır. Kısaca içerisinde sadece trafik işaretleri veya levhaları bulunan bir görüntü düşünürsek, arka planı koyu renkli iken bulunacak olan şekil daha açık renkli olacaktır (Örn: Şekil 1). Veya tam tersi durumlardaki renkler için de geçerli olacaktır.

![image](https://user-images.githubusercontent.com/70964563/152916123-93c76ad7-a5c1-4ee4-a678-052f12816a6a.png)

Top-Hat Dönüşümü arka plandan farklı aydınlık seviyeli nesneleri araştıran gri seviyeli resimlerin segmentasyonunda kullanılan bir dönüşümdür. Gri seviyeli morfolojik işlemler kullanılarak elde edilir. Tepe veya çukur bölgeleri belirginleştirme özelliğine sahiptir.
A giriş görüntüsü ve B yapı elemanı iken;
• Aydınlık bölgeler için,
𝑇𝑜𝑝𝐻𝑎𝑡[𝐴,𝐵] = 𝐴 − (𝐴𝑜𝐵) (denklem-1)
• Karanlık bölgeler için,
𝑇𝑜𝑝𝐻𝑎𝑡[𝐴,𝐵] = (𝐴•𝐵)−𝐴 (denklem-2)
şeklinde hesaplanmaktadır.

• Genişletme İşlemi (dilation): 
İkili imgedeki nesneyi büyütmeye ya da kalınlaştırmaya yarayan morfolojik işlemdir. Sayısal bir resmi genişletmek resmi yapısal elemanla kesiştiği bölümler kadar büyütmek demektir. Kalınlaştırma işleminin nasıl yapılacağını yapı elemanı belirler. ( "⊕" ile gösterilir)
• Aşındırma işlemi (erosion): 
İkili imgedeki nesneyi küçültmeye ya da inceltmeye yarayan morfolojik işlemdir. Aşındırma işlemi bir bakıma genişletmenin tersi gibidir. Aşındırma işlemi ile sayısal resim aşındırılmış olur. Yani resim içerisindeki nesneler ufalır, delik varsa genişler, bağlı nesneler ayrılma eğilimi gösterir. ( "⊖" ile gösterilir)
• Açma işlemi (opening): 
Genişletme ve aşındırma işlemini ardışıl uygulanmasıyla elde edilir. Bu işlemle birbirine yakın iki nesne görüntüde fazla değişime sebebiyet vermeden ayrılmış olurlar. ( "𝑜" ile gösterilir)
• Kapama işlemi (closing) : 
Aşındırma ve genişletme işleminin ardışıl uygulanmasıyla da kapama işlemi elde edilir. Dolayısıyla birbirine yakın iki nesne görüntüde fazla değişiklik yapılmadan birbirine bağlanmış olur. ( "•" ile gösterilir)

Top-Hat Dönüşümü Şekil 2’deki gibi, açma işlemi ile orijinal resmin farkı alınarak bulunmaktadır.
![image](https://user-images.githubusercontent.com/70964563/152916582-f81ecaa0-d675-4a26-ab87-360f2b511d91.png)

## 2.3. Otsu Eşikleme Metodu
Top-Hat Dönüşümü uygulanmış gri seviyeli görüntü sayesinde ilgilenilen açık renkli objeler belirginleşirken, bir sonraki adımda ise Otsu Metodu uygulanmıştır. Otsu metodu, gri seviyeli bir görüntü ikili seviyeye indirgenirken kullanılabilecek en uygun eşik değerinin tespit edilmesini sağlar. Normalde gri bir görüntüyü ikili biçime dönüştürmek için bir eşik değeri belirlenir ve bu eşik değeri üstündeki renkler beyaza, altındaki renkler siyaha eşitlenir. Ancak tüm görüntüler aynı niteliğe sahip olmadığı için sabit bir eşik değeri tüm görüntüler için uygun olmayacaktır. Otsu eşik değeri, görüntünün renk dağılımına uygun olarak belirlemektedir. Bu nedenle bir ikili seviyeye indirgenen bir görüntü için en uygun eşik değerini bulacak ve seçilen 8 adet fotoğraf için her birine farklı bir eşik değeri atayacaktır.
Otsu metodu görüntüyü ön ve arka plan olarak iki gruba ayırmaktadır. Bu sayede artık ön ve arka plan olarak ikiye ayrılmış görüntü üzerindeki piksel değerleri ya 0 değerine ya da 255 değerine sahip olacaktır.
Bu durumda oluşan siyah ve beyaz renkler ile görüntü üzerindeki ilgi alanları daha da belirginleştirilmiş olmaktadır.

Otsu Eşikleme Yöntemi:

➔ Giriş görüntüsü okunur ve görüntünün histogramı oluşturulur. p(i) normalize histogramdır.

➔ Bulunan histogram grafiğinden veya dağılımından bakılarak grupların ağırlıkları olan q1(t) ve q2(t) hesaplanır.

➔ Bulunan p(i) ve q1(t) ve q2(t) kullanılarak her grubun ortalama değeri olan μ1(t) ve μ2(t) hesaplanır.

➔ Bulunan değerler kullanılarak her grubun varyansı hesaplanır. σ2(t)= σ2w(t)+ σ2b(t) formülü kullanılarak σ2b(t) maksimum değeri alması sağlanır.

➔ Bu sayede σ2w(t) = q1(t)* σ12(t) + q2(t)* σ22(t) değerinin minimum olması sağlanır ve eşikleme değeri σ2w(t) olarak belirlenir.

Bu işlemler sonucunda gri seviyeli bir görüntü ikili seviyeye indirgenirken kullanılabilecek en uygun eşik değeri tespit edilmiştir. Ayrıca grupların kendi içindeki standart sapmaları küçüktür ve gruplar birbirinden iyi derecede ayrılmış olarak elde edilmiştir.

## 2.4. Morfolojik Gradient
Otsu Eşikleme Metodu uygulanmış görüntüye bu adımda ise Morfolojik Gradient işlemi uygulanmıştır. Morfolojik Gradient, bir görüntünün genişlemesi (dilation) ve aşınması (erosion) arasındaki farkı temsil eder (denklem-3).
A giriş görüntüsü ve B yapı elemanı iken; 𝐺𝑟𝑎𝑑𝑖𝑒𝑛𝑡(𝑓) =(𝐴⊕𝐵) − (𝐴⊖𝐵) (denklem-3)
Morfolojik Gradient uygulanan görüntüdeki piksel değerleri yakınındaki piksel değerlerinin kontrast yoğunluğunu gösterir. Bu işlem sonucunda görüntüdeki kenarlar daha kalın elde edilmiştir. Morfolojik Gradient sayesinde her piksel değerinin (tipik olarak negatif olmayan) o pikselin yakın çevresindeki kontrast yoğunluğunu gösterdiği bir görüntüdür. Kenar algılama ve segmentasyon uygulamaları için kullanışlıdır.

➔ Morfolojik yöntemleri uygulamak için yapı elemanı kullanılmalıdır. Bu yapı elemanı 3x3 boyutlarında ve kare şeklinde seçilmiştir.

➔ Sonraki adımda genişletme işlemi (dilation) sayesinde denklemin ilk görüntüsü elde tutulur. dilate fonksiyonu sayesinde oluşturulan 3x3 boyutlu yapı elemanı olan kernel ile eşiklenmiş görüntü genişletilir.

➔ Aşındırma işlemi (erosion) sayesinde denklemin ikinci görüntüsü elde tutulur. erode fonksiyonu sayesinde oluşturulan 3x3 boyutlu yapı elemanı olan kernel ile eşiklenmiş görüntü aşındırılır.

➔ Denklemden de yararlanılarak sonraki adımda ise her iki görüntü birbirinden çıkartılır ve “gradient” sonucu elde edilir.

➔ Morfolojik gradient işleminin son adımında ise elde edilen “gradient” görüntüsü giriş görüntüsünden çıkarılır ve new_grad görüntüsü elde edilir.


## 2.5. Canny Kenar Dedektörü
Morfolojik Gradient uygulanmış görüntüye bu adımda ise Canny Metodu uygulanmıştır. Canny Metodu, görüntü üzerindeki kenar tespiti ile o görüntüdeki nesneler tespit edilebilir, sayısı çıkartılabilir ve özellikleri belirlenebilir. Ayrıca Canny Metodu görüntüdeki kenarları algılamak için kullanılan en popüler yöntemdir. Kenarları algılamak için birden fazla işlem uygulanır ve Canny çıkış görüntüsü elde edilir. 
Bu işlemler:

• Gürültü Azaltma: 
Kenar algılama sonuçları görüntü gürültüsüne karşı oldukça hassastır. Görüntüdeki gürültüden kurtulmanın yollarından birisi onu yumuşatmaktır. Gürültüyü azaltmak için orijinal görüntüye 5x5 Gaussian filtresi uygulandı. Gaussian filtresinin matematiksel formülü 4. denklemde belirtilmiştir.
![image](https://user-images.githubusercontent.com/70964563/152916443-7437f3f8-db01-4001-bece-f27eef1c9289.png)

• Gradyan Hesaplaması: 
Kenar algılama operatörlerini kullanarak görüntünün gradyanını hesaplayarak kenar yoğunluğunu ve yönünü tespit eder. Kenarlar, piksel yoğunluğunun değişmesine karşılık gelir. Bunu hesaplamanın en etkili yolu görüntüye “Sobel” filtresi uygulamaktır. Yoğunluk değişimini her iki yönde tespit etmek için x yönünde ve y yönünde farklı filtreler uygulanır. Bu filtreler Şekil 3’teki gibidir. Daha sonra elde edilen Gx ve Gy görüntülerinin mutlak değerleri toplanarak Gradyan hesaplanır.
![image](https://user-images.githubusercontent.com/70964563/152916477-85f3643a-dcb1-40da-8b23-f461379a02c9.png)

• Maksimum Olmayanı Bastırma: 
Yukarıdaki adımlardan sonra görüntünün ince kenarlı olmalıdır. Kenarları inceltmek için maksimum olmayanı bastırma işlemi uygulanmalıdır.
Algoritma, gradyan yoğunluk matrisi üzerindeki tüm noktalardan geçer ve kenar yönlerindeki maksimum değere sahip pikselleri bulur. Detay olarak gradyan yoğunluk görüntüsü üzerindeki tüm pikselleri tarayan matris başlatılır. Gradyan’dan yararlanılarak açı değerine göre kenar yönü belirlenir. Açı yönünde bulunan piksellerin, seçilen pikselden daha yüksek yoğunluğa sahip olup olmadığı kıyaslanır ve maksimum olmayan pikseller bastırılır. Şekil 4’teki görüntüdeki gibi kıyaslama yapılır.
![image](https://user-images.githubusercontent.com/70964563/152916972-88312f73-4581-40fb-a45d-0166ac618e48.png)

• Çift Eşik: 
Çift eşik 3 tür piksel bulmayı amaçlar. Güçlü, zayıf ve alakasız olarak sınıflandırılabilir. Güçlü pikseller için yüksek eşik değeri kullanılır ve kenara katkıda bulunduğu kabul edilir. Alakasız pikseller için düşük eşik değeri kullanılır ve kenar için alakasız olarak kabul edilir.

• Histerezis Mekanizması: 
Eşikleme sonucunda oluşan görüntüdeki piksel değerleri sıra ile taranır. Her piksel değeri için 8 komşuluk kontrol edilir ve komşularından birisi güçlü piksel ise zayıf pikseller kenar olarak kabul edilir.

















