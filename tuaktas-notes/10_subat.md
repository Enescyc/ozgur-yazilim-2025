# Mikroservisler ve DevOps Eğitim Kampı Notları

## Özgür Yazılım
- Satılabilir, istenildiği gibi kullanılabilir ancak bedava değildir.
- Kaynak kodları açıktır, güncellenebilir.
- Farklı lisanslarla yapılan değişiklikler, aynı lisans altında herkese açık olmalıdır.
- Kullanılan kodların lisans uyumluluğuna dikkat edilmelidir.
- Topluluk güçlü ise yazılımın devamlılığı sağlanır.
- Büyük şirketler kâr edebileceğine inanırsa projeye yatırım yapar.
- Topluluk ilgisini kaybederse proje sahipsiz kalır ve ölür.
- Bir ürünün topluluğunun olması, o projenin yaşamasını kolaylaştırır.

## Servis ve Mikroservisler
- **Servis:** Aşırı anlam yüklenmiş bir kelimedir.
- **Bağımsız çalışabilen servislerdir.**
- Örnekler:
  - SoA (Service-Oriented Architecture)
  - RMI (Remote Method Invocation)
  - Web Services (SOAP, REST)

## Veri Formatları ve İletişim
- **XML** en iyi formatlardan biri olarak kabul edilirken, **SOAP** gereksiz yükü nedeniyle kötü olarak değerlendirilir.
- **JSON** doğrulaması (validation) zor, bu nedenle XML benzeri bir hale evrilmektedir.
- **YAML**, JSON'un bir alt kümesidir ve daha okunaklıdır.
- **gRPC** Google tarafından geliştirilen, network trafiğini azaltmak için JSON yerine binary format kullanan bir protokoldür.
- Büyük şirketlerin tercihi genellikle birebir örnek alınmamalıdır.
- PostgreSQL en çok kullanılan veritabanlarından biridir.
- REST mimaride servis yazıldığında, sonucu istemci belirler.
- Facebook GraphQL: Backend API'yi düzgün yazmak yerine frontend'e daha fazla yük bindirir.

## Mikroservis Kullanımı
**Mikroservis Kullanmalı mıyız?**
- Eğer **operasyonel yükü karşılayacak yeterli personel ve yetkinlik** yoksa,
- **Ölçeklenebilirlik problemi** yaşanmıyorsa,
- **Düzenli sürüm çıkarma ve yükleme politikası** yoksa,
  **KULLANMAYIN!** Gereksiz iş yükü yaratır.

### Mikroservislerin Popülerleşme Sebepleri
- Birbirinden bağımsız geliştirilen servisler.
- **Yatay ölçeklenebilirlik problemlerini çözer.**
- Uzun vadede **farklı sistemlere göç imkanı** sunar.
- **Her servis bağımsız çalışır ve ölçeklenebilir.**
- **API kontratları üzerinden haberleşir.**
- **Kendi sürüm politikasına sahip olmalıdır.**
- **Entegrasyon testleri düzenli çalıştırılmalıdır.**

### Mikroservis ile Makroservis Arasındaki Farklar
- **Makroservis:** Monolitik yapı gibi fakat mikroservis kurallarına uyuluyor.
- **Ortak veritabanı kullanımı mikroservis mantığına aykırıdır.** Eğer tüm servisler aynı veritabanına bağlıysa bu, mikroservis değil **makroservis** olur.
- **Kimlik yönetimi (IDM) tek bir sistemde tutulabilir.**
- **Makroservislerde sürüm yönetimi zordur.**
- **Makroservislerde hata ayıklama zordur.**

## Kimlik ve Yetkilendirme
- **Yüksek erişilebilirlik** (Replika alınabilmeli)
- **Yük dengeleme** mekanizmaları olmalı
- **Kimlik doğrulama (AuthN) ve yetkilendirme (AuthZ) yönetimi** için çeşitli yöntemler:
  - **OAuth**, JWT (JSON Web Token)
  - **SAML**, CAS
  - **ABAC, ACL**

## Mikroservislerin Yarattığı Sorunlar
- **Transaction yönetimi zordur.**
- **Operasyonel iş yükü çok fazladır.**
- **Problem tespiti ve hata ayıklama karmaşıktır.**
- **Sürüm yönetimi zorlaşır.**
- **Ek araçlar gerektirir.**
- **Kaynak tüketimi küçük sistemler için gereksizdir.**

### Transaction Problemi
- **Soft delete kullanılmalı.** (Önce işareti kaldır, sonra sil.)
- **Two-Phase Commit** uygulanabilir.
- **Senkronda rollback mümkündür, asenkron işlemlerde garanti edilemez.**

## Sanallaştırma Yöntemleri
- **Sanal makine (VM)**
- **Konteyner** (Docker, Kubernetes)
- **Sanal ağ**
- **Sanal disk**

## Configuration as Code
- **Terragrunt** yapılandırma yönetimi sağlar.
- **Ansible** için ek bir servise ihtiyaç duyulmaz.
- **Konteynerler sadece Linux için çalışır.**
- **Konteynerlar CPU ve RAM açısından hafiftir ama depolama yönetimi kritik bir konudur.**

## Kubernetes ve Konteyner Yönetimi
- **Konteynerlar bağımsız paketlerdir.**
- **Çevresel değişkenler (ENV) ile yapılandırılır.**
- **Docker Engine ve ContainerD konteyner çalıştırmada kullanılır.**

### Kubernetes
- **Pod:** Birden fazla konteyneri içerebilir.
- **YAML dosyaları ile yapılandırma sağlanır.**
- **Popüler Kubernetes dağıtımları:**
  - Rancher
  - OpenShift
  - k3s

### Kubernetes ile İlgili Öneriler
- **Vanilla Kubernetes kullanmamak** önerilir.
- **OKD (OpenShift Community Edition) gelişimini tamamlamamıştır.**
- **Mikroservisler için Kubernetes zorunlu değildir.**
- **Birkaç sanal makine yönetilecekse Kubernetes yerine Terragrunt yeterli olabilir.**
- **Ağ sanallaştırması ve disk yönetimi için Kubernetes faydalıdır.**
- **Kubernetes, büyük ölçekli sistemler için uygundur.**

### Kubernetes Teknolojileri
- **Knative:** Kubernetes üzerinde serverless uygulamalar çalıştırmak için kullanılır.
- **CRD (Custom Resource Definition):** Kubernetes özelleştirilmiş kaynaklar tanımlamak için kullanılır.
- **Tekton:** Kubernetes ortamında CI/CD süreçleri için kullanılır (Jenkins alternatifi).
- **OpenTracing:** Servislerin izlenmesi için kullanılır.

## DevOps ve Monitoring
- **Uçtan uca deployment desteklenmelidir.**
- **Test ortamında hataların görünürlüğü sağlanmalıdır.**
- **Monitoring gereklidir: Dev ve Ops için ayrı süreçler olmalıdır.**
- **Loglama ve izleme süreçleri ayrı olmalıdır.**
- **Grafana ve Prometheus kullanılabilir.**
- **ElasticSearch ve Kibana kullanılabilir.**
- **Jaeger ve Zipkin kullanılabilir.**