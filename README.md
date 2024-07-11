<h1 align="center"> Tanssi

![image](https://github.com/molla202/Tanssi/assets/91562185/209da5fb-efe6-4170-a7ba-90511307e0f7)

> Hızlı, Verimli ve Zahmetsiz Appchain Dağıtımına Gateway'iniz
Tanssi, geliştiricileri, uygulama dağıtımını hızlı ve zahmetsiz hale getirmek için özel olarak tasarlanmış geniş bir altyapı araçları ve hizmetleri kümesiyle donatan bir uygulama altyapısı protokolüdür.

> Uygulama dağıtım süresini aylardan bir saatin altına dönüştürerek Tanssi, blockchain alanında büyük bir değişime yol açarak benzersiz verimlilik ve ölçeklenebilirliğin kilidini açıyor.

 [Topluluk kanalımız](https://t.me/corenodechat)<br>
 [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 [Tanssi Website](https://www.tanssi.network/)<br>
 [Discord](https://discord.gg/WMxTM2fQkr)<br>
 [Twitter](https://twitter.com/TanssiNetwork)<br>
 [Doc](https://docs.tanssi.network/node-operators/block-producers/onboarding/run-a-block-producer/block-producer-systemd/)<br>
 [Explorer](https://polkadot.js.org/apps/?rpc=wss://fraa-dancebox-rpc.a.dancebox.tanssi.network#/extrinsics)<br>
 [Telemetry](https://telemetry.polkadot.io/#stats/0x27aafd88e5921f5d5c6aebcd728dacbbf5c2a37f63e2eda301f8e0def01c43ea)<br>

</h1>

## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	2+|
| RAM	| 4+ GB |
| Storage	| 160+ GB SSD |

### Öcelikle form dolduralım

https://polkadot.js.org/apps/?rpc=wss://fraa-dancebox-rpc.a.dancebox.tanssi.network#/accounts

* Adres olsuturun veya varsa adresinizi alın ve formu doldurun

https://www.tanssi.network/block-producer-form


```
wget https://github.com/moondance-labs/tanssi/releases/download/v0.7.0/tanssi-node && \
chmod +x ./tanssi-node

sudo mkdir /root/tanssi-data/

cd /root/tanssi-data/

mv /root/tanssi-node /root/tanssi-data

```
NOT: molla202 yazan yerleri kendi adınızla değiştirin
```
sudo tee /etc/systemd/system/tanssid.service > /dev/null <<'EOF'
[Unit]
Description="Tanssi systemd service"
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=on-failure
RestartSec=10
User=root
SyslogIdentifier=tanssi
SyslogFacility=local7
KillSignal=SIGHUP
ExecStart=/root/tanssi-data/tanssi-node \
--chain=dancebox \
--name=molla202 \
--unsafe-force-node-key-generation \
--sync=warp \
--base-path=/root/tanssi-data/para \
--state-pruning=2000 \
--blocks-pruning=2000 \
--collator \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--database paritydb \
-- \
--name=molla202 \
--base-path=/root/tanssi-data/container \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
-- \
--chain=westend_moonbase_relay_testnet \
--name=molla202 \
--unsafe-force-node-key-generation \
--sync=fast \
--base-path=/root/tanssi-data/relay \
--state-pruning=2000 \
--blocks-pruning=2000 \
--telemetry-url='wss://telemetry.polkadot.io/submit/ 0' \
--database paritydb \

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable tanssid.service
sudo systemctl restart tanssid.service
```
```
journalctl -u tanssid -fo cat
```

### snap
NOT: çalıştırıp loglar azcuk aktıktan sonra yapın
```
systemctl stop tanssid
cd
sudo apt-get install aria2
sudo apt-get install lz4
aria2c -x 16 -s 16 -o dancebox_snapshot_latest.tar.lz4 https://snapshot.tanssi.johnvnb.com/dancebox_snapshot_latest.tar.lz4
aria2c -x 16 -s 16 -o westend_moonbase_relay_testnet_snapshot_latest.tar.lz4 https://snapshot.tanssi.johnvnb.com/westend_moonbase_relay_testnet_snapshot_latest.tar.lz4
rm -rf /root/tanssi-data/para/chains/dancebox/paritydb/*
rm -rf /root/tanssi-data/relay/chains/westend_moonbase_relay_testnet/paritydb/*
lz4 -dc dancebox_snapshot_latest.tar.lz4 | tar -xf - -C /root/tanssi-data/para/chains/dancebox/paritydb
lz4 -dc westend_moonbase_relay_testnet_snapshot_latest.tar.lz4 | tar -xf - -C /root/tanssi-data/relay/chains/westend_moonbase_relay_testnet/paritydb
```
## Key olusturalım.
```
curl http://127.0.0.1:9944 -H \
"Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"author_rotateKeys",
    "params": []
  }'
```
## Keyi kaydedelim
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://fraa-dancebox-rpc.a.dancebox.tanssi.network#/extrinsics

* 1 cüzdanı seçin olusturmadıysanız accountan olusturup bilgilerini yedekleyin. yok oluşturcam derseniz sunucundanda olusturabilirsiniz `/root/tanssi-data/tanssi-node key generate -w 24`
* 2 resimdeki gibi seçin `setkeys(keys,proof)`
* 3 key olusturmustuk onu girin
* 4 girin `0x`
> 5 Tıklayın İşlem Gönder ve işlemi cüzdanınızdan imzalayıp gönderin

![image](https://github.com/molla202/Tanssi/assets/161881320/8846b070-f1b2-425f-872b-3a8f48602b09)

NOT: şimdi faucet için bir form doldurmalı ve discordan rol almalıyız. dokumanın basında bu kısım var sadece hatırlatma amacıyla yazıyorum.

 https://www.tanssi.network/block-producer-form

## Yaptığımız işlemi kontrol edelim
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/chainstate

* 1 resimdeki gibi önce session sonra keyOwner seçelim
* 2 girin `nmbs`
* 3 key olusturmustuk onu girin
* 4 sağ üstteki + ya basın
> 5 bu kısımda cüzdan adresinizin çıkması gerekiyor

![image](https://github.com/molla202/Tanssi/assets/161881320/42d8b56d-e51e-4924-a54c-1ed794e43692)


## Tokenları delege etmek için istek gönderelim
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/extrinsics

* 1 cüzdanınızı seçin
* 2 resimdeki gibi seçin önce `pooledStaking` sonra `requestDelegate`
* 3 cüzdanınızı seçin
* 4 resimdeki gibi seçin `auto-compounding`
* 5 resimdeki kısma `stake: u128 (Balance)` 10000000000000000 yazalım
> 6 submit transaction diyelim

![image](https://github.com/molla202/Tanssi/assets/161881320/ee8717c2-c55f-4dcb-b953-06fa650dfc65)

## İsteğin hangi session'da yapıldığını kontrol edelim
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/chainstate

* 1 resimdeki gibi seçin önce `pooledStaking` sonra `pendingOperations`
* 2 cüzdanınızı seçin
* 3 bu kısmı kapatalım `include option`
* 4 resimdeki gibi + butonuna basalım
> Cüzdanımızı kontrol edelim, burada isteği kaçıncı session'da (dönemde) yaptığımızı görebiliriz

![image](https://github.com/molla202/Tanssi/assets/161881320/e605e62a-bb5e-476a-be86-d7aac36190d1)

## Ağın güncel olarak kaçıncı session'da olduğuna bakalım
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/chainstate

* 1 resimdeki gibi seçin önce `session` sonra `currentIndex`
* 2 resimdeki gibi + butonuna basalım
* 3 bu kısmı kapatalım `include option`
> 4 bu kısımda güncel ağın güncel olarak kaçıncı sesion'da olduğuna bakabiliriz

![image](https://github.com/molla202/Tanssi/assets/161881320/e63e05fc-cd12-4fa3-9b1d-9749ef964a73)

## Yaptığımız isteği onaylatalım. bunun için yaklaşık 2 session geçmesi gerekiyor
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/extrinsics

* 1 cüzdanınızı seçin
* 2 resimdeki gibi seçin önce `pooledStaking` sonra `executePendingOperations`
* 3 cüzdanınızı seçin
* 4 resimdeki gibi seçin `JoiningAutoCompounding`
* 5 cüzdanınızı seçin
* 6 bu kısma isteği yaptığımız session numarasını girelim
> 7 submit transaction diyelim. bu işlemden sonra artık blok üretmeye başlayabiliriz

![image](https://github.com/molla202/Tanssi/assets/161881320/a959bc52-2f92-4ad7-8692-f8b6f7fef470)

## Onay işlemini kontrol edelim
* Linke gidelim.

https://polkadot.js.org/apps/?rpc=wss://dancebox.tanssi-api.network#/chainstate

* 2 resimdeki gibi seçin önce `pooledStaking` sonra `sortedEligibleCandidates`
* 2 resimdeki gibi + butonuna basalım
> 3 buradan cüzdan adresinizi aratıp doğrulayabilirsiniz

![image](https://github.com/hazennetworksolutions/Tanssi/assets/161881320/4fad10b0-05f8-4338-89f1-14740ab1fe01)
