# Mikroservisler ile Devops Notları

*Hocamız Hakan UYGUN ; Mikroservisler ile Devops Notları ; Mustafa Akgül Özgür Yazılım 2025 Kış Kampı Eskişehir ; Meryem GELEN*

## 10.02.2025 Pazartesi

- neden rancher değil de openshift? Satın alınmış ben de kullanacağım tamam haklı bir gerekçe
- Uygulama yapmayacağız Temel Mimarı Anlatacağım
- Gerçekten mikroservis kullanmalı mısınız?

### Özgür Yazılım Nedir?
- Sahipsiz Lisanssız değildir Açık Kaynak kodludur. İnceleyebilir okuyabilir değiştirebilir dağıtabilirsiniz.
- genel kamu lisansı ile değişiklik yaptıysanız siz de o lisans ile vermelisiniz.
- public lisans gibi lisanslarda mozilla public lisansta referans vermen lazım ibaresi vardır. 
- creative commons lisansları var CC BY Atıf (Attribution) iconlar fontlar fotoğraflar bazı js kütüphaneleri
- bunlar içerik lisansları by cc bunların ne olduğuna bakmalı lisans ihlali yapmamalısınız.
- benim adımı geçirmelisiniz tarzında noncomercial ibaresi varsa ücretli bişey yapacaksanız onu kullanamaz satamazsınız
- kod geliştirirken internetten bulduğunuz bişeyi pata küte kodunuza gömemezsiniz arkadaşlar
- o lisansa telife uymalısınız
- copilot tarzı kod üretme yapay zeka papağan kodu nereden getirdiğine dair geliştiricileri de bilmiyor bu sebeple davalar başladı
- size getirdiği kod gpl kodu olabilir.
- turktelekom önceden ses çevirmede ffl li bir kod kullanmış , Federal Firearms License, gelitiricileri yakalandılar ve gpli sunmak zorunda kaldılar içinde parola varmış
- borland delphi flash geliştiricilere yazık oldu bir gecede aldıkları karar çok kişiyi etkiledi
- microsoft 3 yıl boyunca uğraştılar typescript ile devam et, M'yi boşver dediler ve bir günde M ekibi dağıtıldı 3 yıl boyunca olan emek boşa gitti. 
- yani diyeceğim o ki yöneleceğiniz alanı ona göre seçin.
- google her yıl ürün öldürür 3 yıla anguların ipi çekilebilir. react'ın arkasında da facebook var. 
- tecrübe yenilen kazıkların bileşkesi sonuçta.
- GPL general public lisansı 
- suriyeliler kullanamaz maddesini araştır. lisansın sonundaki cümle 

### Mikroservisler nedir, gerçekten kullanmak gerekli mi
modüler yapılar kendi başına çalışabilen atomik servisler hazırlanmasıdır tam tanımı da yok.

### Servis nedir?
- SoA
- RPC/RMI/IPC
- Web Service
- SOAP
- Rest

Yaklaşık on yılda bir atomu yeniden keşfediyoruz.
- ilk yazılan servisler main frame.lerdi ve birbiri ile konuşması için rpc protokolleri vardı
- binary network tanımlanmıştı (IIOP)
- Java ilk akıllı ev tost makinesi buzdolabı çalıştırmak için geliştirildi.
- x86 ve arm serisi CPU mimarisi kaldı şimdilerde o yıllarda onlarca vardı
- MAC den windowsa veri transferi edemiyorduk çünkü disk yapısı vardı
- RMI remote method inlocation diye bir şey geliştirdi Sun Microsystems javada sonra web servis çılgınlığı başladı
- binary protocoller ile iş yapmak zordu http üzerinden kolay oldu render etmek yerine herkes servis apisini yazıp veri transfer etmek istedi.

bu da sıkıntı yarattı.
- a firması a api seti b firması b api seti yazınca buna simple object protokol standandartı getirildi SOAP
- Servis Orianted Architecture diye bir isim getirildi SoA ürünleri var becerileri SOAP web servis yazıyorsunuz SoA ürünü bu web servisi ayakta tutmak vs için çeşitli araç gereçler sağlıyor

Ya biz niye bu kadar XML ile uğraşıyoruz ki httpnin get put post komutları var zaten CRUD için buunu kullanalım json formatında iletelim dedi bir yazılımcı
- XML adı kötüye çıktı çünkü kullandıkları SoA ürünü kötü XML'in adı kötüye çıktı
- XML sayesinde html var bir araba şeyi bu sayede yapabiliyoruz XML kötü değil ama adı kötüye çıkınca rest geldi
- url get istedği atıyorum json veriyorum siliyorum güncelliyorum pat pat kolayca amaaa sonra json sürümünü nasıl takip edicem
- validation nasıl yapacağım
- json schema geldi bir başka dosya formatı, xml olma yolunda koşuyor
- jsonschema org olması lazım orda standart tanımlıyorlar
- yaml yet another markup language geldi kendini json subseti olarak tanımlamaya başladı 
- grpc  (Google Remote Procedure Call) rpci  ben bu kadar çok serviste network trafiği oluşuyor bende,  ben bunları json dolaştırmam zor binary yapayım dedi
- apache da benzeri apache avro tanımladı, Avro, Apache'nin Hadoop projesi kapsamında geliştirilen satır odaklı bir uzaktan prosedür çağrısı ve veri serileştirme çerçevesidir. 
- arada action commanda göndermek gerekiyor hepsini json yapmak yetmiyor
- remote procedure call ile daha kolay oldu ? RPC

