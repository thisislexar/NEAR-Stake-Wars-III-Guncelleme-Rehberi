# <h1 align="center">NEAR Stake Wars III Güncelleme Rehberi</h1>

![image](https://user-images.githubusercontent.com/101462877/181625932-a7221c87-e28a-4d22-9495-74891139fe68.png)


<h1 align="center"> NEAR Shardnet Ağı'nda 27 Temmuz'da bir hard fork gerçekleşti. Bu hard fork'un ardından node'umuzda yapmamız gereken güncellemeler aşağıda bulunuyor:</h1> 

## Sorularınız için [LossNode Chat](https://t.me/LossNode)

- Data'yı silme

- Yeni `genesis.json` dosyasını indirme

- Node'u güncelledikten sonra eğer staking havuzumuz ağ üzerinde görünmüyorsa yeni staking havuzunu oluşturma

- Node'u tekrar başlatma

Aşağıdaki adımları sırayla ve dikkatlice yapın. Sağ üstten forklamayı ve yıldızlamayı unutmayalım.

# Başlıyoruz. Öncelikle sistemi durduralım ve data'yı silelim.

```
sudo systemctl stop neard
rm ~/.near/data/*
```

# Gerekli ayarlamaları yapıyoruz.

```
cd ~/nearcore
git fetch
git checkout 68bfa84ed1455f891032434d37ccad696e91e4f5
cargo build -p neard --release --features shardnet
```

# `genesis.json` dosyasını siliyoruz ve yenisini yüklüyoruz.

```
cd ~/.near
rm genesis.json
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

# `config.json` dosyasını siliyoruz ve yenisini yüklüyoruz.

```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```

# Sistemi tekrardan başlatıyoruz ve logların akışını kontrol ediyoruz.

```
sudo systemctl start neard && journalctl -n 100 -f -u neard | ccze -A
```

## ÖNEMLİ! BU GÜNCELLEMEYİ YAPTIKTAN SONRA STAKING HAVUZUNUZU SİSTEMDE GÖREMİYORSANIZ HARD FORK DOLAYISIYLA SİLİNMİŞ OLABİLİR. STAKING HAVUZUNUZU TEKRAR OLUŞTURUN.

STAKING HAVUZUMU SİSTEMDE NASIL GÖRÜRÜM DİYENLER İÇİN AŞAĞIDAKİ NEAR'LA BAŞLAYAN KOMUTLARI GİRDİĞİNİZDE:

- `near proposals` : Ağa yeni katılan staking havuzlarını gösterir.

- `near validators next` : Ağa aktif olarak katılmayı bekleyen.

- `near validators current` : Ağda aktif olarak bulunan.

## Dostlar ayrıca node'unuzu yedeklemeyi öneriyorum, nolur nolmaz sunucu patlar bir sıkıntı çıkar yeni sunucu açtığınızda eski validatörünüze devam edebilin. Bu flood'u yazarken node'umu yedeklemediğim için node'um patladı o yüzden yedekleyin :)

WinSCP kullanarak yedeklemeyi gerçekleştireceğiz.

Yedeklemeniz gereken dosyalar: `node_key.json` ve `validator_key.json`

![image](https://user-images.githubusercontent.com/101462877/181651085-464d9102-1e0d-4141-b535-6e7ecb0b7bed.png)

`.near` klasörüne giriyoruz.

![image](https://user-images.githubusercontent.com/101462877/181651121-085aa3c6-6fca-4595-8404-a43b5f377851.png)

!!Bu iki klasörü kopyalayın ve masaüstünde boş bir klasör açın. Ardından bu klasörün içine bir klasör daha açın bunun da `.near` olsun. Yeni sunucuya taşırken hangi dosyayı hangi klasöre koyacağınızı daha kolay bulursunuz bu yolla.

Ben bunların yanında cüzdan dosyalarını da yedekledim nolur nolmaz. Onu da göstereyim.

![image](https://user-images.githubusercontent.com/101462877/181651353-f6daa2f8-b2d2-484e-898e-e01ac2abfb21.png)

`root` yazısına tıklayıp root dizinine dönüyoruz.

![image](https://user-images.githubusercontent.com/101462877/183651093-d2ffc1a3-48dc-41dc-b9d5-1f1cefb1dc3c.png)

`.near-credentials` ve `.near-config` klasörlerini doğrudan kopyalayıp masaüstünde açtığımız klasör içine atıyoruz.

Kendi bilgisayarımızdaki yedek klasörümüzün içinde 3 tane dosya olmuş oldu, `.near`, `.near-credentials` ve `.near-config`



# `config.json` dosyası için yeni bir güncelleme yayınlandı. Aşağıdaki komutları sırayla girelim:

Sistemi durduruyoruz.

```
sudo systemctl stop neard
```

Mevcut `config.json` dosyasını silip yeni `config.json` dosyasını indiriyoruz.

```
cd ~/.near
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```

Sistemi tekrar başlatıyoruz.

```
sudo systemctl start neard && journalctl -n 100 -f -u neard | ccze -A
```

Logların akışı düzgün ise güncellemeyi başarıyla yapmışsınız demektir.


# Yeni bir `commit` güncellemesi geldi. Bunun için aşağıdaki komutları tek tek giriyoruz:

Sistemi durduralım.

```
sudo systemctl stop neard
```

Güncellemeyi yapalım. Burası biraz uzun sürebilir.

```
cd $HOME/nearcore

git fetch

git checkout 68bfa84ed1455f891032434d37ccad696e91e4f5

cargo build -p neard --release --features shardnet
```

Sistemi tekrar başlatalım.

```
sudo systemctl start neard
sudo systemctl daemon-reload
sudo systemctl restart neard
journalctl -n 100 -f -u neard | ccze -A
```

Logların akışı düzgün ise güncellemeyi başarıyla yapmışsınız demektir.


# YENİ GÜNCELLEME

## BU GÜNCELLEMEYİ EĞER NODE'UNUZ SYNC İSE YAPIN. SYNC OLMAYAN BİR NODE'DA DENEDİĞİMDE SORUN ÇIKTI, EMİN OLMAMAKLA BERABER SORUNUN NODE SYNC OLMADIĞI İÇİN OLDUĞUNU DÜŞÜNÜYORUM. SYNC OLUP OLMADIĞINI KONTROL ETMEK İÇİN AŞAĞIDAKİ KOMUT:

```
curl -s http://127.0.0.1:3030/status | jq .sync_info
```
![image](https://user-images.githubusercontent.com/101462877/184243657-cd28d2d5-205d-466b-9050-f85981cdb3e6.png)

Okla gösterdiğim kısım `false` ise node'unuz sync olmuştur. Devam edebilirsiniz.

# Validatör'den validatöre iletişimi artırmak için bir güncelleme eklendi. Detaylar için [buraya](https://docs.google.com/document/d/1RDd9ETfLQL_JfnEePmUPjkLvIxAtLV-c5AKWV8IMJEo/edit#heading=h.x8zofr2t0ji1) bakabilirsiniz.

Sıra sıra acele etmeden dikkatlice yapın, tek bir virgülü unutmanız durumunda bir şeyler ters gidecektir.

![image](https://user-images.githubusercontent.com/101462877/184237443-b9adb19b-951b-47cd-822a-a6e67e3655bb.png)

Her zamanki gibi WinSCP ya da Cyberduck ile sunucumuza bağlanıyoruz. Sunucu adı IP'miz, kullanıcı adı `root` ve şifre de root şifremiz.

![image](https://user-images.githubusercontent.com/101462877/184238744-bb3fe46f-f0db-4cdc-85d1-22c52e0c5cd9.png)

Kenara bir not defteri açıyoruz ve aşağıdaki komutu bu not defterine yapıştırıyoruz.

```
“public_addrs”:[“yyyyyyyyy@xxxxxxxxx:24567”],
```

![image](https://user-images.githubusercontent.com/101462877/184237677-d2ed41de-61f6-4a9f-9d6b-a80436255ed7.png)

`.near` klasörümüzün içine giriyoruz.

![image](https://user-images.githubusercontent.com/101462877/184238935-fc2e2c89-ebaa-4f21-8a8b-fa13b136c128.png)

`node_key.json` dosyamızı açıyoruz.


![image](https://user-images.githubusercontent.com/101462877/184239334-dfc952aa-1e57-4b9f-b1f7-13f36032fec8.png)

Bizi böyle bir dosya karşılıyor, bu kısımda `public_key` yazan yerin karşısındakini ed25519'dan başlayarak tırnağın bitimine kadar kopyalıyoruz.

![image](https://user-images.githubusercontent.com/101462877/184239647-d806ea95-27e7-4bea-9dad-ae923024cec6.png)

Az önce açtığımız not defterindeki SADECE Y'LERİN HEPSİNİ SİLİP `node_key.json`'dan kopyaladığımız şeyi yapıştırıyoruz.

![image](https://user-images.githubusercontent.com/101462877/184239935-167849ee-c368-446e-bdc6-cfa9a8dc56aa.png)

Şimdi de SADECE X'LERİN HEPSİNİ SİLİP kendi sunucu IP'mizi yapıştırıyoruz.

![image](https://user-images.githubusercontent.com/101462877/184240138-94794d2d-d9ce-4c92-9aa3-9765e5b9dfb4.png)

Ardından not defterimizde oluşan bu şeyin tamamını kopyalıyoruz, bize lazım olacak. Şimdi tekrar WinSCP/Cyberduck'a dönüyoruz. 

![image](https://user-images.githubusercontent.com/101462877/184240387-3544753b-ff9a-49f6-9b65-c9908b550bf6.png)


`config.json` dosyasını açıyoruz.

![Screenshot_2](https://user-images.githubusercontent.com/101462877/184240623-0aafc794-e9d8-4c50-a47d-41234ad5214b.png)

Ctrl + F (ya da Command + F) yaparak `boot_nodes`'u aratıyoruz.

![image](https://user-images.githubusercontent.com/101462877/184240908-246efa64-e550-4bf1-8569-7c71658f2ed0.png)

Ardından bu şekilde bir alt satırın başına gelip Shift + Enter basıyoruz ki bize boş bir satır açsın.

![image](https://user-images.githubusercontent.com/101462877/184241125-419591d9-8f3e-4681-ae5a-e91130734b92.png)

`"whitelist_nodes"` biraz sola kayacaktır, onu boşlukla yukarıya hizalıyoruz. Boş satıra az önce not defterinden kopyaladığımız şeyi yapıştırıyoruz. Son görünüm yukarıdaki gibi olmalı. Son olarak Ctrl + S yapıp dosyada yaptığımız değişiklikleri kaydediyoruz.

![image](https://user-images.githubusercontent.com/101462877/184241386-e4434076-82ce-4333-ae76-403116f682df.png)

Şimdi WinSCP/Cyberduck kapatıp Terminalimize gidiyoruz. Aşağıdaki kodlar sayesinde node'umuzu yeniden başlatacağız. Logların akışını kontrol edin, bir sıkıntı varsa [Telegram](t.me/LossNode)'dayım.

```
sudo systemctl daemon-reload
sudo systemctl restart neard
systemctl restart systemd-journald.service
journalctl -n 100 -f -u neard | ccze -A
```

## İşlemler bu kadardı, yeni güncellemelerde görüşmek üzere. Lexar out✌️
