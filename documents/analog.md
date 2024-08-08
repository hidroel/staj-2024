# ADC Nedir?

ADC'ler analog olan sinyali dijitale dönüştüren araçlardır. Gelen sinyalin voltaj değerine göre dijital bir çıktı verir. ADC'lerin hemen hemen hepsi 0-5v aralığında çalışır. Bundan dolayı analog sensörler genelde 0 - 5v aralığındadır. MCU gelen veriyi dijital olarak okuyabildiği için herhangi bir sensörden veri okurken ADC kullanılarak analog olan veri dijitale çevirilerek MCU'ya aktarılır. Basit bir deyişle MCU'nun anlayacağı dilde aktarılır.

# ADC Türleri

- **Çift Eğimli** - Ucuz ve çok yavaş
- **Flaş** - Çok hızlı ama bit çözünürlüğü düşük
- **Boru hattı** - Çok hızlı ama bit çözünürlüğü düşük
- **SAR** - Hızı ve çözünürlüğü iyi (Çok tercih edilen)
- **Delta Sigma** - Hızı ve çözünürlüğü iyi

# Bit Çözünürlüğü

Yüksek bit çözünürlüğünde adım aralıkları daha kısadır. Yani daha hassas ölçümler yapılabilir. Bit çözünürlüğü düştükçe adımların aralığı artacağından gelen verinin hassasiyeti azalır.

# 4-20 mA Protokolü

4-20 mA bir dijital iletişim protokolüdür. Adından da anlaşıldığı gibi 4 ile 20 mA arasında akım değerleriyle haberleşme sağlar. Toplam 16 mA'lık bir aralığı bulunur. 0-5v, 0-10v gibi diğer analog ölçüm çeşitlerine göre avantajları ve dezavantajları vardır. 4 mA en alt seviyeyi, 20 mA ise en üst seviyeyi temsil eder. Sensörden gelen veri tipine bağlı olarak uygun aralıklama yapılır.

4-20 mA akım kaynaklı bir haberleşme çeşidi olduğu için güç kaynağından kesintisiz ve sabit akım alması gereklidir. Bu sağlandığında taktirde veri bozulmadan ve gürültü olmadan uzun mesafelerde yol kat edebilir. Voltaj kaynaklı haberleşmelerde mesafe uzadıkça gürültü artacaktır ve verinin doğruluğu kaybolacaktır.

4-20 mA'da 4 mA altına inme veya 20 mA üstüne çıkma durumu olursa sistemde bir hata olduğuna işaret eder. Gelen veri doğru kabul edilmez.

# 4-20 mA ADC İle Okuma

ADC'lerin büyük bölümü voltaj ile okuma yaparlar. 4-20 mA ise akım ile veri aktarımı sağlar. Bundan dolayı ADC ile bu verinin okunabilmesi için voltaj aralıklarına dönüştürülmesi gereklidir. Veri iletim hattı 4-20 mA protokolü ile ADC'ye kadar gelir. ADC'ye aktarılmadan önce bir direnç devresi kurularak akıma bağlı voltaj değişikliği sağlanır. Bu aşamada genellikle 250 ohm direnç kullanılmaktadır. Bu voltajı ADC okur ve dijitale çevirerek MCU'ya iletir.

# ADC Bit Çözünürlüğü Önemi

ADC'ler 8 bit, 10 bit, 16 bit gibi birçok farklı bit çözünürlüğüne sahiptir. Bit çözünürlüğü kullanım alanına göre oldukça önemlidir. Bit çözünürlüğü ADC'nin dijital adım sayısının belirtir. 2 üzeri bit sayısı şeklinde adım sayısı bulunur. Örneğin 8 bitlik bir ADC 256 adım aralığına sahiptir. 16 bitlik bir ADC 65536 adım sayısına sahiptir. Adım sayısı sensörlerden okunan verinin hassasiyetini belirler. Örneğin 0-256 PSI aralığında basınç ölçen bir sensördeki veri 8 bitlik bir ADC ile okunursa basınç değişimleri 1 PSI olarak değişir, ara değerler ölçülemez. Bu sensördeki veri 16 bitlik bir ADC ile okunursa 0,01 PSI gibi küçük değerlerin değişimi bile dijital olarak okunabilir.

Bir sensörden veri okunduğunda ADC bunu dijital bir sayısal değere çevirerek MCU'ya aktarır. Bu sayede MCU tarafından sensörden gelen veri doğru bir şekilde yorumlanabilir.

# STM32 İle ADC Kullanımı

ADC, STM32 ile denetleyicilerin üzerinde dahili olarak bulunmaktadır. Seçilen MCU modeline göre ADC'nin kaç bit çözünürlüğe sahip olduğu öğrenilmelidir.

Kullanılan MCU'nun datasheet'inde hangi pinlerin ADC olarak kullanılabileceği belirtilmiştir. Öncelikle buradan pin seçimi yapılmalıdır. Ardından bu pin STM32Cube ile ADC olarak yapılandırılmalıdır. Sonrasında ADC'nin çözünürlüğü (bit) yapılandırılmalıdır. Daha sonra Hz cinsinden örnekleme hızı yapılandırılmalıdır. Bu örnekleme hızı ADC'nin saniyede kaç kere analog veriyi dijitale çevireceğini belirler. Projenin ihtiyacına göre burada frekans ayarlaması yapılır.

İlgili sensörün standart VCC GND bağlantıları yapıldıktan sonra analog çıkışı STM32'nin ADC olarak belirlenen pinine bağlanır. Gerekli diğer ayarlar da yapıldığı için bu aşamadan sonra ADC belirtilen sürelerde sensörden gelen analog veriyi dijitale yani sayısal bir biçime çevirerek MCU'ya aktarır. Sensörün ölçüm aralığına göre dijital aralıkların karşılığı hesaplanır ve istediğimiz birimde ölçüm sonucunu alabiliriz. Bu aşamadan sonra MCU'nun sensörden gelen veriye göre ne yapacağı projeye göre belirlenebilir.

Eğer projede birden fazla analog veri toplanacaksa MCU'nun diğer GPIO pinleri de ADC olarak tanımlanıp her birinden farklı kanallar üzerinde veri toplanabilir. Bu sayede tek bir MCU ile birden fazla sensörden veri alınarak yorumlanabilir.

> Not: STM işlemciler genellikle SAR tipi ADC kullanmaktadır.

# ADC'yi Verimli Kullanmak İçin Yapılabilecekler

- ADC pinlerinin girişinde çok küçük (0,1 uF) kapasitörler koyularak gürültü azaltılır.
- Analog veri hattı mümkün olduğunca kısa tutularak gürültü azaltılır.
- Analog veri hattı manyetik alan üreten bileşenlerden uzak tutularak manyetik gürültüden korunur.
- Analog sinyal yolları dijital sinyal yollarıyla çok yakın olmamalıdır.