büyük şirketlerin neyi neden kullandığı sizin için önemli olmamalı
- redis facebook günlük request sayısı sizinki ile karşılaştırılabilir mi
- mongo küçük verilerle çalışmak için arayanlar oldu.
- 1PB 2 PB petabyte verimiz var dediler, 1 mi 2 mi arada dağlar 1024 TB kadar fark var
- ama mimariyi seçerken böyle seçilmez
- kalite maliyet zaman üçgenindeki iki şeyi optimize edebilirsiniz
- maliyeti az olsun zaman az olsun derseniz kaliteden ödün vermelisiniz
- hem maliyeti az olsun hem kaliteli olsun derseniz uzun zaman alacak
- iyi analiz edip neye ihtiyacınız olduğuna karar vermelisiniz
- makinenin skasi kartının pili bitmiş, SCSI(Small Computer System Interface),durmuş,  dışarıdan istek almayı kesmiş, veri almayı durdurmuş, veri kaybı yok backupdan dönüldü, ve cash yapamıyor makine veri tabanı sistemi durmuş terminal açılmıyor, 8 saat sürdü çok dediler,
- cpu sökmüşler felaket kurtarmadan lisans maliyeti düşürmek için database'in ne olduğunu anladık, maliyeti çok gelince haa o zaman 24 saat sürebilir dediler.

### IDE ve Diller
- intelliJ kotlin
- redhat ceylonn diye bir dil ceylon apache'e bağışladı orjinal geliştiricisi dahi bıraktı 
- ilgi community desteği çekince bitiyor
- doğal süreç her şeyin bir ömrü var

Rest ile servis yazınca bu mimari backend sunucu tarafında tanımlanıyor
- bütün frontend tarafındaki apiler zor deyince graphql?  yazılıyor bu iş frontende bırakılıyor
- önceden client db graphql ile storedprocedure ile middleware implement ediyorsunuz şuandaki tüm telefon uygulamalarınız birer masaüstü uygulmasına dönüyor
- bankalar cobol ve plone kullanıyor hala göç maliyeti yüksek geliyor mainframelerin maliyeti yüksek çünkü dikey büyüyor
- biz masaüstü yazalım dediler client server mimarisi geldi
- bankadaki 10binlerce pc donanım seviyesindeki bakımı ciddi maliyet
- bakımı olabildiğince bakımı az sistemler koyalım bakımı merkezi sunucuda yapalım
- html böyle doğdu.
- istemcide sadece browser olması yeterli
- program kurmam merkezi sunucuyu ölçekler yönetirim düşüncesiyle
- sonra cep telefonları geldi
- cep telefonda web uygulaması yerine framework ile native mobil masaüstü uygulması yazalım
- webview yapıyor gerçekten native değil artık
- güncelleme yapmak istediğinizde bir haftaya yakın güncelleme ile uğraşılıyor telefon uygulaması o yüzden artık aynı sorunlara benzer çözümler olduğu için masaüstüne dönecek

[Devam edecek...]

### HTML ve HTTP'nin Doğuşu
bir servisin karşılaması gereken yük çok daha hızlı gelişiyor
- html http
- akademik makale paylaşmaya çalışıyorlar
- earn üzerinde birbirlerine akademik makale atacaklar
- html bir dil yazalım tarayıcı göstersin a href link var başka makaleyi gösteriyor
- ve bir protocol yazalım http yazıldı böyle başladı
- biraz da read only yerine bilgi girelim dediler html formlar başladı
- sonra tarayıcı kavgaları başladı
- netscape ve internet explorer arasında kavga başladı w3c consortium dinlemedi kafalarına göre taglar oluşturdu. netscape batttı yerine mozilla firefox kuruldu.
- internetexplorer yerine chrome sonra safari 
- google chrome, fena değilmiş dedi safarinin kullandığı temelindeki kht kodunu kullandı ve geliştridi  

nasa new horizon pluton fotoları için 5 yıl + 20 yıl + 5 yıl sürüyor ekip hep aynı yerde
Türkiyede 2 yıldan fazla sürmesi problem 

### Mikroservis kullanmalı mısınız?

- sırf trend ve netflix kullanıyor diye kullanmayın
- çok ciddi operasyonel yük getirir
- uygulamanızda ölçekleme problemi yoksa bulaşmayın

- düzenli sürüm çıkartma ve deployment politikanız yoksa kullanmayın
- bağımlılık dependency karışık

netflixin spring clouda bakın servisleri sadece ayakta tutmak için yazdıkları kodun haddi hesabı yok

- caus monkey var netflixte rastgele bir servisi kapatır.
- herhangi bir servisin düşmesi sorun yaratmamalı diyor ve rastgele kapatıyor
- pod kapatıyor ya da network kesiyor servis kapatıyor port kapatıyor random etkilenmesin diye

iyi yapmazsanız gereksiz yere geçerseniz 1 monolith yerine 5 monolith oluyor

farklı departmanlarda gerçekten bağımsız ekiplerin geliştirdiği ürünler için
farklı diller py java vs kullanılıyor diyelim o zaman mikroservis çok anlamlı

ürünlerin farklı parçaları kendi hayat döngüsüne sahip olması
mesela ERP var kocaman diyelim
- farklı modüller var ERP oluşturan 
- stok yönetiminde nadiren değişiklik oluyor
- monolith yapınca herşeyi güncellemelisiniz burda servis bazlı yapacaksınız
- ödemeyi içine gömmek yerine servis olarak yazmalısıınz
- sürekli bir apı değişikliği gerektirebilir bankalarda

bankaların bazı parçalarını isteseniz de mikroservis yazamazsınız transaction gerekiyor dolayısıyla hybrit olmalı

