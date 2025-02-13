# Mikroservisler ve Devops

- [Mikroservisler ve Devops](#mikroservisler-ve-devops)
  - [onceki gunden sorular](#onceki-gunden-sorular)
  - [saga](#saga)
  - [soru](#soru)
  - [devops](#devops)
    - [Pratikte yapilanlar](#pratikte-yapilanlar)
  - [Surum yonetim politikasi](#surum-yonetim-politikasi)
  - [Paket yonetimi](#paket-yonetimi)
    - [Pipeline](#pipeline)

## onceki gunden sorular

- k8s workspace'ten kastı nedir? (**bunu daha aciklamadi**)
- API kirmak abi kırmak
API'de boyle bir metodum var bu parametre alir. yeni bir
parametre eklemek istiyorum. bu durumda API'yi kirmis olur
muyum? evet bu yeni bir ozellik olabilir veya degistirmis
olurum, major versiyon temel fark api kirmak. bir goc
planlamaniz gerekir. 2 farkli uygulamaymis gibi dusunmek
metodum ayni parametrelerim ayni fonksiyounun yaptigi is ayni
degil artik hesabi oyle yapmiyor, cagiran  uygulama ayni ama
ayni sonuca donmuyor
- web hook 5 istekte sonra bağlantıyı kesiyor açıklama
circuit breaker'dan kaynaklaniyor: retry'da deneme araligini uatmaya calisirsiniz

- hangi k8s apilerini kullanıyor
service discovery icin yapiliyor her instance register eder.
hangi instance bilmek zorunda degil
feign client api client, bunun dokuman
onun araclariyla kod uretiyorsunuz
**k8s'in leader election icin api'si var, bu zookeeper yerine**
**leader election'da kullanilabilir**
**db'deki streaming ile replication'daki ayni kavram**

## saga

saga aslinda transaction problemini cozmek icin kullanilan birdesen
ACID transaction diye gecer: bunu destekleyen veritabani
lock'lari kendi koyuyor
normalde 1 trans sistemini garanti etmesi, yaptiginiz islerin
tutarli bir sekilde bir butun olarak garanti etmek.
veritabanini yaptigi is: datayi alicam ve diske tutarli
sekilde yazicam illa db'ler  vermek zorunda degil, mq'lar da
verebilir (pipeline'da bir produce ettiginizde en uctaki
consumer'in tutarli sekilde alacagini, alamiyorsa da rollback
yapacaginin garantisi verir)
relational db'lerin dogasi geregi 1 kaydin 3-4 tabloya etkisi
olabilir, 1 ve 2'ye yazdik,
integrity'si bozulacak

dikey buyurlar  ayni cpu icinde ayni thread yapisi icinde islem
yapmalari lazim
tipki multithread programlamada oldugu gibi senkronizasyon
problemleri, lock'lama
en onemli becerisi lock surelerini en
bazilari page lock, row lock, table lock
page: kac kayit varsa hepsi
table'daki hepsini locklar
yazarken kilitleyecegimiz kesin, okurken de bazen evet, eger
okuyup islem yapacaksam okudugum anda locklamam lazim
select for update
uygulama tarafinda farkli terminolojiler var: java concurrency
api icinde atomic integer: yaptigi is: +1, ama bunun icin once
kac oldugunu ogrenmem gerektiginden lock'luyorum
multi concurrency lock mekanizmalari var: exclusive read, ...
write
butun transaction sistemleri bunlarin hepsini destekelemz,
sizin ihtiyac duydugunuz transaction'u destekleyip
desteklemesi gerektigine bakmaniz gerekiyor
o transaction blogunda yapmak istiyor olabilirim
dilin terimi: koda sen synchronize olacaksin, single tread'in
calisacagi sekilde lock'lar, diger thread''ler okuyamaz bile.
fakat ihtiyacim bu mu? cogu zaman degil, java'daki thread
kutuphaniesinde ayrica bir lock kutuphanesi var: multiple
read, write yapabilen
while (getlock())
try-cache'in icine koymam gerekiyor, mutlaka release etmem
gerekir, etmezsem ne olur? deadlock, bunlar timeout
mekanizmasi varsa ona kadar, yoksa sonsuza kadar
bu nerede gorulur.
db'lrein deadlock detection'i var. uygulama tarafindaysa eger,
monitoring sisteminde hicbir thread kabul etmiyor, cpu bosta,
%99.9 bir yerde, deadlock'tan da olabilir, feature da olabilir
database'in load'u yukseliyor, uygulamalar pool'u doluyor
butun thread'ler dolu bosta
siz lock'lamak istediginizde bunu yapamayabilirsiniz, cunku
baskasi lock'lamis olabilir.
once bir kilit istersiniz bunu
ben transactional'im diyen sistemlerde bunu kullaniyorsun
zaten:
begin tran
...
commit ya da rollback
buradan farkettigin gibi gelistiricinin yapmasi gereken 1 sey:
son satir (hata almazsam com, ya da rollback)
commit'lemedi de rollback etmiyor da olabilir, transaction
acik kalabilir, gercekten alakasiz seyleri ayni trans'ta
yapabilirsin bunlar da tran'in hayatta kalma suresini
uzatiyor, bu da lock'larin suresini uzatir bu da baskalarinin
islem yapma suresini uzatir, tra'larin en kisa surelerde
yapilip kapatilmasi gerekir? varsayilan commit suresi 5
dk.'liktir, tran time-out suresi global ayardir, monolith'te
ayri ayri sure vermen lazim ama mslere bolunce bu sureyi islem
bazina verebilirim (raporlar icin)
tra timeout 5 dk
proxy timeout surem 2 dk: musteri bir istekten bulundu, benim
x,y,z sunucularimdan gece gece geldi db'ye, db'de 4 dk. surdu,
fakat mv'im kullaniciya hata bildiriminde
bunlari da dengeli ayarlamam lazim, birbirleriyle dengeli
olmasi lazim
1 dk.'nin uzerindkei islemler icin uyumlu ayarlanmasi gerekir.
http-client bir sonrakine atacagi request'in suresi
asenkron: kullaniciya sen rahta ol, kullanici 5 dk. boyunca
beklemiyor. adam cevap gelmedigi icin 5-10 kere istek
gonderiyor, bunun icin fe'de ekran kilitliyoruz. 1dk.'yi gecen
tum islemler icin backgroundda asenkron yapilacak sekilde
tasarlamak iyidir.
rapor: dokuman yonetimine mi dosya sistemine mi epostaya mi
konulacak ondan sonra gonderilir.

camel itl araci: 1 tane excel gelmis, insanlar transfer
islemlerini excel'le yapiyorlar, bu memory'e sigmiyor.
muhasebe dokumleri yuzbinlerce sayfa.yapilmasi gereken:
dosyanin icinden bir blok okudum attim, sonra baska bir
tanesini yaptim. buna streaming, batch deniyor.
xml'de 2 kuutphane vardir: DOM daha hizlidir ve socks;
okuyarak ilerler, hepsini memory'e almaz
req geldi, ben tran'i actim, ama bitmiyor, her yer kilitlendi,
verinizin tutarli olmasinin yontemlerini tasarlamaniz
gerekiyor, ben her batch'i commit'lerim, her 100 kaydin
tutarli oldugunu kabul ederim, butun belgenin degil
excel'demusterlerin eposta adresleri var, 1 tanesinde hata
var, transfer programiniz hata verdi, kaydi kapatti, simdi ne
yapacagiz? nerde kalmisitim? tek transaction'da basmaya
calismamasi gerekiyor, evet burada garantiye aldim ama bunun
icin gelistiricinin yapmasi gereken seyler var:
performansin otesinde deadlock'larin kaynaklari uzun suren
trans'lar
thead teknolojisinde izleyebilmek icin farkli metrikler
java'da jvm disariya metric cikarir: kac thread'im var, kaci
idle, kaci calisiyor. izleme sistemi de bunlari
java'da kac tane thread kullanabilecegini onden
belirleyebiliyorsun
geriye 5 thread kalmis, yani bir sey bekliyorlar: illa
deadlock olmasi gerekiyor
pool diye tasarim deseni var, throttling yapmayi ve
performansi saglar.
db'de kendi uzerinde en fazla 500 conn kabul ederim var. dogal
olarak ben java'da conn acarken bir pool yapiyorum: ben 100
tane max acabileyim, 5'ini sen hep acik tut. cunku conn
acmanin bir maliyeti var. fakat acik tutmanin da maliyeti var.
izle, optimum degerleri bul. sonra lazim oldugunda yenilerini
ac,ama hemen kapatma, 1 dk idle kalsin, sisteme yuk bindiginde
bu conn'larin ihtiyaci hemen olabilir, onun icin. bu pool
sistemleri
uygulamadaki thread pool'lar: bu sefer os'tan talep ediyor. kernel'a
gidiyor fork diyor, thread conf'lari icin izinleri veriyor. bu
jvm'de 300 thread olabilir'e denk gelen sey
bunlar gruplanirlar, http thread'lerinki, worker'larinki,
orada bunlar dolduysa ve sistemde yuk yoksa bi sorun var

feature olabilir: network'te,db'de app'te bir sey yok.
usual suspects: pool configurasyonun yanlis.arkadaslar sirada
bekliyorlar cogunlukla burasi patliyor, veya http conn pool'da
bekliyorlar.
daha patlamadan once buralara bak, o yuzden thread'ler
bekliyor

monitoring anlaminda bakmaniz gereken sey: hersey
jvm'le gelen visual vm masaustu uygulamasi

tra'nin acid olabilmesi icin kuralarimiz var bunlarin da
donanim ve os seviyesinde etkileri var: os-cpu'ya giden
assembly denkligi var.
2 farkli node'da ortak calisacak bir trans'a ihtiyacim varsa
yukariyi nasil optimize edeceim:

kullanici --> app --> 2 farkli veri tabani uzerinde islem
yapmasi gerekiyor. basit bir ornek acisindan.
bu islemin transactional olmasina ihtiyacimiz var.
kod yazarken "bunun sonucunda iflerle x 'in sonucu geldiyse"
yap, bunlari yapmak icin 2 face commit kavrami var. db'ler
bunlari destekliyor. dev'in bunu bilmesi lazim uygulama sunucularini conf. ederken
bunlarin karsiligi var: java'larda ayri bir surucu seti
kullaniliyor: JTA if else'i yapiyor, demin anlattigimiz tra
gibi degil, farkli id'leri aliyor sakliyor ve commit mi
rollbackmi buradan takip ediyor, a'ya koyuyor commit'lemiyor,
b'ye koyuyor, hicbir hata yoksa commit'liyor.
commit'te hata olmaz. oluyrosa network'te, disk'te os'ta bi
yerde hata vardir. db'ler bu trans'lari isleyebilmek icin
command pattern'lerini kullanirlar. bunlar once ne yapacagini
bi dosyaya yazilir, commit'ler rollback'ler, replication'lar o dosyalar
uzerinden yazilirlar.
ek conf yapilari isteyecek. ama b usayede ne yaptim, 2 db'ye
oydum ve commitledim. farkindaysam hala alttaki guvendigim subsystemlerin
acid trans destekleri var, ben bir de dosya sistemi koydum,
transactional dosya sistemi hala yok. dolayisiyla
gelisticirinin buradaki isi kendisinin yapmasi gerekir
mesela bir hukuk uygulamasi yazdin: bir dava dosyasina eklenen
dosyalari ekliyor, kullanici dosyalari yukluyor, bi sorun oluyor
db'deki kayit geri aliniyor, fakat yuklenen dosyalar sistemde
olmaya devam ediyor: bunlar oksuz kaliyorlar, gerekli olup
olmadigini bilemiyoruz parent'inin ne oldugunu bilmiyoruz.
tavsiyem dumduz dosya sisteminde tutmayin: bir dokuman yonetim
sistemleri uygulamasinda tutmali. ayni belgeyi 2 farkli
kullanici yukledi. yapiyorlar - disk sisteminde hash'lerini
kontrol eder. daha az yer kaplar. oksuzleri bulmak icin ek
rutinleri vardir, arsivleme konf'lari vardir. yasal imha
surecleri var, imha etmezsen de suc. ozgur yazilim as'nin bir
urunu var

transactional olmayan yapilarda bunu nasil destekleyecegiz,
kendi yazacaraimiza ekleyebilecegimiz yok, hazir kutuphaneler
de yok. baska bir cozume ihtiyac var. aslinda ilke olarak
oldukca basit yaptigimiz islemleri bi kenara topla, bi
hatayla karsilasirsan tersine islem yap. ama bazi islmlerin
tersi zor, mesala bir kayit sildiyseniz onu geri getirmeniz
lazim, o zaman da gercekten silme yapmaniz lazim, yani soft
delete yapman lazim, silindi diye isaretliyorsunuz,
raporlarda vs.'de sorguda silindileri false olarak getir diye
belirtmen lazim, fakat soft delete tek yontem degil.  command
patternden bahsettim ya, bunu kullaniyorsan hayat daha kolay,
command'lari log'luyorsun, normal hali ve tersine cevrilmis
hali bu, ben bunu yaptim diye log'luorsun
is kurallarina bagli olarak bazi seyleri tasarlama sansin
var: infinite state machine, bir durumdan digerine gecis.
sonsuz durum makinasi: pratikte her yerde karsimiza cikar.
kapidan girdiginde telefonu eline aldiginda aslinda durum
makinalari calisiyor
bir durumdan digerine gecislerde ne yapacagimizi
telin calisma - konusma modunda olmasi ac dersem sesi acar,
kapat dersem kapatir, sonra tekrardan bekleme moduna geri
geldi. uygulama veri modellemesi yaparken bir state machine
kurgularsin ve delete operasyonu yapmazsin, bu gecersiz bir
durumdur deyince orada durur. soft delete yerine bunu
kullanmayi tercih ediyorum. cunku soft delete'te corbaya
donuyor, halbuki bunu kullanirsam daha duzgun oluoyr, ornegin
bir siparis ... draft modunda tutsam, siparisi vermedi, henuz
bekliyorum veri tabanina kaydettim ama , cunku tekrar
gelecek, yoneticisine iletti, ben yine bekletiyorum, veri
tabainda kaydettim, onaylandi, yani release stateine aldim, simdi event
firlattim, stok ve siparise bir event firlattim, fakat
musteri siparisi iptal etti, ben tekrardan state'e aldim,
hala eski durum gecerliligini koruyor, duzenleme moduna
aldim, 2 gun surdu, tekrar onaya gitti, onaylandi, ayni
zamanda bu tarihcenin hepsini log'layip kullanicilarina blame
verebiliyorsun. kim ne zaman ne yapti, kpi verebiliyorsun,
teslim alinma suresinin su adimlari su kadar zaman suruyor,
su kisi su kadar zaman harcadi. en sonunda siparis iptal
oldu: iptal state'ine aldim, sonunda ben kac tane iptal
siparisi almisim diye rapor alabilirim. bu state machine'de
yoneterek, is kurallarini sistem analizi zamaninda bunu
tasarlayarak basima buyuk bir dert olacak islemi tersine alma
isine cozum bulmus oldum. aslinda burada 1 kolunda
enumeration'dan bahsediyoruz. bu bir boolean degil, sorunumuz
su: bu state'lerin arasinda gecisler, hangi durumdan hangi
duruma nasil gecilir, onden tanimlamak gerekir, bunlarin her
birine state macine'de action --> transition deniyor. bu
command pattern'ine de cok yakin bir davranis, bu durumdayken
onayla action'u gelirse su duruma gececeksin transition'u
geliyor. dev'in veri modeli ve etkilesen raporundan vs.'yi
tasarlamasi gerek.
bu isin en basit hali: java enumeration taniminin icersiinde yapmak
 stateless kutuphanesi ile
var: bunlarda nbirisini tercih edip kenidniz yapabilirsin,
command pattern'iyle uyumlu olmasini istedigi icin onunla
butunlesik sekilde yazmis.

bundan daha uzmanlasmis bir yapi da var: BPM business process
management (is surecleri yonetimi), bunun da bir dili var:
BMPN (business process model and notation), iddia: sistem
anlaiztleri bunlari hazilrar, bpm engine bunlari calistirir.
bpm engine: bir islemi baslatir, bir islemi durdurur, bir
bunu kullanmasam bile surecini analiz ederken bu akisi cizmek
karsilikli anlastigimiza emin olalim. hayat
kurtariyor.devaminda gercekten bir bpm engine kullanbilirsin.
herbirisi tipki db'de oldugu gibi standart ansi92 sql'de oldugu gibi kendi dillerinin
ozelliklerini de getirirler.
bpmn'ler gelismis bir state machine, tek bir kayit icin degil,
birden fazla akisin state'ini tamamlarlar.
siparis alma aslinda bir satis surecidir. neyle baslar?
ipucuyla basliyor: lead toplama, lead'i aliyorsun, lead'i
satisa donustugunde bir firsat deniyor buna: su hizmeti
satabiliriz belki. sonra bu firsati degerlendirmenin kendi ic
state'leri var, telefonla gorustum, randevu aldim, followup
yaptim vs. en sonunda kapatirsiniz ya da teklif asamasina
gecer. artik firsat tekfife donustu, tekrar verdiniz revize
ettiniz, kabul etmediler. surec bitti, kaubl ettiler, sonra
siparisi gerceklestirdikleriniz, irsaliyle mali gonderdiniz,
hizmetse hizmet tutanagi tuttunuz, parayi kim alacak: fatura
kestin, faturanin odemelerini aldiniz, sifirlandi, satis
sureci simdi bitti, birbiirinden farkli kayitlari biraraya
toplatik, herbirinin kendi state machine'i var, hepsinin
icinde de ... var, su anda bu islemler 1-2 aylik surec,
bunlarin hepsini kocaman bir transaction'da tutamazasin
long-running processes genel olarak bunlarin her birinin birer
ms oldugunu dusunursen:
lead toplayan
stok yoneten
finansi yonten
cari hesap musteri bilgisi
bunlarin arasinda surekl ibir islem ar, ben kayitlarin
statelerini silmeden takip etmeyi basarsam da,
cari sisteme geldim, "biz bunlarin siparisini teslim etmis
miyiz acaba" cari sistemin disinda bir de siparis sisteminden
bilgi almam lazim ya da oradan data getiren bir seylerin
olmasi lazim. bularin arasinda transactional bir davranis
olmasi gerek,. yayinlaidigmida bir event firlatti, ozellikle
tutarli veri olmasi gereken yerlerde bir transaction
kurgulamam lazim. iste simdi SAGAya geldik: bu tarihcenin
tamamini bir log'a dolduruyoruz, bu logun adi SAGA. sonra
sizin sisteminizin hata olarak kabul ettigi (gercek hata
olmayabilir), geri donmeye baslamaniz gerekebilir.ben saga'ya
check koymustum ya
1 bunu yaptik, 2 bunu yaptik, 3 bunu yaptik, 4 bunu yaptik,
geri donerken tersine islemleri yapmamiz gerekiyor, burada
kolay ypamak icin command pattern kullanabilirim, jakarta'nin
icinde long running process var, onlar url'llerle yapmislar,
sagayi rollback etme islemlerinin url'lerle yapiyorum. saga
pattern'inde bu isi yapmak icin 2 farkli yontem var:
1 orchestraction yapan external bir servisiniz olabilir
(eclipse'in urunu, url'lerle yapmislar) temiz bir cozum,
arkasini neyle implement edersen et bir cesit webhook kullaniyor.
2 internal olarak siz yaparsiniz: framework'un icinde saga
pattern implementasyonu yapan kutuphaneler oluyor
begin saga
...
...
...
kancalayara log'luoyr, sonra da onlari geri aliyor.

hangi sistemi kullanirsan kullan trans'in tersini nasil
alacagimizi kendimizin yazmasi gerekiyor. sga patterninin
gerekleri disindaki kismi gelistiricinin yazmasi gerekiyor,
elimizde bazi baska araclar var: tarihce tutabilen, ornegin
db'lerde (hatta mongo'da) butun tarihceyi tutan bir kutupane
var (debezium), bunu kullanarak saga pattern'ini bu ne
yapiyor: bu audit log'lar icin bir cozum de olabilir.
tablo
id a b

tablo 2 (audit)
id action ...
x tarihte su delete calistirildi.
y tarihte su insert calistirildi.

bu tablo cok buyuyor.
bu tablonun farkli desenleri var. birebir koyabilirim veya
key:value yapabilirim: kolon-adi ve degeri
uygulama tarafinda hazir kutuphane orm araclarinin lifecycle
event'lerini kancaliyor, enverse hibernate'in lifecycleini kancaliyor. butun
tarihceyi sana dokuyor. bunun yeirne kendin active record'u da
kancalayabilirsin kendin.
gecmis butun datam burada duruyorsa ben daha kutuphane
safhasinda geri alabilirim diye kod yazabilirim
state machine kullanan bir modeli isareldim, kolonun adini da
biliyorum "state", eski state'in old values'ini da buluyorum
ve generic bir kutuphane yazabilirim.
saga log'u ve veri tarihce log'u hep cok yer kaplar. o
sebepten bunlari opsiyonel tutuyoruz, bu audit log aslinda.
housekeeeping de eklenmeli, 1 aydan onceki saga log'larini
sil". mutababakat tarihleri vardir, gecen ayi kapattiysak
housekeeping yapabilirsin
artik butun saga log'larini silebiliriz. finansal
sistemlerinde de 1 yil onceki sistem kapanir, bunu artik
audit log'larinda da 1 sene
bazen silmek kendi basina bir is oldugunda, hatta bir
key-value veri tabani var, bunda silme komutu yok, fragmante
olur, pg'in vacuum'u gibi araclar kullanilir. mongo'da yeni
node acarsin replike olur, daha optimize olur. bunu runtime'da
yaparsin duzenli. (housekeeping rutini)
bunlar arsivlenmek icin de kullanilir. cunku veri ne kadar
buyukse sorgu maliyetleri de o kadar buyuyecek
btree algoritmasi (binaryTree): butun db'ler bu algoritma veya
bunun varyantiyla indesx tutarlar. tabloda kayitlarim var,
bunlar icin bir agaz olusturmaya baslar: biz bir arama
yaptigimizda index'in icerisinden hizli bir yol bulur, bu
agaci duzgun tutablimek icin tekrar tkerar yazra.
milyon kayit icin 2 indexim var mesela
bunu hesaplayabilen bir algoritma yok, cunku runtime'da bu
agac sarkabiliyor, index'i yanlis kuararsan, surekli yeni
kayit, en sona eklenirse agacin bir yanina dogru sarkma
basliyor
dba'ler index'i arada bir silip yeniden olustururlar.
butun crud'larimiz indexler uzerinde de islmeyapmayi
gerektirir.
oeltp - dss birbiriyle kapisir
index istiyorum - hizli kayit icin min. index istiyorum.
burada da housekeeping ise yariyor: tabloyu bol, arsivle
piyasada disk gibi ramdisk'ler var. cok hizli

saganin hedefi mw'deki business parcalari, benim mw'im baska
bir ms'le konusuyor, 3. parti bir servise gidiyor.

atomic integer kullanabilen hz redis
kac like aldim? bunun rakaminin ne onemi var? count tutma
sistemleri asenkron olurlar trnasacton olmazlar exact kac tane
olmasinin bir anlami yok.
her bir islem bir kayit yazilir, count kullanilmaz, onun
balance'i sum.
hep guncellenmiyor, benim bir ozet tablom var: 1.gun, 2.gun
seklinde, ben bugunu yaziyorsam, dunun devrini aliyorum ve
bugununkileri de sum alip gosteriyorum sana.
butun finansal sistemlerin raporlari ilk satir: 1 onceki
gunden +- devrettim, sonra bugunku islemleri yaptim, dip
islem.
mesela bir bankanin kac kredi karti vardir? bunlarin dokumunu
vermek cok uzun is, gunluk rapor kullanilir.
buraya +100 yazdim, sonra bir kayit daha attim -100, geri
aldimla ugrasmadim, update'in maliyeti tdaha yuksek
last page insert ozel bir becerisi, db'nin son data page'ini 3
tane tutuyorlar oeltp bu; surekli insert olur, tersini islem
yapmak ters bi kayit atmaktan ibaret hale gelir.
zaten is kurali bana dioyr ki be ntarihcesini istiyorum: bu da
bana onu veriyor, burada housekeeping yapamiyoruz.

**SAGA WAL mi?** buna gerek yok, hoca acikladi.

saga aslinda dagitik bir transaction pattern'i, ama bu kavram
two-face'e bu atfedildigi icin kullanmiyoruz

ms'lerde birbiriyle iliskili transactional state tutmaniz
gerektiginde saga'ya ihtiyac var; bunlar desen, birebir
aynisini yapmaniz gerekmiyor, farkli farkli yontemler de var;
state machine kullanmazsiniz da bpmn vs. var.

genel olarak tercih edilen kullaniciyla rest uzerinden
onusmak: req gidiyor json, res geliyor json
fakat ms'lerin kendi aralarinda konusurknn ne tercih edeginiz
size kalmis, gnelde burayi da rest birakiyoruz, cunku gidip
gelen verinin tipi cok onemli. ms'ler aralarinda binary
protokolle mi konussalar, egerki verinizin onemli bir kismi
string'lerden olusuyorsa "cari, siparis, stok" binary
protokollerle ugrasmanizin bir anlami olmuyor cunku bunlar
numerik alanlari sikistiriyor, string'leri sikisitiramiyorlar,
fazladan json'daki ayni attribute adi defalarca
tekrarlaniyorsa bunlarda binary protokollerin bir katkisi
olabilir ama tekrarlanmiyorsa avantaji yok. tekrarlandigi
durumda bile bir metrikle izlenebilir. performans acisindan
cogu sistemde 2 ucta transparan intersecptorler var, siz bir
sye istediginizd networkten once zipleniyor, sonra o zip
aciliyor, network'te zip akiyor, zipin sikistirilmasinin
katkisi string'lerde daha iyi oluyor, binary protkol kullanmak
cogu zaman degmiyor, burada ne tur bir servis kullandiginiz
cok onelmi bir iot servisi kullanacaksaniz binary protokol
tercih edin, bu benim log'larimi da ekliyor, bbinary protokol
kullanirsam
ben suraya bir interceptor koyarim, gelen her se buraya
log'lansin olur? bunu diyoruz da cunku 1 sistem monitoring
hata ayiklamada ise yariyor, digeri de
header'a log id, log icin trace id, transaction icin saga id
gonderiyorsun
http header kullaniliyor, ana verinin icerisinde koymak sacma
header ve payload farkli, interceptor'lerde header elimd,
payload'u interceptor maliyetimi arttiriyor
url kullanmaya yanasmiyor, api versiyon dahil header'da
gondermeyi tercih ediyorum, url'den gonderdigimde course
hatalari cikiyor. http header'larla ilgili soyle bir
guzelligimiz var: gunluk yasamda http1.1 kullaniyoruz. http2
halen oturmus degil, http3 yakinda release olacak, bazi
sorunlari cozecek ve daha yaygin kullanilacak.
header'da gecen bilgiler: ben su dokuman formatini, saga id'im
bu, key value veriler gidip geliyor, bazen payload'dan daha
buyuk oluyorlar ozellikle jwt'niz buyukse, ayni bilgi gidip
geldigi noktada http2'yle gelen, protokolde diyeyim mki
"aynisi var, onu sakla kullan" oturumda index'ini kullanmaya
basliyor. 1.req'te hepsini gonderiyor, 2. req'te hedar zaten
orda, 3. no..u header, bir de 12. nolu header'i kullanacagiz,
ssl kullanmadiginiz
session hijack yaparlar. headerlardan token'lari toplayip
islem yapmaya calisirlar, bunlar http2 ve 3'te de varolmaya
devam edecek, ssl tuneli icinden gecmiyorsa veri, zaten
hepsini  izliyor. hijack nerede? browser aciklarini kullanarak
yapiyorlar. modern tarayicilar suna zorluyor: bu cookie'lerin
guvenligini 3.parti owaspcilarin ugrastigi en temel sey, siz
script inject edebiliyorsaniz zaten headerla ugrasmaya gerek yok
orm tool'lari artik sql injection'u engelliyor.

## soru

bpmn'in cizdigi var mi: bpmn.io dogrudan browser'da ciziliyor.

## devops

asiri yuklenilmis ve tanimi tam olmayan bir sey. burada klasik
sistem yonetimi yapan insanlar olabiliyor.
network performans konfigurasonu icin gereken parcalar, kernel
tuning, mesela: nodejs performansi
sn'de 3000req gelecek ve response zamani 50ms'yi gecmeyecek.
linusta tcp soketi
cevabi verdikten sonr 300ms daha acik kaliyormus
farkli yerleren istek geliyor,
yuk testlerinde sorun yasaniyor
socket timeout suresini dusurunce cozuldu.
devops hem gelistirme sureclerinden hem de sistem yonetimi
acisindan haberdar olmasi gerekiyor.
burada bir urunun detayindan veya bir programlama dilini
yapmaktan bahsetmiyorum, jenkins urunun o konfigurasyon
detayini blmem gerekmiyor, gerektiginde onu arastirip yap
thread management'inin detaylarini blmem gerekmiyor, process sureclerini nasil
isletebilecegimi bilmem gerekiyor.
ara bir alan oldugundan, uzmanliktan ziyade bir ust bilgi
gerektirir.
karmasa arttikca butun bunlara tepeden bakip ekipleri koordine
edecek birisine ihtiyac var. gelistiriciye "derlerken soyle
yapacagiz cunku pipeline'da boyle yazacagiz." diyeblmek lazim
devsecops, gitops gibi kavramlar da geliyor.

### Pratikte yapilanlar

- configuraton-as-a-code
  - gitops (webhook'la yapilmasi)

mutlaka git'te tutulmasi gerekiyor
etckeeper: linux'ta etc klasorunu git'te tutan bir arac.
boylece herhangi bir degisiklikler git'te tutulabilir. en
azindan takip edebilelim.

- ci/cd surecleri:
duzenli derleme
duzenli deploy
ister tekton ister gitlab runner ister jenkins ister github
actions, pipeline'larinin dosya olarak yapilmamasi.

- surum cikarma politikasi ve bunlarin surecleri
gelistiricilerin test sistemleri demo sistemleri, yuk testi
sistemleri sadece canliya almiyoruz. adimlarin birileri
tarafindan denetlenip isletiliyor olmasi gerekiyor.

- **knative gitops**: gitteki bir surec operatoru tetikliyor o da
  bir crd'yi kaldiriyor. **bunu arat kesin ornekler vardir**

semantik ...
bugfix'te baska hicbir sey eklemedin, RN'lerde bu sekilde
duzgun gitmen lazim.

kvkk'ya gore ben ariyorum: benimle ilgili tum datayi silin
diyorum: veriyi anonimize etmek zorunda. bu algoritma olarak
kolay bir sey degil; hedef tespit edilememesi gerekiyor.
bazi sistemlerde ise bunlarin saklanarak kriptolanmasi. peki
nasil sorgu atacaksin, ben operasyoncuyum ama ben bile arayamiyorum.

kurumsal uygulama gelistiren yerler type safe olmayan bir dil
kullanmaya yanasmam. programin surdurulebilir olmasi, omru,
dinamik typesafe olmayan yerlerde yapilan hata miktari cok
fazla.

## Surum yonetim politikasi

SemVer: semantic versisioning, oneriliyor
kurallar zaten belirlenmis, bunun icin ekstra bir sey yapmaya
gerekyok. bazen 3 digit yetmeyebilir, api surumuyole uygulmama
surumunu birlikte takip etmek istiyorum o zaman yetmiyor,
majorden sonraki api surumu diyebilirsin
bunu kullanan ama farkli davranan bir ekip var: javacilar
cunku, bunun pesine -beta, -rc gibi ekler gelir. ozgur
yazilimdakilerse -final, -GA gibi. ozgur yazilimki hicbir
zaman sonuc surum yapmiyoruz ki? GA: general availibility
#copilot_onerdi
yani genel kullanimda, beta: test ediliyor, rc: release
release train: 6 ayda 1, her sprint sonunda surum cikartirim.
ben sona sprint no.sunu ekleyecegim, boylece bilebileyim

- ayni no. 2 farkli surum icin kullanilamaz
- incremental olmali

linux'un yontemi: tek sayilar gelistiriciler , cift sayilar
genel kullanim
bir yontem daha var: biz pazarlamadan bahsetmiyoruz, onlar
baska bir surum yonetimi yaparlar, pazarlama surumleri, teknik
olarak bir sey degismemistir ama teknik surumde bir sey
degismemistir, satis yapabilmek icin yeni bir sayi/isim
verirler.

firefox quantum
active qm artemis bunlar proje adi. urun adi hala active mq,
pratikte kullandigimiz artemis. bazen kafa karisikligina yol
aciyor.
apache jackrabbit content repository kutuphanesi. bu genelde
ozgur yazilim dunyasinda olur, proje
egerki markaniza guveniyorsaniz, ona piyasadaki urunlerinize
marka birlestirmesi yaparsiniz (tum urunlerin sonuna SA koymak)

bu no.'lari koduma ve artifact'lerime bir sekilde vermem
gerek, canlida bana bir hata geldiginde o surum no.sunun
kodundan kaynak koduna erismem gerekir: bizim tag'lememiz

git linked list kullanir

derleme: compile'a da build'e de deniyor. burada kastettigim
build degil

gitlab-github release ile artifact'leri yayinliyor, kendi
api'leri uzerinden, bir sayfa olusturuyor

gcc ile c dosyasini derleme o uzantili dosya sonra linker
calistirip elf ya da exe halinde derledik.
docker imaji olusturmam icin gereken her sey benim icin kaynak
kod ve derleme sistemim icin her sey artifactler

- git flow'un bir adim sonrasi gitops'a donusuyor
branch ismini rastgele veriyoruz convention main: ana branch
baska bir akis turu de mainline: daima ama daima **derlenebilen**
en son en guncel kod olur. surum degil. fonksiyonalitenin
duzgun olmasindan bahsetmiyorum. main'den bir feature branch'i
cikarim. isi bitince bu main'e merge alinir.
hangi sartlarda merge'i kabul edcegiz?
merge talebi (PR)
webhook jenkins'i durter
ci pipeline'da derlenebiliyor mu? hata var mi?
code review'e duser
guvenlik gerekceleriyle soylemiyoruz.
statik kod analizi cihazi mekanik oldugu icin bakip geciyor.
mesela sinif isimleri turkceyse ondan anlamadigindan geciyor.
ayni noktadan cikan 2. branch geldi onu da merge istedik,
pipeinline yine calisti vs. ilk gelen kabul ettigimiz merge,
2.yle ayni noktaya dokunmuslar, conflict var. bunu coz bir
daha gonder.
git'te conflict cekmek rebase gibi sahane bir komutuz var.
ilk commit benim kullandigim bi yere parametre kirmis, api'yi
kirmis, benimi de onunki de derleniyor ama onunkinde nsonra
benimki derlenmiyor. pipeine'inizda bu branch derlendikten
sonra benimki de  derleniyor mu diye bir otomasyon yapmak
gerek.
**rebase**'i sor

mainline'da surum release branch'iyle gider. cunku baskalari
feature'dan bir seyler cikartmaya devam ediyorlar.
surum brach'inde politikamizin gerektirdigi baska yerlere
bakiyoruz: surum adayi cikarticam kullanici kabul test
ortamina koyacagim, bakacaklar, hatalar varsa
eger boyle seyler yoksa bile yeni bir aday cikartacagim, yani
tag'ler hep artiyor burada
surum her zaman ci/cd'den cikar birisinin makinasindan cikmaz
surumum v1.0.0 diye tag'ledim canliya ciktim. hayat main'de
hep devam ediyor. canlida hata bulduk.
simdi v.1.0.0'dan hata branch'i cikilir, hatayi duzelttim ve
feature branch'ine merge'ledim. tek farki hatanin tespit
edildigi yerden acilir. feature branch'i hep main'den acilir.
release'i yaptim, fakat main'de bu hatayla ilgili henuz bir
sey yok. bu surec hep manuel isler. (hayat main'de devam
ediyordu ya) acaba o hata main'de hala var mi, cunku mesela
ben bir kutuphane eskiydi, ama o cozuldu vb. dolayisiyla ayni
hatayi main'de tekrar cozmem gerekir. ama ayni kodu tekrar
koyacaksam, sadece bu commit'i al ve main'e getir derim:
cherry pick'le commit id veya id range'lerini alirsin. fakat
bunu yapabilmek icin baska bir sart var, saglamiyorsan basin
daha cok belaya girer : conventional commit (atomik commit)
bir commit sadece ve sadece 1 seyi cozer. 1 sey mumkunse 1
commit'le cozulur, eger gelistirmede 2 commit'le cikiyorsa
squash'la tek commit'e almak lazim. yarin cherry pick'ler
birden fazla sey aldigimizda, biri hata verirse, ayiklamak cok
zor olur o sebepten yukaridaki sart saglanmali.
cherry pick yaptiginizda yaptiginiz isin izi kaliyor. kodu
elle kopy paste yaptiginda o iz yok, sonra ayni kod blogunda
iz olmadiginda, bu kodun neden tasindigini neyi cozdugunu
bilmiyoruz.
o commit log'u duzgun yazmasi: bunun icin onerecegim
"conventional commit log"
commit mesajinda bir pattern var:

- baslik  (ilk satir baslik kabul edilir)
- bunun onune bir tag koyalim: feat: yeni bir ozellik
- fix: bir hata duzeltildi
- refactor: kodu duzenledim, hicbir seyi degistirmedim.
- docs: dokumantasyonu duzenledim, hicbir seyi degistirmedim.

su dosyada sunu degistirdim yazmayin: neden yaptiniz?
aciklamalardan sonra ref: bu isin yapilmasina gerek olan is
takbinin id'si yazilir. jira gitlog'larini takip edip o isi o
kodu oraya baglar.
changelog, release log yazacaksak, is id'den notu
cikartabilirim.
isler de atomik oldugundan her bir commit 1 isi cozuyor.

- rebase'i biraz acar misiniz merge yerine mi?
bu commit'leri tek bir commit'e sikistir, cherry pick yap
feature branch'inde squash'a zorlamiyorum birden fazla commit
yaptiysan zorlamiyorum, bazen tercih de ediyorum farkli

branch'i surdan almistim conflict'i cozmeye baslamadan once
once rebase yapiyorum **bunu calis**
git changelog diye hocanin da commit'inin oldugu bir urun var,
ref id'leri yakalar, jira redmine api'lerni kullanip
issue'larin icindenbilgiyi alir, sonra da hangi template'in
"handlebar" kullanacaksaniz, bana soyle bir changelog text'i
urup, sonuc urunu atiyor. (jenkins pipeline'i ile)
branch'ler ucar, commit ucmaz.

silip silebileceginiz tek sey son commit'i
push'ladiginiz hicbir seyi yerelde degistirmeyin, cunku birisi
onun uzerine bir sey yapmis olabilir.

merkeze push'landiktan sonra squash yapmak da anlamli degil
henuz kullanicidan kabul almamis hicbir sey main branch'e girmez

o testini mamaladiktan sonra merge request'ini devam ediyor.
2 merge'i aldiniz test ortamina soktunuz, ozel test branch'i
acip oraya merge aliyorsun, sonra asil rleease'e hangisi
girecekse onu aliyorsun.

surum no.yu bunlara bakarak otomatik verme sansiniz var: minor
mu bugfix mi arttiracaginiz noktasinda: commit'te feat
geciyorsa

is takip sisteminde de bunlar belirtilmis; buradan da isin
tipini alabilrisin.

bazi uygulamalar her seyi moduler tasarlamistir, her feature
kendi dosyalariyla ugrasiyordur, sizin directory
structrure'inizla ilgilidir. dil ceviri dosyalari kolaydir,
cunku her ikisini de kabul edersiniz
ide'leriniz utf-8 olsun
satir sonu karakteri ctlf

git'in bir becerisi, commit'leyenle farklidir, kim suclu yazan
ben degilim sansiniz olabilir
linux'larda carriega return sadece
macos'larda linewitdh'tir sadece
ltf: line feed, cr: carriage return

surum conflict: bagimlilik cehennemi bazen api kirilmiyor,
bugfix'ler api kirmaz desek bile surum no. semantic
versioning'e uymamis olabilir. bagimliliklari kimin hangi
surumde kullandigini takip etmeye ihtiyacimiz var.

## Paket yonetimi

dependency graph'ini size dokerler.
butun derleme sistemlerine bagimlilik yazarken npm'de
package.json'un icine A^1.0 (min.1.0, uzeirne en guncel neyse
onu getir.) bu tehlikeli bir yontem, cunku bir derlemeyle
sonraki arasinda surumleri degismis olabilir. farkli derleme
araclari farkli cozumler sunuyor, disarida bir de package.lock
dosyasi var, bu lock odsyalarinin git'e atilmasi lazim. bu
dosya derleme sirasinda hangi surumleri
disariya cikan hicbir sey git'e gitmez ama bu istisna, burada
tutuyor. ya da sapkayla vs.yle degil, exact tag'i neyse onu
yaziyorsun. java'da boyle bir lock dosyasi da yok, ben zaten
istemiyorum, package.json'a da sapkali yazmasinlar zaten, ben
sistemde ne kullaniliyor bilmem lazim.
npm'de transient bagimliliklar da var. orada bu sapkalari
kullanmis olabilirler
devamla: ben kendi urettiklerim icin de ayni sekilde olmak
zorunda. urettiklerimi bir artifact depo'suna koymamiz
lazim.surum no.su ile koyarlar, internal kutuphane
urettiginizde bunu kullanacak birisi tekrar derlemek zorunda
kalmaz. "kutuphanenin uzantisi nedir?"
kurumsal ortamda kurumun artifact deposuna sorar, kendinde
yoksa gider internette o arar. bu aramayi yaparken de
cache'ler.
md5 control var ya; derleme surecinde imzalama yapabiliyorsun.
sonrasinda bu imzayi kontrol edebiliyorsun.
nexus ayrica bilinen bir acik varsa onu da soyluyor.
artifact dememin bir nedeni var; sadece bir jar, npm'den
bahsetmiyorum. jar'i aldim bunlari birlestirdim bi bor yaptim
sonra conf dosyalariyla deployment;a hazir hale getirmek icin
conteyner imaji yaptim, sonra bunu k8s'e atmak icin helm paketi
hazirladim. sen su imaji al su depl.yaml'ini kullanarak deploy
et sonra da configmap'e sunu koy gibi tanimlarin yapildigi
ayni bagimlilik agacini kullanir o da. benim servisimin
yuklenebilmesi icin mq'ya ihtiyacim var, sen git once mq'yu da
deploy et sonra gel benimkini deploy et. bu bagimlilik agacini
yonetiyor
bazen artifact'lerin manuel yuklenmesi gerekebilir. onlar
bazen koyabilirler, gelistiriciler oradan sadece read-only
gorevini yapar, urettiklerinizi koymanin gorevi ci aracinin
gorevi.

gelistiriciler icin: wildfily'i ben rpm yapabilirim. zip'i ben
kendi user seviyemde aciyorum.

wildfly pratikte helmle kurulmuyor mu
helm'in nesi nexus'a atiliyor: helm icin bagimlilik agacini
iyi yonetiyor
nexus'un helm  git deposunun bir branch'ini bir bagimlilik
olarak yazmayin, onlar source code'dur, surum garantisi
vermezler.
nexus gibi bi rseyde tutsan bir onceki versiyonu da var, ama
github'ta tuttugunda boyle bir durum yok.
npm'i isleten bir ozel sirket ve sartlari surtlari yok
maven'i islte nbir vakif, maven centrale bir sey eklemek kolay
degil, asla geri cikaramazsin

adamin birisi bir javascript kutuphanesini devralmis otomatik
surum guncellemeyle kripto mine eden bir kod eklemis.
temizleme uzun surdu.

bazen kaynak kodlarinin fork'larini almakta fayda var.
maven'da kaynak kodlarla ilgili bir sart yok.
merkezi duzgun yonetilmeyen sistemlerin fork'unu aliyorum.

once helm paketi uretip tarihce ve gaimlilik cozumlemesini de
takip ettigi icin onun takibini de sizin yapmaniz gerekiyor.
bu isi gitops'la da cozemezsin onun icin burayi helm'le yap.

ops'ta yapilacak her sey paketlerle ve duzgun otomasyonla
yapilmali: surum takip sisteminde kendi deposunda durmali.
koda erisme hakki olan herkes sizin infrastructure'unuzu ifsa
etmenize gerek yok.

ci her zaman son kodu alir gelir, senin lokalindeki kodu son
kod oldugunun gorevi yok. her sey dokumante durumda adim adim.

uygulamalar ide icerisinden derlenmek zorunda degildir
herhalde
bazi projeleri ide'nin disinda derleyemiyorsan simdiye kadar
konustugumuz her seyi unutun.

open core is modellerinden bir tanesi ozgur yazilima
community'e actigi var, bir de enterprise versiyonu var. bunu
sirket veriyor. fena olmayan bir is modeli; buyuk kurumlarin
ihtiyac duydugu seyler var.
ozgur yazilim dunyasi soyle bir sikinti yasiyor: her sey
cloud'da oldugu icin artik bir sey kazanamiyorsun. adam gelip
benim kodumla benim servisimi calistiriyor, ben yaptigim
commit'leri aliyor ben onun yaptigi commit'leri alamiyorum.
lisansi degistirdiler: "sen kullanma" dediler.
hashicorp'a terraformu kapattilar.
bir urunun base'ini olustur, finansmanini da vakfin onlar
yapiyor, bunlarin bu turden bir lisans kullanmasina herhangi
bir itirazim yok, akademiler devletler, devletler de biz
insanlar tarafindan fonlaniyoruz.

### Pipeline

dumduz groovy'dir, dls yazmak cok kolaydir. sanki groovy degil
de ozel bir configurasion dosyasi
ozellesmis bir syntax'la dsl yazmayi cok kolay kilan, curl
bracket'lara kosulan sartlar yok. ben onlarin parametre
oldugunu zaten biliyorum diyor dil. step fonksiyonun adini
yaz. onlar aslinda degisken isimleri oluyor.
groovy'nin java'dan ozellesmis olmasinin bir anlami yok. kodla
ayni repo'da olmaz, ci/cd surecleriyle ilgili parclalari
gormemesi gerekir. sistem altyapiniza ait bilgiler olacaktir,
bunlarin disariya sizmamasi gerekir. isi olmayanin bunlara
sahip olmasini istemeyiz, bunlar infrastructure'u yoneten
kisiler tarafindan surekli degistirilirler.
github'ta bazi repolarlda jenkinsfile, circleci vb., bunlar
icin ayri repo tanimlayip onu oraya baglamayi yapamadiklari
icin source code'la ayni repo'da yasiyor. mecbur olduklarindan
dockerfile, helm'i surum cikarma repo'sunda tutuluyor,
disariya external edebilen araclari var: jenkins'te yeni job
olusturuyorsun: jobdsl diye bir dili var text dosyay yazip
jenkins bunlari alip otomatik job'lari kendisi olusturuyor

job: tetiklenenbilen pipeline'lar. ne zaman tetiklenecek,
bagimli oldugu job var mi? hata alirsa ne yapar
o pipeline, bunu tetiklemeden once kim nasil tanimlanir,
bunlari sormasi lazim, sonucunda su raporlari uret,
su notif'leri yap (rocket chat)
job-dsl'de yaziyorsun.
pipeline'da ise adim adim derleme surecinin ne yapacagini
yazmaya basliyorsun

- environment hazirlamak. bu yuzden k8s gibi sistemler
  kullanmayi oneririz. jenkins rancher uzerinde kosuyor,
  primary jenkins node'u hicbir sey yapmiyor, sadece job
  tetiklendiginde yeni bir pod aciyor, configurasyonda ayni
  anda 3 pod calisiyor. devamla gitlab tarafindan tetkleniyor,
  merge pipeline'i, merge acildigi anda gitlab jenkins'e diyor ki
  bana su job'u calistir. k8s'e test deployment'u yapiyor, is
  takip sisteminde bir issue aciyor, human adimlari da var.
  bpmn'inkine benzer bir surec hazirliyorsunuz, programlama
  teknikleri kullanmalisiniz (dry- kopyala yapistirla pipeline
  yazmayin) pom.xml'de package number degistiricem, bir
  kutuphane olarak hazirlayin sonra da bunu gelistiriciye
  soyle, "versiyonunu guncelle" diye  **buraya sor**, betik
  olarak yaz (ansible'la) tekrar tekrar ayni seyi calistir.

wildfly glassfish'in aynisi

jenkins main'i pod'da niye tutuyorsunuz
test etmen gereken pipeline'i da kod gibi yazdigim test
jenkins'i aciyorum bi deneeylim kolayligi sagliyor olduysa
merge alalim

tekton
PR'la yonetiyorlar, kurali al, mvn clean install calistir.
maven bagimliliklari kontrol etmeye baslayaca
git suradaki nexus'tan al, onun uzerinde yoksa git proxy'den
interenette al
javayi compile etti, baska bi ton is yapti, n sonunda war
uretti. web uygulamasi paketledim
war'in uzerine testleri kosmaya basladi
functional test'lerini kostu
jenkins gitti sonar'a statik kod analizini yapti
burada sonar build'i kirmadi.
urettigi war'u nexus'a koydu
kullanici testi icin uat'i teste
deployment yamli'ini kullanarak pod aciver bana, imaji da
nexus'a koydu
k8s nexustan imaji alip calistirdi
jenkins pipeline teste deployunu yapti, ilgili kisilere bilgi
vermesi lazim.

**"merge request"**'in altina bir yorum ekliyoruz (gitlab'ta)
sonar'dan su notu aldi.
jenkins job linki surada, deployment da url burada
gidip kullanici ayarina
rocket chat botu: jenkins'e bir job calistir dedigimde hata
olursa gidiyor.
burada jenkins job'unu kapatmak yeirne kullanicidan onay
bekleme moduna alabiliriz, biz job'u bitirmeyi tercih
ediyoruz, kendi basina bir pod'da calisiyor. o pod'u oldursun
istiyoruz.
onay verme surecinin karsiligi olarak jenins'te bi
housekeeping jobu'un var devaminda 2. gun siliyor.
bunlarin hepsi ozgur yazilimlar.

branch bazli taramanin yontemi var. jenkins'te main olarak bak
demek yerine. housekeeping'le kapatiyor.
bir baska insan etkilesimi bekliyor, merge request'i kabul
ettim veya reddettim. bunu jenkins'e soylemek lazim.
maven bir conteyner, node bir konteyner, pipeline'in
icerisinde ben maven'in hangi surumunu kullanacagimi
degistirecegimi yazabiliyourm.

helmin burada numarasi degil, derleme pipeline'inda degil.
deployment pipeline'i calisiyor, o nereye target'ini secmen
lazim, ona gore ayni cluster olabilir olmayabilir bambaska bir
cluster olabilir farkli deployment stratejiniz olabilir,
burada gidip de deployment yaml'ini bulup demiyorsun,
merge request ayni repo icinde 2 branch arasinda
2 farkli depo'da islem yapip repolar arasinda
bunlar git terimi, merge request'i e-posta atabilirsiniz

test containers diye bir porje var ornegin, container bazli
#copilot_onerdi
test yapmak icin, bunu jenkins pipeline'inda kullanabilirsiniz

test icin ayri bir deployment yapip ayrica yapabiliyorsunuz.

nexus'a war, image, helm ciktilarini da atiyorlar,
maven'in 3.9'unda calisip 3.6'sinda caliisamayan bir
pipeline icerisinde kullanamazsiniz.

deployment pipeline'in daha az degisir, nexus'ta soyle bir sey
var, bunu su target'a deploy edeceksin, parametre bu kadar.
geri kalani helm'in icinde yazili.

<gelenmeryem@gmail.com>

gradle'i ureten ekip major surumler arasinda api kiriyor,
6'daki plugin 7 gelince calismiyor mesela.

3'te kullanilan pom.xml'in formati 15 yildir hic degismeden
kullaniliyor.
gradle aslinda ant'la maven'in karisimi bir sey, groovy'le
program yaziyorsun.  mavend var arkada process olarak canli
duruyor, durttukce yeniden ayaga kaldirmadan derliyor.
maven'da "derleme sadece su testi calistir" var maven paralel
derleme becerisi var, multi-modul bir proje, 30kusur modul
paralel 5thread'le hizlica maven install calistirinca her seyi
alip hazirliyor, lokal depoya koyuyor, halbuki benim tek
amacim test calistirmak: `verify` yerelde gelistirici
makinasinda ilk derlemede cache'ler, tum derleme sistmeleri
siz clean demediginiz surece simdikiyle diff'ine bakar, fark
yoksa onceki cache'ten doner

SBOM her seyin log dosyalarinin tutulmasi: bu urunun bagimli
oldugu her sey. onlarin isimleri, dolayli olarak bagimli
olduklari, surumleri, her sey. gelistiriciler sadece dogrudan
kullandigi bagimliliklari biliyor, bu onlarin bagimliliklarini
da cikartiyor. butun bi rpaket yonetim sistemiyle
bagimliliklarinizi tanimlarsaniz guvenlik ekipleri paketler ve
surum no.'lariyla db'lerde tutuluyor. boylece herhangi bir
yerde bir guvenlik acigi varsa onden bilgi sahibi olup
aciktan etkilenen yazilimlar kapanmadan bunlar duyurulmuyor.

pratikteki sorunlar

- acik iceren paketin bagimliliklarini uygulamamiz icin
  yaratacagi sorunlari cozemeyecegimizi biliyoruz ve onu
  yamamamiyoruz.
- uygulamanin 10 yil onceki surumunu kullaniyorlar, rhel
  destegi keseli cok olmus

backport ettik: gidip rhel'in kodunu yamadik.
uygulama hem wildfly hem java ee destekledi. java ee
destegini kestiler: jakarta'ya gecirmemisler, testlerde
aciklar cikti. once jakarta ee'ye goc ettirmeniz gerekiyor.
surecler uzadikca sorunlar da buyuyor
