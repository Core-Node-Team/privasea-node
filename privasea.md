# 🌟 Privanetix Node Kurulumu

![image](https://github.com/user-attachments/assets/79d63a68-6697-41aa-9e7d-766d1fe2a5a9)


## 💻 Sistem Gereksinimleri

| Bileşen | Minimum | İdeal |
|---------|----------|--------|
| İşlemci (CPU) | 4 çekirdek | 16 çekirdek |
| Bellek (RAM) | 4 GB | 8 GB |
| Depolama | 100 GB SSD | 100 GB NVMe SSD |
| İnternet | 100 Mbps | 1 Gbps ve üzeri |

## 📝 Kurulum Adımları

⚠️ **UYARI:** Eğer daha önce kullandığınız bir sunucuysa ve docker yüklüyse "Node Kurulumu" kısmına atlayabilirsiniz.

⚠️ **UYARI2:** Kuruluma başlamadan önce sonuna kadar okumanızı tavsiye ederiz. Okuyup anladıktan sonra başa dönüp başlarsanız daha kolay kurulum gerçekleştirebilirsiniz.

### 1️⃣ Sistem Güncellemesi

```bash
apt update && apt upgrade -y
```

### 2️⃣ Gerekli Araçların Kurulumu

```bash
apt install htop curl git wget make jq build-essential pkg-config ncdu tar clang \
lsb-release libssl-dev unzip lz4 iptables ca-certificates -y
```

### 3️⃣ Docker Kurulumu

Çıktıların en sonunda versiyon çıktısı varsa sorun yoktur devam edebilirsiniz.

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

```bash
docker version
```

### 4️⃣ Docker Compose Kurulumu

Çıktıların en sonunda versiyon çıktısı varsa sorun yoktur devam edebilirsiniz.

```bash
COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
sudo curl -L "https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version
```

### 5️⃣ Docker Yetkilendirme

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

### 6️⃣ Node Kurulumu

Node imajını çekelim

```bash
docker pull privasea/acceleration-node-beta:latest
```
Çalışma dizini oluşturalım

```bash
mkdir -p ~/privanode/config && cd ~/privanode
```

### 7️⃣ Cüzdan Oluşturma

```bash
docker run --rm -it -v "$HOME/privanode/config:/app/config" \
privasea/acceleration-node-beta:latest ./node-calc new_keystore
```

⚠️ **ÖNEMLİ:** Bu adımda oluşturulan şifre ve cüzdan bilgilerini güvenli bir yerde saklayın!

### 8️⃣ Keystore Yapılandırması

```bash
mv $HOME/privanode/config/UTC--* $HOME/privanode/config/wallet_keystore
```

### 9️⃣ Dashboard Ayarları

1. [Dashboard](https://deepsea-beta.privasea.ai/privanetixNode) sayfasına gidin
2. Ödül cüzdanınızı bağlayın - node cüzdanını import etmeyeceksiniz.
3. "Set up now" butonuna tıklayın
4. Komisyon oranını belirleyin
5. Node adresinizi girin ve onaylayın

![image](https://github.com/user-attachments/assets/fd22113f-1dc4-4cb8-bd64-aaf2b30aa1da)


### 🔟 Node'u Başlatma

⚠️ **ÖNEMLİ:** <şifreniz> kısmını düzenlerken "<>" kısımlarını da silin.

```bash
KEYSTORE_PASSWORD=<şifreniz> && docker run -d --name privanode \
-v "$HOME/privanode/config:/app/config" \
-e KEYSTORE_PASSWORD=$KEYSTORE_PASSWORD \
privasea/acceleration-node-beta:latest
```

### 1️⃣1️⃣ Cüzdan Dosyasını Yedekleme

En son bu dizindeki wallet_keystore dosyasını yedeklemeyi unutmayın

⬇

$HOME/privanode/config/wallet_keystore

## 🔍 Kontrol Komutları

Node durumunu kontrol etme

```bash
docker ps | grep privanode
```

Logları görüntüleme

```bash
docker logs privanode -f
```

Node'u yeniden başlatma

```bash
docker restart privanode
```
## 🔍 Stake ve Diğer İşlemler

Bu [sitedeki](https://www.privasea.ai/privanetix-node) 3. adım ve sonrasını kontrol ederek geri kalan bilgilere ulaşabilirsiniz. Şuan ilk iki adımı tamamladık.

## ❓ Muhtemel Soruların Cevapları

- Node cüzdanınızı platforma bağlamayacaksınız. Bir ödül cüzdanı belirleyin veya oluşturun.

- Sitedeki adım 4'te faucetları ödül cüzdanınıza isteyeceksiniz. Stake-Unstake-Claim her şey ödül cüzdanı ile yapılıyor.

- Taşıyacaksanız ya da silip yeniden kuracaksanız "Çalışma dizini oluşturalım" adımına kadar yapın sonra $HOME/privanode/config içine yedeklediğiniz wallet_keystore dosyasını atın ve direkt "Node'u başlatma adımı"na geçin.