mikroserviste yük dengeleme sadece bir servis yük yiyor yatay ölçekleme ile yaparsanız neyin ölçeklenmesi gerekiyorsa onu ölçeklersiniz

makroservis de var monolithten farklı olarak bir ya da iki işi servis olarak verir bu yüzden mikroservis şart değil

uzun vadeli farklı göç olanakları sağlar

AB testi
kullanıcıların bir miktarına yeni yazılmış servis kullandırıyorum ona göre karar verebilirim  mikroserviste bunu yapmak mümkün

mikroservis tasarım desenleri kurallarına göre yazıyorsunuz, monolithte böyle değil

### Her mikroservis;
- kendi içinde bağımsız olmalı, bunun için bir tane daha servis olması gerek vs böyle olmaz,
- hayır. kendi içinde atomik olmalı.
- diğer ilişkili olduğu diğer servislerle API kontratları ile anlaşmalı
- bağımlılık tanımlarken sitenin sepetini yaptım diyelim mikroservice yazdım
- stok servisi ile konuşabilmeli
- ödeme sistemine haber verme kısmına gideceğim konuşmam gereken başka servisler var
- stok servisinin bana api sağlaması gerekiyor
- ekipler contracts üzerinde baştan anlaşır
- bu yüzden otomatik entegrasyon testlerine ihtiyacınız var, kontractı yanlış anlayığ yanlış implementasyon yaptıysanız misal

product jre yeter
geliştirme ortamına jdk jre gerek
ikisi sdk

iot internet of things o işe özel binary protocoller hazırlanabiliyor
kocaman json göndermek yerine byte gönderecek sıcaklık azaldı ise bir gönderecek tamam

Türkiyedeki tüm yazarkasalardan kocaman data almak yerine binary alacak bitti

klasik rest; conract openAPI swagger eski adı, temel hikaye şu, bu bir format 
- get dersin şunu veririm
- post dersin json cevap veririm
- bu dokumanı yazarak openapı contracktını yazarım ve el sıkışırım
- bu tasarım için araçlar da var
- elinizde böyle bir servis varmış gibi oluyor

per database per service her mikroservice kendi databasei ile haberleşmeli
- sipariş servisinin db.sinde cari servisin bilmemne bilgisini tutmamalıyım
- çünkü caride değişiklik yapınca sipariş servisinin patlamayacağının garantisini vermez ama yine de sıkı bağlı mikroservisiniz olur bu da kabul edilebilir gerekçeniz varsa
- zayıf bağlı olsun istemedik dersiniz ama zorunda tasarımınıza zorunlu kalıyorsunuz, 
- mutlaka veritabanı değişiklik yönetimi ile beraber bunları yapmalısınız
 
ortak database antipattern servisin bağımsız olmasını engeller

s1 + db1
s2 + db2
s3 + db3

s1 s2'ye bağımlı olabilir ama gidip de db2 ye bağımlı olamaz

IDM ıdentitiy management monolith de yazıyor olsanız kullanıcı yönetimini ayrı yazmalısınız

kullanılan en kesitrme çözüm LDAP directory access protocoll aktive directory openLDAP apache vs birsürü var LDAP, http gibi bir protokel, ilk yazılma amacı telefon defteri yönnetmekti şimdi kullanıcı yönetiminde kullanılıyor
artık kullanıcı parolalası yetmiyor yetki vs de gerek

macrofronted ile yazdığım uygulamaları küçük küçük paketledik.

single sign on SSO biyerden login oldum bana bir daha kullanıcı adı sorma ya

OAuth JWT en yaygın kullanılanı

tekerleği yeniden keşfetmeyin IDM ürünü var JBOSS var, SSO desteği sunuyor
bu token içinde jwt esnek ama en az olmalı kullanıcı adı olmalı
jwt içini doldurmayın esnek diye çünkü her requestte bu dolaşacak

(stackness servis yazabilmek için?)
iki temel sorun yük dengeleme ve yüksek erişilebilirlik
yüksek erişilebilirlik, ne kadar süre kapalı kalabilir, kurumsal uygulamalr gece kapalı kalsa da olur ancak sağlık finansal vs açık kalmalı yüksek erişilebilir olması için ondan repica almalısınız.

free otp var sms desteği için ama dünyada sms kullanan tek ülkeyiz doğrulama için:D

key cloud başarılı inceleyin?  , oAuth için de geçerli signout olduğunda token akışı açısından access token süresi var refresh token ile 5 dk bitince yeniden alman lazım.

oAuthdan eski SAML var bir de sunucu taraflı OAuth client dağıtıyor
authentication authorization
authz authc kısaltması
kimlik doğrulama bu adam bu mu
yetkilendirme kısmı var 
rolebase access control yaptınız önden tanımlayarak

jwt içine rolleri de göüyor yetkilendirmeyi de oauth ile yapıyorsunuz

kullanıcı hangi yetkilere sahip olduğunu tutmak için ek tabla tutmanız gerekir ıdm üründe

## 11.02.2025 Salı

