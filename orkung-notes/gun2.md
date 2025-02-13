# Mikroservisler ve Devops 2

- [Mikroservisler ve Devops 2](#mikroservisler-ve-devops-2)
  - [Sistem Mimarisinin Yazilim Uzerine Etkisi](#sistem-mimarisinin-yazilim-uzerine-etkisi)
  - [Mikroservis Tasarim Desenleri](#mikroservis-tasarim-desenleri)
    - [Ileri takip edilecek konular](#ileri-takip-edilecek-konular)

## Sistem Mimarisinin Yazilim Uzerine Etkisi

yazilimin ozelligi (uygulamanin), devopscu daha sonradan
mudahale ederek bunlar isaglayamaz.

- yuksek erisilebilirlik: cluster olusturmamizin 2 nedeni var:
  - HA: 7/24 fakat bunun bedeli var:
    - MW: middleware uygulamalarini coklamak gorece kolaydir. Active/Active
    - DB: dikine buyumek zorundadirlar: Active/Passive
      (otomatik olmasi icin farkli servisler var ama maliyet
      tabi ki, alternatifi manuel yapiyor.)
      pg icin patroni ile replikasyonlari garanti altina
      aliyor. patroni pgpool'dan daha iyi (best practice db
      makinalari uzerinde calistirmak)
      LB: de 2 tane olmasi lazim
- felaket kurtarma merkezi (disaster recovery): burada baska
  bir sehirde olmak gerektigi, kurallar biraz daha farkli
  otomatik olmazlar. manuel yapilir. Buradaki sistem canliyla
  birebir ayni olmak zorunda degil, sadece felaket aninda
  nelerin canlida tutulacagini onceden uygulama tarafinda
  belirleyip buna gore hareket edilir.

klasik veritabani disk uzerinde disk io yapmak isterler,
araya k8s'in controller'lari giriyor volume management
normalde ihtiyac duymadigin bir sey de; surekli backup
alman gerekiyor.

blok disk alanlariyla daha duzgun
disk seviyesinde replike edilmesinde garanti veriyor.
veri tabanlari makinanin diskinde durmazlar, nas'ta dururlar

aktif/aktif sistemde tum istekleri karsilar;
uygulamalari yaymak bu anlamda daha kolay
uygulamanin ayni anda 2 kopyasinin olmasinin getirdigi kurallari

lb round robin'i dagitiyor
browser app1'de session'u acti,sonra digerine dagitti,session
orda yok, yeniden session acti form doldurdu data'yi
session'da tutuyorsaniz o session'larin replike olmasi lazim
surekli veri paylasmanin bir yolu olmasi lazim: cache
yazilimlarinin distributed olmasi

http cache
session cache'lerini yonetimiyle ilgili bir yontem
verir: kafadan session replikasyonu yapma becerisiyle gelir.
node express de sagliyor.ana bu isi yapacak duzgun bir cache
sistemi soyle 2 app arasina oturacak hz'ye ihtiyac var
app cache:  uygulama icinde bazi seyleri hesapliyorum, sonra
tekrar yapmamak icin ram'de bir yerde tutuyorum. bir list
collection veya mapping yontemiyle data'yi koyuyor, x node'un
ram'ine koyuyor, diger node'un bundan haberi yok; mutlakada
duzgun bir distributecache yapisini tasarlamasi gerekiyor
bir api'yi kullandigin kutuphaneler tanimlar: spring cache
diye bir api var, o arkadaki confi'tan hangi implementasyonu
aliyor: in-memory mi hazelcast mi?
araclar hep var sizin implement etmeniz gerekmiyor.
db cache: jpa/hibernate second devel cache

phpciler memcache
javacilar infinity, hazelcast
webciler redis kullaniyor

eski sistemler icin pansuman tedbir network icinde lb'de
sticky session**

lb her zaman session yonetimi icin mutlaka bir coookie
gonderiyo, sonrasinda bu cookie'yi dolastir: sid random no.
tarayiciya gonderdi, sonrasinda lb bir liste tutuyor ve hep
ayni sunucuya gonderiyor.
ip bloklariyla da tutabilirsin x bloktan bu sunucuya, y
bloktan digerine

mesela; sabah basladi 100 kisinin 50sini a, 50sini b'ye verdi
1 saat sonra timeout'lar basladi, birinde 10, digerinde 40
kisi kaldi, yuk dengesiz dagildi.
mesela x sunucusu down olursa
sticy session yapsan bile veri tabani ve uygulama cache'iyle
ilgili sorun var
api cache'teki data yeniden hesaplanabilir bir sey ise bunu
kaybedebilirim. illa replike olmasi gerekmiyor, diger yontem
eviction (x'te sorun olursa y'yi de sil.)
memory grid (veritabani gibi kullaniyorsam, datanin
kaybolmamasi gerek)

gelistiricinin bunu ongormesi gerekiyor, cunku devopscu
bunlarin konfigurasyonlarini yapacak.

**http cache'i sor** : syn cookie kullanmiyoruz, o
networkculerin isi

her makinadan 2tane: sizin kayip siniriniz? 1 tane kaybolursa
ben onu otomatikmtan cover ederim, arkadaslar 2.yi kaldirir,
yok kaldiramazlarsa
db maliyetli bir lisans her seyi kaybedresem yedekten geri
donecegim diyorsun ama bu da saatlerce surebilir.

ikitelli'yi su basti
vodafone veri merkezinin tavani coktu

**DNS gecisi de otomatik olabiliyor. buna bak**

daha modern sistemlere ihtiyac var:
mongodb kumesi kurdugumda:

- primary - leader election: en az 2 replika, quorum: oylama
  yapiliyor. bazen birisi disk i/o yavas, buna gore configure
  ediyorsun. bunlar hep aralarinda konusuyor protokolun adi da
  gossip. artik aglarimiz da sanal ya; arada kopmalar olursa?
  3servis ayaktayken1 tanesi yalniz kalirsa, 2sinden birisini
  primary seciyor. mongo'da leader secemiyorsa veri kaybetme
  riski oldugundan  kendisini pasife ceker
  automatic failover: datayi replike edip
  disk'leri de raid yaptik 6 tane

  ne yapiyoruz: sharding: yataya buume, 300gb'lik diskler alip
  bunlardan yana dogru 3x300gb

mongoose ( bir cesit lb) siz req'leri ona
gonderiyrosunuz,dagitma isini o yapiyor
1 tb'in 300gb'ini a1, a2, a3'e dagitiyor, lazim oldugunda da
bunlari topluyor. surekli de bunlar arasinda bir iletisim var.
(abi bendeki data col oldu sana gonderiyorum,) sharding'in de
replikasini aliyorsun ki bir node'u duserse data kaybi olmasin

boyle bir kume kurmak istediginde en az 10 makina kurmak
gerekiyor.
1tb'in fiyatiyla 300gb'liklar arasinda cok ciddi fiyat farki var:
relational yataya buyuyemez bu net

mongo verinin transactional yapisini sizin saga gibi
yontemlerle sizin garanti altina almaniz gerekir, benzer durum
cache sistemleri icin de gecerli.

hz 2 tane - digerleri min 3 tane oluyor

veriyi 3e bolelim datanin replike olmasi
A     B     C
c1    a1    b1

dikkat edersen 1 tanesi kaybolsa da tum data garanti altinda
cache sistemlerinin yataya buyume ihtiyaci
chip'in uzerindeki ram sayisi fiziki sinirli: 64 islemcinin
siniri var, onun icin yataya buyume var
iostore'daki sorgulara hizli cevap veriyormus

bunlarin kendi ic network'leri olusturulur, udp konusur,
multicast, unicast, fiziki network kartlarini bile ayirirlar

veriyi diske yazmak istiyorsaniz redis degil couchbase oneririm

sharding konusuna biraz bak

multicast, unicast yapamiyorsam ben tcp'den konusurum
diyebilirler bazilari, bazilari tcp'den konusamayabilir.
farkli port'lardan konusurlar o port'larin acilmasi gerekiyor
(kendi ic konusmalarini yapacaklari port'lar)
farkli cluster'daki makinalar
bazen hz'ler cok yavasliyor: arastirdiklarinda 1 gelistirici yerelinde servis
aciyor, datayi lokaline replike ediyor
sirket icindeki network'le prod network'u ayirmak gerekiyor
k8s kendi kapali network'uyle geliyor.

- Cluster state durumu: izlenmesi gerekiyor, kume saglikli mi?
  bunu izlemezseniz node'larin durumundan haberiniz olmaz.
  cluster'larda izlenecek sey: uygulamanin duzgun calismasi
  kumenizin saglikli oldugu anlamina gelmez.
- Distributed cache kullanimi: buna degindik. (tek request'le
  claisan bir collection'a yazdiniz okudunuz, lokal makinada
  sorun cikarmaz ama sunucuda ayni sn.'de 10 thread seklinde
  calisiyor, thread safe kodlanmayan programlardaki hatalari
  tespit etmek cok zor olabilir. hatayi tespit etmek icin
  breakpoint koyuyorsun, sorun cikmiyor. thread safe uygulama
  gelistirmek sikintilidir, duzgun bir distributed cache
  kutuphanesi kullanmak gerekir, butun her sey implement
  edilmistir. multithreaded programlama yaziyorsan bu fonk.
  thread safe'tir, bu degildir gibi uyarilar dokumana
  konulmali, bir BE uygulama mutlaka multi-thread calisir,
  fe'de de artik bunlar yapiliyor) daha basta duzgun yazilmasi
  lazim. sirf bunun icin bile distributed cache
  kullanilabilir.
  ornek: 1 fonkisyonun var, collection'a bakiyor, yoksa
  ekliyorum, 2 req geldi, x ve y, ayni anda collection'a
  bakti, yok, ekledi, bu kod blogu thread safety ise bekle,
  ben yazayim sonra sen yazarsin; cache sistemleri butun
  bunlari yaparlar. onlari kullanin (global cluster log gibi bu
  mekanizmaya ihtiyac var) bunlari yazmaya calismayin, duzgun
  implement etmek zor, hazir yazilmisi var.
  (sizin yazdiginiz sistemlerin de leader election yapmasi
  gerekir, mesela yukaridaki global cluster log sisteminde
  sadece leader yapsin dersen rahatlarsin, bunun icin de senin
  kullandigin sistemdeki lider secimini cozmen gerekiyor,
  kafka zookeeper'i bunun icin kullaniyor. bastan yazma,
  yazilmislari kullan. urunu sadece k8s'te kullaniyorsan
  k8s'in lider secimi icin olan api'lerini kullanirsin,
  boylece if blogunda bir lidersen bundan sonrasini yap, else
  yapma secenegini koyabilirsin)
  cogu programlama icin bunu yapan bir kutuphane vardir.
  zookeeper'la mi, k8s'le mi yapmak istiyorsun, sec
  kutuphaneyi kullan. lider seciimi icin kullanilan araclar,
  service discovery bonusu da veriyor, cogu ayni zamanda "ben
  boyle bir servis ariyorum nerde o? Bir  esit servis)

  otomatik testler icin test container kutuphanesi var bu
  docker kullaniyor.

- veritabani degisiklik yonetimi: uygulamanin x surumunden y
surumune gecilirken veri tabanina su tablodaki y kolonuna
eklenecek vs gibi degisikliklerin surumle beraber yonetilmesi
gerekir, bunu surum yonetimi sistemine dahil etmiyorsaniz
adam gitti pod acti veritabanina ekleme yapti. 2 tane ozgur
yazilim urun var: flyway - liquibase her ikisi de bu konu icin
gelistirilmis. ruby'deki migration rutinleri.
tek tek dosyalari sql cmleleri yaziyorsun, naming
conventionlari flyway bunlari tektek uyguluyor,
uyguladiklarini bir tabloda  tutuyor, bunlari uyguladim diye
flyway uygularken dosya ismi uzerinden siraliyor, uygun bir
naming convention tutmuyorsan veya ekipte birileri
rollback yetenekleri yok (liquibase'de var ama yazman lazim)
liquibase'de benim kodum oracle pl/sql'ine hem de pgsql'ine
uygun dataset'i uretebilir.
liquibase'de hangi dosyanin ne zaman uygulanacagini kendin
sirali verebiliyorsun (birden fazla gelistiricinin)
liquibase'i java kutuphanesi var, uygulamayi ayaga kaldirirken
once bunlari uyguluyor. repo'da kodla beraber yasamasi
gerekiyor; release note'ta bunlari da belirtmen gerekiyor.
veri tabaninda hangi surumde ne degistigini takip ediyorum.
surum ciktiginda bir yeni surum cikiyorsun: duzeltme surumu
geri almiyorsun. surum politikasini nasil yazdiginizla
iliskili. geri donmektense sadece o hatayi duzeltip yeni surum
cikmayi tercih ederim.
sql dosyalarini manuel yurutuyorlar dba'ler tek tek bakmak istiyor
ne zamna yedek alinmali, oncesinde bir yedek alinabilecek bir
sitemle alinamacay sistem arasinda fark var. politikanin
belirlenmesi lazim.
ci/cd kadar onemli
bu patch'i uygularsan bir patch cikartiyor sana, veri tabani
degisiklik yonetimi: ddl'den bahsediyorum, kodunuzdaki sinif
modeliyle veri tabaninin diff'ini aliyor.
12tb. datayi bir yerden bir yere networkte transf etmek uzun
zaman
failover senaryosu: bunlari calistirdim
varolan tablonun data tipini degistirim, artik rollback
yazamiyorum ya da drop ettim, artik donemiyorum

deployment'la release cikartmak ayri seyler

- servicebus veya eventbus aslinda bir mq sunucusundan
  bahsediyordur. / messagequeue / distrubuted event bus/ event
  sourcing (bir kuyruk sistemi kullanarak event ureip
  dagitmaktan bahsediyoruz.) pub/sub goruyorsaniz bu asenkron
  yontemlerden bahsediliyor veya topic sunucusundan (kafka
  kuyruk desteklemez sadece topic destekler)
  hepsi urunun icinde implement ediyorsun: mesela activemq'da

yuk dengeleme soz konusu oldugunda once metriklere ihtiyacim
var. performans ihtiyaci nedir, onden alalim bunu da. ayni
anda gelebilecek max. request sayisi: sn.'de 10 mu 3000 mi?
response suresi: 1ms mi 1sn mi dakikalar surebilir mi?
bu bilgiyi zor da olsa alalim. nereden alinabilir?
peek time'lari bulmamiz lazim ki o zamanki yuku kaldirmak
lazim. bizim buna hazirlikli olmamiz lazim
benim burada yuku kaldirma derdim oldugundan sticky session
yapabilirim manuel. login olurken yuku dengeli dagitirim.
burada HA derdim yok
yukun nereye geldigi? yuk testi yaparak analiz etmek gerek.
monolith'ler x module de gelse biz burada coklarken tamamini
coklamak durumundayiz, mikroservislerde 1 servisi coklamam
yeterli (manuel)

- lb darbeyi yedi, test ettik kaldiracak hale getirdik.
- uygulama sunuculari aldi, kaldiracak hale getirdik.
- db'ler aldi, patladik

apache jmeter http client olarak calisiyor
paralel calisitabliiyorsun

benim yuku karsilayabilecek duzeyde donanima ihtiyacim var

mq sunuculari http degil tcp protokoluyle konusabilmek icin,
ingress buna izin vermiyor, o zaman worker node'un ip'sine
routing yazilacak, ben req'i ona atacagim, o da pod'a
yonlendirecek.

dns over http: dns'in portlarini kullanmiyorsun http uzerinden
gidiyor dns sorgusu yapiyor

once izle, sonra iyilestirme yaptim tekrar izleyelim
yuk testlerini kafaniza gore yapamazsiniz, planlamaniz lazim,
canli sistemleri calisamaz hale getireceksiniz, butun ilgili
taraflarin bu testin bir parcasi olmasi lazim. cunku burada
(sunu bir degistireyim hizlica bakalim), mesela networkcu o
arada bir seyi degistiriyor, veri tabanindaki arkadas
index'lemeyi duzeltilebilir, bu sebepten orada olmalilar.

testi sistem patlayana kadar yuk bindirmeye devam ediyorsun,
onun sinirlarini da ogrenemk icin. ileride bunu ne zaman
buyultebilecegimizi ogrenelim: 6 ay sonraki beklentiniz nedir?
cunku yazilimda surekli yeni isterlerle kullanici kitlesi
arttirilir. x yeni modulu gelince 5 kat artacak. bu karara
gore donanimi da planlamamiz lazim

genelde fiziki teknik sinirlara gelmeden once ekonomik
sinirlara geliyoruz.

kurumun verecegi karar: exadata'ya 1m $ vermek mi, musteri
hedef kitlemi degistiriyorum, yoksa yazilimimi mimari olarak
degistireyim:

bit vise operator'lerle 30 byte'lik datayi 1 byte'a
sikistirmayi bilirim

cogu programin icerisinde memory leak
tarayicida da garbage collector var: ram kullanma sinirina
gelir, ram'i temizler, sonra tekrar kullanir bu egri
monitoring aracinda grafigi surekli tepede gezinirse memory
leadk vardir. sorun nedir?
cok sik yapilan hata: jvm icin dogru yontem degil: 1 tomcat'e
1 wildfly'a 5-10 tane uygulama aciyor, jvm'in kaynak dagitiyor
size gelen metrikler jvm basina gelecek ve hangi uygulama
ne kadar tukettigini goremezsin

uygulamanin class'larinin memory'de durmasi gerekiyor,
undeploy ettiklerimiz de memory'de durmaya devam ediyor.
meta space size'imiza bakalim.
bunlarin tespiti icin monitoring yapilmasi gerekiyor

yazilimin okunakli ve surdurulebilir olmasi, onun 1 gb fazla
ram tuketmesinden daha onemli, cunku o 1 gb'in maliyeti
apex-opex ayrimi genelde bakim maliyeti satinalma maliyetinden
kat kat fazladir.

k8s'in bonusu
k8s'te service tanimla: buraya gelen servisilerin yukunu
izleyiver, x sinirini gecerse yeni bir node kaldir.

farkli farkli sistemler arkli peek zamanlarinda yuk aliyorlar,
birisi saat 5'te, digeri sabah saat 3'te, kaynak paylastirarak
auto scaling'le bosta yatmasini engelleyebiliriz. butun
bunlari manuel yapmak

hibrid cloud cozumu k8s'in otomatik olmaz, mesela rancher,
openshift bunlari yapabiliyor: otomatik req karsisinda size
kaynak verecek bir servisin olmasi lazim. amazon'la
anlasiliyor, donanim sinirina gelince oraya cikiyorlar, sonra
donanim temin edip tekrar cikiyorlar.

os calismaya devam edebilmek icin OOMKiller ile en cok ram
kullanan uygulamayi oldurur. genelde de jvm kullanir, mesela
tomcat'in uzerine apache koydun, statikleri oradan almak icin,
apache fork yapisinda calistigi icin kucuk boyutlu binlerce
child olusturur, ram doldugunda jvm'i oldurur.

production'da swap kullanilmamali. ama biz swap'leri acik
tutariz. bi sorun cikti iceri girip bi sey yapmam lazim, en
azindan

spring batch kutuphanesi yerine apache camel var, bunu
kullanabilirsin, dosyada filan da tutmaz.

**veri tabani degisiklik yonetimi politikasini arastiramaya devam et**

ic SLA: surum politikasi belgesi, kurumdaki ust yoneticilerden
onaylanmasi gerekir.

per service per db: oracle'da farkli bir schema, postgre'de
farkli bir instance olabilir. ayni schema'daki tablolara 2
farkli servisten gidemez.

cd sureci icindeki dml'e en basta dba'in bakmasi lazim.

ddl tablo desenleri de yaziliminizin bir parcasi: liquibase
relational'larla calisiyor, mongo icin de bir destekleri var,
mongo yari schemal diye gecer, collection'a bi dokuman
koydugunuzda semasinin suna uyup uymamasini denetleyebilirsin,
birbirinden farkli dokuman semalarini biraraya toplayabilirsin
ya da bunlari yapmayabilirsin,
key-value: dogal olarak semasizlar.
hiyerarsik verittabanlari: ldap, tablo deseni gibi olmuyor ama
onden bir sema tanimlaman gerekir.
protobuff, avro protokolleri icin de bir schema tanimliyorsun:
birinci field'in adi bu, data tipi bu: bunun icin de bir degisiklik
yonetimine ihtiyac var

buradaki hile, hicbir attirubute'u silmezsen eskiden onu
kullanan kullanmaya devam ediyor ama bu bir hile, bir sure
sonra sisiyor

rest yazarken degisiklik yonetimi icin kullanilan yontemler
var: url'in uzeirnde surum no /v3 gibi gidip gelen datanin
icinde olabilir, http header'ina koyulabilir, hangisi uygunsa
kullan.

- asenkron calisma: http senkron olduugndan, response'u ben
aldim deyip
tamam benim isim bitti, karsilikli endpoint'ler tnaimlaniyor.
su islem bitince bi post vs ativer: web hook ile
bunlardan da eskisi, messaging queue

: istemc: producer, kuyruga mesaji
atiyor, karsiliginda consumer; tuketici var
mq sistemlerinin gorevi kuyruk sunucusunu yonetmek, imp'ten
imp'e farkeder, kendi kodunda yaparsin:fifo
inmemory yapabilirsin

kuyruklama su acidan da farkediyor 1000 kisiye e-posta'da
atacagim ama onlara x anda atmamin bir manasi yok, burada cpu
kaynagi musaitken atayim dersen burada da ise yarar

ben aldigimi baska bir kuyruga cevap gonderebilirim, tipki
eposta'da oldugu gibi, sana attim ben kendi zamaminda ,kendi zamaninda veri kaybetmek istemiyorsan diske
process becerileri var
queue disinda pub/sub var, mesaji urettim n tane farkli
consumer kullanabilsin istiyorsam topic uretmem lazim:
publicatin (producer), n tane tuketici ()
distributed event diye geciyor: siparis geldi, ben siparisi
kaydettim, farkli servisler bu event'i alip isini yapiyor, bu
event'i
fire and forget diye de geciyor, cevap beklemiyorsun, kim
alirsa alsin loose couple servisler tanimlayabiliyorsun

ben bir esaj urettigimde bu mesaji biriinin belli
bir zamanda tuketmesii  gerekiyor mu, kimse tuketmezse baska
bir is yapmam gerekiyor mu

*event based*
actor tanimlama

EIP entegration bunlarin hepsi camel'da var
python icin de var

burada camel'la yazacaginiz kodu, kendi basina bir servis
olarak da implement edebilirsiniz, bi k8s operatuyle
calistirabilirsin

asenkron yapinin farkli yerlerde farkli kullanim amaclari var:
event sourcing: servislerin bi olayi publish etmesi, 3rd party
servislerin bunlara baglanip bir sey yapmasi
audit log'larda kullanilabilir, bundan nasil bir audit logi
sterler diye kafa yormadan uygulamadaki butun olaylarla ilgili
event uretirim, o event servisi nasil nereye log'layaracaksa
log'layabilir.
web hook mekanizmasini event sourncing'le yapabilirisn
siralama, zaman ayarlam conf'larla yapabiliyor
throotling: bir anda glen request'leri belli sirayla alma,
bunu ops yapiyor.

servisler arasinda islem yapabilmek icin webhook'tan deha
izleme de yapabiliyorum

butun bunlar entegrasyon patternlerinde var: consumer -
producer
camel gmail'a balanip maili okuyor, illa ftp yapmiyor, bunlar
entegrasyon, hazir component sistemi var butun bunlar
kuyruklama meanizmasindan geciyor. bu olmadan yapamazsiniz.
pool: dakkata 1 kalk, webservisi cagrisi yap, isle suraya yaz.
implementasyonda

mq sunucular kendi api'leriyle gelirler, http gibi bir
protokolleri vra, tercihiniz bunlardan birisi olsun: amqp,
mqtt
daha kucuk binary message protokol,
stomp bunlar acik standart javascript protokolu
bunlari kullanirsan bu protokollerinden birisini kulanin
kodunuzu baska bir ortama
duz bir mq sunucunun yapabliecegi seyi kafka'ya yaptiriyorlar
dokuman veri tabani
veri tabani olarak sakliyip connect olan consumer'in bir
pointer'i var en son hangisiyse onu gosteiryor, okuma rutini
client'in isi (consumer) kafka datayi da replike edip ha
olarak tutmaya calisiyor
event sourcing kafka'yla yapilir gibi bir algi var.

genelde urun secerken suna dikkat et: arkadaki urunun
degistirilmesi: ben amqp kullaniyorum, bunu destekleyen
herhangi bir mq sunucu benim isimi gorur.

benim butun stack'im java ise bunun icine rabbit mq sokmam.
yavas olmasina ragmen activemq kullanirim, elimdeki know-how'u
kullanirim, rabbtmq'yu nasil monitor edicem, jvm'in icini
izliyordum active mq ok, log formati java'ya gore hazir sablon
benim butun stack'im java ise bunun icine rabbit mq sokmam.
yavas olmasina ragmen activemq kullanirim, elimdeki know-how'u
kullanirim, rabbtmq'yu nasil monitor edicem, jvm'in icini
izliyordum active mq ok, log formati java'ya gore hazir sablon
var ok. buradaki performans farki

java sistemlerinde redis kullanilmamali net, jvm icinde
entegre oldugundan calisma performanslari hz icin cok daha
uygun.

**Event Driven Architecture**
event sourcing
webhook

buyuk sirketler seni kendisine bagimli kiliyor.

dotnetciler session saklamak icin redis
session yonetmek icin altyapi yok, java'da var: application
server jakarta ee icinde var. varolani kullanin
disaridaki bir servise mem'deki nesne modelini gondermek
istediginizde serialize / marshalling edeceksiniz, o nesnemi
diskte saklanabilecek bir hale getirmem ve tersini yapip
okuyabilmem gerekir, redis'e mesela json/xml vs yapip
gonderecegim, ve tersi. fakat jvm'de kosan bir sey
kullaniyorsam java nesnesin binary hale getirmem yeterli. bu
onemli. ama ozel olarak sikinti operasyonel maliyeti (erlan'in
log'larinin cozulmesi) en iyi ihtimalle syslog'a bakmaniz
gerekir

bir job sistemini mq'da kullanabilirsin, producer'in
zamanlayici olur cosnsumer'in bir job olarak onu tuketir.

queue'ya ilk giren mesaj ilk cikar (fifo)

webhook'u calistirabilmeniz icin o servisin ayakta ve oinliyor
olmasi lazim, burada ise butun consumer'lar kapaliyken bile
queue'da tutuyoruz, mesala gun icinde cpu'yu tuketmemek icin
kapattik, cpu'lar baksa bir is yapiyor, o is bittikten sonra
ayaga kaldiralim.

webhook'lar 3-5 istekten sonra kendisini kapatiyor bu da baska
bir pattern
batch icin camel oneriyor (zamanlanmis gorevler)
camel pipeline'i yaptim, quartz producer'u var. cronjob
notasyonuyla timer veriyorsun calisiyor

jenkins'in butun job'lari icin cron yazabiliyorsun.

programala dilinin timer kutuphanesi var
k8s'in job diye bir deployment type'i var, tanimlayabiliyorsun

jta: java transaction api jta active mq destekler ama zaman
performans maliyeti korkunc

topic implementasyonu icin n tane queue tanimlamak, eip
tarafinda tanimlanmis durumunda bu pattern'ler

genel olarak tasarim desenlerinin hepsi icin soyluyoruz.
mikroservis tasarliyorken iclerinde bu maddeyi mutlaka
bulundurun, implementasyon sirasinda olacak ve dagitik event
mekanizmasina ihtiyac duyuyorsunuz.

bazi durumlarda ayni transaction blogu icerisinde davranmasini
istiyorsuuz, ama bunu yapiyorsan transaction'u kullanmamani
tavsiye ederiz, cunku event queue'da cevap bekledigimi
tanimliyorum long-running process de deniyor buna ya da bir
timeout mekanizmasi giriyor ve onun hata aldigini varsayarak
ona gore davran

## Mikroservis Tasarim Desenleri

bir iki yazilim tasarim desenlerini kullanmak gerekir
motto olan: dry (don't repeat yourself), bir kodu bir yere
kopyala yapistir yapma, onu bir fonksiyon, bir sinif, bir
kutuphane neyse o sekilde tanimlayiniz. size hatayi
bildirdikleri yerde duzeltirsin ama asil koydugun yerde hala duruyordur.
bir semptom durumu: "biz yazmadik" nothing write gibi bir sey
multi-thread programlama zordur, cahil cesaretinde
bulunmayiniz, apache camel'in kodunu yamamaya calisan stajyer?

java'da fonksiyonel program ozellikleri geldi, single lining
"yapmayin"
reactiv programlama: ihtiyaciniz yoksa kullanmaya klakmayin,
fonksiyonelin bir uzantisi olarak asenkron calisir, yazim
teknigi bambaska, trend yonetimi bambaska, cok
perfromanslidir, ama sizin buna ihtiyaciniz var mi?

MVC: veri modellerimiz, bunlari sunum katmani ve bunlarin
kontrolunu yapan temel is kurallari birbirindne ayri yazilir,
programlama dilinin paket sistemlerini ayirmak gerekir
model verileriniz tekrar tekrar kullanilacak, bunlardaki
degisimler sizin api kirip kirmadiginiz belirleyecek
view katmani: birden fazla olabilir; sunum katmani. bir json
olabilir, bir xml olarilir, mq'ya mesaj atan bir sey olabilir,
soap zarfi olabilir, 1 servisim 5 farkli sunum servisi
olabilir, bunlarin is kurallarini kontro leden controller
katmaniniz orada olmali, farkli view'ler ayni model yapisini
kullanarak ayni controller'la gelirler
bu tasarim desenini recursive bicimde, ayni deseni tekrar
tekrar gormeniz lazim, urettiginiz html icin de gecerli html
modeldir, css view, controller javascript
model: persistent veri (db), ayrilmalidir
bu deseni hep gormeli, bunlari ayirmazsan spagettiye doner,
ozellik degistirmek cok zor, yapilan degisikliklerin nereleri
etkiledeginiz gormek cok zor
yataya dogru da moderizasyon yapmalisin, ayri paket
sistemindeki ayri paketer olmalirlar, siparis moduluyle stok
modulu ayni jarin icinde olmamali, sonra ben derleme sirasinda
bunlari makroservis mi mikroservis mi yapacagima karar
verebileyim
mvcc- mvt gibi farkli versiyonlari var ama kavram ayni,
recursive gorunmeli, sunumun modeli dto/vo diyorlar veri
tabanina kaydettigimiz veri olmayacak,
view katmanim html'di rest istediler yanina, view katmaniyla
business katmaninin yanina
view controller sadece ui'i derliyor, bunun rest'te bi
karsiligi yok, uzerine bir de is kurali kontrolunu "bunun ici
ne olsun" diye yazdiysaniz
bagimlilik yonetimi acisindan: view bagimliliklari sadece
controller, controller sadece bir baska controller'a bagimli
olur: bagimlilik agaci
duzen derleme sistemlerinde architecture'u kontrol eden
unit'ler, bir famework
bak bu isimli package bunun icinde olabilir, bu bundan
bagimlilik alamaz.
model baska yerlerde de kullanilmak icin ayri sekilde
paketlenmesi lazim, ayri durmasini tavsiye ederim. paket
sistemini ayirmasaniz bile, butun dillerde elinzde bir isim
uzayi var, bunlari mutlaka ayirmalisiniz: model isimleri boyle
verilir ve boyle bir paket icerisinde bulunur, bu kurallara
gore yapilmali ki, herhangi bir seyi ararken bulabileyim

kullanip kullanmamak yazacaginiz uygulamaya bagli, klasik
veritaban iuygulamasi yaziyorsaniz
DDD: domain driven design : busineess domain: bir is kural
butunu: muhasebe-stok yonetimi-siparis takibi bunlarin her
birisi boyle, implementasyonda bunlar daha alta kirilir,
bunlarin denkligi: her bir domain bir dokuman turune denk
gelir, relationlal'da birden fazla tabloya dnek gelir ama
birbirleriyle  tutarli
bir domain diger domain'le sadece onun deklare ettiggi api'ler
uzernden islem yapar ve bu bir dil olarak tanimlanmalidir
(sepete ekle, sepetten cikart gibi)
domain driven design entity'in internal stete'leri tutacak
baska bir tabloya ihtiyacim var
validasyonla ilgili bir is kurali midir degil midir
"hatanin neresinden donulurse kardir" ilk valid ui'da, 2.
view'da
3une de ayni validasyon kurallarini tasiyabilmek icin ayri bir
yerde yapiyorum.

ilk validasyon, ui'da olan seyi js'in kontrol etmensinin bir
bana gelen datayi vaska bir veri tipine cevirecegim icin,
sonra bu veriyi business katmanina aldigimda yine kontrol
ediyorum.
restapi kullaniyorum o bunu yapiyor.
buna ek olarak su 2 dizayni birlestirip kullaniyorum. cqrs ile
command driven design'i
onun yerine command'larim olacak, boylece bir abstraction
sagliyorum., herhangi bir yerden gelirse gelsin.
bu da bir tasarim deseni (langfor, modelinizle executeriniz
ayni pakette)
command'larim quque'dan akiyorlar, bir baska servise de
command'i gonderebilirim.

user domainim var view katmaninda 1 tane endpoint'e ihtiyacim
var, 1 tane modele ihtiyacimvvar user, bunu tasiyacak bir user
dto'ya ihtiyacim var. view katmanlarinda user summary - vs
isimlendirmis durumdayim
scalffolding aracima diyorum ki template'ten kodlari uretiyor,
cok olmakla beraber
commandlari uret: create, update, delete
dto'dan entitiy'e ve tersine
butun bunlari scaffolding araci uretti
pratikte yapacagim model sinifinin attribute listesi
uzeirne de validasyon kurallarini tanimliyorum

V Modul1 modul2
C
M

- saga: transaction yonetimini saga patterniyle implement
  ettim, servisler arasinda dagitik transaction'u bununla
  yaptigim icin relational'e gerek yok
- database per service (bunu implement etmenn operasyonel
  maliyeti yuksek. foreigin key'la baglarim.) iliskileri nasil
  tnimliycam, veri tabaninda tamamlayamam, dolayisiyla bunu
  middleware'de yapmam lazim, cari serviste deggisiklik
  yaptigimda, x servisinin bundan etkilenmeyecegini kim
  garanti edebilir? bu yuzdn db per service, yapmazsaniz
  tightly couple olursunuz, biri degisti mi digeri de (ayri
  servisler yaptik cunku k8s'den faydalanmak icin yaptik) ama
  aslinda her seyi birlikte derleyip birlikte release
  cikiyoruz. bir eri tabani degisiklik yonetimi politikasiyla
  birlikte yapmalisin,peki ben 2 tabloya bakiyorsam, ben niye
  iliskisel veri tabani kullaniyorum? api uzeirnden iliski
  kuracaksam transaction'u per service'te nasil yaptim

- circuit breaker: siparisi icin hazirladigim be for fe,
  cariye erisemedi. standart http timeout'u ayarlamazsan 2
  dk.. kullanici bu kadar beklerken 3 kere daha basiyor, bu
  sekilde bizim istegimiz 6dk. bekledim, timeout'um geldi,
  artik servisimin orada olmadigini ognredim, artik isteklerin
  atilmamasi gerekiyor, devre kesici, artik her istekte kafadan hatayi
  dondurmeye basliyorum, bu desen arka tarafta cari servisini
  dener, acilinca da canliya alir, bunlar multi-thread
  calisirlar. bazen de servise giderim cevap alamasam da bir
  daha denerim belki gelir (retry pattern), hata aldim 10sn,
  hata aldim 20sn, belki yeniden baslayan bir pod acilmistir.
  k8s'in servisin api'siyle benim istek yaptigim api farkli.
  bunlar elle implement etmiyorsunuz, illa ki kutuphanesi
  vardir, yoksa da thread safe ve tracing api'leriyle
  ayarlanmasi gerekiyor, bu uygulamanin bir parcasi.
- cdc (change data capture): databaes per service
  kullaniyorsak bunu da kullanmamiz lazim, verileri keyfi
  saklamiyoruz, bunlari bilgiye donusturmek icin, raporlar
  almamiz lazim BI (business intelligence) yapiyor, karar
  destek sistemleri (bizim sistemlerimiz) / oeltp (online
  transaction processing) birbirine ters dusen sistemlerdir.
  birebir bagli referencial
  query performansini hizlandirmak icin
  bir baska sisteme o da kendi veritabaninda rapor almak icin
  kendi vtabanina koydu. arac ailelerinden bir tanesi pentao
  raporlari business ekibinden birilerinin hazirlamasini isteriz
  olap sistemler: excel gibi sum vs almak istiyorlarsa, o
  verilerin bir araya getirilmesi gerekiyor.
  ornegin powerbi'yi aldiginda o kendi veri tabanina cekiyor
  etl yapiyor. sonrasinda
- event sourcing
- backend for frontend
- api gateway: akis basit kural seti basit, reaktif prog.
  kullanabilirim, ama bu urunu yazan arkadas daha senior ve bu
  cesarete kavusmus kisilerdendir, ortalama bir programci
  bulasmamali. spring apigateway projesini kullanin, ama
  spring mvc (classic olani ) yazdi cunku kimse java
  dunyaisnda ornegin kullanamadi akka (reactor engine) isletim
  sistemi thread, klasik os thread'i degil, bubble tarzi
  nedir? arkada 5 servisim var: fe yazan arkadasa her birini
  ayri ayri tanimlamak yeirne 1 tane servis hazirliyorum be
  for fe'ye yakin bir tavir, sen buraya at req'ini ben arka
  tarafta hangi servise gidecekse ona redirect edicem gelen
  cevabi vericem, circuit breaker'i implement etmem lazim ki
  api gw beklemesin, bunlari uygulamaniz desteklemiyorsa onu
  operasyonla yapmaniz zor. api gw'ler baska ne yapar?
  bankayim ve disariya bir api aciyorum, buna kac request
  atabilirsin, yaptiginiz sorgu basina token dusuyorum, bunlar
  da apigw uzerinde implement edilir, farkli servislerin
  api'lerini biraraya toplayip yetkilendirme, throttling,
  billing controlu yapan sistemler, genelde api uzerinden para
  kazanan herkes bunu yapmak zorunda, guvenlik, authz autch'de
  yapiyorsun, butun gw'ler icin gecerli olan kural seti var,
  dinamik yazdigimiz, operasyon yonetebiiyor, a/b testi yapmak
  istiyorum.  bu sekilde frontend 1 yer biliyor, oraya
  geliyor. servis sayisiniz biraz artiyorsa kullanmak iyidir.
- sidecar:
- service discovery: hizlica: dns gibi dusun: apigw arkadaki
  servie gidcek, hangi podda oldugu bir
  registry'denbahsediyoruz: zookeeper, consul service
  discovery de yaparlar. k8s de yapar. dagitik bir service
  kullaniyorsaniz k8s'inki olabilir. ondan bir service
  spring'in icine bind eden client kutuphanesi var.
  service ayaga kalkarken service discovery'e gidiyor ben
  geldim diyor. artik dns'lerimiz de programatik, k8s icinde
  kosan ingress bunu otomatik yapar, artik gercek veri tabani
  kullanmaya basladilar. bu mekanizma olmadan loosely copule
  olma sansi olmaz, tek tek configure etmeniz gerekir,
  ozellikle ip adresleri uzerinden binding yapmayin. k8s'te
  statik ip vermek mumkun ama bu sefer normal kurallari
  isletmek sikintili (stateful deployment), kodu yazan
  arkadas boyle yazmis
- cqrs (command query responsibility segregation): API'de
  komutlarinizla (crud islemleri) sorgularinizi ayrica
  tanimlayin birbirinden ayirin, get'le post'la query'leri,
  hatta bunlari 2 farkli servis yapin, query cektigiizle crud
  yaptiklarinizi ayirin. bu durumda graphql elde eddiyorsun,
  tanimlari kolaylastiriyor senin icin. kodun icinde bu ayrimi
  net olarak yapmanizi tavsiye ederim. query'ler read-only ve
  cacheable, command'lar change data capture icin veri uretir,
  cache'leri invalide eder, asenkron, remote calisir, cache
  yonetimi vs. yuku daha duzgun dengelemek mumkun hale gelir.

eger 1 aydan kisa bir is yapacaksan 1 classin icine yaz gec
ancak; kurumsal uygulamanin surduruilebilirligi icin ayni yontemi
kullanmak daha verimli olacaktir.

bahsettiklerimiz ust duzey pattern'ler var
kodun icinde daha alt pattern'ler var
jakarta ee'de uyulmasi gereken pattern'ler var
kotu egilim: tasarim desenlerini okudum, her yere onu
uygulamaya basliyorsun, bu kotu bir egilim, bu bir sablon,
birebir kitapta ne yaziyoru degil

singleton pattern'i en basit olanidir. ama gercekten
yazamazsin garbage collector class loader'i kancalamaniz
gerekiyor, ama buna yakin bir sey yazmak yetiyor.

douglas adams'tan gorunmezlik kalkani

frontend'imin bazi ihtiyaclari var, bunlarin ufaktan merge
olmasi lazim. peki yuku nereye bindiricem, browser veya mobil
uygulama, acaba bunlari yazan arkadaslarin boyle bir
capabiities'i var mi? sadece o fe'nin istedigi sekilde
cevapnbir be servis yazilr, fe'nin istedigi sekilde cache'ler.
bunu yetki mekanizmalarini kontrol etmek icin de
kullaniyorlar.
siparis servisini cevaplayan bir be yaziyorsun, bu arkadas
cache'liyor ve hazir sekilde veriyor. fe for be proxy pattern

genelde son kullanicinin istedigi seyleri anlik cevaplamaniz
gerekir. isteginiz alindi, cunku psikolojik mevzuu.

circuit breaker: servisten servise a'dan b'ye dogrudan
giderken yapmak gerekir. circuti

service mesh'le yaparim bunlar tasarimin parcasidegil daha
sonradan gunu kurtarmak icin. onun calistigi kodun icine 1 sey
daha koyuyourm configurasyonla, sen o pod'a bunu da ekle. ilk
tasarim da bunu kurgulamayin, pansuman tedbir olarak.

gelen nework req'lerini
uygulamada circuit braeker yok, sidcar once o aliyor proxy
olarak, sonra asil servie gonderiyor, yapamiyorsa circuit
breaker kesiyor, ozellikle network araya girmeleri.

k8s'in yeni surmlerinde traefik'i implement eder, artik
tanimlayabiliyoruz
eskiden sidecar olarak inject ediyorduk.

### Ileri takip edilecek konular

- traefik'e bak
