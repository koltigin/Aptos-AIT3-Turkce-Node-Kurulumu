<a href="https://aptos.dev">
	<img width="100%" src="https://github.com/koltigin/Aptos-AIT2-Turkce-Node-Kurulumu/raw/main/.assets/aptos_banner.png" alt="Aptos Banner" />
</a>

---

[![Aptos Rust Crate Documentation (main)](https://img.shields.io/badge/docs-main-59f)](https://aptos-labs.github.io/aptos-core/)
[![License](https://img.shields.io/badge/license-Apache-green.svg)](LICENSE)
[![CircleCI](https://circleci.com/gh/aptos-labs/aptos-core/tree/main.svg?style=shield&circle-token=d248cf1c0580eb69a507a71c0d238e1eaf193767)](https://circleci.com/gh/aptos-labs/aptos-core/tree/main)
[![grcov](https://img.shields.io/badge/Coverage-grcov-green)](https://ci-artifacts.aptoslabs.com/coverage/unit-coverage/latest/index.html)
[![test history](https://img.shields.io/badge/Test-History-green)](https://ci-artifacts.aptoslabs.com/testhistory/aptos/aptos/auto/ci-test.yml/index.html)
[![Automated Issues](https://img.shields.io/github/issues-search?color=orange&label=Automated%20Issues&query=repo%3Aaptos%2Faptos%20is%3Aopen%20author%3Aapp%2Fgithub-actions)](https://github.com/aptos-labs/aptos-core/issues/created_by/app/github-actions)
[![Discord chat](https://img.shields.io/discord/945856774056083548?style=flat-square)](https://discord.gg/aptoslabs)

# Aptos Incentivized Testnet 3 Türkçe Node Kurulumu

## Takvim
* Kayıtların başlangıcı: 19 Ağustos 2022
* Kayıtların bitişi: 25 Ağustos 2022
* Kayıt bildirimlerinin gönderilmesi: 29 Ağustos 2022
* Testnet 3'ün başlaması: 30 Ağustos 2022
* Testnet 3'ün bitişi:  9 Eylül 2022

## Sistem gereksinimleri (Minimum)
* CPU: 8vcpu
* Ram: 32GB RAM
* SSD: 300GB 
* İşletim sistemi: Ubuntu 20.04+

## Kurulum

## Root Yetkisi Alma ve Root Dizinine Geçme
```shell
sudo su
cd $HOME
```

### Gerekli Güncelemelerin ve Kütüphanelerin Kurulması
```shell
sudo apt update && sudo apt upgrade -y
```
```shell
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony unzip liblz4-tool -y
```

### Docker Kurulumu
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### Docker Compose Kurulumu
```shell
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### Aptos Kurulumu
```shell
wget -qO aptos-cli.zip https://github.com/aptos-labs/aptos-core/releases/download/aptos-cli-v0.3.1/aptos-cli-0.3.1-Ubuntu-x86_64.zip
unzip -o aptos-cli.zip
chmod +x aptos
mv aptos /usr/local/bin
```

### Aptos Klasörü Oluşturma
```shell
export WORKSPACE=aptos-test3
mkdir ~/$WORKSPACE
cd ~/$WORKSPACE
```

### validator.yaml ve docker-compose.yaml Dosyalarını İndirme 
```shell
wget -O $HOME/$WORKSPACE/docker-compose.yaml https://raw.githubusercontent.com/mmc6185/node-testnets/main/aptos/Aptos%20AIT%203/docker-compose.yaml
wget -O $HOME/$WORKSPACE/validator.yaml https://raw.githubusercontent.com/mmc6185/node-testnets/main/aptos/Aptos%20AIT%203/validator.yaml
```

### Çalışma Dizininizde Anahtar Çiftleri (node owner key, consensus key and networking key) Oluşturma
```shell
aptos genesis generate-keys --assume-yes --output-dir $HOME/$WORKSPACE
```

### Validator Bilgilerini Yapılandırma 
`IP` değişkenine node ip ve portumuzu ilişkilendiriyoruz.
* `NODE_ADINIZ` bölümünü node adınız ne olacaksa onu yazıyoruz.
```shell
IP=$(curl icanhazip.com):6180
cd ~/$WORKSPACE
aptos genesis set-validator-configuration \
    --local-repository-dir ~/$WORKSPACE \
    --username NODE_ADINIZ \
    --owner-public-identity-file ~/$WORKSPACE/keys/public-keys.yaml \
    --validator-host $IP:6180 \
    --stake-amount 100000000000000
```

### `layout.yaml` Dosyası Oluşturma
* `NODE_ADINIZ` bölümünü node adınız ne olacaksa onu yazıyoruz.
```shell
echo "root_key: "D04470F43AB6AEAA4EB616B72128881EEF77346F2075FFE68E14BA7DEBD8095E"
users: [NODE_ADINIZ]
chain_id: 43
allow_new_validators: false
epoch_duration_secs: 7200
is_test: true
min_stake: 100000000000000
min_voting_threshold: 100000000000000
max_stake: 100000000000000000
recurring_lockup_duration_secs: 86400
required_proposer_stake: 100000000000000
rewards_apy_percentage: 10
voting_duration_secs: 43200
voting_power_increase_limit: 20" >layout.yaml
```

### AptosFramework Move bytecode Dosyasını İndirme
```shell
wget https://github.com/aptos-labs/aptos-core/releases/download/aptos-framework-v0.2.0/framework.zip
unzip framework.zip
```

### Genesis blob ve waypoint Derlemesi Yapma
```shell
aptos genesis generate-genesis --local-repository-dir ~/$WORKSPACE --output-dir ~/$WORKSPACE
```

### Docker Başlatma
```shell
docker-compose down -v
docker-compose up -d
```

### Node Durumunu Kontrol Etme
* API kısmını 80 olarak değiştiriniz
https://aptos-node.info/

## Petra Aptos Wallet İndirme
[Buradan](https://aptos.dev/guides/install-petra-wallet-extension/) cüzdanı indiriyoruz.

## Kayıt Olma
https://aptoslabs.com/it3
* Discord ile siteye bağlanıp giriş yapıyoruz.
* Ardından Petra cüzdanımızı bağlıyoruz.

### Key Bilgilerini Görme 
`NODE_ADINIZ` kısmına node adımızı ne yazdıysak onu yazıyoruz.
```shell
cat $HOME/aptoss/NODE_ADINIZ/operator.yaml
```


## Kaynaklar

* [Aptos Labs](https://aptoslabs.com/)
* [Aptos Developer Network](https://aptos.dev)
* [Getting Started](https://aptos.dev/guides/getting-started)
* [Life of a Transaction](https://aptos.dev/guides/basics-life-of-txn)
* [Aptos Discord](https://discord.gg/aptoslabs).

## Katkıda Bulunma

 [Katkıda Bulunma Rehberimizi](https://github.com/aptos-labs/aptos-core/blob/main/CONTRIBUTING.md) ve [Davranış Kurallarımızı](https://github.com/aptos-labs/aptos-core/blob/main/CODE_OF_CONDUCT.md) okuyarak Aptos projesine katkıda bulunma hakkında daha fazla bilgi edinebilirsiniz.


Aptos Core is licensed as [Apache 2.0](https://github.com/aptos-labs/aptos-core/blob/main/LICENSE).