Mikroservislerde transaction yönetmek zor
böyle bir sisteminiz varsa mikroservis kullanmaya kesin karar vermeniz lazım
transaction yönetimini genelde kütüphaneler destekliyor birinde hata varsa transaction rollback yapıyor geri alıyor
birden fazla servise gidiyorsa orneğin dosya yükleme transaction ya yok ya da zor
servis tabanlı mimarilerde örneğin ödeme sistemleri e ticaret banka ile anlaşmışım ödemeyi aldı mı almadı mı
kullanıcı bilgilerini aldım db.ye yazdım bankaya gittim banka böyle bir kullanıcı yok dedi geri almam lazım
1-3 dk arasında transactionı açık tutup db.yi de block edemem
biz bunu farklı yapacağız
geriye dönüp haydi az önce yaptıklarını geri al demem lazım
işlemin yapılıp yapılmadığını anlamak için durum kısmına ihtiyacım var
muhasebe yazılımlarında yapılan işlem silinmez ters kayıt yapılır
ek flagler tutulur kayıtın son halimi mi değil mi
gerçekten silmeyiz soft delete yaparız
silindi diye işaretlenen kayıtları house cleaning yaparak çöp toplayarak gerçekten silme yapılacaksa yapılır
çoğu yerde gerçekten silinmez arşive alınır
o yüzden bu bir sistem tasarımı değil mikroservis yazılımı ile yazılım geliştirmelisiniz ki başarılı bir şekilde mikroservis mimarisini uygulayabilin
JTA java transaction api var javada
two face commit diye bir yapı var yaklaşık 40 yıldır
önce 1. transaction yapıp 2. sefer üzerinden geçip gerçekten commit mi yapacaksınız kararı verilir
saga patterni örneği var mikroserviste transaction problemini çözmek için uygulanan yöntem
bunu baştan planlamalısınız.

asenkronda hiç haber almayabiliriz işin içine timeout giriyor bu yüzden
sorun network problemi de olabilir
transaction garantisi veren mq veren bir şey kullanacaksınız ya da asenkron transactiion yapmayacaksınız

bilet alırken belirlenen süre içinde koltuk rezerve ediliyor alamazsanız geri açılmalı koltuk. 

fener galatsaray bileti satılacak biletix 10 kişi alıp geri kalanı binlercesi bekleme odasında firewall önünde redirect ediyorsun

pattern nedir? tasarım deseni bu bir mimarlık terimi
yazılım mühendisliği diye bir dal henüz yok
bir tane ölçülebilir mühendislik çalışması var o da net değil, bizimkisi zanaat
3 gün de olur 1 ay da olur kestiremiyoruz
önce tüm isterler çıkarılır vs vs yazılımda mümkün değil

1 projede çalıştım 3 kere müdür 2 kere kanun değişti bir yılda iki üç kere yeniden başladık projeye

algoritma tek başına yetmiyor 
gang4 ekibi 40 tasarım deseni üretiyorlar singleton bir tane instance oluşturacak
factory pattern
builder pattern
antipattern abi bunları yapma tarzında yazılıyor
en yaygın kullanılan 7-8 tanesi
bunlar yönergeler çözüm yolları
implementasyonları farklı farklı olabilir size kalmış

algoritmalar ve tasarım patternlerini mutlakaya okuyunuz.
aklınıza gelmeyen bir problem ve çözümü var kullandıklarınız da vardır

mikroservis yapıp saga patterni duymayan var
oyundaki bir araba yükü dağıtmak için istatistik bilgisi toplamak için çıktı ilk başta saga patterni.

mikroservice yarattığı sorunlar;
- operasyonel yük
- problem tespiti zor
- sla service layer agreement canlı sistemin ne kdar ayakta kalabilir?
- düzgün sürüm politikası şart, 
- servislerimiz diğerinin apisine bağlı bu yüzden bir uygulama sürümü iki api sürümü yapılmalı
- apı abi var application binary interface ? 
- abi kırmak ?
- implementasyon kırmak ? apide değişiklik yok sonuçlar farklı bağımlılığı yönetmeye ihtiyacınız var dependency matrix tutmanız gerekiyor
- SemVer semantic versioninng şiddetle bunu kullanmanızı tavsiye ederim sürüm numaralandırma için 
- 1-0-2 major minor bugfix.
- major 1 den 2'ye değişince major değişikliktir göç planlamayı gerektirir kullanıcı arayüzü değişti. minor numara değişmişse yeni bir özellik eklenmiştir yeni bir ayar gelmiştir. bugfix değişmişse gözü kapalı değiştirmeniz gerekiyor sadece hata değişmiştir.
- bu değişiklikleri dikkatli yapmalısın
- cuma akşam canlıya birşey konmaz
- sürüm politikanız olmalı bir belgede
- release stream java 6 ayda bir sürüm çıkarır
- proje takvimi içinde product ownerın tercihleri ile belirlenir ne zaman olacağı
- hata düzeltmeleri 
- eğer sistem durmuşsa anında
- zorlaştırıyorsa bir haftadır
- formun şurasında hata varsa bir ayda

cı/cd 
bazen geliştirici paketler sürüme kadar çıkar

ek araç gereçler kullanmak şart

SDLC software development life cycle
- git 
- sürüm takip sistemi
- iş takip sistemi
- düzenli derleme sistemi cı sıfırdan derleme
- static code analizi kod yazım kurallarını belirlemeli ve uymalısınız sonar
- otomatik birim testleri fonskiyon testleri
- code reviewlar

bunları uygulamadan mikroservise geçmemelisiniz

[Devam edecek...]

## 12.02.2025 Çarşamba

api kırmak , abi kırmak
api için declaration yapıyorsunuz methodum şu şu paremetreler alır
yeni bir method geliştirdiniz apinizi şu şu özellikler eklediniz api kırdınız
major versiyonda göç planlamanız gerekiyor yani iki farklı uygulama gibi

abi kırmak
methodum ve parametrelerim duruyor ama fonksiyonun yaptıgı iş aynı değil

siz minor sanıyorsunuz major çıkıyor bugfix sanıyorsunuz minor çıkıyor 
herkes semantic versioningi doğru yapmıyor

### SAGA
transaction problemini çözmek için kullanılan bir desen

