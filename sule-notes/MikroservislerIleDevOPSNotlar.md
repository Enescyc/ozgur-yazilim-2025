
# Mikroservisler ile DevOPS 
> Mustafa Akgül Özgür Yazılım 2025 Kış Kampı
> Eğitmen :  Hakan UYGUN

## İçindekiler
- [Özgür Yazılım Nedir?](#özgür-yazılım-nedir) 
- [Servis Kavramı (SOA, Web Servis, Makroservis, Mikroservis, RPC, gRPC)](#servis-kavramı) 
- [Mikroservis Kavramı](#mikroservis-kavramı)
- [Json Şema Kavramı](#json-şema)
- [Yaml Kavramı](#yaml-nedir)
- [API ve ABI Kavramı](#api-ve-abi-nedir)
- [Semantik Versiyonlama](#semver-semantik-versiyonlama)
- [Paketleme Sistemleri ve Yönetimi](#paketleme-sistemleri-ve-yönetimi)
- [Nexus Depolama](#nexus-nedir)
- [Konteyner Yönetimi İçin De Facto Araç Setleri](#konteyner-yönetimi-için-de-facto-araç-setlerinden-bazıları)
- [Mikroservis Tasarım Desenleri (Saga, 2PC, Circuit Breaker, CDC, Event Sourcing, Streaming, Backend for Frontend, API Gateway, Sidecar, Service Discovery)](#mikroservis-tasarım-desenleri)
- [Düzenli İzleme (CM)](#duzenli-izleme)
- [Kubernetes Kavramı](#kubernetes-nedir)
- [Sistem Mimarisi](#sistem-mimarisinin-yazılım-üzerine-etkisi)
- [Cluster Kavramı](#cluster-nedir)
- [etcd Kavramı](#etcd-nedir)
- [Cluster State Durumu](#cluster-state-durumu)
- [Yüksek Erişilebilirlik (High Availability - HA)](#yüksek-erişilebilirlik-high-availability---ha)
- [Performans ve Ölçeklenebilirlik (Load Balancing & Scalability)](#performans-ve-ölçeklenebilirlik-load-balancing--scalability)
- [Distributed Cache Kavramı](#distributed-cache-nedir)
- [Felaket Kurtarma Merkezi (FKM)](#felaket-kurtarma-merkezi-fkm)
- [MQ Implementasyonu](#mq-implementasyonu)
- [Kubernetes API'leri](#kubernetes-apileri)
- [DevOps Yöntemleri](#devops-yöntemleri)
- [Custom Resource Definition (CRD)](#custom-resource-definition-crd)
- [Operator Kavramı](#operator-nedir)
- [GitOps](#gitops-nedir)
- [DevSecOps](#devsecops-nedir)
- [KNative](#knative-nedir)
- [Git Komutları ve Kullanımı](#git-komutları-ve-kullanımı)
- [Jenkins Nedir?](#jenkins-nedir)





## **Özgür Yazılım Nedir?**

Özgür yazılım (Free Software), kullanıcıların yazılımı **çalıştırma, kopyalama, dağıtma, inceleme, değiştirme ve geliştirme özgürlüğüne** sahip olduğu bir yazılım türüdür. **Özgür yazılım, ücretsiz olmak zorunda değildir**; burada özgürlük, kullanım haklarını ifade eder. Özgür yazılımın temel prensipleri Richard Stallman tarafından tanımlanmış ve **Özgür Yazılım Vakfı (FSF - Free Software Foundation)** tarafından desteklenmiştir.

Özgür yazılımın sunduğu dört temel özgürlük şunlardır:

1.  **Özgürlük 0:** Yazılımı herhangi bir amaçla çalıştırma özgürlüğü.
2.  **Özgürlük 1:** Yazılımın nasıl çalıştığını inceleme ve onu kendi ihtiyaçlarına göre değiştirme özgürlüğü. (Kaynak koda erişim gereklidir.)
3.  **Özgürlük 2:** Yazılımın kopyalarını paylaşma ve dağıtma özgürlüğü.
4.  **Özgürlük 3:** Yazılımı geliştirip iyileştirme ve bu değişiklikleri toplulukla paylaşma özgürlüğü.

Özgür yazılımın en bilinen örnekleri arasında **Linux, GNU, LibreOffice, GIMP, Mozilla Firefox** gibi yazılımlar bulunur.

----------

## **Özgür Yazılım Lisansları Nelerdir?**

Özgür yazılım lisansları, yazılımın özgürlüklerini garanti altına almak için belirlenen lisanslardır. Başlıca özgür yazılım lisansları şunlardır:

#### **1. GNU Genel Kamu Lisansı (GPL - General Public License)**

-   En yaygın özgür yazılım lisanslarından biridir.
-   Yazılımın değiştirilmiş veya değiştirilmemiş hallerinin de özgür kalmasını zorunlu kılar.
-   "Copyleft" prensibini uygular, yani GPL ile lisanslanan bir yazılımın türevleri de GPL lisansı ile dağıtılmalıdır.

#### **2. MIT Lisansı**

-   Kullanıcıya yazılımı özgürce kullanma, değiştirme ve dağıtma hakkı verir.
-   Ancak yazılım "olduğu gibi" sağlanır ve herhangi bir garanti sunulmaz.
-   Kapalı kaynak projelerde de kullanılabilir.

#### **3. Apache Lisansı**

-   Kullanıcılara özgürlük tanırken, patent haklarını da koruma altına alır.
-   Değiştirilen yazılımların da Apache Lisansı ile lisanslanması zorunlu değildir.

#### **4. BSD Lisansı**

-   MIT lisansına benzer, ancak türetilmiş yazılımların lisanslarını değiştirme özgürlüğü tanır.
-   Kısıtlamaları az olduğu için hem açık kaynaklı hem de kapalı kaynaklı projelerde tercih edilir.

#### **5. Mozilla Kamu Lisansı (MPL)**

-   Hem özgürlük sunar hem de ticari kullanım için uygundur.
-   Kodu açık tutarken, bazı bileşenlerin kapalı olmasına izin verir.

Bu lisanslar, özgür yazılımın nasıl kullanılabileceğini, dağıtılabileceğini ve değiştirilebileceğini belirleyerek geliştiricilere ve kullanıcılara yasal güvence sağlar.

# Servis Kavramı
## **1. Service-Oriented Architecture (SOA) – Hizmet Odaklı Mimari**

SOA, bağımsız olarak çalışan ve birbirleriyle iletişim kurabilen servislerin oluşturduğu bir yazılım mimarisidir. Servisler genellikle belirli işlevleri yerine getiren bağımsız bileşenlerdir.

### **SOA'nın Temel İlkeleri**

-   **Gevşek Bağlılık (Loose Coupling):** Servisler birbirine sıkı şekilde bağımlı olmamalıdır.
-   **Tekrar Kullanılabilirlik (Reusability):** Servisler farklı uygulamalar tarafından kullanılabilir olmalıdır.
-   **Standart Arabirimler (Standardized Interfaces):** Servislerin ortak bir iletişim standardı kullanması gerekir (örneğin, SOAP veya REST).
-   **Bağımsızlık (Autonomy):** Her servis kendi içinde bağımsız olarak çalışmalıdır.

### **SOA'nın Avantajları**

-   Servisler, farklı teknolojilerle yazılmış bileşenlerle çalışabilir.
-   Uygulamaları ölçeklendirmek kolaydır.
-   Tekrar kullanılabilir bileşenler oluşturmayı sağlar.

### **SOA'nın Dezavantajları**

-   Karmaşık yönetim gerektirebilir.
-   Servis çağrıları genellikle ağ üzerinden yapıldığı için gecikme (latency) olabilir.
-   Servislerin birbirine bağımlılığını yönetmek zor olabilir.

----------

## **2. RPC / RMI / IPC**

Bu kavramlar, farklı sistemler veya süreçler arasındaki iletişim yöntemlerini ifade eder.

### **a) RPC (Remote Procedure Call) – Uzaktan Yordam Çağrısı**

RPC, bir uygulamanın başka bir ağ üzerindeki bilgisayarda bulunan bir fonksiyonu çağırmasını sağlar.

-   Çağrıyı yapan kod, fonksiyonun uzakta olduğunu bilmez ve onu yerelmiş gibi çağırır.
-   RPC genellikle TCP/IP protokolü üzerinden çalışır.

**Örnek RPC Protokolleri:**

-   gRPC (Google RPC) – Protobuf kullanır.
-   XML-RPC – XML tabanlıdır.

----------

### **b) RMI (Remote Method Invocation)**

Java'nın spesifik bir uzaktan çağrı mekanizmasıdır.

-   Java nesnelerinin ağ üzerinden birbirleriyle iletişim kurmasını sağlar.
-   RPC'nin nesne yönelimli versiyonu gibi düşünülebilir.
-   **RMI ile RPC Arasındaki Fark:** RMI, sadece Java nesneleriyle çalışır, RPC ise dil bağımsız olabilir.

----------

### **c) IPC (Inter-Process Communication) – Süreçler Arası İletişim**

Aynı makinedeki farklı süreçlerin birbiriyle iletişim kurmasını sağlayan yöntemdir.

**IPC Yöntemleri:**

-   **Shared Memory (Paylaşılan Bellek):** İki süreç ortak bellek alanı kullanarak veri paylaşır.
-   **Message Queue (Mesaj Kuyruğu):** Bir süreç diğerine mesaj bırakabilir.
-   **Pipes (Boru Hattı):** Bir süreçten diğerine veri akışı sağlar.
-   **Sockets:** Ağ üzerinden veya aynı makinedeki süreçler arasında veri aktarımı sağlar.

----------

## **3. Web Service (Web Servisleri)**

Web servisleri, uygulamaların internet üzerinden birbirleriyle iletişim kurmasını sağlayan teknolojilerdir.

-   HTTP, XML, JSON gibi standartlar kullanılır.
-   Genellikle **SOAP** veya **REST** tabanlı olur.

----------

## **4. SOAP (Simple Object Access Protocol) ve XML**

SOAP, XML tabanlı bir web servisi iletişim protokolüdür.

-   **Özellikleri:**
    -   **Dil ve platform bağımsızdır.**
    -   **XML formatında veri taşır.**
    -   **WSDL (Web Services Description Language)** kullanarak servisleri tanımlar.
    -   **HTTP, SMTP, JMS gibi farklı protokoller üzerinde çalışabilir.**

### **SOAP Mesaj Yapısı:**

SOAP mesajları şu bölümlerden oluşur:

1.  **Envelope (Zarf):** Mesajın başlangıcını belirler.
2.  **Header (Başlık):** Güvenlik, kimlik doğrulama gibi meta veriler içerir.
3.  **Body (Gövde):** Gerçek veriyi taşır.
4.  **Fault (Hata Mesajı):** Hata olması durumunda geri döndürülen bilgi.

### **SOAP Avantajları**

-   **Standartlaştırılmış ve güvenli** (WS-Security ile).
-   **Çeşitli protokollerle çalışabilir.**
-   **Güvenilir mesajlaşma desteği var** (örneğin, ATM gibi kritik sistemlerde kullanılır).

### **SOAP Dezavantajları**

-   **Daha yavaşdır**, çünkü XML kullanır.
-   **Daha karmaşıktır**, REST’e göre daha fazla yapı gerektirir.

----------

## **5. REST (Representational State Transfer)**

REST, HTTP protokolü üzerinden çalışan hafif bir web servis mimarisidir.

-   **JSON veya XML formatında** veri taşır.
-   **Stateless (Durumsuzdur):** Her istek bağımsızdır, sunucuda durum saklanmaz.

### **RESTful API İlkeleri**

-   **Client-Server Mimari:** İstemci ve sunucu birbirinden bağımsız çalışır.
-   **Stateless:** Her istek tüm veriyi içermelidir.
-   **Cacheable (Önbelleklenebilir):** Cevaplar önbelleğe alınabilir.
-   **Layered System (Katmanlı Mimari):** Farklı bileşenler katmanlı bir yapıdadır.


### **REST Avantajları**

-   **Hafif (Lightweight):** JSON ile daha az veri taşınır.
-   **Daha hızlıdır:** SOAP’a kıyasla daha basittir.
-   **Kolay anlaşılır ve uygulanabilir.**

### **REST Dezavantajları**

-   **Güvenlik:** SOAP’a kıyasla daha az güvenlik mekanizması vardır.
-   **Standart eksikliği:** REST’in belirli bir standart tanımı yoktur, farklı uygulamalar farklı yaklaşımlar kullanabilir.


----------

## 6. RPC (Remote Procedure Call) Nedir?

**RPC (Remote Procedure Call - Uzak Prosedür Çağrısı)**, bir programın, ağ üzerinden başka bir bilgisayarda bulunan bir prosedürü (fonksiyonu) çağırmasını sağlayan bir protokoldür. RPC, yerel bir fonksiyon çağrısı gibi çalışır ancak çağrılan fonksiyon, uzak bir sunucuda çalışır.

**Temel Özellikleri:**

-   Uzak bir sistemdeki prosedürü çağırırken, geliştirici bunu yerel bir fonksiyon çağırıyormuş gibi kullanır.
-   Ağ iletişimini soyutladığı için geliştiricinin düşük seviyeli soket programlama ile uğraşmasına gerek kalmaz.
-   Genellikle **senkron** çalışır, yani istemci, çağrının tamamlanmasını bekler.
-   XML-RPC, JSON-RPC, SOAP gibi farklı RPC türleri vardır.

----------

### gRPC Nedir?

**gRPC (Google Remote Procedure Call)**, Google tarafından geliştirilen modern, açık kaynaklı bir **RPC** çerçevesidir. **Protobuf (Protocol Buffers)** serileştirme formatını kullanarak hızlı ve verimli veri iletimi sağlar.

**Özellikleri:**

-   **Performanslıdır:** **Protobuf**, JSON veya XML'e kıyasla daha hızlı ve hafif bir serileştirme sağlar.
-   **Çoklu Dil Desteği:** C++, Java, Python, Go, Node.js, .NET gibi birçok dili destekler.
-   **Çift Yönlü İletişim:** **Streaming (Akış)** desteği ile istemci-sunucu arasında çift yönlü veri akışı sağlar.
-   **Güvenli ve Hızlı:** TLS (Transport Layer Security) desteğiyle güvenli veri aktarımı sunar.
-   **HTTP/2 Kullanır:** HTTP/2 üzerinden çalışarak daha iyi bağlantı yönetimi ve performans sunar.

gRPC’ye olan ihtiyaç, geleneksel **RPC mekanizmalarının eksiklikleri ve modern mikroservis mimarisinin gereksinimleri** nedeniyle doğmuştur.



# **Mikroservis Kavramı**

Mikroservis mimarisi (Microservices Architecture), büyük ve karmaşık yazılım sistemlerini küçük, bağımsız ve birbiriyle gevşek bağlı servisler halinde geliştirme yaklaşımıdır. Her mikroservis belirli bir işlevi yerine getirir ve kendi veritabanına, mantığına ve bağımsız dağıtım süreçlerine sahip olabilir.

----------

### **Neden Mikroservis Mimarisine İhtiyaç Duyuldu?**

Geleneksel monolitik mimarilerde, tüm uygulama tek bir kod tabanı içinde geliştirilir ve dağıtılır. Ancak büyük ölçekli sistemlerde bu yaklaşım şu gibi sorunlara yol açar:

-   **Bakım ve Güncelleme Zorlukları:** Küçük bir değişiklik bile tüm uygulamanın tekrar dağıtılmasını gerektirir.
-   **Ölçeklenebilirlik Problemleri:** Uygulamanın sadece belirli bir bölümü yoğun kullanıldığında bile tüm sistemin ölçeklenmesi gerekir.
-   **Teknolojik Kısıtlar:** Monolitik yapı, farklı teknolojilerin aynı projede esnek şekilde kullanılmasını zorlaştırır.
-   **Ekiplerin Bağımsız Çalışamaması:** Büyük ekipler aynı kod tabanı üzerinde çalışırken çakışmalar yaşanır.

Bu nedenlerden dolayı, özellikle büyük ölçekli ve sürekli büyüyen projelerde mikroservis mimarisi tercih edilmeye başlanmıştır.

----------

### **Mikroservis Mimarisi Avantajları**

✅ **Bağımsız Geliştirme ve Dağıtım:**  
Her mikroservis bağımsız olarak geliştirilebilir, test edilebilir ve dağıtılabilir.

✅ **Kolay Ölçeklenebilirlik:**  
Sadece ihtiyaç duyulan servisler ölçeklendirilebilir, kaynak kullanımı optimize edilir.

✅ **Farklı Teknolojilerin Kullanımı:**  
Her servis ihtiyacına uygun programlama dili, veri tabanı ve çerçeve (framework) kullanabilir.

✅ **Hata İzolasyonu:**  
Bir servis hata verdiğinde tüm sistem yerine sadece o servis etkilenir.

✅ **Ekiplerin Verimli Çalışması:**  
Büyük ekipler, farklı mikroservisleri bağımsız olarak geliştirebilir ve yönetebilir.

✅ **Güncellenebilirlik ve Esneklik:**  
Servisler kolayca güncellenebilir ve yeni özellikler eklenebilir.

----------

### **Mikroservis Mimarisi Dezavantajları**

❌ **Dağıtık Sistem Karmaşıklığı:**  
Birden fazla servis arasında iletişim kurmak, hata yönetimini zorlaştırabilir.

❌ **Ağ İletişimi ve Performans Sorunları:**  
Servisler genellikle HTTP, gRPC veya mesaj kuyruğu üzerinden haberleşir. Ağ gecikmeleri performansı etkileyebilir.

❌ **Veri Tutarsızlığı Riski:**  
Her servis kendi veritabanını kullanabilir, bu da tutarlılığı sağlamak için ek çözümler (event sourcing, saga pattern) gerektirebilir.

❌ **Gelişmiş DevOps ve İzleme Gereksinimi:**  
Servislerin sağlıklı çalıştığından emin olmak için merkezi logging, monitoring ve otomasyon araçlarına ihtiyaç vardır.

❌ **Dağıtık Kimlik ve Yetkilendirme Yönetimi:**  
Kullanıcı kimlik doğrulama (authentication) ve yetkilendirme (authorization) sistemleri daha karmaşık hale gelir.



## **JSON Şema**

JSON Şema, JSON (JavaScript Object Notation) formatındaki verilerin yapısını tanımlamak ve doğrulamak için kullanılan bir şema tanımlama dilidir. JSON nesnelerinin veri türlerini, gerekli alanlarını, varsayılan değerlerini ve diğer kısıtlamalarını belirlemek için kullanılır.

### **JSON Şema’ya Neden İhtiyaç Duyulmuştur?**

JSON, hafif ve esnek bir veri formatıdır ancak bu esneklik bazen hatalara yol açabilir. JSON Şema, aşağıdaki nedenlerle geliştirilmiştir:

1.  **Veri Doğrulama (Validation)**
    
    -   JSON belgelerinin belirli bir yapıya uygun olup olmadığını kontrol etmek için kullanılır.
    -   Örneğin, bir API’nin istemciden beklediği JSON verisinin doğru olup olmadığını doğrulayabilir.
2.  **Veri Tutarlılığı (Consistency)**
    
    -   JSON nesnelerinin belirli bir formatta olmasını sağlayarak veri tutarsızlıklarını önler.
    -   Örneğin, `age` alanı sayısal bir değer olmalı ve negatif olmamalıdır gibi kurallar koyabilirsiniz.
3.  **Dokümantasyon ve Anlaşılabilirlik**
    
    -   JSON veri yapısının nasıl olması gerektiğini açık bir şekilde tanımlar.
    -   JSON tabanlı API'ler geliştiren ekipler arasında ortak bir anlayış sağlar.
4.  **Otomatik Veri Üretimi ve API Sözleşmesi (Contract Enforcement)**
    
    -   Şemaya uygun olarak otomatik test verileri oluşturulabilir.
    -   API tüketicilerinin hangi alanları bekleyebileceğini ve hangi veri türlerini kullanması gerektiğini belirler.
5.  **Kod Üretimi ve Otomasyon**
    
    -   JSON Şema'dan otomatik olarak kod üretmek mümkündür.
    -   Örneğin, bir JSON Şema'dan TypeScript veya Java sınıfları oluşturulabilir.

## **YAML Nedir?**

YAML (YAML Ain’t Markup Language), insan tarafından kolayca okunabilen ve yazılabilen bir veri serileştirme formatıdır. JSON ve XML gibi yapılandırılmış verileri saklamak ve iletmek için kullanılır. Ancak, özellikle yapılandırma dosyaları için daha okunaklı ve esnek olmasıyla öne çıkar.

----------

## **YAML'a Neden İhtiyaç Duyuldu?**

JSON ve XML gibi formatlar zaten mevcutken YAML'nin ortaya çıkmasının temel nedenleri şunlardır:

### **1. Daha Okunaklı ve Kolay Kullanılabilir Olması**

YAML, insan tarafından daha kolay okunabilmesi için geliştirilmiştir. Örneğin, JSON’daki `{}`, `[]` ve `"` gibi fazladan karakterler YAML’de yer almaz, bu da onu daha az karmaşık hale getirir.

**Örnek: JSON vs YAML**  
🔹 **JSON**


````json
{
  "database": {
    "host": "localhost",
    "port": 5432,
    "username": "admin",
    "password": "secret"
  }
} 
````
🔹 **YAML**



````yaml
database:
  host: localhost
  port: 5432
  username: admin
  password: secret 
````
📌 **Görüldüğü gibi YAML daha az karakter kullanarak daha okunaklı bir yapı sunar.**

### **2. Yapılandırma Dosyaları İçin Daha Uygun Olması**

YAML, genellikle uygulama ve sistem yapılandırma dosyaları için tercih edilir. Kubernetes, Docker Compose, Ansible, GitHub Actions gibi birçok modern araç, yapılandırma dosyaları için YAML kullanır.

Örnek: **Kubernetes Pod Tanımı (YAML Formatında)**

````yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
  containers:
    - name: my-container
      image: nginx
      ports:
        - containerPort: 80

````
📌 JSON ile de aynı şeyi yapabiliriz, ancak JSON’un fazladan süslü parantez `{}` ve tırnak `""` kullanımı daha karmaşık hale getirebilir.

### **3. JSON ve XML'e Göre Daha Esnek Olması**

-   YAML, JSON'un bir üst kümesi olduğundan, JSON verileri doğrudan YAML içinde kullanılabilir.
-   Boşluk duyarlı olduğu için gereksiz semboller içermez.
-   Çok satırlı metinleri ve yorumları destekler.  
    **Örnek: Çok Satırlı Metin**
````yaml
description: |
      Bu bir YAML dosyasıdır.
      Çok satırlı açıklamalar desteklenir.

````

----------

### **4. Yorum Satırlarını Desteklemesi**

JSON, doğrudan yorumları desteklemezken YAML içinde `#` karakteri ile yorum eklenebilir.

````yaml
# Bu bir YAML yorum satırıdır
database:
  host: localhost  # Sunucu adresi

````

----------

### **5. Hiyerarşik Veri Yapılarını Desteklemesi**

YAML girintileme kullanarak iç içe geçmiş veri yapılarını kolayca ifade eder.

````yaml
person:
  name: John Doe
  age: 30
  address:
    city: New York
    zip: 10001

````
Bu yapı JSON ile de oluşturulabilir, ancak daha karmaşık ve uzun olur.

----------

### **ÖZET**

**YAML, özellikle yapılandırma dosyaları için daha okunabilir, esnek ve pratik bir alternatif sunmak amacıyla geliştirilmiştir.**  
📌 **JSON daha sıkı bir yapı ve makineler için optimize edilmişken, YAML daha okunaklı ve insan dostudur.**  
🚀 **Kubernetes, Docker Compose, Ansible, CI/CD araçları gibi birçok modern sistem YAML'yi tercih etmektedir.**





## **API ve ABI Nedir?**

#### **API (Application Programming Interface)**

-   Bir yazılımın, başka bir yazılımla nasıl iletişim kuracağını belirleyen kurallar bütünüdür.
-   API seviyesinde yapılan değişiklikler, mikroservislerin birbirleriyle nasıl etkileşime girdiğini doğrudan etkiler.

Örnek:

-   `GET /users/{id}` gibi bir HTTP endpoint’i olan bir API, kullanıcı bilgilerini döndürebilir.
-   Eğer bu endpoint’in dönüş formatı değişirse veya parametreler farklı hale gelirse, bu **API değişikliği** olur.

#### **ABI (Application Binary Interface)**

-   Derlenmiş (binary) seviyede programların birbirleriyle nasıl çalışacağını belirler.
-   API seviyesinden farklı olarak, burada yazılımın **bellek düzeni (memory layout), fonksiyon imzaları, veri türleri** gibi detaylar önemlidir.
-   ABI değişirse, eskiye uyumlu olmayan durumlar ortaya çıkabilir ve bu durumda derlenmiş kodların tekrar derlenmesi veya güncellenmesi gerekir.

Örnek:

-   Bir kütüphane içinde bir fonksiyonun parametre tipinin değişmesi veya bellek adresinde farklı bir yerde olması **ABI değişikliği** oluşturur.

----------

### 📌 **Neden API / ABI Matrisi Tutulmalı?**

-   Mikroservisler birbirlerinin API ve ABI sürümlerine bağımlı olduğu için, **hangi sürüm hangi diğer sürümle çalışır, hangi sürümde uyumluluk bozulur** gibi bilgileri saklamak gerekir.
-   Özellikle **geriye dönük uyumluluk (backward compatibility)** sorunlarını anlamak için API ve ABI değişikliklerini takip etmek gerekir.

**Örneğin:**


|       Servis         |API Sürümü                        |ABI Sürümü |      Desteklenen API'ler                 | Desteklenen ABI'ler
|----------------|-------------------------------|-----------------------------|-----------------------------|-----------------------------|
|A|`1.2.0`            |`2.1.0`           |`1.0.x - 1.2.x`            |  `2.0.x - 2.1.x`        |
|B|`1.1.0`            |`2.0.0`           |`1.0.x - 1.1.x`            |  `2.0.x`        |



Bu tablo sayesinde:

-   Hangi sürümlerin birbiriyle uyumlu olduğunu görebiliriz.
-   Eğer `A` servisi API sürümünü `1.3.0` yaparsa, `B` ile hala çalışıp çalışmayacağını kontrol edebiliriz.
-   `ABI` seviyesinde değişiklik olup olmadığını takip edebiliriz.

----------

## **SemVer (Semantik Versiyonlama)**

Burada **SemVer (Semantic Versioning)** kullanılarak API ve ABI değişikliklerinin nasıl takip edilmesi gerektiği anlatılmış. SemVer şu şekilde işler:

-   **MAJOR (Büyük değişiklikler) → `1.x.x → 2.0.0`**
    
    -   Geriye dönük uyumsuz bir değişiklik yapılmıştır.
    -   Örneğin, eski endpoint’ler kaldırılmış veya parametre yapıları değiştirilmiş olabilir.
    -   Eğer bir API `v1.0.2` iken `v2.0.0` olursa, bu sürüme geçerken servislerin güncellenmesi gerekebilir (migration/göç).
-   **MINOR (Yeni özellikler) → `1.1.0 → 1.2.0`**
    
    -   Geriye dönük uyumlu yeni bir özellik eklenmiştir.
    -   Örneğin, yeni bir endpoint eklenmiş veya bir opsiyonel parametre eklenmiş olabilir.
    -   Eski sistemler hala çalışmaya devam edebilir.
-   **PATCH / BUGFIX (Küçük düzeltmeler) → `1.0.2 → 1.0.3`**
    
    -   Küçük hata düzeltmeleri ve değişiklikler yapılır.
    -   Geriye dönük uyumluluk korunur.
    -   **"Drop-in replacement" yapılabilir**, yani eski sürümle birebir uyumlu olup hemen değiştirilebilir.

----------

### 📌 **Özet ve Öneriler**

1.  **API ve ABI seviyesinde değişiklikleri takip etmek için bir matris oluşturulmalı.**
2.  **SemVer kullanılarak API ve ABI sürümleri net bir şekilde belirlenmeli.**
3.  **Mikroservisler arasındaki bağımlılıklar, versiyon bazında kontrol edilerek test edilmeli.**
4.  **Major değişikliklerde servislerin uyumluluğu tekrar değerlendirilerek migration planı hazırlanmalı.**
5.  **Patch seviyesindeki değişikliklerin "drop-in replacement" yapılabilir olması sağlanmalı.**

Bu sistem sayesinde, **mikroservislerin birbirleriyle uyumlu olup olmadığı net bir şekilde anlaşılabilir ve yönetilebilir.** 🚀

## Paketleme Sistemleri ve Yönetimi
1.  **Pod İmajları (Docker, Kubernetes)**:
    
    -   **Pod İmajları**, genellikle **Docker** ile oluşturulmuş konteyner imajlarıdır. Kubernetes üzerinde çalışan uygulamalar için bu imajlar kullanılır. Docker imajları, uygulama kodunun ve gerekli bağımlılıkların bir araya getirildiği çalıştırılabilir dosyalardır.
    -   **Helm Chart** ise, Kubernetes için bir paket yöneticisidir ve uygulama dağıtımını daha kolay hale getirir. Helm, bir uygulamanın gereksinimlerini, yapılandırmalarını ve dağıtım ayarlarını içeren bir "chart" paketinde toplayarak Kubernetes'e dağıtılmasını sağlar.
2.  **Helm Chart**:
    
Helm Chart, Kubernetes üzerinde çalışan uygulamaların paketlenmesi, dağıtılması ve yönetilmesini sağlayan bir şablonlama ve paketleme sistemidir. **Helm**, Kubernetes için bir paket yöneticisidir ve **Helm Chart** ise Kubernetes kaynaklarını tanımlayan YAML dosyalarından oluşan bir pakettir.
-   **Tek Tıklama Dağıtım:** Karmaşık Kubernetes uygulamalarını tek komutla yükleyebilirsiniz.
-   **Yeniden Kullanılabilirlik:** Bir kez oluşturulan Helm Chart, farklı ortamlarda (test, staging, production) tekrar tekrar kullanılabilir.
-   **Kolay Güncelleme ve Versiyonlama:** Uygulamanın yeni bir versiyonunu dağıtmak veya geri almak için kolayca sürüm kontrolü yapılabilir.
-   **Konfigürasyon Yönetimi:** Farklı ortamlar için özelleştirilebilir değerler (`values.yaml`) kullanılabilir.
-   **Bağımlılık Yönetimi:** Uygulamanızın ihtiyaç duyduğu diğer servisleri (örneğin, bir veritabanı) bağımlılık olarak belirleyebilirsiniz.

Bir Helm Chart genellikle aşağıdaki dosyalardan oluşur:

		📂 `my-chart/`  
		├── 📄 `Chart.yaml` → Chart hakkında temel bilgiler (isim, sürüm, açıklama vb.)  
		├── 📄 `values.yaml` → Varsayılan konfigürasyon değerleri  
		├── 📂 `templates/` → Kubernetes manifest dosyalarının şablonları (`Deployment`, `Service`, vb.)  
		│ ├── 📄 `deployment.yaml`  
		│ ├── 📄 `service.yaml`  
		│ ├── 📄 `_helpers.tpl` → Şablonlama için yardımcı fonksiyonlar  
		└── 📂 `charts/` → Bağımlı olduğu diğer Helm chart'lar


3.  **RPM/DEB**:
    
    -   **RPM** (Red Hat Package Manager) ve **DEB** (Debian Package), Linux tabanlı sistemlerde kullanılan iki popüler paket formatıdır. RPM, özellikle Red Hat ve türevlerinde, DEB ise Debian ve türevlerinde kullanılır.
    -   Bu paketleme formatları, yazılımın bağımlılıklarını ve yapılandırmalarını içeren dosyaları tek bir paket altında toplar.
4.  **NuGet**:
    
    -   **NuGet**, .NET projelerinde kullanılan bir paket yönetim sistemidir. NuGet, .NET kütüphanelerini yönetmek ve paylaşmak için kullanılır. NuGet paketleri genellikle DLL dosyaları ve diğer bağımlılıkları içerir.
5.  **NPM**:
    
    -   **NPM (Node Package Manager)**, JavaScript ekosisteminde kullanılan bir paket yöneticisidir. Node.js projelerinde bağımlılıkları yönetmek için kullanılır ve uygulama geliştirme için gerekli modülleri içerir.
6.  **Maven**:
    
    -   **Maven**, Java projelerinde kullanılan bir otomasyon ve bağımlılık yönetim aracıdır. Maven, projelerin derlenmesi, bağımlılıkların yönetilmesi ve dağıtılması için kullanılır. Maven, Java ekosisteminde yaygın olarak kullanılır.
7.  **SBOM (Software Bill of Materials)**:
    
    -   **SBOM**, bir yazılımın içeriğinde bulunan tüm bileşenlerin bir listesini içerir. Bu liste, yazılımın güvenliğini ve uyumluluğunu sağlamak için önemli bilgiler sunar. SBOM, yazılımın bileşenlerini takip etmeye, güvenlik açıklarını tespit etmeye ve yazılım lisanslarını yönetmeye yardımcı olur.

Bu araçlar, modern yazılım geliştirme sürecinde uygulama dağıtımı, bağımlılık yönetimi ve güvenlik sağlama açısından kritik bir rol oynar.

## Nexus Nedir?

**Nexus** bir repository manager (depo yöneticisi) olup, yazılım paketlerini merkezi bir depo içinde yönetmenize olanak tanır. Bu, özellikle sürekli entegrasyon (CI) ve sürekli dağıtım (CD) süreçlerinde oldukça önemlidir. Nexus, özellikle Java, npm, Docker, Maven, NuGet gibi farklı paket formatlarını destekler.

Paketleri depolamak ve sürümlemek, özellikle yazılım geliştirme süreçlerinde birden çok avantaj sunar:

1.  **Sürüm Kontrolü ve Yönetimi**: Paketler sürümlendirilerek, her bir sürümün doğru bir şekilde takip edilmesini sağlar. Bu, belirli bir sürümdeki bir hatayı izole etmeyi veya belirli bir versiyonla çalışmayı kolaylaştırır.
    
2.  **Bağımlılık Yönetimi**: Yazılım projelerinde kullanılan kütüphaneler ve paketler, genellikle başka paketlere bağımlıdır. Bu bağımlılıkların belirli sürümleriyle tutarlı bir şekilde çalışmak için, bu paketlerin bir depo içinde tutulması gereklidir.
    
3.  **İzleme ve Güvenlik**: Paketler Nexus gibi bir depoda tutulduğunda, kullanılan paketlerin güvenliği ve güncellemeleri düzenli olarak izlenebilir. Özellikle açık kaynaklı paketlerde güvenlik açıklarının hızlıca tespit edilip uygulanabilir çözümler sağlanabilir.
    
4.  **Performans ve Dağıtım Kolaylığı**: Nexus, paketlerin merkezi bir depoda tutulmasını sağlayarak, ekipler arası dağıtımda kolaylık ve hız sağlar. Ayrıca paketlerin çevrimdışı kullanımı mümkün hale gelir, çünkü her bir proje bağımlılıklarını doğrudan internetten almak yerine yerel bir depodan çekebilir.
    

**npm ile JSON Lock Paket Versiyonlarının Sürümlenmesi**: Bir proje üzerinde çalışırken, kullanılan bağımlılıkların versiyonlarının `package-lock.json` dosyasında yer aldığı görülür. Bu dosya, bağımlılıkların kesin versiyonlarını belirtir. Bu dosyayı bir Nexus gibi bir repo yönetim sistemine yüklemek, projedeki tüm ekip üyelerinin aynı paket sürümünü kullanmalarını ve uyumsuzlukların önüne geçilmesini sağlar.

Bunun dışında, sürüm yönetimi yaparak her bağımlılığın doğru versiyonlarıyla tutarlı bir ortam yaratmak mümkündür.


## Konteyner yönetimi için **de facto** araç setlerinden bazıları

### 1️⃣ **Docker** – Konteynerleştirme

-   Konteyner oluşturma, çalıştırma ve yönetme için en yaygın kullanılan araçtır.
-   Alternatifler: **Podman**, **containerd** (Docker’ın runtime’ı), **CRI-O**.

### 2️⃣ **Kubernetes** – Konteyner Orkestrasyonu

-   Birden fazla konteyneri ölçeklendirme, dağıtma ve yönetme için kullanılır.
-   Alternatifler: **Docker Swarm** (daha az popüler), **Nomad**.

### 3️⃣ **Helm** – Kubernetes Paket Yönetimi

-   Kubernetes üzerinde uygulama dağıtımını kolaylaştırmak için şablonlaştırılmış yapı sağlar.

### 4️⃣ **Container Network Interface (CNI)** – Ağ Yönetimi

-   Kubernetes gibi platformlarda ağ yönetimi için standart bir arayüz sunar.
-   Popüler eklentiler: **Calico**, **Flannel**, **Cilium**, **Weave**.

### 5️⃣ **Container Storage Interface (CSI)** – Depolama Yönetimi

-   Konteynerlerin depolama kaynaklarını dinamik olarak yönetmesini sağlar.
-   Örnekler: **Longhorn**, **Ceph**, **Rook**, **OpenEBS**.

### 6️⃣ **Prometheus & Grafana** – İzleme ve Gözlemlenebilirlik

-   **Prometheus**: Metrik toplama ve uyarı sistemi.
-   **Grafana**: Görselleştirme ve analiz aracı.

### 7️⃣ **K8s & Lens** – Kubernetes Yönetim UI’leri

-   **K8s**: Terminal tabanlı bir Kubernetes yönetim aracı.
-   **Lens**: Kullanıcı dostu Kubernetes GUI.

Bu araçlar konteyner yönetimi için en yaygın kullanılan ve sektörde standart haline gelen bileşenlerdir. **Kubernetes, Helm, Prometheus, Docker gibi araçlar konteyner ekosisteminin omurgasını oluşturur.** 

## Mikroservis Tasarım Desenleri

### 1. **Saga Pattern**

**📌 Amacı:** Dağıtık işlemleri yönetmek ve veri tutarlılığını sağlamak.

### **Problem**

Mikroservislerde klasik ACID (Atomicity, Consistency, Isolation, Durability) işlemleri yerine, her servis kendi verisini yönetir. Ancak, birden fazla servisi kapsayan bir işlem başarısız olursa, tüm sistemin nasıl tepki vereceğini belirlemek gerekir.

![image kaynak linki](https://miro.medium.com/v2/resize:fit:875/1*b4fFk1k5PfPdI3mAC1Rskg.png)

[image kaynak linki](https://miro.medium.com/v2/resize:fit:875/1*b4fFk1k5PfPdI3mAC1Rskg.png)


### **Çözüm**

Saga, uzun süreli işlemleri (long-running transactions) yönetmek için **Choreography** ve **Orchestration** olmak üzere iki yöntem sunar:

*  **Choreography (Otomatik Yönlendirme)**
    
    -   Mikroservisler birbirlerine doğrudan olaylar (event) göndererek iletişim kurar.
    -   Örneğin, bir sipariş alındığında **OrderService** bir "OrderCreated" olayı yayınlar. **PaymentService** bunu dinler ve ödemeyi başlatır.
    -   Eğer bir hata olursa, geri alma işlemleri de eventler üzerinden yapılır.
* **Orchestration (Merkezi Koordinasyon)**
    
    -   Merkezi bir **Saga Orchestrator** tüm süreci yönetir.
    -   Örneğin, bir sipariş oluşturulurken **Saga Orchestrator**, önce ödemeyi başlatır, sonra stoğu günceller ve son olarak kargoyu planlar.
    -   Eğer herhangi bir adımda hata olursa, orchestrator geri alma işlemlerini başlatır.
Saga Pattern'de işlemlerin tarihçesini tutmak, özellikle dağıtık sistemlerdeki işlem akışlarını takip etmek için önemli bir ihtiyaçtır. Bu tür bir tarihçe, her bir adımın başarılı veya başarısız şekilde tamamlandığını kaydederek işlem sırasını ve durum değişikliklerini izlemeyi sağlar. Saga Pattern'in tarihçesi için genellikle şu yöntemler kullanılır:

* **Event Logging (Olay Günlüğü)**
    
    -   Her bir saga adımının başlangıcı ve sonucu (başarı veya başarısızlık) birer olay olarak kaydedilir.
    -   Olaylar, bir event log sistemi aracılığıyla depolanır (örneğin, Kafka, RabbitMQ gibi bir mesaj kuyruğu ya da bir veritabanı tablosu).
    -   Bu olaylar, daha sonra saga işleminin ilerleyişini izlemek için kullanılabilir.
* **Saga State Store (Saga Durum Deposu)**
    
    -   Saga'nın her aşaması, saga'nın durumunu ve adımlarını izleyen bir veri yapısında saklanır. Bu depolama genellikle bir veritabanında yapılır.
    -   Saga'nın başlangıcından itibaren her adım, başarı veya başarısızlık durumu ile birlikte kaydedilir.
    -   Örneğin, bir `SagaState` tablosu, her saga'nın kimliğini, adım numarasını, işlem durumunu, zaman damgasını ve hata mesajlarını tutabilir.
* **Compensation Actions (Tazminat Eylemleri)**
    
    -   Saga'nın adımları başarılı bir şekilde tamamlanmadığında, önceki adımların geri alınması gerekebilir. Bu geri alımlar da bir log olarak tutulur.
    -   Her tazminat eylemi bir event olarak kaydedilir ve önceki adımların nasıl iptal edileceği ile ilgili bilgiler saklanır.
* **Correlations and Unique Identifiers (Korelasyonlar ve Benzersiz Kimlikler)**
    
    -   Her saga işlemi için benzersiz bir kimlik (örneğin, `sagaId`) oluşturulur. Bu kimlik, işlemin her adımını birbirine bağlar.
    -   Her adımda, saga kimliği, zaman damgası ve adım durumu gibi bilgilerin saklanması önemlidir.
* **Audit Logs (Denetim Kayıtları)**
    
    -   Denetim logları, her işlem adımının ne zaman başladığını ve bittiğini, hangi kullanıcı veya sistem bileşeninin bu işlemi başlattığını kaydeder.
    -   Genellikle güvenlik ve uyum gereksinimlerini karşılamak için kullanılır.

Bu yöntemler, Saga Pattern içinde işlemlerin tarihçesini tutarak, hata ayıklama, izleme, ve işlem geri alma gibi işlemlerin daha verimli yapılmasını sağlar.

----------

### 2. **Circuit Breaker Pattern**

**📌 Amacı:** Hata toleransını artırmak, servis bağımlılıklarındaki hataların yayılmasını önlemek.

### **Problem**

Bir mikroservis, çağırdığı başka bir servis yanıt vermediğinde sürekli tekrar deneyerek sistem kaynaklarını tüketebilir. Bu, zincirleme hatalara neden olur.

### **Çözüm**

**Circuit Breaker**, servisler arasında bir sigorta görevi görür ve aşırı yüklenmeyi engeller.

-   **Closed (Kapalı) Durumu** → Tüm istekler normal şekilde çalışır.
-   **Open (Açık) Durumu** → Belirli sayıda hata aldıktan sonra bağlantıyı keser ve alternatif yollar sunar (örneğin cache'den veri döndürmek).
-   **Half-Open (Yarı Açık) Durumu** → Servisin tekrar çalışıp çalışmadığını test eder.

💡 **Kullanım Durumu:** Ödeme servisleri, üçüncü taraf API entegrasyonları.

🔧 **Araçlar:** Netflix Hystrix (deprecated), Resilience4j, Polly (C#).

----------

### 3. **Change Data Capture (CDC)**

**📌 Amacı:** Veritabanındaki değişiklikleri tespit ederek olay temelli mimariyi desteklemek.

### **Problem**

Mikroservislerde bir servisin verisi değiştiğinde diğer servislerin de haberdar olması gerekebilir. Polling yapmak verimsizdir.

### **Çözüm**

CDC, **veritabanı değişikliklerini (insert, update, delete)** dinleyerek bir olay (event) olarak yayınlar.

💡 **Kullanım Durumu:** Veri senkronizasyonu, Event Sourcing, Audit logging.

🔧 **Araçlar:** Debezium, Kafka Connect, Outbox Pattern.

----------

### 4. **Event Sourcing**

**📌 Amacı:** Verileri geleneksel CRUD yerine olaylarla (events) saklamak ve geçmiş işlemleri tekrar oynatabilmek.

### **Problem**

Geleneksel veritabanı modellerinde sadece son durumu saklamak mümkündür. Ancak geçmişte ne olduğu kaybolur.

### **Çözüm**

-   Veritabanında "state" yerine olaylar saklanır.
-   Örneğin, **Bir siparişin oluşturulması, ödenmesi, iptali** gibi tüm olaylar kaydedilir.
-   Bir nesnenin son durumu, bu olayların sıralı işlenmesiyle hesaplanır.

💡 **Kullanım Durumu:** Bankacılık, finans, muhasebe uygulamaları.

🔧 **Araçlar:** EventStore, Kafka, Axon Framework.

----------

### 5. **Streaming**

**📌 Amacı:** Gerçek zamanlı veri işleme ve olay odaklı mimariyi güçlendirmek.

### **Problem**

Geleneksel istek-cevap modeli, yüksek hacimli veri akışlarını yönetmekte zorlanır.

### **Çözüm**

-   **Event-Driven Architecture** ile servisler birbirine bağımlı olmadan veri akışını yönetir.
-   **Stream Processing** araçları verileri anında işler.

💡 **Kullanım Durumu:** Canlı analiz sistemleri, log işleme, IoT veri akışı.

🔧 **Araçlar:** Apache Kafka, Apache Flink, Apache Pulsar.

----------

### 6. **Backend for Frontend (BFF)**

**📌 Amacı:** Mobil, web, IoT gibi farklı istemcilere özel backend servisleri sunmak.

### **Problem**

Tek bir API, farklı istemciler (mobil, web) için fazla veya eksik veri döndürebilir.

### **Çözüm**

-   Her istemci türü için özel bir BFF geliştirilir.
-   Örneğin, **Mobile BFF** mobil uygulama için optimize edilmiş, **Web BFF** web için optimize edilmiş uç noktalar sunar.

💡 **Kullanım Durumu:** Mobil ve web uygulamaları.

🔧 **Araçlar:** GraphQL, Node.js, Apollo Server.

----------

### 7. **API Gateway**

**📌 Amacı:** Mikroservis çağrılarını yönlendirmek, güvenliği sağlamak ve merkezi yönetim sunmak.

### **Problem**

Mikroservislerin doğrudan istemcilere açılması yönetim, güvenlik ve performans sorunları yaratır.

### **Çözüm**

-   API Gateway tüm istekleri karşılar, **load balancing, authentication, caching, rate limiting** gibi işlemleri yönetir.
-   Örneğin, bir kullanıcı giriş yaptığında **API Gateway** isteği doğrular, ilgili mikroservise yönlendirir.

💡 **Kullanım Durumu:** Büyük ölçekli dağıtık sistemler.

🔧 **Araçlar:** Kong, Nginx, Zuul, Traefik.

----------

### 8. **Sidecar Pattern**

**📌 Amacı:** Mikroservislerin çapraz kesişen ihtiyaçlarını yönetmek (logging, monitoring, security).

### **Problem**

Her mikroservis için **güvenlik, logging, service discovery** gibi özellikleri tekrarlamak maliyetlidir.

### **Çözüm**

-   **Sidecar** olarak adlandırılan küçük bir yardımcı konteyner, ana mikroservisin yanında çalışır.
-   Örneğin, **Envoy** servislere gözlemleme ve güvenlik ekler.

💡 **Kullanım Durumu:** Servis mesh yapıları.

🔧 **Araçlar:** Istio, Linkerd, Envoy Proxy.

----------

### 9. **Service Discovery**

**📌 Amacı:** Mikroservislerin dinamik olarak birbirini bulmasını sağlamak.

### **Problem**

Statik IP adresleri kullanarak mikroservislerin birbirini bulması zordur.

### **Çözüm**

-   Merkezi bir **Service Registry** kullanılır.
-   Servisler kaydolur ve diğerleri bunu keşfederek iletişim kurar.

💡 **Kullanım Durumu:** Kubernetes, AWS ECS.

🔧 **Araçlar:** Consul, Eureka, Kubernetes Service Discovery.


### 10 . **2PC (Two-Phase Commit)**

1.  **Temel Amaç**: 2PC, bir dağıtık sistemdeki tüm katılımcıların bir işlemde başarılı olup olmadığını garanti etmek amacıyla kullanılan bir protokoldür. Bu protokol, işlem ya tamamen başarılı olur ya da tamamen başarısız olur.
2.  **Çalışma Prensibi**:
    -   **Hazırlık Aşaması (Phase 1)**: Koordinatör, tüm katılımcılara işlemde yer alıp almadıklarını sorar. Katılımcılar ya "commit" (onay) ya da "abort" (iptal) cevabını verir.
    -   **Karar Aşaması (Phase 2)**: Koordinatör, tüm katılımcılardan onay aldıysa işlem tamamlanır (commit), aksi takdirde işlem geri alınır (abort).
3.  **Güvenilirlik**: 2PC, katılımcıların eşzamanlı olarak tüm işlem adımlarını tamamlamasını bekler. Ancak ağ sorunları veya sistem arızaları durumunda, bazı katılımcılar sistemin durumunu kaybedebilir ve işlem bloke olabilir.
4.  **Ağ Bağlantısı Gereksinimi**: Katılımcıların her zaman birbirleriyle iletişimde olması gerekir, aksi takdirde işlem tamamlanamaz.

## Düzenli İzleme <a id="duzenli-izleme"></a>

###  **1. Monitoring (İzleme) ve Ops Ekibi**

-  **Monitoring**, sistemin **çalışma durumunu ve performansını takip etmek** için kullanılır.

-  **Ops ekibi (Operasyon ekibi)**, sistemin sağlıklı çalışmasını sağlayan ve problemleri izleyen birimdir.

-  Mikroservislerin birbirleriyle nasıl iletişim kurduğunu, hangi servislerin ne kadar süre çalıştığını anlamak için **izleme araçlarına ihtiyaç duyulur**.

----------

###  **2. Loglama ve Trace ID Kullanımı**

-  **Loglar**, sistemin çalışmasını analiz etmek ve hataları tespit etmek için tutulur.

-  Mikroservisler arasında **bir isteğin hangi servislere uğradığını** takip etmek için **Trace ID (veya Request ID)** kullanılmalıdır.

-  Bu sayede, dağıtık sistemlerde **hata ayıklamak ve performansı analiz etmek daha kolay olur**.

-  **Log yönetiminde Elasticsearch ve Greylog gibi araçlar** kullanılabilir.

----------

###  **3. OpenTracing ve OpenMetrics Kullanımı**

-  **OpenTracing**, mikroservisler arasında bir isteğin **hangi servisten geçtiğini ve ne kadar sürede işlendiğini** takip etmek için kullanılır.

-  **OpenMetrics**, sistem performansını izlemek ve **metrikleri** toplamak için kullanılan bir standarttır.

-  Bu veriler **Grafana gibi görselleştirme araçlarıyla analiz edilerek sistemin durumu takip edilebilir**.


## **Kubernetes Nedir?**

Kubernetes (K8s), **konteynerleştirilmiş uygulamaların yönetimini otomatikleştiren** açık kaynaklı bir **orkestrasyon** platformudur. Google tarafından geliştirilmiş ve daha sonra **Cloud Native Computing Foundation (CNCF)** tarafından desteklenmiştir.

**Temel Görevi:**

-   Konteynerleri dağıtmak, ölçeklendirmek, yönetmek ve izlemek.
-   Uygulamalar için güvenilir, ölçeklenebilir ve verimli bir çalışma ortamı sağlamak.

----------

### **Neden Kubernetes’e İhtiyaç Duyuldu?**

Kubernetes'in ortaya çıkma sebebi, geleneksel uygulama dağıtım ve yönetim yöntemlerinin yetersiz kalmasıdır. Kubernetes’e duyulan ihtiyacın başlıca nedenleri:

#### **1. Geleneksel Sunucu Tabanlı Mimari Yetersizdi**

Eskiden uygulamalar fiziksel sunucularda çalıştırılıyordu. Ancak:

-   **Kaynaklar verimli kullanılamıyordu.**
-   **Bir uygulamanın çökmesi tüm sunucuyu etkiliyordu.**

Bu nedenle sanal makineler (VM) ortaya çıktı, ancak VM’ler de ağır kaynak tüketimi nedeniyle yavaş ve maliyetliydi.

#### **2. Konteynerlerin Yönetimi Zorlaştı**

Docker gibi konteyner teknolojileri, uygulamaları izole ve hafif çalıştırmayı sağladı. Ancak büyük sistemlerde:

-   **Konteynerlerin manuel yönetimi zorlaştı.**
-   **Yüzlerce konteynerin ölçeklendirilmesi ve güncellenmesi zor oldu.**

Bu noktada **Kubernetes devreye girerek** tüm bu süreci otomatikleştirdi.

#### **3. Uygulamaların Dinamik Ölçeklenmesi Gerekliydi**

Modern uygulamalarda:

-   Trafik arttığında **otomatik ölçeklenme**,
-   Çöken servislerin **yeniden başlatılması**,
-   Yeni güncellemelerin **sorunsuz dağıtılması** gerekiyor.

Kubernetes, **bu süreçleri otomatik hale getirerek** büyük ölçekli sistemleri yönetmeyi kolaylaştırıyor.

#### **4. Mikroservis Mimarisi ve Dağıtık Sistemlere Geçiş**

-   Geleneksel monolitik uygulamalar yerine **mikroservisler** popüler hale geldi.
-   Mikroservislerin birbirleriyle haberleşmesi, ölçeklenmesi ve yönetilmesi karmaşık hale geldi.
-   Kubernetes, **servis keşfi (Service Discovery), yük dengeleme ve hata toleransı** sağlayarak mikroservislerin kolay yönetilmesini sağladı.

----------

### **Kubernetes Neden Gerekli?**

✅ Konteynerleri **otomatik** yönetir ve ölçeklendirir.  
✅ **Yüksek erişilebilirlik** sağlar.  
✅ **Kaynakları verimli kullanır** ve maliyetleri düşürür.  
✅ **Kesintisiz dağıtım (Rolling Updates, Rollbacks)** yapmayı kolaylaştırır.  
✅ **Mikroservis mimarisini destekler** ve modern bulut sistemlerine uygundur.

## **Cluster Nedir?**

Cluster (küme), birden fazla fiziksel veya sanal makinenin bir araya gelerek tek bir sistem gibi çalışmasını sağlayan bir yapıdır. Kubernetes’te bir **Cluster**, uygulamaları yönetmek için düğümlerden (node) oluşan bir sistemdir.

----------

### **Kubernetes Cluster Yapısı**

Bir Kubernetes Cluster’ı **iki ana bileşenden** oluşur:

1.  **Control Plane (Kontrol Düzlemi)** → Kümenin yönetiminden sorumlu olan bileşenlerdir.
2.  **Worker Nodes (İşçi Düğümler)** → Uygulamaların çalıştığı sunuculardır.

#### **1. Control Plane (Yönetim Katmanı)**

Kubernetes kümesini yönetir ve aşağıdaki bileşenlerden oluşur:

-   **API Server (`kube-apiserver`)** → Tüm Kubernetes isteklerini yöneten ana bileşendir.
-   **Controller Manager (`kube-controller-manager`)** → Kubernetes nesnelerinin istenen durumda olmasını sağlar.
-   **Scheduler (`kube-scheduler`)** → Yeni pod’ları uygun işçi düğümlerine yerleştirir.
-   **etcd** → Cluster'ın tüm verisini saklayan dağıtılmış anahtar-değer veritabanıdır.

#### **2. Worker Nodes (İşçi Düğümler)**

Uygulamaların çalıştığı düğümlerdir. Her worker node aşağıdaki bileşenleri içerir:

-   **Kubelet** → Pod’ları yöneten bir aracı.
-   **Container Runtime (Docker, containerd, CRI-O)** → Pod’ları çalıştıran konteyner motoru.
-   **Kube-Proxy** → Ağ trafiğini yöneten bileşen.

----------

## **etcd Nedir?**

**etcd**, Kubernetes’in **Cluster State**’ini saklayan **dağıtılmış bir anahtar-değer (key-value) veritabanıdır**.  
Kubernetes'te **tüm yapılandırmalar, pod bilgileri, servisler ve küme durumu** gibi kritik veriler etcd içinde tutulur.

#### **etcd'nin Temel Özellikleri:**

-   **Dağıtılmıştir** → Veriyi birden fazla düğümde saklayarak yüksek erişilebilirlik sağlar.
-   **Tutarlıdır** → Birden fazla kopya olmasına rağmen her zaman aynı veriyi döndürür.
-   **Kubernetes İçin Kritiktir** → Eğer etcd çökerse, Kubernetes çalışamaz.

#### **etcd Komutları ve Yönetimi**

etcd'nin mevcut verisini görmek için:
````sh
ETCDCTL_API=3 etcdctl get / --prefix --keys-only
````

etcd yedeği almak için:

````sh
ETCDCTL_API=3 etcdctl snapshot save snapshot.db
````


# Sistem Mimarisinin Yazılım Üzerine Etkisi
* Yüksek Erişilebilirlik
* Felaket Kurtarma
* Lider Seçimi
* Cluster State Durumu
* Distributed Cache Kullanımı
* Veritabanı Değişiklik Yönetimi
* ServiceBus/MessageQueue/DistributedEvent/EventSourcing/EventBus  (MQ implementasyonu)



## Cluster State Durumu
**Cluster State**, bir Kubernetes kümesinin mevcut durumunu ve yapılandırmasını temsil eden, küme içindeki tüm bileşenlerin birbiriyle tutarlı çalışmasını sağlayan kritik bir veritabanıdır. Kubernetes’te **etcd** içinde saklanır ve **kube-apiserver** üzerinden erişilir.

----------

### **Cluster State İçeriği**

Cluster State, aşağıdaki bilgileri içerir:

1.  **Node Durumları** → Kümedeki düğümlerin sağlık durumu, kaynak kullanımı ve rollerini içerir.
2.  **Pod ve Dağıtımlar** → Çalışan pod'ların, replikaların, dağıtımların (Deployment, StatefulSet vs.) ve bunların istenen durumlarının kaydı.
3.  **Ağ Yapılandırması** → Servisler, Ingress, NetworkPolicy gibi ağ bileşenleri.
4.  **Depolama Durumu** → PersistentVolume, PersistentVolumeClaim gibi bileşenlerin durumu.
5.  **RBAC ve Yetkilendirme** → Kullanıcı izinleri ve roller.
6.  **ConfigMap ve Secret’lar** → Uygulama yapılandırmaları ve gizli bilgiler.

----------

### **Cluster State Yönetimi**

-   **kube-controller-manager** → İstenilen ve mevcut durumu karşılaştırarak dengelemeye çalışır.
-   **kube-scheduler** → Pod'ları uygun düğümlere yerleştirerek kümenin istenen durumu korumasına yardımcı olur.

----------

### **Cluster State’i Görüntüleme**

Kubernetes kümesinin mevcut durumunu görmek için aşağıdaki komutlar kullanılabilir:

````sh
kubectl get nodes
kubectl get pods -A
kubectl get deployments
kubectl cluster-info
````
Daha ayrıntılı JSON veya YAML formatında cluster state almak için:

````sh
kubectl get all -o yaml
kubectl get nodes -o json
````

----------

### **Cluster State Problemleri**

Cluster State bozulursa küme düzgün çalışamaz. **etcd kaybı, ağ hataları veya yanlış yapılandırmalar** cluster state’in bozulmasına neden olabilir.  
Bunu önlemek için:

-   **etcd yedekleme** yapılmalı.
-   **kube-apiserver logları** incelenmeli.
-   **kubectl describe ve logs** komutları ile sorunlar araştırılmalı.


### Cluster(Kümeleme)'nin temel 2 nedeni vardır:

### **1. Yüksek Erişilebilirlik (High Availability - HA)**

-   **Maliyet Artışı:** Kümeleme maliyeti 2-3 kat artırabilir, ancak bu genellikle kullanılan teknolojiye ve altyapıya bağlıdır.
-   **Patroni & PostgreSQL:** Patroni, PostgreSQL için **otomatik failover ve yüksek erişilebilirlik yönetimi** sağlar. Ancak tek başına bir replikasyon aracı değildir; Patroni, **etcd veya Consul gibi bir dağıtık anahtar-değer mağazası** ile çalışarak lider seçim sürecini yönetir.
-   **Aktif/Aktif Mimariler:**
    -   **Aktif/Aktif veritabanı yapıları** genellikle **konflikt yönetimi** (conflict resolution) gerektirir. Örneğin, PostgreSQL gibi ilişkisel veritabanlarında **çakışma çözümleme (conflict resolution) zor olabilir**.
    -   Çoğu sistem **Aktif/Pasif** yapıyı kullanır (Örn: Primary-Replica). **MongoDB gibi NoSQL sistemleri** ise Aktif/Aktif mimarilerde daha yaygın kullanılır.
-   **Session Yönetimi:**
    -   **Sticky Session**, her isteği aynı sunucuya yönlendirerek oturum tutarlılığını sağlar. Ancak **yük dengeleme açısından risklidir** çünkü belirli sunuculara aşırı yük binmesine neden olabilir.
    -   **Tercih edilen yöntem:** **Session replication veya merkezi session store (Redis, Memcached, Hazelcast gibi çözümler)** kullanmaktır.
-   **Split-Brain Problemi:**
    -   Kümeleme sistemlerinde **lider seçimi (leader election)** mekanizması önemlidir.
    -   MongoDB’de **Replica Set** lider seçimi için **Raft algoritmasını** kullanır.
    -   **Split-brain senaryosu**, iki veya daha fazla node’un iletişim kopukluğu yaşaması durumunda **hatalı lider seçimi veya veri tutarsızlığı** yaratabilir.

### **2. Performans ve Ölçeklenebilirlik (Load Balancing & Scalability)**

-   **Yük Dengeleme (Load Balancing):**
    -   Genellikle **Round-Robin, Least Connections, IP Hashing gibi algoritmalar** kullanılır.
    -   Sticky Session, **horizontal scaling’i zorlaştırabilir**, bu yüzden **token-based authentication (JWT gibi) veya merkezi cache mekanizmaları** önerilir.
-   **Gerçek Zamanlı İzleme:**
    -   **Prometheus, Grafana, Datadog gibi araçlarla metrikler izlenmeli.**
    -   Yük testi için **Apache JMeter, Locust, k6 gibi araçlar** kullanılarak **belirli senaryolar test edilmelidir**.
-   **Yatay ve Dikey Ölçekleme:**
    -   **Dikey ölçekleme (Vertical Scaling):** Daha güçlü donanım ekleyerek performansı artırır.
    -   **Yatay ölçekleme (Horizontal Scaling):** Daha fazla node ekleyerek sistemin kapasitesini artırır.
    -   Genellikle **Yatay ölçekleme (scale-out) tercih edilir**, çünkü daha esnektir ve maliyet açısından avantajlıdır.
 
## **Distributed Cache Nedir?**

**Distributed Cache (Dağıtılmış Önbellek)**, birden fazla sunucuya dağıtılmış, genellikle bellek tabanlı bir önbellekleme sistemidir. Uygulamaların veriye daha hızlı erişmesini sağlamak için kullanılır ve merkezi bir veritabanına bağımlılığı azaltarak performansı artırır.

----------

### **Özellikleri ve Avantajları**

✅ **Yüksek Performans:** Verilere merkezi bir veritabanından daha hızlı erişim sağlar.  
✅ **Ölçeklenebilirlik:** Yatay olarak ölçeklendirilebilir; yeni düğümler eklenerek kapasite artırılabilir.  
✅ **Düşük Gecikme Süresi:** Bellek tabanlı çalıştığı için disk tabanlı sistemlerden çok daha hızlıdır.  
✅ **Veri Tutarlılığı:** Çoğu sistem, tutarlılığı sağlamak için belirli replikasyon ve güncelleme mekanizmaları sunar.  
✅ **Bağımsız Çalışma:** Tek bir sunucuya bağımlı olmadığından, bir düğümün çökmesi sistemi tamamen devre dışı bırakmaz.

----------

### **Kullanım Senaryoları**

🔹 **Web Uygulamaları:** Kullanıcı oturumlarını veya sık kullanılan verileri saklamak için.  
🔹 **Mikroservis Mimarileri:** Servisler arasında hızlı veri paylaşımı yapmak için.  
🔹 **Big Data ve IoT:** Büyük ölçekli veri işlemlerinde anlık analizler için.  
🔹 **Makine Öğrenimi ve Yapay Zeka:** Model önbellekleme ve ön işleme için.

----------

### **Popüler Distributed Cache Teknolojileri**

🟢 **Redis** – Açık kaynak, in-memory key-value store, genellikle NoSQL veritabanı olarak da kullanılır.  
🔵 **Memcached** – Hafif ve yüksek performanslı bir key-value cache sistemidir.  
🟡 **Hazelcast** – Java tabanlı, dağıtık önbellekleme ve veri paylaşımı sağlar.  
🟣 **Apache Ignite** – Bellek içi veri işleme ve önbellekleme sunar.  
🟠 **NCache** – .NET uygulamaları için geliştirilmiş güçlü bir cache çözümüdür.


## **Felaket Kurtarma Merkezi (FKM)**

#### **Çalışma Prensibi:**

-   FKM süreçleri, **manuel müdahale gerektiren** operasyonlarla yürütülür; otomatikleştirilmiş işlemler genellikle sınırlıdır.
-   **Kriz yönetimi** için özel bir **Kriz Müdahale Ekibi** oluşturulmalıdır. Bu ekip, felaket senaryolarında hızlı ve etkili müdahaleyi sağlar.
-   Kriz yönetim süreci, **önceden tanımlanmış senaryolara** göre planlanmalı ve düzenli olarak **test edilmelidir**.

#### **AutoScaling ve Küme Yönetimi:**

-   **AutoScaling**, sistemin yüküne göre **node ekleyip kaldırma işlemleri** gerçekleştirir, böylece kaynak kullanımı optimize edilir.
-   Küme sağlığının sürekli izlenmesi gerekir. **Health checks** ve izleme araçları kullanılarak, node'ların durumu düzenli olarak kontrol edilmelidir.
-   **Yüksek erişilebilirlik (HA)** gerektiren node'lar için **yüksek riskli durumlarda otomatik failover** yapılabilir. Ancak, bu node'larda **kritik bir arıza** meydana gelirse, sistem **otomatik olarak devre dışı** kalabilir veya yeniden başlatılabilir.
-   **AutoScaling** ve **yük dengeleme** politikaları, **kesinti sürelerini minimize etmek** amacıyla doğru yapılandırılmalıdır.

#### **Genel Hususlar:**

-   **Veri güvenliği** için düzenli **yedekleme (backup)** ve **veri replikasyonu** stratejileri uygulanmalıdır. Bu, veri kaybı durumunda hızlı geri dönüş imkanı sağlar.
-   **Felaket senaryoları** önceden tanımlanmalı ve belirli aralıklarla **geri kazanım (disaster recovery) testleri** yapılmalıdır. Bu testler, sistemin felaket sonrası toparlanma hızını ve doğruluğunu ölçer.
-   **Coğrafi yedeklilik** sağlamak için alternatif **veri merkezleri** veya **bulut tabanlı çözümler** kullanılabilir. Böylece, bir veri merkezinin kapanması durumunda diğer merkezlerden hizmet devam edebilir.
-   **İletişim planı** ve **müdahale planları** net bir şekilde oluşturulmalıdır. Ekiplerin her biri için görev tanımları yapılmalı ve **acil durum iletişim kanalları** belirlenmelidir.

## MQ Implementasyonu
 
MQ (Message Queue) implementasyonu, yazılım uygulamalarında ve sistemlerinde asenkron iletişim sağlamak amacıyla kullanılan bir yöntemdir. MQ, mesajları bir kuyruğa yerleştirip, tüketicilerin bu mesajları alıp işleyebilmesini sağlar. Bu, özellikle mikroservis mimarisi, dağıtık sistemler ve yüksek performans gerektiren uygulamalar için oldukça yaygın bir iletişim modelidir.

MQ implementasyonunun temel bileşenleri şunlardır:

1.  **Mesaj Kuyruğu**: Mesajların sıralandığı ve tüketiciler tarafından alınıp işlendiği yer.
2.  **Üreticiler (Producers)**: Kuyruğa mesaj gönderen uygulamalar veya sistemler.
3.  **Tüketiciler (Consumers)**: Kuyruktaki mesajları alıp işleyen uygulamalar veya sistemler.
4.  **Mesaj**: Üretici tarafından kuyruğa gönderilen veri.

MQ kullanımı şunları sağlar:

-   **Asenkron işlem**: Mesajlar kuyruğa gönderildikten sonra, üretici işlemine devam edebilir; tüketiciler mesajları sırayla işler.
-   **Mesaj dayanıklılığı**: Mesajlar kuyruğa iletildikten sonra kaybolmaz ve sistem çöksede mesajlar yeniden işlenebilir.
-   **Yük dengeleme**: Tüketiciler, kuyruğun yoğunluğuna göre iş yükünü paylaşarak sistemin daha verimli çalışmasını sağlar.
-   **Geçici depolama**: Mesajlar kuyruğa yerleştirildikten sonra belirli bir süre saklanabilir, böylece mesajların kaybolması engellenir.

MQ implementasyonu, örneğin **RabbitMQ**, **Apache Kafka**, **Amazon SQS** gibi popüler mesaj kuyruğu sistemleriyle yapılabilir.


## Kubernetes API'leri

Kubernetes API'leri, Kubernetes kümesini yönetmek ve uygulama dağıtımlarını, hizmetleri ve diğer bileşenleri kontrol etmek için kullanılan bir dizi RESTful API sunar. Bu API'ler, kullanıcıların küme üzerinde işlem yapmasını sağlar. Kubernetes API'lerinin bazı temel bileşenleri:

1.  **Core API (Core Group)**: Bu, Kubernetes'in temel işlevselliğini içerir ve aşağıdaki kaynakları yönetir:
    
    -   Pods
    -   Services
    -   Deployments
    -   Namespaces
    -   ConfigMaps
    -   Secrets
2.  **Apps API (Apps Group)**: Kubernetes'in uygulama düzeyindeki kaynakları içerir ve aşağıdaki bileşenleri içerir:
    
    -   Deployments
    -   StatefulSets
    -   ReplicaSets
    -   DaemonSets
    -   StatefulSets
3.  **Batch API (Batch Group)**: Bu grup, zamanlanmış işler ve işler için API'leri içerir:
    
    -   Jobs
    -   CronJobs
4.  **Networking API (Networking Group)**: Ağ yapılandırması ile ilgili kaynaklar içerir:
    
    -   Ingress
    -   NetworkPolicies
    -   Services
5.  **Storage API (Storage Group)**: Depolama kaynaklarını yönetir:
    
    -   PersistentVolumes (PV)
    -   PersistentVolumeClaims (PVC)
    -   StorageClasses
6.  **RBAC API (RBAC Group)**: Erişim denetimi ve kullanıcı izinlerini yönetir:
    
    -   Roles
    -   RoleBindings
    -   ClusterRoles
    -   ClusterRoleBindings
7.  **Admission Control API**: Kubernetes'e gelen istekleri kontrol etmek için kullanılan API'ler içerir:
    
    -   ValidatingAdmissionPolicies
    -   MutatingAdmissionPolicies
8.  **Custom Resources (CRDs)**: Kullanıcıların Kubernetes'e özgü kaynaklar tanımlamalarına olanak tanır.
    
9.  **Metrics API**: Küme ve pod seviyesinde metrikleri sağlamak için kullanılır.
    

API'lere `kubectl` komut satırı aracı, K8s Dashboard veya doğrudan HTTP istekleriyle erişilebilir. Kubernetes API server'ı, gelen istekleri işler ve küme içindeki kaynakları yönetir.




#  DevOps Yöntemleri

## Custom Resource Definition (CRD) 

**Custom Resource Definition (CRD)**, Kubernetes’e özel kaynaklar eklememizi sağlar. Kubernetes varsayılan olarak Pod, Service, Deployment gibi kaynakları yönetir, ancak bazen uygulamamız için **özel bir kaynak türü** tanımlamak gerekebilir. CRD'nin neden kullanıldığına dair bazı temel sebepler:

----------
## CRD’ye Neden İhtiyaç Duyarız?

### **1. Kubernetes'in Varsayılan Kaynakları Yetersiz Kalabilir**

Kubernetes’in sunduğu **Deployment, StatefulSet, ConfigMap, Secret** gibi varsayılan kaynaklar birçok senaryoya uygundur, ancak **özelleştirilmiş iş mantıkları** için yeterli olmayabilir.

Örneğin:

-   Kubernetes'te **veritabanı oluşturmak** için varsayılan bir kaynak yoktur.
-   **CI/CD işlemlerini yöneten bir kaynak** tanımlamak istiyorsak (örn: `Pipeline` kaynağı), Kubernetes'in mevcut kaynakları bunu doğrudan desteklemez.

CRD sayesinde, **Database**, **Pipeline**, **Backup**, **MonitoringRule** gibi kendi özel kaynaklarımızı oluşturabiliriz.

----------

### **2. Kubernetes İçinde Kendi İş Süreçlerimizi Customize Olarak Tanımlamak (CRD)**

Eğer Kubernetes üzerinde çalışan **özel bir iş akışı** oluşturmak istiyorsak, bunu CRD ile tanımlayabiliriz.

Örneğin:

-   **Kendi Load Balancer'ımızı yönetmek istiyoruz:** Kubernetes’te `Service` ve `Ingress` var, ancak özel bir **API Gateway** yönetmek için `ApiGateway` adında bir CRD tanımlayabiliriz.
-   **Otomatik Yedekleme Yapmak:** Kubernetes’in içinde çalışan veritabanlarımızın **otomatik yedeklenmesini** istiyorsak, bir `BackupPolicy` kaynağı oluşturup, bir operatör ile bunu işletebiliriz.

----------

### **3. Kubernetes Operatörleri ile Özel Kaynakları Yönetmek**

CRD genellikle **Kubernetes Operator** mekanizması ile birlikte kullanılır.  
Bir **operator**, belirli bir CRD’yi izleyerek onun mantığını uygular.

Örneğin:

-   Bir **Database CRD** oluşturduğumuzda, bir **Database Operator** bunu dinleyerek **PostgreSQL veya MySQL sunucusunu otomatik başlatabilir.**
-   Bir **Backup CRD** tanımlarsak, bir operatör bunu okuyarak **otomatik yedekleme işlemi** başlatabilir.

Bu sayede, **kendi Kubernetes-native servislerimizi** yazabiliriz.

----------

### **4. Kubernetes API'sini Genişletmek**

CRD'ler Kubernetes API’sini genişletir ve Kubernetes’e **kendi özel API kaynaklarımızı ekleyebiliriz.**

Örneğin: Varsayılan Kubernetes API çağrıları şu şekilde olur:


````sh
kubectl get pods
kubectl get services
````


Ancak bir `Database` CRD oluşturduğumuzda, artık şu komutları da çalıştırabiliriz:

````sh
kubectl get databases
kubectl describe database my-db
kubectl delete database my-db
````



Bu, Kubernetes'i **daha genişletilebilir bir platform** haline getirir.

----------

### **5. Multi-Tenancy ve Özel Konfigürasyonlar**

Büyük organizasyonlarda farklı takımların farklı konfigürasyonlara sahip olması gerekebilir. CRD sayesinde:

-   **Takım bazlı özel kaynaklar** tanımlayabiliriz.
-   Örneğin, her takım için **Quota, Policy, Role** gibi özel kaynaklar oluşturabiliriz.
-   Böylece merkezi yönetim daha kolay olur.

----------

### **Özetle CRD Kullanmanın Avantajları**

✅ **Kubernetes API’sini genişletir** – Özel kaynaklar ekleyebiliriz.  
✅ **Otomasyon sağlar** – Operator ile özel kaynakları yönetebiliriz.  
✅ **Özel iş süreçlerini tanımlamamıza izin verir** – Veritabanı, yedekleme, pipeline gibi süreçleri Kubernetes’e entegre edebiliriz.  
✅ **Büyük ölçekli sistemlerde yönetimi kolaylaştırır** – Multi-tenancy için uygundur.

## Operator Nedir?

Kubernetes **Operator**, belirli bir uygulamayı veya sistemi **otomatik olarak yönetmek** için kullanılan bir bileşendir.  
Bir **Custom Resource Definition (CRD)** ile birlikte çalışarak **özel kaynakları izler ve yönetir**.

Bir Operator, Kubernetes **Controller mantığını** genişleterek, bir uygulamanın **yaşam döngüsünü** (deploy, update, backup, ölçekleme vb.) otomatik hale getirir.

----------

### **🎯 Operator Ne İşe Yarar?**

Operator’lar Kubernetes’in **yerel kaynaklarını** (Pod, Service, Deployment vb.) yönetebildiği gibi, **karmaşık uygulamaları** da yönetmek için kullanılır.

**Örneğin:** ✅ **Veritabanı yönetimi:** PostgreSQL veya MongoDB gibi veritabanlarını otomatik olarak kurup yönetebilir.  
✅ **Backup & Restore:** Belirli bir uygulamanın yedeklerini belirlenen zamanlarda alabilir.  
✅ **Ölçekleme (Scaling):** Trafiğe bağlı olarak bir uygulamayı **otomatik olarak yatay veya dikey ölçeklendirebilir.**  
✅ **Uygulama Güncelleme:** Yeni sürüm çıktığında **sıfır kesintiyle güncelleme** yapabilir.

Operator’lar, Kubernetes’in **self-healing (kendini iyileştirme) özelliğini genişleterek**, belirli bir uygulama için özel işlemler tanımlamamızı sağlar.

----------

### **🔧 Operator Nasıl Çalışır?**

Bir Operator, Kubernetes’in **Reconciliation (Uzlaşma) Mekanizması** ile çalışır.  
Yani, sürekli olarak **mevcut durumu (current state) izler** ve istenen duruma (desired state) uygun olup olmadığını kontrol eder.

1️⃣ **CRD Tanımlanır:** Önce özel bir **Custom Resource Definition (CRD)** oluşturulur.  
2️⃣ **Operator, CR’leri İzler:** Operator, bu CRD’ye dayalı olarak oluşturulan tüm **Custom Resource (CR)** nesnelerini takip eder.  
3️⃣ **İstenen Durumu Kontrol Eder:** Kubernetes içindeki mevcut durumu inceler.  
4️⃣ **Gerekirse Müdahale Eder:** Eğer mevcut durum ile istenen durum arasında fark varsa, Operator gerekli işlemleri yapar.

----------

### **🛠 Örnek: PostgreSQL Operator**

Örneğin, bir PostgreSQL veritabanını yöneten bir Operator oluşturduğumuzu düşünelim.

#### **1️⃣ İlk olarak bir CRD tanımlayalım:**

````yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: databases.example.com
spec:
  group: example.com
  scope: Namespaced
  names:
    plural: databases
    singular: database
    kind: Database
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                engine:
                  type: string
                  enum:
                    - postgresql
                    - mysql
                version:
                  type: string
                storageSize:
                  type: string
````


#### **2️⃣ Sonra bir `Database` nesnesi oluştururuz:**

````yaml
apiVersion: example.com/v1
kind: Database
metadata:
  name: my-postgres-db
spec:
  engine: postgresql
  version: "15.2"
  storageSize: "10Gi"
````

Bu YAML dosyası uygulandığında Kubernetes’te `my-postgres-db` adlı bir özel kaynak (CR) oluşur.

#### **3️⃣ Operator, `Database` CR’lerini sürekli izler**

Eğer bu **PostgreSQL pod’u çökerse**, Operator bunu fark eder ve **otomatik olarak yeniden başlatır**.

**Ek olarak:**

-   Eğer `storageSize: "20Gi"` olarak güncellenirse, Operator **PVC’yi büyütebilir**.
-   `version: "16.0"` olarak değiştirilirse, **PostgreSQL upgrade işlemini başlatabilir**.

Böylece **operatör, manuel müdahale gerektirmeden veritabanını yönetir!** 🎯

----------

### **🚀 Operator Kullanmanın Avantajları**

✅ **Otomasyon Sağlar** – Kubernetes kaynaklarını otomatik olarak yönetir.  
✅ **İnsan Müdahalesini Azaltır** – Operatörler **backup, upgrade, ölçekleme** gibi işlemleri kendileri yapabilir.  
✅ **Kubernetes ile Uyumlu Çalışır** – CRD kullanarak, Kubernetes API’sine **native olarak entegre edilir.**  
✅ **Self-Healing Mekanizmasını Geliştirir** – Eğer bir bileşen çökerse, Operator **otomatik onarım yapabilir.**

----------

### **📌 Popüler Operator Örnekleri**

-   **[PostgreSQL Operator](https://github.com/zalando/postgres-operator)** → PostgreSQL için otomatik yönetim.
-   **[MongoDB Operator](https://github.com/mongodb/mongodb-kubernetes-operator)** → MongoDB kümesini Kubernetes içinde yönetmek için.
-   **[Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator)** → Prometheus monitoring için Kubernetes-native yönetim sağlar.
-   **Cert-Manager** → Kubernetes’te otomatik TLS sertifika yönetimi için kullanılır.
-   **Istio Operator** → Kubernetes Service Mesh yönetimi için.

----------

## GitOPS Nedir?

GitOps, tüm altyapı ve uygulama yönetiminin Git ile sağlandığı bir yaklaşımdır. Sürekli Entegrasyon ve Sürekli Dağıtım (CI/CD) süreçleriyle bağlantılı olarak, GitOps şu prensiplere dayanır:

-   **Versiyon Kontrolü:** Tüm yapılandırmalar Git üzerinde tutulur.
    
-   **Otomatik Senkronizasyon:** Sistem durumu, Git deposundaki durumla sürekli uyumlu tutulur.
    
-   **Gözlemlenebilirlik:** Git, altyapı değişiklikleri için tek bilgi kaynağı (Single Source of Truth) olarak kullanılır.
    

GitOps araçları arasında ArgoCD, Flux ve Jenkins X gibi popüler çözümler bulunmaktadır.

## **DevSecOps Nedir?**

**DevSecOps**, **Development (Geliştirme)**, **Security (Güvenlik)** ve **Operations (Operasyonlar)** kelimelerinin birleşiminden oluşan bir yaklaşımdır. Geleneksel DevOps modeline **güvenliği entegre** ederek, güvenliği yazılım geliştirme sürecinin başından itibaren ele almayı hedefler.

#### **DevSecOps’un Temel İlkeleri:**

-   **Shift-Left Security:** Güvenlik testlerini geliştirme sürecinin en erken aşamalarına taşımak
-   **Otomasyon:** Güvenlik analizlerini CI/CD süreçlerine entegre etmek
-   **Sürekli İzleme:** Güvenlik tehditlerini proaktif olarak tespit etmek
-   **İş Birliği:** Geliştiriciler, güvenlik ekipleri ve operasyon ekipleri arasındaki iletişimi artırmak

#### **DevSecOps Araçları:**

-   **Güvenlik Analizi:** SonarQube, Snyk, Checkmarx
-   **Container Security:** Aqua Security, Trivy
-   **Otomatik Test:** OWASP ZAP, Burp Suite
-   **CI/CD Entegrasyonu:** GitHub Actions, GitLab CI, Jenkins, ArgoCD

----------

## **KNative Nedir?**

**KNative**, Kubernetes üzerinde **serverless (sunucusuz)** uygulamalar ve event-driven (olay güdümlü) iş yükleri çalıştırmak için geliştirilmiş açık kaynaklı bir framework’tür.

#### **KNative’in Bileşenleri:**

1.  **Knative Serving**
    
    -   Serverless uygulamaların ölçeklenmesini ve deployment’ını yönetir.
    -   HTTP isteklerine göre otomatik ölçeklenme sağlar (scale to zero).
2.  **Knative Eventing**
    
    -   Mikroservisler ve event-driven iş yükleri arasında mesaj alışverişini yönetir.
    -   Kafka, Google Pub/Sub gibi sistemlerle entegre olabilir.
3.  **Knative Functions**
    
    -   Serverless fonksiyonları kolayca çalıştırmaya olanak tanır.
    -   AWS Lambda gibi FaaS (Function as a Service) mantığında çalışır.

#### **KNative’in Avantajları:**

✅ **Kubernetes Native:** Kubernetes ekosistemine tam uyumludur.  
✅ **Otomatik Ölçeklenme:** Kullanılmadığında sıfır kaynağa düşebilir.  
✅ **Esnek Çalışma Modeli:** REST API, gRPC ve event-driven mimarilerle uyumludur.  
✅ **Çeşitli Bulut Destekleri:** Google Cloud Run, AWS EKS, Azure Kubernetes Service ile uyumludur.

Eğer **serverless mimariyi Kubernetes üzerinde yönetmek** istiyorsan, **Knative** iyi bir seçenek olabilir.

## Git Komutları ve Kullanımı

## 1. Sürümlendirme Numaraları ve Commit İşlemleri

### 1.1. Commit Tagleme

Git'te belirli bir commit'e versiyon etiketi eklemek için aşağıdaki komut kullanılır:

```
 git tag -a v1.0.1 -m "Version 1.0.1"
```

Bu komut, `v1.0.1` etiketiyle commit'i işaretler. Daha sonra bu etiketi remote repository'ye göndermek için şu komut kullanılır:

```
 git push origin v1.0.1
```

### 1.2. Git Akış Modelleri (Git Flow & Mainline)

-   **Git Flow:** Feature, develop, release ve hotfix branch'leri kullanılarak geliştirme yapılan bir modeldir.
    
-   **Mainline (Trunk-Based Development):** Ana branch üzerinden sürekli entegrasyon sağlanan bir geliştirme modelidir.
    

### 1.3. Başka Bir Branch’ten Commit Çekme (Cherry Pick)

Belirli bir commit’i başka bir branch'e almak için kullanılır:

```
 git checkout hedef-branch
 git cherry-pick <commit-hash>
```

Bu komut, belirli bir commit’i mevcut branch'e uygular.

### 1.4. Conventional Commits

Commit mesajlarını belirli bir formatta yazmayı sağlayan bir konvansiyondur:

```
 git commit -m "feat: yeni bir özellik eklendi"
 git commit -m "fix: hata düzeltildi"
```

### 1.5. Git Squash

Birden fazla commit’i tek bir commit haline getirmek için kullanılır:

```
 git rebase -i HEAD~3
```

Açılan ekranda ilgili commit’leri `squash` veya `fixup` olarak işaretleyerek birleştirebilirsiniz.

### 1.6. Commit Mesajlarına Referans Eklemek

Bir commit mesajında bir Jira numarası veya referans belirtmek için:

```
 git commit -m "fix: hata düzeltildi (Refs: #123)"
```

### 1.7. Commit Yapan ve Yazarı Ayrı Kişiler Olabilir

Commit yaparken farklı bir yazar belirtmek için:

```
 git commit --author="İsim <email@example.com>"
```

## 2. Versiyon Yayınlama

Versiyon çıkarken bir örnek veya şablon oluşturulabilir:

```
 echo "Sürüm Notları v1.0.1" > RELEASE_NOTES.md
 git add RELEASE_NOTES.md
 git commit -m "docs: v1.0.1 sürüm notları eklendi"
```

## 3. Belgelendirme

ASCII Art veya README dokümantasyonunda single sourcing kullanılabilir:

```
 echo "(\_/)
( •_•)
>  ( ASCII Doge )" > README.md
```

## **Jenkins Nedir?**

Jenkins, açık kaynaklı bir **otomasyon sunucusudur** ve yazılım geliştirme süreçlerini **CI/CD (Continuous Integration / Continuous Deployment)** prensiplerine uygun olarak otomatikleştirmek için kullanılır. Java tabanlıdır ve geniş eklenti desteği sayesinde farklı teknolojilere entegre edilebilir.

### **Jenkins Ne İşe Yarar?**

Jenkins, yazılım geliştirme sürecinde manuel işlemleri otomatikleştirerek **hata oranını azaltır, geliştiricilerin verimliliğini artırır ve teslimat sürecini hızlandırır**. Başlıca kullanım alanları şunlardır:

✅ **Sürekli Entegrasyon (CI - Continuous Integration)**

-   Geliştiriciler kodlarını bir **merkezi depoya (Git, SVN, vb.)** gönderdiğinde, Jenkins otomatik olarak derleme, test ve analiz işlemlerini başlatır.
-   Entegre edilen kodlar **sorunsuz çalışıyor mu?** Kontrol edilerek hatalar erken tespit edilir.

✅ **Sürekli Dağıtım (CD - Continuous Deployment/Delivery)**

-   Başarılı geçen testlerden sonra uygulamayı otomatik olarak **test veya canlı ortama (Production, Staging)** dağıtabilir.
-   Kubernetes, Docker, AWS, Azure, Google Cloud gibi ortamlara entegrasyon sağlanabilir.

✅ **Otomasyon İşlemleri**

-   Yazılım projelerinin **derleme (build)**, **test**, **kod analizi**, **deployment** gibi adımlarını belirli kurallar çerçevesinde **otomatik olarak çalıştırabilir**.
-   Örneğin, belirli zamanlarda yedekleme yapabilir veya günlük raporlar oluşturabilir.

### **Jenkins'in Temel Özellikleri**

-   **Açık kaynak ve ücretsizdir.**
-   **Eklenti (plugin) desteği geniştir.** (Docker, Kubernetes, Git, SonarQube vb. ile kolayca entegre olabilir.)
-   **Web tabanlı arayüzü vardır.**
-   **Pipeline (Boru Hattı) Sistemi** ile süreçleri tanımlayıp yönetebilir.
-   **Çoklu platform desteği vardır.** (Windows, Linux, macOS üzerinde çalışabilir.)

### **Jenkins Nasıl Çalışır?**

1.  **Geliştirici kodu bir Git deposuna (örneğin GitHub veya GitLab) gönderir.**
2.  **Jenkins, belirlenen pipeline’a göre bu değişikliği algılar.**
3.  **Kodun derlenmesini, test edilmesini ve analiz edilmesini otomatik olarak başlatır.**
4.  **Hatalar varsa rapor oluşturur, eğer her şey başarılıysa dağıtım yapar.**
5.  **Sonuçları geliştiricilere e-posta veya bildirim yoluyla iletir.**

### **Jenkins Kullanım Senaryoları**

-   **Kod değişikliklerini sürekli olarak test etmek için**
-   **Uygulamaları test ve üretim ortamına otomatik dağıtmak için**
-   **Docker ve Kubernetes ile konteyner tabanlı dağıtımları yönetmek için**
-   **Veritabanı yedekleme ve bakım işlemlerini zamanlayarak otomatik hale getirmek için**

### **Jenkins’in Git Commit’lerini Görmesi için Gerekenler:**

1.  **Jenkins’in Git Deposu ile Bağlantısı Olmalı**
    
    -   Jenkins’e **Git Plugin** kurulmalı.
    -   Jenkins’e Git repo URL’si eklenmeli.
    -   Gerekirse kimlik doğrulama için **SSH Key** veya **Access Token** tanımlanmalı.
2.  **Jenkins Pipeline veya Freestyle Job ile Git’ten Kod Çekilmeli**
    
    -   **Freestyle Job:** Kaynak kod yönetimi olarak Git seçilir.
    -   **Pipeline:** `checkout scm` veya `git` komutları ile repo çekilir.
3.  **Jenkins Git Commit’lerini Build İçinde Kullanabilir**
    
    -   **Son commit mesajı veya hash’ini görmek için:**
        
        
        `git log -1 --pretty=%B` 
        
    -   **Belirli bir branch’teki commit’leri görmek için:**
        
        
        `git log origin/main --oneline` 
        

### 📌 **Jenkins, Commit’lerden Otomatik Build Tetikleyebilir mi?**

Evet! **Git Webhook** kullanarak, Git’e commit attığınızda **Jenkins otomatik olarak build başlatabilir**. Bunun için:

1.  Git tarafında bir **Webhook** eklenmeli (örneğin, GitHub, GitLab veya Bitbucket).
2.  Jenkins’de **"Git hook trigger for GITScm polling"** seçeneği etkinleştirilmeli.
