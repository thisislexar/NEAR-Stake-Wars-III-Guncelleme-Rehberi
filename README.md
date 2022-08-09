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