### ACID
normalde transaction sisteminin garanti etmesi gereken şey yaptıgınız işlemin tutarlı bir bütün olarak yapılıp yapılmadıgını garanti etmek
transactional asıl dbde kaydınızın tutarlı yazılıp yazılmadıgını garanti eder 
illa db bunu vermek zorunda değil 
mq sistemleri ben transaction destekliyorum der consumer işinin bitirene kadar işlemin tutarlı olacagının ya da olmayacagının garantisini verir
transactional relational dbde çok önem taşır , ACIDi garanti etmek için dağıtık çalışamazlar, dikey büyürler
aynı anda iki kişi birden çalışma yapıyorsa db.de o alanda db kendini gitlemesi lazım
log sürelerini ne kadar kısa tutabildiği ile ilgili bazıları page kitler, bazıları table kitler, okurken kitlememiz de gerekibilir bazen okuyup o veri için işlem yapacaksam
uygulamada atomic integer, atomic long diye geçer, multiconquirency kilidiniz okumaya izin verir güncellemeye izin vermiyor olabilir farklı yorumlar var sizin ihtiyacınız olan hangisi, örneğin javada syncronize koyarsanız java compileri single thread kullanacak şekilde kitler okuyamaz bile, ihtiyacım bu mu çoğu zaman değil
javada ayrıca log kütüphanesi var while içine konulur while(getLog()) bu tür apiler benzer notasyonda yazılır
işiniz bittiğinde de geri release etmeniz lazım. etmeyince deadlock olur. iki farklı thread birbirini bekler. eğer ki bir timeout varsa ona kadar yoksa sonsuza kadar bekler.
sistemde yük yok bişey yok cevap yok thread havuzunda bütün threadler dolu ama işlem yok
db.de çogunda deadlock detection sistem var eskiden yoktu. mysql postgre mysql var. %99.9 arada deadlock sandıklarınız feature da olabilir.
transaction açık duruyor begin tran dedmişsiniz commit ya da rollback yoksa..
ya da içinde başka işlemler içeren yerlerde yapıyorsanız sistem bekler, olabildiğince küçük sürelerde transacitonal yapılması lazım.
transacional timeout süresi değiştirmediyseniz bellidir
transacional timeout süresi  5 dk proxy timeout süresi 2 dk ama cevap yok uyumlu olmalı bu süreleriniz
proxy web sunucularınızı kim ayarlıyor apache nginx 
3-5 dk süren işlemleri arka tarafta yapın kullanıcı browserında 5 dk beklemesin ya da engelleyebilmek için frontendde ekran kitliyoruz 

excelde jsonda vs 365bin veriyi işlemek yerine bach streaming yapıp bloklar halinde kullanında dom kütüphaneleri yerine socks kütüphanelerine okuyarak ilerleyin, yoksa veriniz ram'e sığmaz
ben her batch commitlerim 100 kayıt 100 kayıt atarım, 100 kayıtın tutarlı oldugunu bilirim
hangi 100 kayıtta patladıgımı biryere atmam lazım ki
1000 kayıtlık bişeyde 500 kaydı attım gerisinde hata çıktı tek bir kayıtta atmaması lazım nerde kaldım hangi kayıtta
uzun süreli transaction açmamalısınız.
(connection pool denen yöntemler thread pool yöntemleri var
her zaman deadlock olmaz ama threadler doludur feature deme sebebim bu
100 connecitonun 5 ini kafadan aç lazım olunca kullan
ama memoryde yer kaplıyor her daim açık olan da
bir dk boyunca boş kalabilir sonra kapat
jvm.de max 300 thread olabilir şu kadarı açık kalsın şu kadar süre açık kalsın
kullanıcı yavaşlıktan şikayet ediyor, yük yok deadlocktan önce bakacağınız yer,
pool configürasyonunuz yanlış ! monitoring anlamında bakmanız gereken yer heryer
http connection pool ya da biryerde sıraya girmiş bekliyorlar.)

bibirinden iki bağımsız ayrı makinede transaction bloguna ihtiyacım varsa ne yapmalıyım? eski bir problemdir bu.
kod çalışıyor ve iki ayrı db üzerinde işlem yapması gerekiyor ve transaction yapmamız lazım, bir de dosya sistemi koydum (transactional dosya sistemi henüz yok)
two face commit denen iki fazlı commit denen bir yapı var, dblerde kendi içlerinde bu çözüm var,
connection stringleri farklı javada JTA kütüphanesi ile geliyor, iki db.ye de kayıtları koyuyor commitlemiyor hata yoksa commit yapıyor en son.
commit yaparken hata alıyorsanız diskte vs hata var ve geri alınır
transaction ne kadar uzunsa replication gap'iniz o kadar uzundur.

db kendi desteklemiyorsa;

