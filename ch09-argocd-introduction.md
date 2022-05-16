# Argo CD

[GitHub Homepage](https://argoproj.github.io/cd/) | [Alternative Homepage](https://argo-cd.readthedocs.io/en/stable/) | [Argo CD Example Apps](https://github.com/Scope-Infrastructure-Calismalari/argocd-example-apps)

*"Argo CD is a declerative, **GitOps** continuous delivery tool for Kubernetes."*

**Dikkat! GitOps aracı olan Argo CD'nin ne olduğunu okumadan önce eğer GitOps'un ne olduğunu bilmiyorsanız ilk olarak [GitOps yazımızı](GitOps.md) okumanızı rica ederiz, sonrasında bu yazı daha anlaşılır olacaktır.**

Yazının devamında daha detaylı olarak anlatılacak olan Argo CD mimarisinin bir örneğini aşağıdaki görselde görebilmekteyiz. GitOps yazısını okuduktan sonra "GitHub Repo" ifadesinin neden mimaride yer aldığını, alması gerektiğini hemen anlayabilmiş olmalıyız.

<p align="center"><img src="./images/Argo-CD/image-1.png"></p>

## Argo CD Nedir?

Argo CD, adından da anlaşılabileceği üzere aslında bir **C**ontinuous **D**elivery (Sürekli Teslim) aracıdır. Argo CD'yi anlamadan önce CD'nin projelere nasıl eklenip uygulandığını, ve birçok projenin ortak tercihi olan Jenkins ve GitLab gibi CI/CD araçlarının anlayıp sonrasında Argo CD'yi bunlarla kıyaslayarak devam edeceğiz.

### Birçok projede CD nasıl kullanılmaktadır?

Aşağıdaki görsellerin solundaki gibi birçok mikro servisimizin olduğunu ve sağ görseldeki gibi bunları Kubernetes(K8s) cluster'ına taşıdığımızı düşünelim.

<p float="center">
  <img src="./images/Argo-CD/image-2.png" width="49%">
  <img src="./images/Argo-CD/image-3.png" width="49%">
</p>

Uygulamamızın kaynak kodunda değişiklikler yapıp (yeni özellilk eklemek veya bugfix yapmak gibi), Git repo'suna push'ladığımızı düşünelim.

<p align="center"><img src="./images/Argo-CD/image-4.png"></p>

Bu değişiklik ve push'lama işleminden sonra sistemimizde kurulu olan ve bu Git repos'su ile eşlenmiş olan Jenkins vb. uygulamalarımızın CI pipeline süreci otomatik olarak tetiklenecektir.

<p align="center"><img src="./images/Argo-CD/image-5.png"></p>

Tetiklenen bu pipeline süreci otomatik adımlarla önce uygulamamızı test sürecinden geçirecek, hata ile karşılaşmazsa yeni bir Docker imajı oluşturacak ve bunu Docker Repo'ya push'layarak CI sürecini tamamlayacaktır.

<p align="center"><img src="./images/Argo-CD/image-6.png"></p>

Artık önümüzde yeni bir soru var: *"Yeni oluşturulan bu Docker imajı K8s cluster'ına nasıl deploy edilecek?"*

  1. K8s deployment YAML dosyasını Docker imajının yeni versiyon numarasını yazarak güncelleriz.

<p align="center"><img src="./images/Argo-CD/image-7.png"></p>

  2. Değiştirilen YAML dosyasını K8s'e uygularız (apply).

Docker imajını Docker Repo'ya push'lama işlemine kadar olan basamakların tümü CI sürecini oluştururken güncellenmiş YAML dosyasını K8s cluster'ına apply etmek ise CD sürecini oluşturmaktadır.

<p align="center"><img src="./images/Argo-CD/image-8.png"></p>

**Bu CI/CD sürecinin zorlukları ve sıkıntıları:**

- K8s cluster'ına erişebilmek ve değişiklikler yapabilmek için kubectl, helm gibi araçların bu örnekte Jenkins olarak varsaydığımız ve sistemimizde kullandığımız *Build Automation Tool* 'larına yüklenmesi gerekliliği

- K8s'e erişimin sağlanması da gerekmekte çünkü kubectl yalnızca K8s client aracıdır ve K8s'e erişebilmesi için "credential"ların tanımlanması gerekmektedir. Eğer AWS gibi bulut sunucuları kullanıyorsak bunlara da erişim için ayrıca credentials tanımlamamız gerekmektedir.

- Bu credential tanımlamaları yalnızca konfigürasyona emek vermek değil bunun yanı sıra güvenlik konusunu da gündeme getirmektedir. Cluster credential'larını external servislere ve araçlara da vermemiz gerekmektedir. Örneğin 33 adet uygulamamız varsa her uygulama kendisi için ayrıca credential talep etmektedir. Ancak bu şekilde her uygulama cluster'da kendisi için tanımlı uygulama kaynaklarına erişebilmiş olacaktır. Eğer tek bulut servisimiz değil de başka cluster'larımız da varsa bunların herbiri için de yine credential tanımlamaları gerekecektir.

- **En önemli sorun** ise K8s'e uygulama deploy eden veya K8s konfigürasyonlarında bir değişiklik yapan Jenkins, bu deployment'ların durumları ile ilgili bilgi sahibi olamamaktadır. Bir defa "`kubectl apply ...`" komutu çalıştırıldığında Jenkins aslında bu execution'ın durumu hakkında bilgi sahibi olamamaktadır. "Uygulama kuruldu mu?", "Uygulama durumu healty mi?", veya "Uygulama başlama aşamasında hata mı verdi?" gibi soruların yanıtını Jenkins takip edememektedir.

**Bu durumlardan dolayı CI/CD sürecinin CD kısmının iyileştirilebileceğini görmekteyiz.**

İşte Argo CD bu özel durumlar göz önüne alınarak K8s cluster'larına daha efektif bir şekilde *"delivery"* yapılabilmesi için GitOps prensipleri baz alınarak geliştirildi. Argo CD için **CD for Kubernetes** diyebiliriz.

### Argo CD ile CD Sürecinin Yürütülmesi

İlk olarak Argo CD, Jenkins vb. gibi K8s cluster'ına dışarıdan erişen bir araç olmak yerine bilakis bu cluster'da kurulan ve içerisinde çalışan bir uygulamadır. Yani Argo CD K8s cluster'ının bir parçasıdır.

Bu şekilde K8s cluster'ının içerisinde kurulmuş olmasının sağladığı en büyük avantaj: **Jenkins gibi değişiklikleri K8s'e push'lamak yerine pull etmesidir**. Zaten K8s cluster'ının içerisinde yer aldığı için aslında dışarıda Git repo'sunda yer alan ve değişikliğe uğrayan K8s manifest dosyasındaki değişikliği pull ederek K8s cluster'ına bu yeni dosyayı iletmiş olmaktadır.

Peki bunu nasıl yapabiliriz:

  1. İlk olarak Argo CD uygulamasını K8s cluster'ına deploy ediyoruz.

  2. Argo CD'yi takip etmesini istediğimiz Git repo'suna bağlayıp o repo'daki değişiklikleri anlık takip etmesini sağlıyoruz.

  3. Bu repo'da herhangi bir değişiklik olduğunda K8s cluster'ına kurduğumuz ve bu repo'yu takip etmesini söylediğimiz Argo CD uygulaması bu değişikliği kendiliğinden algılayacak ve bunu otomatik olarak K8s cluster'ına çekecektır.

Sürece en başındak baktığımızda şu şekilde özetleyebiliriz.

 1. Developer bir geliştirme veya bugfix yaparak bunu Git repo'suna push'lar.

 2. Jenkins vb. araçlarda tanımlanmış olan CI süreci otomatik devreye girer ve yukarıda bahsettiğimiz adımları uygulayarak yeni bir Docker imajı oluşturup K8s manifest dosyalarında, örneğin deployment.yml, değişiklik yapar.

 3. Argo CD ise bu değişikliği algılayıp yeni manifest dosyasını K8s cluster'ına pull eder.

**Best Practice for Git Repository**: Git repo'sunu *application source code* ve *application configuration* (K8s manifest files) olarak ayırmalıyız. Hatta *system configuration* için de ayrı bir repo oluşturmalıyız.

Peki neden bu yaklaşımı uygulamamız lazım? Çünkü:

- Uygulamanın konfigurasyon kodları yalnızca deployment dosyasında değil, bununla beraber configmap, secret, service, ingress vb. dosyalarında da olup uygulamanın cluster'da çalışması için gerekli olabilir.

- K8s manifest dosyaları kaynak kod'dan bağımsızdır. Örneğin uygulamanın service.yml dosyasında bir değişiklik yaptığımızda tüm CI sürecinin baştan başlamasını istemeyiz çünkü kodlarda bir değişiklik yapılmamıştır.

- CI pipeline sürecine K8s manifest dosyalarının da eklersek karmaşık bir pipeline süreci oluşturmuş oluruz.

- Yalnızca *App Configuration* Git repo'sunu takip etmesini söylediğimiz Argo CD uygulaması, Jenkins uygulamasının CI sürecini tamamlayıp K8s manifest dosyasını (örneğin Deployment.yml) değiştirip bu repo'ya push'lamasından sonra otomatikl olarak çalışıp bunu K8s cluster'ının içine çekecektir.

<p align="center"><img src="./images/Argo-CD/image-9.png"></p>

Argo CD K8s manifest dosyalarının "Plain(K8s) YAML Files", "Helm Charts", "Kustomize Files" veya diğer K8s manifest dosyaları oluşturan diğer template dosyaları desteklemektedir.

Bu dosyaların olduğu ve *App Configuration* olarak adlandırığımız repo aslında GitOps repo'su olmuş olmakta ve Argo CD'ye burayı dinlemesini söylemekteyiz. Bu repo'daki dosyalar Jenkins CI süreci ile değiştirilebileceği gibi doğrudan DevOps mühendisleri tarafından da değiştirilebilecektir.

<p align="center"><img src="./images/Argo-CD/image-10.png"></p>

Argo CD kurulumu ve kullanımı sonrasında artık CI ve CD pipeline'larımızı ayırmış oluyoruz. Bunun bize sağladığı avantaj ise CI süreçlerini, kodları geliştiren developer'ların yürütebilmesini ve kendi yazdıkları kodların paketlenmesini takip edebilmesini sağlamak. Aynı zamanda daha çok operasyonel işlerle ilgilenen kişilerin de developer'ların düzenlediği pipeline'lar sonucunda üretilen paketlerin alınması ve doğru şekilde çalışmasını sağlamaya odaklanabilmeleridir. Böylece farklı odakları olan iki farklı takım kendi süreçlerine odaklanabilecektir.

<p align="center"><img src="./images/Argo-CD/image-11.png"></p>

Git reposunun "Single Source of Truth" olarak kullanılması aynı zamanda K8s cluster'ının tamamen şeffaf olmasını sağlamaktadır çünkü cluster'da çalışan uygulamalar, bunların ayarları vb. tüm bilgiler kod ile açıkça belirlenmiş ve Git repo'sunda kayıt altına alınmıştır. Sürüm kontrolünü zaten söylemeye gerek yok :)

Gerçekten cluster'da çok hızlı bir şekilde güncelleme yapılması gerektiği durumlarda Argo CD'nin otomatik senkronizasyonu kapatılıp manuel değişikliklere açık hale getirilebilinmekte ve manuel değişiklik yapılması durumunda dışarıya bir sinyal/uyarı gönderilebilmekte ve bu değişikliğin kodda da yapılması sağlanmaktadır.

### Faydaları

- Tüm K8s config dosyaları kod olarak tanımlanıp Git repo'sunda tutulması

- Argo CD'nin bize sağlayacağı fayda, daha önceden de GitOps yazısında belirtildiği gibi, herkesin kendi bilgisayarından çalıştırdığı "`kubectl apply ...`", "`helm install ...`" gibi komutlar yerine artık ortak Git repo'sundaki dosyalar üzerinde değişiklik yapıp bu şekilde K8s cluster'ını güncelleyebilmek

- Takımdaki tüm kişilerin aynı arayüz ile değişiklik yapabilmesinin sağlanmasıdır. "`git commit ...`" komutunun çalıştırılması yeterli olacaktır.

- **En büyük fayda ise -> "Single Source of Truth"**. Eğer takımdan biri K8s cluster'ına kendi bilgisayarından bağlanıp bir değişiklik yaparsa (bir uygulamanın replika sayısını 1'den 2'ye çıkarmak gibi), Argo CD

### Cluster'ın Kurtarılması

<p align="center"><img src="./images/Argo-CD/image-12.png"></p>

Geliştirdiğimiz uygulamalarının kaynak kod'larının Git repo'sunda yer alması kodların güvenle saklanmasını, yerel makinede sorun olması/sistemin çökmesi/makinenin kaybolması durumunda bizi kurtardığı gibi cluster'ın çalıştığı bilgisayarın sorun yaşaması durumunda da bu yöntemle repo'laştırdığımız configuration dosyaları ile de cluster'ımızı aynı şekliyle yeniden ayaklandırabilmiş olacağız. Manuel değişiklikler ile ilerlenmiş olsaydı daha önceki tüm düzenlemelerin hatırlanıp tekrar yapılması gerekecekti, tabi bu ne kadar mümkün olabilirse.

Aslında bu Argo CD'nin getirdiği bir şey değil, GitOps'un sağladığı faydalardan biridir. Argo CD ise bizim GitOps prensiplerini uygulamamıza yardımcı olmaktadır.

### K8s'in Git ile Erişim Kontrolü

<p align="center"><img src="./images/Argo-CD/image-13.png"></p>

Git repo'su ile yönettiğimiz K8s cluster'ları için *"Cluster Role"*
ve *"User resources"* gibi tanımlamalar yapmamız gerekmeyecektir. Cluster erişimlerini direkt olarak Git ile yönetebilmiş olacağız. Örnek olarak yukarıdaki görselde de olduğu gibi tüm takıma "merge request" açma yetkisi verilebilecekken "merge" yetkisini sadece "Senior Engineers"a atayabiliriz.

Takıma sadece Git repo'suna erişim yetkisi verip K8s cluster'ına takımın erişimini tanımlamadan işlemlerimizi daha güvenli şekilde devam edebilmiş olacağız.

<p align="center"><img src="./images/Argo-CD/image-14.png"></p>

Bunu aynı zamanda "non-human users"a da uygulayacağız. Örneğin Jenkins gibi build automation tool'larının direkt olarak K8s erişiminin tanımlanmasına gerek olmayacaktır çünkü Argo CD zaten cluster'ın içinde çalışmaktadır ve değişik değişiklikleri cluster'a uygulayan tek o olacaktır.

**Özetle** artık cluster credential'larının cluster dışında olmasına gerek yoktur çünkü zaten agent cluster'ın içinde çalışmaktadır. Bu bize tüm cluster'ların güvenliğinin yönetiminin de basitleşmesini sağlamaktadır.

<p align="center"><img src="./images/Argo-CD/image-15.png"></p>

### K8s Eklentisi Olarak Argo CD

Argo CD yalnızca K8s cluster'ına deploy edilmiş bir uygulama değildir, eğer böyle olmuş olsaydı Jenkins de aynı şekilde içeri deploy edilebildiğinden bir farkları olmamış olacaktı. Argo CD'nin burada farklılaşmasını sağlayan şey aslında K8s API'ının bir uzantısı olmasıdır. Aslında Argo CD bir şeyleri yeniden inşa etmek, tüm fonksiyonları yeniden sunmak yerine K8s'in fonksiyonlarına katkı sağlayıp zenginleştirmektedir. Kendi işlemlerini yapabilmek için de K8s'in fonksiyonlarını kullanmaktadır, örneğin verileri saklamak için *"etcd"* kullanmaktadır.

Argo CD'nin bize büyük avantaj sunmasını sağlayan şeylerden biri de aslında K8s'in kontrolcülerini kullanmasıdır. Bu sayede K8s'in anlık durumunu takip edebilmekte, istenen durum ile anlık durumu kıyaslayabilmektedir. Bu sayede Jenkins'in bize sunamadığı "Visibility in the cluster"ı da sunabilmektedir. Yeni bir uygulama deploy edildiğinde pod'un oluşturulma durumunu, uygulamanın sağlık durumunu (healty status), pod'un fail olma durumunu, rollback ihtiyacı olup olmadığını Argo CD UI ile takip edebilmiş olacağız.

Eğer büyük resme bakarsak, aslında bir tarafta Git repo'muz, diğer tarafta K8s cluster'ı ve bu ikisinin tam ortasında ise Argo CD yer almakta. Git repo'su burada **istenen durum**u, K8s cluster'ı ise **gerçekte çalışan durum**u belirtmektedir. Argo CD ise bu ikisinin senkronize olduğundan emin olmak ile yükümlüdür. İkisi arasında ayrılık/farklılık olduğu durumda ilk fırsatta güncelleme yapmaktadır.

<p align="center"><img src="./images/Argo-CD/image-16.png"></p>

*Notların çıkarılmasına kaynaklık eden YouTube videosuna (ArgoCD Tutorial for Beginners | GitOps CD for Kubernetes) [buradan](https://www.youtube.com/watch?v=MeU5_k9ssrs&ab_channel=TechWorldwithNana) ulaşabilirsiniz.*
