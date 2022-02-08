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