### Mola sonrası
transaction olmayan yapılarda nasıl yapacagız?
- kayıt silme işlemi ise kaydı geri getirmeniz lazım
- ya da soft delete yapmanız lazım silinmeyenleri getir şeklinde
- command pattern kullanıyorsanız command bu tersi bu şeklinde..
- silme noktası açısından da bazı şeyleri tasarlamanız lazım
- infinite state machine sonsuz durum makinesi state durum makineleri konusunu hatırlayın uygulama tasarımı yaparken bunu kullanır ve delete yapmazsınız, soft delete yerine bunu kullanıyorum
- örneğin sipariş kodunu ilk oluşturdugumda taslak kısmında duruyor tamamladı diye müdürüne gönderdi onay state'ine geçti yayınlandı artık event fırlattı
- müşteri itiraz etti eksik girmişsiniz dedi tekrardan edit moduna aldım iki gün sürdü diyelim tekrar onaya gitti yeni durum bu
- hepsini loglayıp kullanıcılara blame verebilirsiniz
- en son sipariş iptal oldu diyelim siliyor muyum hayır iptal state'ine alıyorum. delete operasyonum hiç olmadı farkındaysanız. işlemin tersini alma operasyonuna çözüm olmalı bu.
- hangi durumda hangisine geçilir önden tanımlanır state machinelerde bu her bir state arasındaki işleme action denir.
- uygulama geliştiricinin bunu önden tasarlaması gerekiyor
- java enum içinde yapmak en basit hali
- stateless kütüphanesi de var bunun için
- bunun bir de abisi var,
- BPM adı da BPMN business process management notation,
- sistem analizleri tasarlar çalışır olması lazım ama gerçek hayat böyle değil
- start -> servicenode(human task var içinde) -> kararr noktaları -> betik taskı, eposta taskı
- süreçi analiz ederken, müşteri ile konuşurken kullanıyorum ben bunu.
- BPMN engine kullanabilirisiniz;  activitiy bonito vs zibille ürün var 
- BPMN' ler gelişmiş state machinelerdir
- satış süreci ipucu ile başlar lead toplar satış süreçlerinde bunlar, en son fırsat ya kapanır ya da teklife dönüşür, kabul ettiler sipariş aldınız, hizmet ya da mal şeklinde, parayı kim alacak nasıl alacak fatura kestiniz ödeme aldınız süreç ancak bitti, her birinin süreç akışı içinde yeri var
- bunun transaciton ile ne alakası var?
- bu iki bir iki aylık süreç 
- long running process ; uzun süren işlemler denir buna
- cari sistemine girip sipariş sisteminden bilgi almanız lazım misal, bu etkileşim tutarlı mı bunun için bir çeşit transaction kurgulamam lazım.
- bunun için geldik sonunda SAGA'ya
- bu hikayenin hepsini bir loga dolduruyoruz, bu logun adı SAGA
- sonra herhangi bir şekilde sizin sisteminizin hata kabul ettiği bir durumda geri dönmeye başlamanız gerekiyor
- SAGA loguna tarihçe koymuştum ya tersine işlem ne yapacaksam yaparım,
- command patterni kullanabilirsiniz zorunda değilsiniz jakarta içinde bu işi URL ile yapıyorlar.
- orcestration yapan bir servis SAGA'yı yönetir, ya da eclipse'de long running kütüphanesi var onu kullanırsınız
- ya da SAGA kullanan kütüphaneler var begin saga commit saga rollback saga tarzında kullanıyorsunuz
- işlemin tersini nasıl alacağız bunu eninde sonunda bizim yazmamız lazım, ürün kütüphaneler var ama geri alacağız nasıl alacaıgız eninde sonunda geliştirici yazacak
- mongoda tüm tarihçeyi tutan javerse var.(The Leading Framework for Object Audit and Diff in Java- JaVers is the lightweight Java library for auditing changes in your data.)

Tüm loglama sistemleri çok yer kaplar, sen istiyor SAGA şeklinde opsiyonel tutuyoruz. 

bütün bu sistemlere bir yan sistem de eklemeliyiz
housekeeping

mongoda yeni bir nodeaçar ona replica olur 
postgrede vacum kullanılır
reac diye bir db.de silme komutu yok çünkü daha maliyetli silmek tutmaktan diyor gerekince housekeeping yapıyor

B Tree algoritması ya da varyanti ile tüm veritabanı sistemleri veri tutar
dolayısıyla bir arama yapmaya başlayınca hızlıca veriyi bulur ama veriyi tekrar tekrar yazar ağaç hiyerarşisi ya da indexinize dahil
runtimeda index yanlış kurulursa sürekli alta ek ya da ağaç sağa kayar
insert update delete işlemlerinizdeki tüm işlemler indexte işlem yapmayı da gerektirir, ne yaparlar dbadminler yeniden index oluşturur
Bu tür sorunlara çözüm için de housekeeping yapılır
gerçekten lazım olan veriyi farklı yere nadir bakacagınız veriyi farklı yere koyarsınız.

### Mola sonrası
kullandığınız db.e göre last page insert olan sistemler oltp sürekli veri giren insert olan sistemlerde tersine kayıt almak updateden daha az maliyetli hale gelebilir?
5 yıldan fazla veri kanunen saklanmamalı, unutulma hakkı, 5 yılı da digitalde tutma zorunlulugu yok kağıt olarak da tutuluyor,
sağlıkta on yıl, adli vakada 100 yıl,anominize etmek de çözüm olabiliyor artık, crypto olarak saklanmalı,

intel ve arm.da açıklık var tüm cpuları değiştiremeyeceklerine göre şuan örtbas ediliyor

ruslar bir unixe honeypot koyup yıllarca bu unix sistemlerinin kullanıldıgı heryeri izliyorlar
pardus ordudaki birim nato ürünlerini kullanınca komutlar gelmiyor vs
Tubitak UEKE başlattı ancak şuan debianın paketlenip sürüldüğü hale dönüşüyor
pardus önce sunucularla başlayıp community isteyince masaüstüne yönelseydi...
pissy paket sistemi ile başlayıp debian redhat baz seçilmeyip üzerinde tonla ürün geliştikten sonra vazgeçtik değiştiriyoruz olmaz

