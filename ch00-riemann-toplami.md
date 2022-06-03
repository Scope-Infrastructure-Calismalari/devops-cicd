# Riemann Toplamı

#### Next Chapter: [01 - Introduction](ch01-introduction.md) | Return to [Main Page](README.md)
---

## Riemann Toplamı - Konuya felsefik bir yaklaşım

<p align="center"><img src="images/Riemann-Toplami/image-1.png"></p>

Yukarıdaki şeklin alanını nasıl hesaplayabiliriz?

Hayat, problemler, projeler kenarı köşesi belli olmayan, eğik-bükük şekillerle dolu ama biz sadece üçgen, dörtgen gibi düzgün şekillerle net hesaplamalar yapmayı biliyoruz. Bernhard Riemann isimli bir matematikçi düzgün olmayan şekillerle ilgili hesaplamalar yapma konusunda çok sade bir yol bulmuş.

*"Madem dörtgenlerin alanlarını hesaplamayı biliyoruz, o zaman düzgün olmayan bu şekli, dörtgen parçalardan oluşan bir bütün olarak düşünelim ve bu dörtgenlerin alanlarını toplayarak tüm şeklin alanının yaklaşık değerini bulalım"* diye düşünüp, bu düşünceden temel alan bir formül üretmiş.

<p align="center"><img src="images/Riemann-Toplami/image-2.png"></p>

Bugün yaygın olan yazılım geliştirme metodolojilerinin mantığı Riemann’ın mantığına benziyor. Uygulamaları özelliklere bölüp, belli zaman dilimleri içerisinde bu özellikleri tamamlayarak projeleri inşa ediyoruz.

Bu yaklaşımın sağladığı faydaların başında kapsamımızı daraltarak projenin isterlerinde boğulmamızı engellemesi geliyor. Her ister özelinde bağımsız şekilde derinlemesine düşünebiliyoruz. Belli bir anda spesifik bir özellik üzerinde çalışırken, bu özelliği daha odaklı şekilde test edebilme şansı buluyoruz. Bir özelliğin tamamlanması, projede çalışan kişilere “Closure”, "Done" hissini yaşatıyor.

Bir yazılım ürününü ufak özelliklerden oluşan bir bütün şeklinde görmek, yazılımda MVP(Minimum Valuable Product) fikrinin ortaya çıkmasına da yardımcı olmuştur. Bu sayede bir yazılım projesinden değer üretebilmek/para kazanabilmek için tamamen bitmesini beklemek zorunda kalmıyoruz. “Kimsenin kullanmadığı” projeler için geliştirme yapmıyor, boşa emek vermiyoruz. Projeyi “iş görür” hale getirebilecek özellikleri ekler eklemez ilk versiyonu çıkabiliyoruz. Yeni özellikler ekledikçe versiyonu arttırıyoruz. Kodunu yazdığımız özellikler hemen kullanılabiliyor. Hatalı çalışan noktalar anında ortaya çıkabiliyor.

Tüm bunlar yazılım projelerinin başarıyla tamamlanmasını, projelerin ürünleştirilebilmesini sağladığından, bu yaklaşım artık default hale gelmiş durumda.

### Esas kısım

Parçalara bölünüp bu şekilde geliştirilen bir projeye sürekli olarak eklemeler yapmak, bir veya birden fazla kişi tarafından yazılmış kodların birleştirilmesi, yeni bir versiyonun başarılı bir şekilde üretilmesi de başlı başına bir iş haline gelmekte çünkü yeni yazılan kodların bütünleştirmeden sonra diğer kodlara zarar vermemesi, tüm kodların hala çalışabilir durumda olması gerekiyor. Bu yüzden *"CI - Continuous Integration - Sürekli Bütünleştirme"* bir ismi ve üzerinde düşünülmeyi hak eden bir süreç haline geliyor.

---
#### Next Chapter: [01 - Introduction](ch01-introduction.md) | Return to [Main Page](README.md)
---
