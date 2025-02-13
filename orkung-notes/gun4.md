# Mikroservisler ve Devops

- [Mikroservisler ve Devops](#mikroservisler-ve-devops)
  - [Guvenlik](#guvenlik)
  - [CD](#cd)
  - [Soru](#soru)
  - [git](#git)

## Guvenlik

**imajlarinizi neden rhel tabanli degil de alpine'i sectiniz**
**ci'dan cd'ye gecis nasil oluyor, otomatik mi?**

bir urunun efektif kullanma maliyetinin min. 3yil olmasi
lazim. yeniden yazmak icin de en az 1 yila ihtiyaciniz var.
guncelleme adimlarini duzgun takip etmeniz lazim.

sbom'lari taniyan web uygulamalari var. maven vs gibi
araclardan yukluoyrsun, uzeirne de guvenlik veri tabanlarini
tarayarak uyari mekanizmasi isletiyor.

github da bu aciklari yapiyor, ya da nexus ambar enterprise
surumu ayni isi yapar. her artifact'in
sbom'la

fortify kod acisindan guvenlik analizini yapar.

java'daki bir kutuphaneyi ariorsun, bu surumde su aciklar var. der.
aslinda sonar da guvenlik aciklarini statik kod anlaizlerinde
yakalayabilecekleriniz var.
constant diye bir deger vermez.

uygulama bagimliliklari bir tarafa, alpine'i kuruyorsun, ne
kadar az sey var, o kadar iyi (guncellemeleri takip edebilmeniz
icin), bunlarin da guncellenmesi gerekir, belli surelerle imaj
yeniden paketlenmeli.

**genel ilke: kullanmadiginiz hicbir uygulama sisteminizde**
**tutmayin**

en basit hali: nexus nasil  sha'lari kontrolediyor
e-imza ne: sha'yi aliyor, imza icin kullanilacak CA'dan bir
sertifikayi aliyor, onun icindeki anahtarlarla sha'yi
imzaliyorsun, devaminda indiren kisi imza dogrulama rutininde,
once CA'nin sertifikasi sana onay vermis mi, imza gecerli mi,
sonra sha iceriklerini kontrol ediyor, ok ise devam
ms sertifikayi caldirdi.: gidip hemmn iptal et diyorsun

temel algoritma: pub-priv key dogrulama:
bi sey priv ile kriptoladiysam pub ile acabilirim
bi sey pub ile kriptoladiysam priv ile acabilirim

priv key'i cok iyi korumak lazim
ssh-key  pem'i koruma anahtarina almak
bir adim otesinde cift tarafli ssl
e-posta sistemlerinde kullanilan pgp gpg
gonderecegim kisinin pub anahtari bende var
karsi tarafin public anahtariyla kriptoluyorum
uzeirne kendi priv key'imle kriptoluyorum, karsi taraf da
biliyor ki bu hakan'dan geldi.
butun programlama dillerinde hazir programlama dilleri var.
source code'una bakamiyorsam mumkunse imzali olmasi lazim.

waf'lar ssl'i nasil kiriyor. sizin waf'tan gecerken sizin
ssl'i aciyor, e-devlete kendi ssl'ini koyup aradaki trafigi
dinliyor. kurumsal bilgisayarlariniza sertifikalarini
yukluyorlar.

ssl varken man-in-middle yapamamasi lazim.
uzerine saga id'si ekliyoruz.

internet dedigin sey
Ana dagitici isp--> yerel isp--> modem--> yerel ag
sana verdigi ip karsiliginda her seyi izleyebiliyor.
butun http protokolu duz text, izleyebilen herkes okuyabilir
ilk request gidiyor: bunu isp mutlaka yakaliyor. (dns'imi)
tunel acacagiz su sertifikalari kullanacagiz, el sikistik sunu
kullanacagiz diye
gonderdiklerin once onun pub ile kriptolaniyor
uctan uca sifrelenmis
tunel actiktan sonra hangi ip ile hangi ip konusuyor
gorebiliyorsun ama icerigini okuyamiyorsun
anonim degil de karsilikli konusmak isterseniz two-way ssl

bit sayisini arttirdigimiz icin kullanilan cpu zamanini
arttirmis oldu. 512-1024-2048 bit ssl
cunku arkadaki veri cpu zamanini harcamaya yetecek bir veri

## CD

mobios seridinin sol tarafi ci, bu tamamlandi. CD yapmak
urunle, onu hayat dongusuyle, ekibin becerisiyle cok iliskili.
deploment'larin da otomatize edilmesi lazim.

bunu bir insanin tetiklemesine ya da otomatik tetiklenmesine
kalsin is. deployment'in bir parasi olarak runtime conf
degisiklik yonetimini yapmaniz gerekir. uruun her yeni surumu
geldiginde configurasyon isteklerinde degisiklik olabilir, cd
pipelineinizda, gelistiricinin producton konfigurasyonunu
kurcalasin istemem.

surum politikasina bagli olarak uat'ye (pre-prod ortami)
otomatik deployment yapabilirsin. (su tarihe kadar onay verin
ya da vermeyin demeniz gerekiyor)
buradaki surum dogrudan canliya giden paket (binary) olmasi
gerek, burada bir fark olabilir, bu fark sadece configurasonla
yonetilebilen bir fark olmali, farkli db'de olabilir, kaynak
az olabilir vs. ama eger ben prod'a cikmak icin yeniden
derleyeceksem orada yeni bir sey gelmeriski var, bu da yeni
bir acik/hata demek, bunlar kozmetik hata da olabilir.

datadan kaynakli hatalari minimize etme yollari he zaman var,
veri kumesi hazirlarken production'da
veri tabani degisiklik yonetimi araciniz, production'a da
uygulayacagim, cunku urunumun butunu bunlardan olusuyor

relational veritabaninin test ortamiyla prod'da ayni sema
yapisina sahip olmasini nasil saglihorsunuz?

TDD (test driven) -once yapilacak seyin testini yaziyorsuuz,
sonra kodu yazip test ediyorsunuz, bu da maliyetleri
arttiriyor. test senaryonuzun dogru oldugunu kim soyledi.
kodun icinde dallanmalar varsa bu da coverage'ini belirler.
ayrica hata ureten durumlarin da testlerini yazmaniz lazim.
BDD  (behaviour driven): sistem analizi yaparken daha, oturun
senaryolari yazin (use case'leri) belli bir formatla yazin,
sonra bunu calistiracak kodu yazarim.
gerkin (acur demek), kutuphanenin kendisi de cucumber.

bu 2 yontemi de kullanmamakla beraber, prod'a cikti hata
bildirimi geldi, reproduce etmemiz lazim, bunu tekrarlayacak

compiler zaten benim hatalarimi duzelttiriyor.
unit test olmasi gereken kritik yer: hesaplama yapiyorsunuz.

uat: kullanici kabul testinin temel amaci bu, bir yere
yapilacaklari yaziyorsun, insan geliyor ve hepsini yapiyor

api'ler uzeirnden fonksiyonel test yazmak kolay: su http req'i
attim, body'e de su json'u koydum, bana soyle bir json gelmesi lazim.

entegrasyon testi: benim sistemim diger sistemlerle birlikte
duzgun calisiyor mu? bunu otomatize edemiyoruz.

nightly build yapiyoruz: main branch'indeki en guncel
derlenebilen. o benim surum branch'imden cikiyor

tum butestlerin surum cikmadan calimasi gerekiyor, deployment
surecinde pre-prod'larda tekrar ayni sureci
calistirabilirsiniz. canlida sadece yuk testi kosulabilir.
o da canlidaki ortami baska bir ortama kuramiyorsam eger.

test yazma surecleri oyle kolay degil. ozellikle isin icinde
zamanla ilgili bir durum varsa: biletin fiyati kalkis saati
yaklastikca degisir. testi statik yazamiyorum. test suitine
teste baslamadan once bana 2 gun boyunca datayi hazirlayacak
algoritme yazdik.

- **test containers** en cok veri tabanlari icin kullaniyor
lazim oldugunda veritabanini ayaga kaldir, isin bittiginde sil.
oracle'i koyamiyoruz ama mysql, postgres, mongo, redis'in test
container'lari vardir.

#copilot_onerdi

- **mockito** bir sinifin bir metodu cagirildiginda bana ne
  dondurecek
- **wiremock** bir http endpointi cagirildiginda bana ne dondurecek
- **cucumber** bir feature dosyasi yaziyorsun, bunu calistiracak
  kodu yaziyorsun
- **junit** testi yazarken kullanilan bir kutuphane
- **testng** junit'in alternatifi
- **selenium** web uygulamalarinda kullanilan bir kutuphane
- **jmeter** performans testi yapmak icin kullanilan bir kutuphane
- **postman** api testi yapmak icin kullanilan bir kutuphane
- **newman** postman'in cli versiyonu
- **k6** performans testi yapmak icin kullanilan bir kutuphane
- **gatling** performans testi yapmak icin kullanilan bir kutuphane

teknik borc, duzeltmediginiz surece borcun artar, bu bir
tefeci borcudur; duzeltmediginiz surece faizi artar.

sonar teknik borclari cikartir.

prod-test ortamlari arasinda fark var; bu fark nedeniyle de
sorun yasaniyor. configurasyon
ayni anda kac multithread calisabilir, yazilimda bu tur
degerleri ortam env variable'larindan almasi lazim., cunku
ortamdan ortama bu degisecek. disaridan confiurasyon dosyalari
olur, configmap'lerden aliriz.
`.sample` dosyasiyla baslayan dosyalarin icindeki degerleri
git'e gonderebilirsin.

neyi ENV'de tutabiliriz?
ortamlari kurar gibi, altyapiya bagli olarak

spring config server var, hashicorp consul, ya da zookeeper
var, configmap var. buralarda tutulmasi gerekiyor.

"tag'e su sekilde gonderirsem
spring-config-additional-location

256 ver

helm: su dpeloymant.yaml'lari kullanacaksin, uzerinde de
template'de birbaska sefer de kullanacaksin, bunu bir
arayuzden yapmayin helm tarafindaki dosyalar uzerinden

yaml'da yazdiginizi varsayalim
aslinda configmap bir key-value veritabani
etcd'de tutuluyor.

uygulamanin buna uygun yazilmasi gerekiyor.
imaj ayaga kalkarken pod servise baglanip konfigurasyonu
aliyor

burayi sorabiliriz: kodda guncelleme yapildikca configmap
surekli yeni bir alan geliyor. helm'de olunca bu bize yuk
cikartir mi?

k8s'te secretmap baska bir conf sistemi var, ya da hashicorp
vault var. java jks icinde saklar.

## Soru

- configserver veya consul dinamik yonetebiliyor muyum?
test ve prod diye 2 ayri statik jenkins job'u tutmuyorum da
configserver'a yaziyorum prod oldugunu anlarsa onunkileri
aliyor
- squash yaptigim commit setini cherry pick yapamam degil mi?
- traefik'i oneriyor musunuz?

test icin prod branch'i aciyor
net olmasi gereken; bunlarin external'dan tanimlaniyor

spring'te dosyadan okumakla env'dan okumak arasinda fark yok
spring cevirirken otomatik ceviriyor, operasyon env. varialbe
olarak gecirebiliyor.
env'dan ne gcirir, dosyadan ne gecirir?
ayni makinanin
her pod'un port adresi degisiyorsa onlari env'dan alirim
jvm'in
1 podda 1gb'la baska bir podda 5gb'da alirim

k8s'te kosarken k8s'inkini de kullanmak, cesit cesit olmasin
job sayfasindan verilmesz, sonuc urunun nasil bir kaynak
tuketecegini umaja niye yaziyorum

## git

asla bir sey silemezsin...
mesajin header'i var bir de bodys'i var, merge commit'inin 2
parent'i vardir.
body'de de diff/patch var
bir onceki parent commit'in farki var, butun kod yok
commit id var ya; butun mesajin + commit log'n hash'ini
aliyoruz, id diye bunu kullaniyoruz. artik bu diff'te 1 byte
dahi degisse bu o commit degil'i garanti altina aliyoruz.

her commit kendi parent'inin id'sini tasiyor. agac dagiliyor;
dolayisiyla commit silemezsiniz.

git revert
sifirdan bir git repo'su olusturup onun olmadigi bir tool'larla
legal olmayan yontemler var, maliyeti cok agir
git revert ile commit'i tersten uyguluyoruz

son commit'teki degisiklikleri yapabilirsin. bu sadece yerel
git repo'sunda yapabilecegin bir sey.

son commit'i push'ladiysam artik dokunmak yok, cunku clone
alanin link'leri bozulur.

http'de neden header diye bir sey var? header ne ise yariyor?

body'de ne oldugu beni ilgliendirmiyor, header key:value bi
string yapi, is kurallarimi yazarken, validasyonlari vs.
kargoya verirken kargocu icindeki payload'la ilgilenmiyor,
onun isi sadece adresi bulup teslim etmek.
body'nin icinde ne olacagi header'da yaziyor
apigw hedard'da bulunan url'e traceid, cookie'e bakiyoro
asil sunucu; kod yazdi oraya routing'i yazmak icin header'a

bu mesaj transfer pattern'i

e-postadaki mimetype'larinin bir registry'si var. eskisi gibi
uzantiya bakmiyoruz. header'da content-type var, bunu okuyoruz
ve ona gore islem yapiyoruz.

RFC: request for comment (10000'e yaklasmis durumda, standart
oldular, onlar da gelisiyorlar, buradan protokollerin nasil
olacagini anlayabilirsin, kutuphanelerde bu yazar: RFC sunu
bunu destekliyorum diye yaziyor.)