SAGA'ya geri dönecek olursak bunlar desen birebir kullanmanız gerekmiyor.
hem SAGAyı hem aradaki networkü ilgilendiren 
mikroservis rest üzerinden konuşur 
kullanıcı ile ne tercih edeceğiniz tartışma konusu gidip gelen verinin tipi çok önemli
binary protocollerle mi konuşşsa sunucular vs bakınıyordum
eğer ki veriniziniz önemli bir kısmı stringden oluşuyorsa binary protocollerle uğraşmanızın anlamı yok çünkü verideki stringleri sıkıştırılamıyor
ama atributelar tekrarlanıyorsa avantajı olur. tekrarlandıgı durumda bile bir metricde izlenebilir
http client ve http server arasında transparan iletişim var
önce zipleniyor sonra açılıyor. string verilerden zipin sıkıştırması binaryprotocollerden daha iyi olabiliyor
iot cihazi ise json kullanmayın tabi binary kullanın.

log için sagaid gönderiyorsunuz http headerda, interceptor maliyeti fazla.
api version dahil headerda göndermeyi tercih ediyorum
url gönderirseniz api rulelarla uğraşırsınız

http3 denk geldiniz mi? 
günlük yaşamda http1.1 kullanıyorsunuz http2 tüm dünyada yayılmış değil
http3 daha yaygin kullanılacak artık
jwtniz büyükse bu headerlarınz payloaddan daha büyük oluyor
23 fıkrası gibi tüm fıkraları numaralandırmışlar gülüyorlar :D bu tip headerlarda numaralandırıyoruz

sql injection yerine script injection deniyorlar ormler kullanıldıgı için sql injection pek mümkün değil artık, owasplar
developer tool açıksa engellemezler

tasarımlar bitti diyebiliriz.
şimdi OPS kısmındayız
DevOps terimi tanımı çok net değil
nodejs performans ihtiyacı saniyede 3000 request gelecek response zmanı 50 milisaniyeyi geçmeyecek
burada nerede takıldım linux socket açınca 300 milisaniye daha açık kalıyormuş direkt kapatmıyormuş
3000 request açamıyormuşum
linux configürasyon ayarlarından socket timeout süresini 50 milisaniyeye girince sorun çözüldü

DevSecOps,için işine güvenlik denetimleri de girer
GitOps operasyonel süreçlerin yapılma methodolojisidir

farklı ihtiyaçlar için farklı varyasyonlar tanımlanıyor birşey daha ekleniyor. CI/CD pipelan hazırlama yerine DEVOPS süreçlerinizi tasarlamak diyecekler için bir de reklam kısmı var

Configurastion as a Code,  tüm süreçler code olarak yazılmalı ve takip edilmeli
Gitops da tüm süreçleri git üzerinden takip edecel
Jenkins pipelanlarınızın bir git sunucusunda hazırlanması tasarlanması ve Jenkinsin ordan alması gerekiyor

etcKeeper linuxdaki tüm configürasyon dosyalarını etc içinde tutar git gibi etcKeeper da tüm süreçleri buradan takip eder

CI/CD düzenli derleme ve düzenli yükleme mekanızmalarına ihtiyaç var
her defasında manual derlemek ve yüklemek mümkün değil
ister jenkins ister gitlab runner ister tenkton kullanın pipelineların code olarak hazırlanıp takip edilmesi lazım elle bir kere yaparsınız o kağıttan kule yıkılır.

devopsçuların sorumluluğunda bu deployment politikalarını hazırlamak var

Knative gitops da duyarsınız yapılan iş gitteki işi alıp k8se yükler
Knative, serverless workloadların Kubernetes clusterlarında çalışmasını sağlayan ve bulut tabanlı serverless uygulamaların Kubernetes'e deploy etmek, çalıştırmak ve yönetmek için componentler ekleyen open-source bir frameworktür. 2

SemVer semantic versioning
major minor şöyle yapılır kuralları belli
size her zaman uymuyor olabilir 
1.0.1 gibi bişey yeter da artar deniyor
bazen yetmiyor 
api ve numara sürümünü biryerde takip etmek istiyorum ben derseniz yetmez
1.1.0.1 deyip ikinci kısım api numarasıdır da diyebilirsiniz
redhat tarafı 
javacılar farklı davranıyor 1.1.0.Beta tipinde
özgür yazılım ise sonuncu sürümü de işaretlerler 1.1.0.1- beta rc final ga şeklinde
incremental olmalı, aynı numara iki farklı sürüm için kullanılmaz
tek sayı çift sayı kullanılıyordu bir ara geliştirici sürümü genel kullanım sürümü
pazarlamacılar yeni sürüm çıktı versiyon 2 çıktı şeklinde farklı bir versiyonlama kullanabilirler teknik sürümün bunla alakası yok
cibase veritabanı 4 ten 10a zıplar oracle 10 sürümü oldugu için pazarlama için :D
apache mq server aratınca active mq artemis diye bulacaksınız çünkü kodu baştan yazdılar, pazarlama değildir bu proje adıdır. eski hali devam eder.
marka birleştirilmes sabancı sonuna sa koyar
mal farklılaştırması konusu da var
ürün optimize noktası fiyat ve kişi ortasıdır. bu noktayı buldum
aynı kahve adını değiştirelim daha yüksek fiyat koyalım buna mal farklılaştırması nedir
benzeri yazılım için de geçerli

[Devam edecek...]

## 13.02.2025 Perşembe

### Sürüm Yönetimi ve Artifactlar
Sürüm numaraları düzgün verdiniz diyelim nereye koyacaksınız artifact(paket)leri ürününe koymanız gerekecek ve kodunuza ilişkilendirmeniz lazım
o sürüm numarasından kodunuza erişebilmeniz lazım
git link list kullanır çift taraflı yapar
biliyorum ki sürümü git tag bla bla etiketlemem lazım
1.0 1.1
herhangi bir zamanda o sürümü oluşturan kaynak koda geri dönebilmem gerekir, kim yaptı ne zaman yaptı ne değiştirdi
compile ve build ikisine de derleme diyoruz ama burada kastettiğim build

