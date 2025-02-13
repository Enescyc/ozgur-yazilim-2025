# Mikroservisler ve Devops

- [Mikroservisler ve Devops](#mikroservisler-ve-devops)
  - [ozgur yazilim nedir](#ozgur-yazilim-nedir)
    - [kodlama lisanslari](#kodlama-lisanslari)
    - [genel icerik lisanslari](#genel-icerik-lisanslari)
  - [Mikroservis Mimarisine Gecis](#mikroservis-mimarisine-gecis)
    - [Mikroservis kullanmali misiniz?](#mikroservis-kullanmali-misiniz)
    - [Mikroservisler Hangi Problemleri Cozer?](#mikroservisler-hangi-problemleri-cozer)
    - [Mikroservisler Nasil Yazilir? (Temeller)](#mikroservisler-nasil-yazilir-temeller)
    - [Mikroservislerin yarattigi sorunlar](#mikroservislerin-yarattigi-sorunlar)
    - [Surum politikasi](#surum-politikasi)
    - [Sanallastirma Yontemleri](#sanallastirma-yontemleri)
    - [Kubernetes (k8s)](#kubernetes-k8s)
    - [Ileri takip edilecek konular](#ileri-takip-edilecek-konular)
    - [linkler](#linkler)

## ozgur yazilim nedir

acik kaynak kodlu yazilim, ozgur yazilim, freeware (bedava)
yazilim arasindaki farklara degindi.

- yazilimi ozgur yapmanin dezavantaji: topluluk urune olan
  ilgisini kaybederse urun dogal omrunu tamamlar. big tech bir
  dilden, teknolojiden vazgecerse onu apache'ye, eclipse'e
  bagislar. mesela piyasada kotlin'in baskin olmasiyla redhat
  seylon'u apache'ye bagisladi, 3 ay once de dili gomduler.
- linux'un baslangici, torvalds'in lab'de kullandigi unix'i
  evdeki x386 makinesine de kurmak istemesi
- postgresql universitelerin buyuk destegi ve bir de birkac
  ana sirket sayesinde gelisimini surduruyor.

### kodlama lisanslari

- GNU GPL lisansta 4 madde var.
  - Programı sınırsız kullanma özgürlüğü.
  - Programın nasıl çalıştığını inceleme ve amaçlara uygun
    değiştirme özgürlüğü.
  - Programın kopyalarını sınırsız dağıtma özgürlüğü.
  - Programın değiştirilmiş hâlini dağıtma özgürlüğü.

GPL disinda ozgur yazilim lisanslari var, detaya girmedik.

- GNU LGPL
- GNU AGPL
- GNU FDL

bu lisanslar para kazanma modeli de sagliyor. Lisanslar
arasindaki temel degisken, kendi kodunu yazilimin koduna da
eklemen gerekir.

- apache public license'ta ise farkli: ( #copilot_onerdi: mit
  license, bsd license) istersen kendi kodunu kapat, istersen
  acik birak, lisans buna karismiyor.
- mozilla lisansinda referans vermen lazim (hakan'in kodunu kullaniyorum)

### genel icerik lisanslari

- CC-BY-SA: creative commons attribution, share alike
  ikonlar, icerikler, fotograflar, javascriptler, cssler, html'ler
- sadece BY geciyorsa benim adimi mutlaka orada soyleyeceksin
- NC: non commercial, ucretsiz dagitabilirsin. ticari
  amaclarla kullanamazsin: t-shirti istedigim gibi
  bastirabilirim ama bu basili olani satamam.
- bir cok icerik github'ta public olarak yayinlansa bile alip
  kullanamazsin, yayimlaniyor diye herhangi bir kod parcasinin
  anonimligi yok.
- tr'de bir firma ffmpeg'in bir kod blogunu kullanmis, ffmpeg
  gpl'li oldugu icin yakalaninca yuz kizartici sekilde bir
  listeye eklendiler, sonrasinda kodlari acik yayinlamak
  zorunda kaldilar.

## Mikroservis Mimarisine Gecis

[Definition](https://en.wikipedia.org/wiki/Microservices):
There is no single, universally agreed-upon definition of
microservices. However, they are generally characterized by a
focus on modularity, with each service designed around a
specific business capability. These services are loosely
coupled, independently deployable, and often developed and
scaled separately, enabling greater flexibility and agility in
managing complex systems. Microservices architecture is
closely associated with principles such as domain-driven
design, decentralization of data and governance, and the
flexibility to use different technologies for individual
services to best meet their requirements

- zayif baglilik: derleme seviyesinde kimin implement ettiginin onemi yok.

- Servis Nedir?
  - SoA: normal bir SOAP web servis olusturuyorsun, SoA
    urunleri izliyor, ayaga kaldirmak icin arac-gerecler
    sagliyor. onlari kontrol ediyor
  - RPC/RMI/IPC
  - WebService
  - SOAP
  - REST
  - gRPC: google tarafindan gelistirildi, cok sayida servis
    oldugundan, surekli json dolastirip parse ederek surekli
    network trafigi olusuyor, protobuff olarak bunu binary
    tanimlayayim. burada apache de avro diye bir binary
    protocol tanimladi http put-post yetmedi, bazen action
    command gondermen gerekiyor. tasarim deseni olarak
    yetmemeye basladi. parametrelerini tanimlayarak RPC
    yapmaya basladi
  - GraphQL: facebook'un gelistirdigi: rest mimaride dosya
    formatlari backend'de sunucu tarafinda tanimlaniyorlar. su
    query'i atarsan sana soyle bir json veririm. facebook
    ekosisteminde butun frontend ihtiyaclarini karsilayacak
    bir backend api hazirlamak cok sikinti, biz bunu fe'de
    yapalim. bir cesit rpc calistiriyor, bende su komutlar
    var, fe'deki gelistirici de neyi sececegine karar verip
    query atiyor.

bu neye benziyor: zamaninda, client-server mimari vardi.
sadece verileri veritabaninda saklar, geri kalan tum isleri
masaustunde yapilirdi, burada client tarafinda kaynak sorunu
cikardi, store procedure yapar veri tabanina sorgulari store
procedure'larla  middleware'e implement edip oradan cagiralim.

ilk yazilan mainframe'ler de servis mimarisinde calisiyordu
(RPC) ve bir procedure bir servis olarak kabul ediliyordu.
birbirleriyle konusabilmeleri icin kendi aralarinda bir
networkleri vardi. sun microsystems java'yi da iot icin
gelistirdi. ama asil kitlesi kurumsal sirketlerin main
frame'leri. 90'larin basinda birbirinden farkli 10 tane
islemci mimarisi vardi. java'yi kurumsal pazara getirmek
istediler. RMI, as400'lerle de konusuyor, kucuk uygulama main
frame'e bununla baglanip is yaptirip geri gelecek. sonra
webservis cikti, binary protokollerle is yapmak zor, herkes
kendi servis api'sini yaziyordu ama burada bir standart yok,
uygulama yaziyorum, a firmasinda farkli, b'de farkli api'ler
var.  iste burada SOAP geldi. XML'i butun teknolojiler
okuyabiliyor, http protokolunu kullanarak herhangi bir SOAP
servisi yazabilirsin.  wsdl ve uddi ile tanimlandi. wsdl ile
servis tanimlaniyor, biz bunlara web servis demeyelim,
kurumsal firmalara satista sikinti cikiyor; SoA: Service
Oriented Architecture.  REST SoA'ya alternatif cikti, xml ile
ugrasmayalim, CRUD icin biz http metodlarini kullanalim. data
gondermek icin de json kullanalim.  fakat json icinde aradigim
seyi bulmam, validasyonunu nasil yapari gibi ihtiyaclar icin
json schema geldi.  tipki xml'de oldugu gibi standardize
edilmeye calisildi.  sonra json'a bir alternatif olarak YAML
geldi.  artik kendisini bir json subset'i olarak tanimliyor,
onunla ayni kurallari uygulamaya calisiyor

aslinda hep 10 yilda bir dongu halinde bir onceki mimariye
donerek yeni bir isim veriliyor.  her seferinde sorunun hem
olcegi degisiyor, hem de nitelikleri degisiyor. bir servisin
karsilamasi gereken yuk, kullanicilardan gelen arayuz
istekleri

dil html, protokumz http. cikma nedeni: CERN'de akademik
makale paylasmaya calisiyor, ern agi uzerinde text dosyalari
paylasmak zor oluyor, ne yapalim? bir dil yazalim, bunla
yazalim makaleleri, bir de link koyalim, bir de resim koyalim.
bu dilin adi html, bir de protokol yazalim, browser'dan
gostersin, sonra: biz burada sadece read-only bilgi aliyoruz,
biraz da bilgi girelim: formlar cikti.

### Mikroservis kullanmali misiniz?

Mikroservisi ilk tanimlayan ve uygulamaya baslayan netflix,
baslarda netflix'in servis tanimlari icin yazdigi kodun haddi
hesabi yok.

#copilot_onerdi

- Mikroservislerin avantajlari
  - hizli gelistirme
  - hizli deploy
  - hizli test
  - hizli geri donus
  - hizli hata bulma
  - hizli hata duzeltme
  - hizli buyume

- Mikroservislerin dezavantajlari:
  - operasyonel yuk getirir
  - uygulamada olcekleme problemi yoksa ek yuk getirir
  - bilgi birikimi gerektirir.
  - ms'lerde bagimlilik problemleri oldukca zorlayici olabiliyor.
  - ms diye basliyorsun ama onlar da gunun sonunda monolith'e donuyorlar.

### Mikroservisler Hangi Problemleri Cozer?

makroservis: monolith'ten farkli olarak 1-2 servisi bundle olarak verir

- Farkli gelistirme ekipleri birbirinden bagimsiz oldugu icin
  farkli teknolojiler gelistirebiliyorlar. Aslinda servis
  mimarisi eski bir yapi, bunlar kendi baslarina monolith
  servisler olabilirler de.. ms olmalari gerekmiyor. 2 farkli
  uygulama birbiriyle konusabilirdi etl yapmak zorunda
  kalmazdik.
- urunun farkli parcalarinin kendi hayat donguleri, mesela
  ERP'yi ele alirsak, muhasebede surekli bir seyler yapman
  gerek, ama tedarikte bir boyle bir durum yok, servis bazli
  tanimladiginda servisi guncellersin gecer, monolith'te butun
  sistemi guncellemen gerekir.
- yataya olcekleme icin, monolith'te 1 module yuk geliyor,
  sirf bunun icin butun monolith'i olceklemek gerekiyor.
- uzun vadeli gocler:  monolith'te bir modulu html sunucu
  tarafinda render edecek sekilde yazdik, artik yetmiyor, biz
  bunu frontend'de client tarafinda yapalim, burada ms'ler
  farkli cozumler yapmaya imkan saglior: A/B testi:
  kullanicilari farkli ortamlara bolerek tepkileri olcuyor.
  (monolith'te bunu yapamazsin)
- monolith'lerde bir modul digeriyle servis uzerinden calismiyor.
- bankalarda transaction gerektiginden bunu servis ile yapamazsin

#copilot_onerdi

- monolith-ms arasindaki fark: monolith'te bir hata oldugunda tum uygulama
  durur, ms'de ise sadece o servis durur.

### Mikroservisler Nasil Yazilir? (Temeller)

- atomik olmali. Her mikroservise kendi basina bir urun olarak bakmalisin.
- iliskili oldugu servislerle API'ler uzerinden konusmali.  o
  kontrata gore mock bir servis aciyorum, urunu gelistirirken
  bu mock servisle calisiyoruz, urunu cikarken de sadece
  url'leri degistiriyoruz. Ama gercek hayatta her sey
  teorideki gibi olmuyor, bunun icin  otomatik entegrasyon
  testlerine ihtiyaclarin var. yine de gelistirme surecindeki
  taraflar bir seyleri yanlis anlayarak hatalar
  yapabiliyorlar.
- SDK tek tek butun google cloud servislerini yazmiyoruz da,
  google bunlari bir kutuphane halinde veriyor, tek tek
  indirmiyorum.
- Kontrat dememin sebebi, kendim de api yazarken protokol tanimlayabilirim, veya
  rest kullanmayabilirim, ozellikle iot'de buna benzer durumlar oluyor, mesela
  sadece isiyi arttiricam, bunun icin diyelim bir endpoint'e 1 byte gondermem lazim,
  bu durumda rest kullanmak mantiksiz olur. yazarkasalar da buna bir ornek.

klasik REST protokolu: openapi-swagger
post -> bana json'u gonderirsin ben de cevap olarak json donerim

gelistirmeye baslamadan api kontrat yapiyoruz, ben boyle bir
sey uretecegim, sen de boyle bir sey, bittiginde api
dokumantasyon araclari var, bunlarin bir kismi otomatik olarak
mock servisleri de olusturuluyor. [redhat
microcks](https://microcks.io/) soap+wsdl hazirlamakla
rest+openapi uretmek ayni sey

Mikroservisler sadece kendi veritabaniyla konusmali,
baskasinin veritabanina gitmemeli, oraya erisebilen baska
mikroservislerle konusmali. Ornegin:

ms1 -> db1
ms2 -> db2
ms3 -> db3

bi uygulamam var, bu uygulama ms1'e gidiyor, o da ihtiyac duyarsa ms2'ye
gidebilir, ama db2'ye gidemez (anti-pattern)

monolith de yazsan mikroservis de, IdM'i ayirmaniz gerekir. En
kestirme cozum LDAP kullanmak. bu bir protokol, urun var bir
suredir.

ms'lerle gelen 1 trend daha var: (egitimin konusu degil) micro
frontend, sadece be'yi degil, fe'yi de kucuk kucuk paketledik,
saydada biraraya getiriyoruz. Diyelim ben x sayfayi react'le
yaziyorum, siz baska bir seyle.

SSOn-SSOut (bir yerden login oldum ve logout oldum, bana artik
ne durumda oldugumu sorma) burada kullanilan 1-2 standart var:
defacto: OAuth2, JWT, OpenID Connect gibi. bunlari
isteyebilmek icin ne yapabilirim? Cesitli IdM urunleri var,
keycloak burada ozgur yazilim cozumu

ms'in icinde, sen idm uzerinden login ol, bir token al, butun
cagrilar sirainsda bu token'i dolastirdim. en az kullanici
adin olmasi gerek. (jwt bu konuda esnek) jwt kullanmamn sebebi
bu: idm servisine her seferinde gitmeme gerek kalmiyor

keycloak'ta sms servisi desteklenmiyor. android google'in bi
servisi var, freeotp var auth flow'u kendi senaryonuza gore
sifirdan bir flow olusturabilirsin

burada oauth'un icinde varalon flow'lar: login oldugumda
aldigim jwt'yi kim kullanabilir? erismek istedigindeki aldigi
bilgiyi aldi ya, sunu da yapiyor: bu adam signout oldu, access
token (genelde 5 dk. olur) refresh token ile idm'den yeni
access token almam gerekiyor, diyleim ki sso ile idm'e
gidemedi, bu durumda en fazla 5 dk. daha kullanabilirsin.
runtime registration yapiyor

SAML da oauth'tan daha eski, frontend tanimli, token'i
tarayici alip dagitiyor. SAML'da ise token'i sunucu alip
dagitiyor. burada her sunucu tanir. Bunun daha oncesi de
kerberos'tur. JWT'de ise sunucu alip tarayiciya dagitiyor.

bugun artik sifirdan baslayan bir projede oAuth kullaniyoruz.
user id'yi artik jwt ile imzaliyoruz, bu imzayi kontrol
ediyoruz

oAuth'un 2 parcasi var:

- authc: kim oldugunu anlamak (bu adam bu mu?)
- authz: ne yapabilecegini anlamak
burada gelistiriciye is dusuyor, rollbase access , ldap
uzerinde bu rolleri onden tanimla, jwt'nin icine de login
sirasinda gomuyorsun acl kullandiysan idm urunleri sana bir
cozum sunamaz, mecburen bir yetki-rol tablosu tutuyorsun.

kendi sitemde bir login sistemi kuracagim, bunu kendi db'mde
tutabilir miyim? Hayir; bu durumda eger uzmanligin guvenlik
degilse, tekerlegi yeniden kesfetme, hazir urunu kullan.
(keycloak, auth0, cognito)

### Mikroservislerin yarattigi sorunlar

login icin uniq id olmak zorunda, bu id'yi olustururken bir
algoritma kullanmak gerekiyor.

- transaction yonetimi zor: servisleri nasil blok ayiracagina
(mikro-makro gibi) cogu servisi makro omasini oneriyoruz. ayni
transaction blogunda olmasigereken 3-5 servisin bir arada
olmasi kolaylastiriyor, developer ilgilenmiyor, alttaki
kutuphaneler destekliyor, 5 farkli kaydi tablolara
yerlestiriyor, birinde hata olursa transaction roll back
oluyor. birden fazla servise gidiyorsaniz,
dosya yukleme servislerinde boyle bir sorun yok.
dosyada mesela, db'den rollback ettin, fakat dosya yukleme,
ayni transaction blogu icinde degiller, dosya orada kaldi.
kullanicidan bilgileri aldim kendi db'me yazdim
bankanin servisine gittim, banka dedi boyle bir kart yok, bu durumda kendi
db'mdeki kaydi silmem lazim. otomati kbir transaction blogunda gerceklesemez 3
dk. boyunca transaction'u acik tutamam da, cok uzun. uygulama icinde bu isi
yonetebilecek bir mekanizma yazmam lazim.  bir de bunu 3-4 servise dagittimi
dusunelim. diyelim 3. servis hata verirse, benim geriye donup 2 servise geri al
demem lazim. (db'deki degil, uygulamanin bunlari kontrol etmesi lazim
(thread'lerle)) bunu otomatik olarak yapamazlar, uygulamanin icinde yapmak
lazim.  fis silinmez, ters kayit yenibir fis atilir (zettelkasten'deki gibi),
mesela bu burada bir cozum  olabilir.

bunun icerisinde bir soft delete gelir, insert-update'i yapabiliriz ama
delete'de soft yapmamiz lazim, sonra da gercekten silecek housekeeper yapmamiz
gerekiyor. bazi sistemlerde gercekten hicbir kayit silinmek, arsivlenir.

bu sistem tasarimi degil, bu ms tasarimiyla yazilim gelistirme lazim.

- asenkron baska problemler geliyor: burada timeout giriyor. cevabi ne kadar
bekletecegim, ya bir network sorunu varsa? aslinda bana sadece cevap veremedi, 2
dk. bekledim, garant iveren transactional bir mq servisi var.

long-running transaction'lar asiri yuk getirir
burada ne yapiliyor; belli bir koltuk sayisi var, once siin icin rezerve
ediliyor. timeout'ta islemi bitirmezsen rollback oluyor
sistem eger max. satissa, 100 koltugu satmaya bakiyorum.

sistemin bu yuku kaldirma sansi yok; ne yapiyoruz, trafigi kesiyoruz.
loadbalancer'dan bir sayfaya redirect ediyoruz. bi geri sayma koyuyoruz.

senkronda bu db'lerde rollback ediyoruz.
klasik java'nin middleware'inde hepsi buraya baglaniyor
once 1 tur yapiyoruz sonra commit

ms'te en cok kullanilan pattern SAGA, orchestration bunun bir parcasi,
transaction sorununu cozmek icin uygulama mimarisinde kullanilan

SAGA pattern (tasarim deseni) mimariden alinan bir kavram.
yazilim muhendisligi: bir seyin muhendisliginin yapilabilmesi icin, olculebilir
seylere ihtiyaciniz var, su an elimizde sadece karmasiklik  bunu bir olcu birimi
olarak kabul edilemeyebilir.
algoritmalar yetmiyor (arama, siralama gibi); mimarlar: bir banyo ornegin x,y,z
sartlarini saglamadan bir banyo olusturamazsin.
40 kusur tane tanimliyorlar: en basiti: singleton pattern: uygulamanin icinde
sadece 1 instance olacak.
factoring, building pattern gibi desenler var, bir de anti-pattern'ler var

implementasyonlar farkli farkli olabilir, dagitik transaction yontemiyle ilgili
nasil cozumler getirebilirsini. ya da siz oturup implement edebilirsiniz.
algoritmalar ve design patterns mutlaka okuyunuz.

oyundaki event'ta anlik statu gostermeye ihtiyaclari var; bir araba yuku nasil cozecegiz.

- operasyonel yuk fazla: etki analizi icin hazirlayan kisi kendi tecrubelerini
katmis ama burada ortak bir metrik yok.
**overhead** nedir? (sistemden allocate ettigi bir servis mi)
- problem tespiti zor: debug ypamaya kalktigimda hangisine breakpoint
koyacaktim, o log'lari nasil okuyacaktim
- **mikroservisleri trace edebilmemiz opentrace tracebilitiy**
- surum politikalarinizin net olmasi gerek, servisler birbirinin api'lerine
bagli, dolayisiyla api'nin surumu de belirleyici.  klasik derleme sistemi
kutuphaneler icin api ya da Abi diye gecer. abi degistiginde api de degisir. bu
durumda abi'yi degistirirken api'yi de degistirmeniz gerekir.  abi: application
binary interface, api kirmiyorum ama bi kiriyorum. drop/replacement yapmadan o
binary'i  kullaniyorum.  binary yapisinda degisiklik olursa diger programlarin
derlenmesi gerekiyor.
implamantasyon kirma: service call yaptim, api de kirilmadi. abi degisti, sistem
calisiyor ama sonuclar farkli.  bagimliligi yonetmeye ihtiyacimiz var:
dependency matrix.
sadece kurum icinde hizmet veriyorsan degisiklik olsun olmasin tum surumler icin
bi tag cikip hepsini arttiriyorsun
release notlarini cikarttim bunlar bu bu surumlerle eslesiyor.
surum numaralandirilirken SemVer (semantic version) bunu kullanmak lazim.
1.0.2 major - minor - bugfix
maj: 1'den 2'ye cikarsa goc planlamamiz gerekir. ciddi bir degisiklik oldu.
ornegin UI tasarimini degistirdiyseniz kullaniciya yeniden egitim vermek
gerekebilir.
min: uygulamada yeni bir ozellik var. eskilerde bir degisiklik yok. ne
degistigini okumanizda fayda var.
bugfix: bu durumda sadece hata duzeltme yapilmistir. (idealde drop-in
replacement yapabilmeniz lazim), gelistiricilerin de ne yaptigini bilmesi lazim.
(bu release'de neler cikti.)
aksi taktirde numralandirma patlar, her sey birbirinin uzerine yikilir.

### Surum politikasi

surum numarasi politikanin bir parcasi sadece
ne zaman cikartacaginiz,

release train: belli bir takvim var, o takvimde ne cikitiysa
onu canliya cikilir, o surume yetisen girer yetisemeyen 6 ay
sonraya kalir.  product owner'in tercihleri dogrultusunda
olur, su ozelliklerin olmasi lazim der, ne zamana olur, diye
bir politika bugfix'in bunlar disinda ayri bir politikasi
olabilir. SLA (service level agreement) o sozlesme
cercevesinde hata duzeltme cikilir.

bazilari gecmis surume destek vermez. ayni anda canlida farkli
surumleriniz varsa, her bir surum icin farkli guncellenme
surumleri cikmak gerekir burada ci/cd olmazsa olmaz

ci: sifirdan derlenmesi gerekir
statik kod analizi: kurallarin tanimlanmasi

programlama dilleri insanlar icin, bilgisayar sadece
assembly'den anliyor. programlama dilleri insanlari belli
sekilde davranamaya yonlendirir, ornegin python'daki
indentation, java'da sinif isimleriyle dosya isimlerinin ayni
olmasi gibi.

unit test ve functional testler code review bunlari
uygulamadan ms'e girerseniz halen cozdugunuz yontemlerle
calismayacak

- monitoring araci, servisleri izleyemiyorsa ona gecmeniz lazim.
- operasyon anlik verilerle ilgilenir, su anki durum nedir.
  developer gecmisle ilgilenir. merkezi log toplama sistemi
- log'lara ozel trace id'si konmasi lazim, farkli log'lar
  dusen islemler var (trace, request id gibi)
- open tracebility'de performans problemi tespit edebilmek icin hangi serviste
  neyin ne kadar surdugunu tespit edebilmek icin servisin icerisindeki
  fonksiyonlar seviyesinde metric toplamaya ihtiyac var opentraceing,
  openmetric, zipkin raporlama araclari icerisine koyun ve kullanin
- kaynak tuketimi kucuk sistemler icin fazla, ama sistem
  buyudukce monolith'e gore uygundur. overhead: giris
  maliyeti: odemeniz gereken bir bedel var. Programlama

Not:dilleri açısından çağırma ek yükü (call overhead) ismi
verilen kavram, bir fonksiyonun çağrılması sırasında yaşanan
ek yüktür. Bir programlama dilinde, herhangi bir fonksiyon
çağrıldığında, bu fonksiyonun çalışması sonucunda döneceği yer
bir yığında (stack) tutulur. Fonksiyon çalışıp işi bittikten
sonra program akışına bu noktadan devam edilir. Bu şekilde
fonksiyonlar birbirini çağırarak devam edebilir. Örneğin A
fonksiyonu B’yi, B fonksiyonu C’yi … şeklinde birbirilerini
çağırdıklarında fonksiyonun çalışması için gereken yere ilave
olarak fonksiyon bilgisi için ilave bir yer tutulması gerekir.
Bu yere ve bu yer üzerine yapılan işlemlere çağırma ek yükü
(call overhead) ismi verilir.

### Sanallastirma Yontemleri

bare metal uzerinde kosan sadece db'ler kaldi. cunku vm'leri
tasimak ve aradaki kaynak farki.  cpu simuluasyonu yapiyor,
cpu'dan payini aliyor.

- vm: vmware, hyper-v vagrant paketlersem mesela orada birisi
  girip daha sonra  degistirebilir.
- container: docker, lxc: atomik olarak bir servisi
  paketleyeyim, bir daha kimse dokunamasin ona. ci/cd'de 1 sey
  uretip onu surumlendirdiysem bu sonsuza kadar oyle olacak.
  burada prod'a yukleme yaparken sadece ENV parametlerlerini
  tanimlayabilir, env'dan portunu alabilir. veri tabanina
  baglanmak icin gereken username/password. container
  dokumanlarinda hangi env'lerin tanimlanacagi belirtilir.
  dikkat edilmesi gereken: containerd ve mobi var, bazi urunler
  mobi'deki ozel tanimlari kullanmak istiyorlar, boyle bir sorun
  yoksa containerd kullaniyoruz. docker da yakinda contariner

#copilot_onerdi

- container'larin avantajlari:
  - hizli deploy,
  - hizli test,
  - hizli geri donus,
  - hizli hata bulma,
  - hizli hata duzeltme,
  - hizli buyume
- container'larin dezavantajlari:
  - operasyonel yuk getirir,
  - uygulamada olcekleme problemi yoksa,
  - bilgi birikimi gerektirir,
  - ms'lerde bagimlilik problemleri oldukca zorlayici olabiliyor,
  - ms diye basliyorsun ama onlar da gunun sonunda monolith'e
    donuyorlar.

- sanal ag: vm'lerin birbirleriyle konusmasi icin gereken ag
  (makina icindeki servisleri) java ugulama sunucularinda ozel
  olarak tanilmamak isterler guvenlik: hangi ip blogundan
  istek kabul etsin. mesela burada 0.0.0.0 vermek gibi.

ovm (open virtual machine), farkli ortamlardaki vm'leri
birbirlerinde kosturabiliyorsun.  opentofu ile Vagrantfile'i
kendin olusturabiliyorsun, buna bakalim. configuration as code

acilabilecek dosya sayisi sinirli (her bir connection bir
dosya acar), ulimit'le bunu arttirabilirsin

- sanal disk: bunu nasil yapiyoruz? sunucuda en iyi ihtimallde
  raid0 (2disk), normalde raid10 (6 disk), en iyi ihtimalle
  300gb disk takmak icin 6 tane takiyorum. sadece disk takmak
  icin ozellesmis kartlar var: NAS/SAN

#copilot_onerdi

vmware'de bir disk olusturuyoruz, bu disk bir dosya, bu
dosyayi bir baska makineye tasiyabiliriz. bu disklerin boyutu
sabit olabilir, dinamik olabilir. dinamik olani daha cok
tercih edilir, cunku sabit olani olustururken diskin tamamini
alir, dinamik olani ise ihtiyac duydugu kadar

### Kubernetes (k8s)

- kubernetes'te her sey bir nesne olarak kullaniliyor: CRD
  (custom resource definition), kendi veri modelini
  tanimlayabiliyorsun. k8s artik stabil; yeni gelecek api'yi
  onden soyluyorlar, drop olacak mumkunse vanilla k8s
  kullanmayin, isiniz k8s dagitimi hazirlamak degilse api'yi 2
  surum oncesinden soyluyorlar tanzu paas oluyor rancher,
  openshift gibi k8s dagitimlar var. okd openshift'in
  gerisinden geliyor. pek onerilmiyor. api surumlerini vs.
  test ederek paketliyorlar.
- konteyner vs pod: pratikte 1 pod'da 1 konteyner
  calistiriyoruz. Pod'u vm gibi dusun. podman yerelde de pod
  calistiriyor, konteyner calistirmiyor.
- KNative: normal bir servisten farki yok, hazirlanan bir
  uygulama dogrudan dogruya k8s api'lerini kullaniyor demek.
  leader election yontemini yapmak icin zookeeper yerine
  k8s'inkini kullaniyor. ya da CRD kullaniyor (tekton knative
  jenkins) sen bu CRD'yi yukle ben onu oradan yakalayacagim.
  Traefik (crd isimlendirme kuraliyla rule'lari yaziyoruz.) etcd
  dagitik veri tabani sagliyor, standar rest api'ler sagliyor.
  sen (bunlar alt yapidan gelecek diye daha az kod yaziyorsun)
- netflix zuul apigw projesi tum ortamlarda
- operator pod'a denk dusuyor: senin yazdigin bir uygulama
- rolling release ile yeni acilanlarin beklemesi
- sidecar container ornegi: startup script: for ile don (fe
icin mesela, be donmeden sen bekle) (birbirine bagli
sistemlerin birbirini kontrol edebilmesi) yeni surumlerde bu
geldi, baska ne olabilir "leader election'u bekle bakalim",
burada devaminda gelen servislerde cluster lock koyma lazim.
consul, zookeeper k8s kendi yapiyor (knative), son surumde
var. linux'ta initd'de bu var, systemd'de bu var.

#copilot_onerdi

bunun icinde bir api management var.

### Ileri takip edilecek konular

- configmap'e tanilmadigimiz env'lere ne gerek var, neden
- isletim sisteminin bilmesi lazim?
change data system
- deploymentset (bakalim)

### linkler

- api
[redhat api cozumu](https://www.apicur.io/)
[redhat api management](https://www.redhat.com/en/technologies/jboss-middleware/api-management)
[k8s patterns]

SDLC software development life cycle
