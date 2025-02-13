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
hangi 100 kayıtta patladıgımı biryere