git üzerinde gitflow gitin branch tag commitleri nasıl kullanıcağınıza dair politika akışı belgeniz olması lazım bununda bir üstü gitops zaten
ama ben kişisel olarak daha basit bulduğum için mainline kullanıyorum.
mainden bir feature branch açacaksınız isimlendirme kuralı uygulayın, jira vs bişey kullanıyorsunuz iş bitince maine merge olur
otomasyon kuralı birim testi şu bu bişey yaptım merge request yaptım, gitlab bitbucket github istekte bulununca CI pipeline'ı jenkinsi dürter
üzerinde derlenebiliyor mu statik kod analizi (sonar) vs herşeyi yapar, ben baktım sen bak kısmı code review kısmına geçer, birbaşka kişinin bunu onaylayıp içeri alması gerekir
kurallara uysak da otomatik mekanik oldugu için o süreçler gözden kaçabilir misal sonar Türkçeden anlamıyor isimlendirme İnglizce mi?
ya da algoritma ya da değişken tanımına bakılması lazım, code reviewda reject olabilir kodunuz düzenler tekrar atarsınız ya da merge kabul olur.

ilk gelen merge ile ikinci gelen merge.de conflict varsa,
git rebase komutu ile conflicte baktık ve çözdük
yakın satırlarda bişey varsa çıkıyor ama ya benim methoduma parametre girmişse api kırmışsa, pipeline'nızda bunu da mergeden sonra derlenebiliyor mu diye otomatize edebilirsiniz

mainlineda sürüm kendi başına release branchine gider
sürüm çıkarıtrken önce sürüm adayı çıkacağım politikalarımda bunlar olmalı.
sürüm her zaman cı/cd sisteminden çıkar, birinizin makinesinden çıkmaz !
canlıya çıktım hata var
hata branchleri hatanında tespit edildiği yerden açılır 
release branchine mergeledim hata branchini ve çıktım
mainde henüz bişey yok.
releasedeki hatayı mainde tekrar denemem lazım
cherypick kiraz toplama sadece bu commiti al ve maine getir dedim
bu işi yapabilmek için başka bir şart var conventional atomic commit yapmanız lazım
bir şey mümkünse bir commitle çözülür eğer bir committe birden çok şey yaptıysanız squash yapın sıkıştırın // git merge --squash new-feature
cherrypick ile çözülemezse otur tekrar yaz, o yüzden commitler atomic olmalı bir iş yapmalı
cherrypick ile yapmalısınız iz kalsın
git cherry-pick <commit-hash>
commit logları düzgün yazılmalı
conventional commit yöntemleri var.

feat:Başlık
fix:Başlık
açıklama

<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
refs: #123 // jira numarası redmine numarası yazılır

şu dosyada şunu değiştirdim yazmayın, neden yaptıgınızı yazın
iş takip sisteminde yer alan şeyleri refs kısmına yazmalısınız bu sebeple iştakip numarasını yaz
gitten bişey silemezsiniz son commiti silersiniz ancak revert dediğinizde tersine commit çalıştırır.
fix geçiyor bugfix artırdım, feed geçiyor minor artırdım

şimdi geldik mikroservis versiyonlamasına

satır sonu karakteri utf-8 virgül tab boşluk parantez sonrası nasıl olacak gibi yazım kurallarınız olmalı.
ide formatterı düzgün bir formata göre olmalı, bir kural seti hazırlayıp herkesin idesine bu formatı import edebilirsiniz
Kubernetes YAML Linter for vscode kullanabilirsiniz.

CRLF carriage return line feed and LF line ending on windows/linux
windowsta satır sonu 2 bytetır
carriage return linux enter
macosda blank

### Bağımlılık Yönetimi
bağımlılık yönetimi ve sürüm numaraları
npm package.json A ^1.0 yeni sürümü çıktı indiriyor
dışarıda bir de package-lock.json var bunların gite gitmesi gerekiyor
dışarıda lock dosyası varsa bundan sonra o sürümleri kullanacak tekrar silip build almadığım sürece
ya da exact numarası neyse onu yazıyorsunuz çünkü neyin hangi sürümle çalıştıgını bilmek zorundayım.

ben bir paket üretmeliyim ve versiyonları ile birlikte bir artifact deposuna sona type nexusuna misal koymalısınız.
kütüphane lazım olunca herkes ordan alır
kurumun internet deposunda nexusda gider internetten o cachler
indirilen paketin sha1 mda5 ini kiminiz kontrol ediyorsunuz?
içinde başka cryptominer gömmüşler mi
bütün paket sistemleri derleme sırasında elektronic imzalı 
ama kimse uygulamıyor imza atma süreçlerini
nexus indirdiği paketin doğrulugunu test ediyor ve güvenlik açıkları varsa uyarılar da veriyor
o nexus deposunda sizin ürettiğiniz artifactler de duracak
jar war container image'ı hepsini oraya attım kubernete atmak için helm paketi hazırladım onu da oraya attım
kritik olan ne, o depoya kişisel bilgisayardan bişey koymamalısınız
geliştiriciler sadece read only okuma yapmalı o depoya bişey koyma kısmı herkeste olmamalı
derleme cı aracından olmalı
bende çalışıyor problemi
test ortamının çalışması
cı gider son kodu alır gelir.
ama yerel bilgisayardaki derlemenin son kod oldugunun garantisi yok
Türklerden leftpad sola dayama fonksiyonu npm packetini kızmış silmiş, 2016  Azer Koçulu 
tembellik etmeyin kodunu da alın githubta fork almak tek tuş

[Devam edecek...]