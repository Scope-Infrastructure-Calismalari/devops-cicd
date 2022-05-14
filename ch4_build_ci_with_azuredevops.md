# How To Build a CI/CD Pipeline In Azure DevOps?

Azure DevOps by Microsoft Azure is one of the leading tools that automate CI/CD’s process and, in turn, supports automatic builds and code projects to make them available to others. The Azure pipelines combine Continuous Integration (CI) and Continuous Delivery (CD) to consistently test and build the code and ship it to the target environment.

In this section, we will learn how to configure an Azure CI/CD pipeline and integrate it to Azure DevOps for bug tracking.

Table of Content:

- [What is a CI/CD Pipeline?](#what-is-a-ci-cd-pipeline)
- [What is Azure DevOps?](#what-is-azure-devops)
- [How to build Azure CI-CD Pipeline?](#how-to-build-azure-ci-cd-pipeline)

## What is a CI/CD Pipeline?

A CI/CD pipeline is used to automate the process of continuous integration and continuous deployment. The pipeline facilitates the software delivery process via stages like Build, Test, Merge, and Deploy.

In simple words, a pipeline may sound like an overhead, but it isn’t. Instead, it’s a runnable specification of steps that reduce developers’ manual work by delivering a new version of a software productively and saves time.

**Stages of a CI/CD Pipeline:**

  1. **Source Stage** – In most cases, when a change is attempted to the central repository, a pipeline run is triggered. These triggers are set by the CI/CD pipeline tool in the source stage.
  2. **Build Stage** – The combination of source code and its dependencies when building into a runnable instance corporate to the end-user application. The built-in application languages like Java need compilation too, which is done in the build stage. If docker images are to be constructed, that can also be facilitated in this stage. Failing this stage marks a potential error in the code or its dependencies.
  3. **Test Stage** – This stage corresponds to automated tests running to validate our code and its behavior accordingly. This stage acts as a sieve that prevents the bugs from reaching the end-user. There can be multiple stages, from smoke tests to end-to-end integration tests. Failure at this stage will expose errors in the code.
  4. **Deploy Stage** – Once we have a runnable code, the deployment is processed with all predefined tests passed. There are a lot of stages like “Beta,” “Staging,” etc., for the product team. A “Production” stage for the end-users is also present.

Remember, the stages mentioned above are the basic stages, and different steps can be added to make the CI/CD process more automated. To bring a new life to these stages, we have Azure DevOps CI/CD.

## What is Azure DevOps?

Azure DevOps is a collection of services given by Microsoft Azure. It provides development services for a team to support, plan, collaborate, build, and deploy applications. It provides integrated features in a browser or an IDE(Integrated Development Environment). Some of the services for developers are:

- Azure Repos
- Azure Pipelines
- Azure Boards
- Azure Test Plans
- Azure Artifacts

These resources are quite useful, especially Azure Pipelines. In this section, we will be using Azure Pipelines to create a CI/CD pipeline for a .NET project.

## What is Azure Pipelines?

The Azure CI/CD pipeline simplifies continuous integration and continuous delivery (CI/CD) in the application development process. You can start from the source stage with existing code on GitHub or on-premise containers. The Azure Repos can maintain a central repository, and the Azure pipelines maintain build and release pipelines for the given project. The Azure DevOps CI/CD process is a crucial process with all the required dev services.

Apart from continuous integration and continuous deployment with Azure DevOps, these pipelines are used to construct build-deploy-test workflows used mainly in continuous testing (CT). This tests the changes in a fast and scalable routine.

## Advantages of Azure Pipelines

The Azure Pipelines can be multifactored, and in the Azure DevOps CI/CD practice, they provide various advantages:

  1. **Version Control Systems** – Having the code into a version control system is the first step of building an Azure CI/CD pipeline. You can manage your source code in GitHub, Bitbucket, Subversion, or any other Git repository. It also supports Team Foundation Version Control (TFVC).
  
  2. **Programming Languages and Application Types** – You can use different languages with Azure pipelines like Java, Ruby, C, C++, Python, PHP, Go, and JavaScript.
  
  3. **Deployment Targets** – The applications with Azure CI/CD pipelines can be deployed to multiple target environments. This includes Virtual Machines, Containers, or any On-prem or Cloud Platform.

## How to build Azure CI-CD Pipeline?

This is a step-by-step guide to using Azure Pipelines to build a sample application. This guide uses YAML pipelines configured with the [YAML pipeline editor](https://docs.microsoft.com/en-us/azure/devops/pipelines/get-started/yaml-pipeline-editor?view=azure-devops). If you'd like to use Classic pipelines instead, see [Define your Classic pipeline](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/define-multistage-release-process?view=azure-devops).

### Create the first Java pipeline

1. Azure DevOps platformundan üzerinde çalışılan "collection" açılır.

2. Sol menüde yer alan seçeneklerden "Pipelines"a tıklanıp drop-down menü genişletilip çıkan menüdeki "Pipelines"a tıklanır.

<p align="center"><img src="../images/CI-surecleri/image-15.png"></p>

3. İlk defa pipelines oluşturulacakda ekrana gelen "Create your first Pipeline" penceresinin altında yer alan "Create Pipeline" butonuna tıklanır.

<p align="center"><img src="../images/CI-surecleri/image-16.png"></p>

4. Azure DevOps'un yeni versiyonlarında artık varsayılan ayar olarak **yaml** dosyası ile pipeline tanımlanması ve yönetilmesi sağlanmaktadır.

    - *(Biz de bu dokümanda [Neden Pipeline as Code Yaklasimi Uygulanmali?](#neden-pipeline-as-code-yaklasimi-uygulanmali) başlığının altında avantajlarının açıklandığı "Pipeline as Code" yaklaşımı ile devam edeceğiz.)*

    - *Ekrana gelen pencerenin sol altında yer alan "Use the classic editor" butonu ile klasik pipeline tasarımına devam edilebilir.*

    "Where is your code?" penceresinden "Azure Repos Git" seçilir.

<p align="center"><img src="../images/CI-surecleri/image-17.png"></p>

5. Collection altındaki tüm repo'ların sıralandığı pencereden üzerinde çalışacağımız repo seçilir.

<p align="center"><img src="../images/CI-surecleri/image-18.png"></p>

6. "Configure your pipeline" penceresinde "Maven" seçeneği seçilir.

7. Artık önümüzde **"azure-pipelines.yml"** isimli, pipeline ayarlarımızı içeren ve otomatik oluşturulmuş YAML dosyasının taslak hali yer almaktadır. Buradaki önemli kısımlar:

- **trigger**: Bu seçenek oluşturacağımız pipeline'ın ne zaman tetikleneceği belirttiğimiz yerdir. Eğer buraya "main" yazarsak main branch'ine, eğer "master" yazarsak veya "develop" yazarsak **"Bu branch'lere her commit geldiğinde bu pipeline'ı tetikle"** komutunu tanımlamış oluyoruz.
  - **pool**: Hangi agent havuzunu kullanacağımızı burada belirtiyoruz. Varsayılan olarak "default" gelmektedir.
  - **steps**: Burada ise pipeline'ımızın sırasıyla çalıştıracağı task'ları belirlemiş oluyoruz.

    Bu örnek için oluşturduğumuz *"azure-pipelines.yml"* dosyası:

    <p align="center">
      <img src="../images/CI-surecleri/image-19.png">
    </p>

    **Pool ismini collection için tanımlı olan agent pool ile değiştirmezsek pipeline çalışacağı agent'ı bulamayacaktır.**

    <p align="center">
      <img src="../images/CI-surecleri/image-20.png">
    </p>

8. İlgili kısımları değiştirip istediğimiz komutları ve ayarlamaları yaptıktan sonra sağ üstte yer alan **"Save and run"** butonuna tıklıyoruz. Karşımıza gelen pencere "azure-pipelines.yml" dosyasının direkt main branch'ine mi yoksa yeni oluşturulmak istenen branch'e mi commit edilmesini sormaktadır.

    *(Eğer pipeline'lar için ayrı bir branch oluşturulmak istenmiyorsa direkt main branch'ine commit atılarak devam edilebilir.)*

    Ayarlamadan sonra "Save and run" butonuna tıklıyoruz.

<p align="center"><img src="../images/CI-surecleri/image-21.png"></p>

9. Artık karşımızda belirtilen agent tarafından çalıştırılmak üzere sıraya girmiş beklemekte olan pipeline'ın görüntüsü yer almakta:

<p align="center"><img src="../images/CI-surecleri/image-22.png"></p>

    Eğer aşağıdaki gibi hata alıyorsanız gerekli izinleri vererek hatayı çözebilirsiniz.

<p align="center"><img src="../images/CI-surecleri/image-23.png"></p><p align="center"><img src="../images/CI-surecleri/image-24.png"></p><p align="center"><img src="../images/CI-surecleri/image-25.png"></p>

10. İzinler verildiktan sonra derleme işlemine alınan pipeline her bir adımındaki çıktıları tıklanabilir şekilde göstererek işlemi tamamlayacaktır:

<p align="center"><img src="../images/CI-surecleri/image-26.png"></p>

11. Main branch'ine commit geldiğinde tekrar tetiklenmek üzere ayarladığımız pipeline'ımızı test etmek için "Demo.java" dosyasına bir satır ekleyip tekrar commit'liyoruz:

<p align="center"><img src="../images/CI-surecleri/image-27.png"></p><p align="center"><img src="../images/CI-surecleri/image-28.png"></p>

12. Sol menüde yer alan ve daha önceden pipeline oluşturmak için kullandığımız "Pipelines" butonuna tıklayarak derlemeye alınan pipeline'ımızı görmüş oluyoruz. Bu bize main'e gelen commit ile pipeline'ımızın otomatik olarak tetiklendiğini göstermektedir. *(Derleme işlemi tamamlandığında mavi simge yeşile dönüşecektir.)*

<p align="center"><img src="../images/CI-surecleri/image-29.png"></p><p align="center"><img src="../images/CI-surecleri/image-30.png"></p>

13. "Tik" simgesine dönen pipeline'ımızın üstüne tıkladığımızda karşımıza bu pipeline'ın daha önceki çalıştığı zamanlar çıkmakta. Önceki "Run"larla ilgili ne kadar süre önce çalıştığı, çalışmasının ne kadar sürdüğü, hata ile mi yoksa başarıyla mı sonlandığı bilgileri yer almakta:

<p align="center"><img src="../images/CI-surecleri/image-31.png"></p>

  Herhangi bir çalışmanın (run) içerisine girildiğinde daha da detaylı bilgilere yer verilmekte.

<p align="center"><img src="../images/CI-surecleri/image-32.png"></p>

  Bu yeni pencerenin altında yer alan "Jobs" başlığı altındaki "Job"a tıkladığımızda ise çalıştırılmış olan o pipeline'ın step'leri gözükmekte ve üzerlerine tıklanarak detaylı incelenebilmekte.

<p align="center"><img src="../images/CI-surecleri/image-33.png"></p>

**Bu çalışma ile "example-repo-ci-cd-java-pipeline" repo'su için oluşturulan, SCOPE-CICD havuzundaki agent'lar tarafından derlenen, main branch'ine her commit geldiğinde otomatik olarak tetiklenen, YAML formatındaki "azure-pipelines" dosyası ile konfigüre edilen bir Build Pipeline oluşturmuş olduk. CI adımı için olan bu pipeline'ı CD adımı için olan Release Pipeline takip etmektedir. İkisinin birleştirilmesi ve uç uca çalışması ile CI/CD süreci tamamlanmaktadır.**

## Neden Pipeline as Code Yaklasimi Uygulanmali?

### Halen klasik editör kullanılabilir mi?

Azure DevOps platformu bize iki çeşit pipeline oluşturma seçeneği sunmakta; klasik editör ile yaml. Yeni pipeline oluşturmak istediğimizde Azure DevOps platformu bizi varsayılan olarak yaml ile pipeline oluşturmaya yönlendirse de aşağıdaki görselde de belirtilen "Use the classic editor" butonu ile klasik pipeline yapısında kurulum yapılabilinir.

<p align="center"><img src="../images/CI-surecleri/image-34.png"></p>

Template ile devam edilebildiği gibi "Empty job" ile de devam edilebilinir ve istenilen daha spesifik task'lar seçilip sıralanarak pipeline oluşturulabilinir.

<p align="center"><img src="../images/CI-surecleri/image-35.png"></p>

Aşağıdaki görselde de görülen şekilde agent'a "+" butonu ile yeni slot açılıp arama penceresi ile ihtiyaca uygun task'lar eklenip pipeline oluşturulabilinir.

<p align="center"><img src="../images/CI-surecleri/image-36.png"></p>

### Neden klasik editör kullanılmamalı?

**"Pipeline as Code"** yaklaşımının klasik pipeline oluşturulma sisteminden en büyük farkı bu yaklaşımın bize pipeline'ları da versiyonlamamızı sağlayacak olmasıdır. Repo'nun root ('/') klasöründe yer alan "azure-pipelines.yml" da bir dosya olduğu için Git yapısı altında versiyonlanarak tutulmakta, yapılan geliştirmeler, eklemeler ve çıkarmalar saklanmaktadır. Yani iki yıl öncesinin kod'larını ve versiyonları görebileceğimiz gibi o kod'ların pipeline versiyonlarını da görebileceğiz. **Kod ile pipeline'lar eşleşmiş olacak**, *"Şu zamanın artifact'ini oluşturmalıyız"* denildiği zaman kodların o versiyonu için oluşturulup çalıştırılmış olan pipeline dosyasını kullanıp yine aynı artifact'i oluşturbilecek imkana sahip olunmuş olacaktır.

*NOT: Global'de devam eden süreçte developer'lar kendi geliştirdikleri kodun pipeline'ını da kendileri yazmaktadırlar, CI süreçlerinin yönetimi developer'lar yapmakta, kodları ile birlikte pipeline yapısını da idame etmektedirler.*
