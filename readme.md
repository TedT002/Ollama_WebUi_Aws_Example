# Ollama WebUI ile AWS'ye Yapay Zeka Uygulaması Dağıtımı (DevOps Örneği)

Bu proje, Ollama ve Ollama WebUI'ı Docker konteynerleri aracılığıyla AWS üzerinde çalıştırmak için bir örnektir. Ayrıca basit bir izleme çözümü için Prometheus ve Grafana entegrasyonunu da içerir. Bu yapılandırma, bir DevOps iş akışının temel unsurlarını göstermeyi amaçlamaktadır.

## Önkoşullar

* Docker kurulu olmalıdır.
* Docker Compose kurulu olmalıdır.
* AWS hesabı.

## Kurulum

1.  Bu repository'yi klonlayın:
    ```bash
    git clone https://github.com/TedT002/Ollama_WebUi_Aws_Example.git
    cd Ollama_WebUi_Aws_Example
    ```

2.  AWS üzerinde bir EC2 instance başlatın (Ubuntu Öneriyorum). Güvenlik grubunda aşağıdaki portlara izin verdiğinizden emin olun:
    * `8080` (Ollama WebUI)
    * `11434` (Ollama API)
    * `9090` (Prometheus)
    * `3000` (Grafana)
    * `8081` (cAdvisor)

3.  EC2 instance'ına SSH ile bağlanın.

4.  Docker ve Docker Compose'u EC2 instance'ına kurun (önce bi güncelleme tabi):
    ```bash
    sudo apt update
    sudo apt install docker.io -y
    sudo apt install docker-compose -y
    ```

5.  Bu `README.md` ve `docker-compose.yml` dosyasını EC2 instance'ına aktarın (örneğin `scp` kullanarak).

6.  `docker-compose.yml` dosyasının bulunduğu dizine gidin ve konteynerleri başlatın:
    ```bash
    docker-compose up -d
    ```

## Uygulamaya Erişim

* **Ollama WebUI:** Tarayıcınızda EC2 instance'ınızın genel IP adresini ve 8080 portunu kullanarak erişebilirsiniz: `http://<EC2_GENEL_IP>:8080`
* **Ollama API:** API'ye EC2 instance'ınızın genel IP adresi ve 11434 portu üzerinden erişilebilir.
* **Prometheus (isteğe bağlı):** `http://<EC2_GENEL_IP>:9090`
* **Grafana (isteğe bağlı):** `http://<EC2_GENEL_IP>:3000`. İlk giriş için kullanıcı adı `admin`, şifre `your_admin_password`'dur ( `docker-compose.yml` dosyasında belirtilen).

## DevOps Notları

Bu yapılandırma, basit bir DevOps örneği sunmayı amaçlamaktadır. Daha gelişmiş senaryolar için şunlar düşünülebilir:

* **CI/CD Pipeline:** Kod değişikliklerinin otomatik olarak Docker görüntülerini oluşturup AWS'ye dağıtmasını sağlayacak bir pipeline (örneğin AWS CodePipeline) kurulabilir.
* **Altyapı Yönetimi (IaC):** Terraform veya CloudFormation gibi araçlarla AWS altyapısının kod olarak yönetilmesi sağlanabilir.
* **Gelişmiş İzleme:** Prometheus ve Grafana daha detaylı metrikler toplayacak ve anlamlı dashboard'lar oluşturacak şekilde yapılandırılabilir. AWS CloudWatch ile entegrasyon da düşünülebilir.

## Temizlik

Konteynerleri durdurmak için:

```bash
docker-compose down