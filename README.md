## Artela

### Adım 1: VPS'inizi sipariş edin ve yapılandırın

Node'unuzu kurmaya başlamak için öncelikle bir VPS (Sanal Özel Sunucu) kiralamanız gerekir. Bir VPS kullanmak gelişmiş performans, ölçeklenebilirlik ve güvenilirlik gibi çeşitli avantajlar sunar. Daha düşük maliyetle bir VPS kiralama sürecinde size rehberlik edecek ve nasıl yapılandıracağınızı göstereceğim. VPS'nizi node kurulumuna hazır hale getirmek için aşağıdaki adımları izleyin. **VPS sağlayıcılarını araştırın:** Piyasada bulunan farklı VPS sağlayıcılarını keşfetmek için biraz zaman ayırın. İhtiyaçlarınıza en uygun olanı bulmak için fiyatlandırma, itibar ve müşteri yorumları gibi faktörleri göz önünde bulundurun. Node'unuzu desteklemek için uygun miktarda kaynak (CPU, RAM, depolama) sunan planları arayın. Contabo, uygun fiyatlandırma, güvenilir performans ve kullanıcı dostu arayüz kombinasyonu nedeniyle benim tercihim.

**Önerilen Donanım Gereksinimleri:**

![Ekran Resmi 2024-02-15 15 20 44](https://github.com/0xCallmeOzzy/Artela/assets/132007152/e094b8a7-0d2d-477c-bac9-69b26e9a8ffe)


Yüksek donanım özellikleri ve **Cloud VPS 3 paketinde** yalnızca 1 TB SSD bulunması göz önünde bulundurulduğunda, performans ve verimlilik arasında uygun bir denge kurduğu için bu seçeneği tercih edeceğiz. 

> [!TIP]
> **Cloud VPS 2 paketini** tercih etmeyi düşünebilirsiniz, ancak bu node'u ne kadar süre çalıştırmayı planladığınıza bağlı olarak 400 GB disk alanının gelecekte yeterli olmayabileceğini unutmayın.


![Ekran Resmi 2024-02-15 15 22 42](https://github.com/0xCallmeOzzy/Artela/assets/132007152/eca6a779-e5a9-4bd0-acb3-20e5ba193cd0)


Başlangıçta 1 aylık bir kiralama süresi seçeceğim, gerekirse daha sonra uzatma esnekliğine sahip olacağım:


![Ekran Resmi 2024-02-15 15 23 52](https://github.com/0xCallmeOzzy/Artela/assets/132007152/de46ba57-ab1e-4fff-913d-c9274fdb49a3)

En iyi gecikme süresi için "Avrupa Birliği (Almanya)" seçeneğini seçtiğinizden emin olun:

![Ekran Resmi 2024-02-15 15 24 25](https://github.com/0xCallmeOzzy/Artela/assets/132007152/6d489a7e-06db-48b0-8cb5-5022ced8ca6f)

En iyi depolama çözümü için 1,2 TB SSD depolama türünü seçmenizi öneririm:

![Ekran Resmi 2024-02-15 15 24 56](https://github.com/0xCallmeOzzy/Artela/assets/132007152/e912c1aa-0c58-4de4-94f6-35314e2599cc)

"Popular" ya da "OS" sekmesine gidin ve buradan standart Ubuntu 22.04 imajını seçin:

![Ekran Resmi 2024-02-15 15 25 54](https://github.com/0xCallmeOzzy/Artela/assets/132007152/62522e91-f288-417a-84a5-4bc90f887081)

Güvenliğe öncelik vermek için, güçlü bir parola oluşturmanızı ve maksimum koruma sağlamak için güvenli bir şekilde saklamanızı şiddetle tavsiye ederim:

![Ekran Resmi 2024-02-15 15 26 23](https://github.com/0xCallmeOzzy/Artela/assets/132007152/df2333be-8a66-400e-ab90-7a2f565ffc4b)

Son adım için, "Object Storage", "Networking" ve "Add-Ons" başlıklı bölümleri herhangi bir değişiklik yapmadan varsayılan durumlarında bırakmanız önerilir. Contabo'da yeniyseniz, lütfen bir hesap oluşturun; aksi takdirde, mevcut kimlik bilgilerinizi kullanarak giriş yapın. Gerekli alanları doldurarak kişisel bilgilerinizi girin. Kalan alanları kişisel bilgilerinizle doldurun ve ödemeye devam etmek için "İleri" düğmesine tıklayın. Siparişiniz için ödemeyi başarıyla tamamladıktan sonra, bir ilk e-posta alacaksınız. Yaklaşık 15 dakika içinde, VPS'nize bağlanmak için gereken tüm bilgileri içeren ikinci bir e-posta alacaksınız.

### Adım 2: Hazırlıklar

Devam etmek için, aşağıdaki komutları terminalde çalıştırarak paketleri güncelleyebiliriz. Komutun ilk kısmı (sudo apt update) yükseltmeler için paket listelerini günceller ve ikinci kısmı (sudo apt upgrade -y) yükseltme işlemi sırasında istemlerin otomatik olarak onaylanmasını sağlayan "-y" bayrağı ile yükseltmeleri gerçekleştirir:

```
sudo apt update && sudo apt upgrade -y
```

Araç setimizi gelecek görevlerimiz için ihtiyaç duyacağımız tüm temel paketlerle kurmaya devam edelim. Bu, Git (kod sürümleme), Curl, WGET (indirme), htop (sistem izleme) ve kurulum sürecimiz için önemli olan diğerleri gibi sahip olunması gereken araçları içerir. Gerekli paketleri yüklemek için terminalinizde aşağıdaki komutu çalıştırın:

```
apt install curl iptables build-essential git wget jq make gcc nano tmux htop lz4 nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Adım 3: GO'yu yükleyin

GO'nun en son sürümünü yüklemek için, paketi resmi GO web sitesinden indirmeniz, mevcut GO kurulumunu kaldırmanız, yeni sürümü `/usr/local` içine çıkarmanız ve aşağıdaki komutları çalıştırarak PATH ortam değişkeninizi güncellemeniz gerekir. **Lütfen daha yeni sürümler için web sitesini kontrol edin ve gerekirse sürüm numarasını ayarlayın:**

```
ver="1.22.0"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Son olarak, GO sürümünü kontrol edin. Daha önce bildirilen sürümle eşleşmelidir:

```
go version
```

### Adım 4: Değişkenleri Tanımlama ve Profilinize Kaydetme

```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="moniker"" >> $HOME/.bash_profile
echo "export ARTELA_CHAIN_ID="artela_11822-1"" >> $HOME/.bash_profile
echo "export ARTELA_PORT="45"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

### Adım 5: Artela Kodunu Klonlayın ve Oluşturun

Ardından, Artela kaynak kodunu resmi GitHub depolarından edineceğiz ve kullanım için derleyeceğiz. **Komutları teker teker çalıştırdığınızdan emin olun**, çünkü hepsini bir kerede girmek hatalara yol açabilir:

**Artela deposunu klonlayın:**

```
git clone https://github.com/artela-network/artela
```

**Dizini klonlanmış depoya değiştirin:**

```
cd artela
```

**Ana dalda olduğunuzdan emin olun:**

```
git checkout main
```

**ArtelaD'yi kurun:**
```
make install
```

> [!NOTE]
> Bu adımları tamamladıktan sonra Artela node yazılımı kullanıma hazır olacaktır.

## Adım 6: Düğümünüzü Başlatın ve Yapılandırın

Düğümünüzü başlatmak için aşağıdaki komutu kullanın:

```
artelad init "$MONIKER"
```

Bu komutu çalıştırdıktan sonra, çıktınız aşağıdaki gibi görünmelidir:

![Ekran Resmi 2024-02-15 16 36 38](https://github.com/0xCallmeOzzy/Artela/assets/132007152/e65bf3bb-a302-4bf0-acb3-0c9e971c8c19)

Başlamak için, `genesis.json` dosyasını çalıştırarak yapılandırma dizininize indirin:


```
wget -qO $HOME/.artelad/config/genesis.json https://docs.artela.network/assets/files/genesis-314f4b0294712c1bc6c3f4213fa76465.json
```

Ardından, `config.toml` dosyasındaki `seeds` alanına kalıcı tohumlar ekleyeceğiz. Yapılandırma dizinine gidin:

```
cd $HOME/.artelad/config
```

... ve `sed` kullanarak tohumlar alanını kalıcı `seeds` güncelleyin:

```
SEEDS="8d0c626443a970034dc12df960ae1b1012ccd96a@artela-testnet-seed.itrocket.net:30656"
PEERS="5c9b1bc492aad27a0197a6d3ea3ec9296504e6fd@artela-testnet-peer.itrocket.net:30656,e60ccf5954cf2f324bbe0da7eada0a98437eab29@[2a03:4000:4c:e90:781d:c8ff:fe57:726a]:9656,cc926b13a1be8b3c82cbca5bc137c04055c29d66@54.197.218.54:26656,9142bc72d918a36754d64e90f66b382f6d98f67b@161.35.157.41:45656,615a32fbf484e711562fe93b64cc069e1e5f49ab@185.230.138.142:45656,4ff33861644ebda5fb004130de5167a5a39637a9@95.216.192.62:45656,3a280a539aa874a98e4d2cdfa70118e8c14b6745@95.214.55.138:3656,a5dcbab0007379945faf3b32399ef7d7cdaa8b2c@84.247.160.45:45656,2264c863bead1e2822c52d1b553becf9d8089855@[2400:8905::f03c:94ff:fecf:710]:26656,9646a4585197da3edd5b91c73b0ffa6e2b47c36a@45.85.147.242:45656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.artelad/config/config.toml
```

Durum senkronizasyonunu ayarlamak için, yapılandırmayı gezginden en son güvenilen blok ve yükseklik ile güncelleyeceğiz. Durum senkronizasyonunu etkinleştirin ve `trust_height` ve `trust_hash` değerlerini **gezginde bulduğunuz en son değerlerle** güncelleyin. 

`3102877` 'yi gördüğünüz en yüksek blok numarasıyla değiştirin ve `trust_hash` `916dad7db7d08b0136608e1cc2421d699b13d0d786d1a45843212cf623de33bb` 'yi buna göre güncelleyin:

```
sed -i 's/enable = false/enable = true/' config.toml
sed -i 's/trust_height = 0/trust_height = 3102877/' config.toml
sed -i 's/trust_hash = ""/trust_hash = "916dad7db7d08b0136608e1cc2421d699b13d0d786d1a45843212cf623de33bb"/' config.toml
```

*"Total blocks"* alanından `trust_height` değerini elde edin:

![Ekran Resmi 2024-02-15 16 45 33](https://github.com/0xCallmeOzzy/Artela/assets/132007152/487bbe8d-1464-494d-bb62-35c33d607240)

`trust_hash` için aşağıdaki adımları izleyin. Tarayıcınızda blok ayrıntılarını açın. *"transactions"* sekmesine gidin:

![Ekran Resmi 2024-02-15 16 46 25](https://github.com/0xCallmeOzzy/Artela/assets/132007152/863d2fb7-07e9-4003-9259-f301979748fd)

... ve herhangi bir başarılı işlem karmasını seçip kopyalayın. Baştaki "0x" harfini hash'ten çıkarmayı unutmayın:

![Ekran Resmi 2024-02-15 16 46 53](https://github.com/0xCallmeOzzy/Artela/assets/132007152/89620b2f-c1e5-4cf2-9745-a625c5fdbeca)

Ardından, `rpc_servers` 'ı bilinen iyi sunucuları içerecek şekilde güncellemeniz gerekir. Düzenlemek için `config.toml` dosyasını açın:

```
nano ~/.artelad/config/config.toml
```

345. satıra gidin ve `rpc_servers` 'ı aşağıda gösterildiği gibi değiştirin:

```
rpc_servers = "http://47.254.66.177:26657,http://47.254.93.86:26657"
```

Bu dosyayı kaydetmek için **CTRL + X** tuşlarına ve ardından **Enter** tuşuna basın. Yapılandırma dosyanızdaki bu bölümün tamamı tamamlandığında aşağıdaki gibi olmalıdır:

![Ekran Resmi 2024-02-15 16 49 22](https://github.com/0xCallmeOzzy/Artela/assets/132007152/a210a1f8-3ef4-49ba-8aff-bbf83b4f4ab3)

`app.toml` dosyasındaki budama ayarlarını düzenlemek için aşağıdaki komutları uygulayın. Bu ayarlar budama işlemini özelleştirerek düğüm performansını optimize etmek için verilerin nasıl depolandığını ve yönetildiğini etkiler:

```
sed -i 's|^pruning *=.*|pruning = "custom"|g' app.toml
sed -i 's|^pruning-keep-recent  *=.*|pruning-keep-recent = "362880"|g' app.toml
sed -i 's|^pruning-interval *=.*|pruning-interval = "100"|g' app.toml
sed -i 's|^snapshot-interval *=.*|snapshot-interval = 0|g' app.toml
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.02uart"|g' app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' config.toml
sed -i 's|^indexer *=.*|indexer = "null"|' config.toml
```

`app.toml` dosyanızı daha fazla yapılandırmak için, ağ iletişimi ve durum senkronizasyonu işlevleri için doğru şekilde ayarlandıklarından emin olmak üzere çeşitli hizmetlerin ayarlarını yapmanız gerekecektir. Yapılandırma dosyanızı düzenlemek için, dosyayı açmak üzere `nano app.toml` komutunu çalıştırın. Açtıktan sonra, ayarlamak istediğiniz belirli ayarları bulmak için bölümler arasında gezinin ve gerekli değişiklikleri yapın:

```
[api]
enable = true
address = "tcp://0.0.0.0:1317"

[grpc]
address = "0.0.0.0:9090"

[grpc-web]
address = "0.0.0.0:9091"

[state-sync]
snapshot-interval = 1000
snapshot-keep-recent = 10

[json-rpc]
address = "0.0.0.0:8545"
```

Bu değişikliklerden sonra `app.toml` dosyanız API erişimi, gRPC, gRPC-Web ve JSON-RPC hizmetlerini etkinleştirmek ve düğümünüzü durum senkronizasyonu için optimize etmek üzere uygun şekilde yapılandırılmış olmalıdır. Bu açıklayıcı bir örnektir, bu nedenle `[grpc]`, `[grpc-web]`, `[state-sync]` ve `[json-rpc]` bölümlerini de ayarlamayı unutmayın:

![Ekran Resmi 2024-02-15 16 51 38](https://github.com/0xCallmeOzzy/Artela/assets/132007152/78e7536b-aee8-4a6b-82ea-8456f00f2ee0)

Yapılandırma sürecindeki son adımda, uygun iletişim ayarlarını ve mutabakat parametrelerini sağlamak için `config.toml` dosyasındaki belirli bölümleri ayarlamanız gerekecektir. Yapılandırma dosyanızı düzenlemek için `config.toml` dosyasını çalıştırarak dosyayı açın. Açıldıktan sonra, belirli ayarları bulmak için bölümler arasında gezinerek bunları ayarlayın:

```
# config.toml
proxy_app = "tcp://0.0.0.0:26658"

[rpc]
laddr = "tcp://0.0.0.0:26657"

[consensus]
timeout_commit = "500ms"
```

`config.toml` dosyanızdaki bu bölümleri güncelleyerek, düğümünüzü optimum ağ erişilebilirliği için ayarlar ve daha iyi performans için mutabakat mekanizmalarını ayarlarsınız. Bu değişiklikler, düğümünüzün ağ ile etkili bir şekilde iletişim kurmasını ve mutabakat sürecine verimli bir şekilde katılmasını sağlamak için önemlidir.

## Adım 7: pm2'yi Kurun ve Artela Node'u Başlatın

Başlamak için, Node.js uygulamaları için bir süreç yöneticisi olan pm2'yi kuracağız. Bu komutları teker teker çalıştırdığınızdan emin olun:

**Paket listelerinizi güncelleyin:**

```
sudo apt update
```

**npm yükleyin:**

```
sudo apt install npm -y
```

**npm kullanarak n'yi (Node.js sürüm yöneticisi) global olarak yükleyin ve en son Node.js sürümüne güncelleyin:**

```
sudo npm install -g n
n latest
```

**Yeni yüklenen Node.js sürümünü kullanmak için kabuğunuzun yolunu yenileyin:**

```
hash -r
```

**pm2'yi npm kullanarak global olarak yükleyin:**

```
npm install pm2@latest -g
```

Tüm ağı sıfırdan senkronize etmekten kaçınmak için ArtelaD dizinimizi sıfırlayacağız ve **CoreNodeHQ** tarafından sağlanan ve bu yazının yazıldığı sırada 3.150.000 bloktan yaklaşık 2.600.000'inde bulunan bir anlık görüntüyü kullanacağız. Bu işlem senkronizasyonu önemli ölçüde hızlandıracaktır. İşte adımlar:

**1. ArtelaD Dizinini sıfırlayın:** Bu komut blok zinciri verilerini temizler ancak düğümünüzün ağ bağlantılarını sürdürmek için adres defterini tutar:

```
artelad tendermint unsafe-reset-all --home $HOME/.artelad --keep-addr-book
```

**2. Anlık Görüntüyü İndirin ve Çıkarın:** Anlık görüntüyü indirmek ve doğrudan ArtelaD veri dizininize çıkarmak için curl kullanın:

```
curl -L http://37.120.189.81/artela_testnet/artela_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.artelad
```

Bu adımları izleyerek, düğümünüz 2.600.000 numaralı bloktan senkronize edilmeye başlayacak ve en son bloğa yetişmek için gereken süreyi önemli ölçüde azaltacaktır. Sorunsuz bir senkronizasyon sürecini kolaylaştırmak için node yapılandırmanızın doğru ve ağ bağlantınızın sabit olduğundan emin olun. Artık Artela Node'umuzu pm2 ile başlatabiliriz:

```
pm2 start artelad -- start && pm2 save && pm2 startup
```

Düğümünüzün durumunu kontrol etmek için şunu kullanın:

```
pm2 status
```

Çıktı, süreç hakkında bilgi sağlayacaktır:

![Ekran Resmi 2024-02-15 17 42 16](https://github.com/0xCallmeOzzy/Artela/assets/132007152/05ab29a3-0765-4054-90c4-eaaadd84c6d9)

Günlükleri kontrol etmek ve senkronizasyon sürecini izlemek için şunu kullanın:

```
pm2 logs
```

Düğümünüzün blok zincirinin en son durumuyla güncel olduğundan emin olmak için, düğümünüzün çıktısındaki en son blok numarasını *blockchain explorer* inde bulunan en son blok numarasıyla karşılaştırın. Bu adım, node'unuzun doğru bir şekilde senkronize edildiğini ve en güncel bilgilerle ağa aktif olarak katıldığını doğrular. **Benim node'umun bu işlemi tamamlaması yaklaşık 24 saat sürdü.**


![Ekran Resmi 2024-02-15 17 43 45](https://github.com/0xCallmeOzzy/Artela/assets/132007152/4e144264-0730-4c3e-be94-17797e159025)


## Adım 8: Doğrulayıcı Operatör Hesabınızı Oluşturun

Yeni bir cüzdan oluşturmak için aşağıdaki komutu kullanın ve `<ACCOUNT_NAME>` yerine istediğiniz cüzdan adını yazdığınızdan emin olun:

```
artelad keys add <ACCOUNT_NAME>
```

Bu komutu çalıştırdıktan sonra, bir şifre girmeniz ve ardından onay için tekrar girmeniz istenecektir. Parolayı belirledikten sonra, bir anımsatıcı ifade görüntülenecektir. Bu anımsatıcı ifadeyi yazmanız ve güvenli bir yerde saklamanız önemlidir. Bu ifade, şifrenizi unutmanız durumunda hesabınızı kurtarmanın tek yoludur.


![Ekran Resmi 2024-02-15 17 56 42](https://github.com/0xCallmeOzzy/Artela/assets/132007152/9bd8a3ca-9524-4ffc-8db3-fe0b308ce85b)


## Adım 19: Testnet Token Alın

Cüzdanınızı oluşturduktan sonra, çıktı cüzdan adresinizi içerecektir. Ardından, Artela testnet'e katılmak için Artela Discord Sunucusuna katılın ve **#testnet-faucet** kanalına gidin. Şu anda, musluk yalnızca EVM uyumlu cüzdanları desteklemektedir, bu nedenle aşağıdaki komutu kullanarak Artela cüzdan adresinizi EIP-55 eşdeğerine dönüştürmeniz gerekecektir. `<YOUR_ART_ADDRESS>` yerine gerçek Artela cüzdan adresinizi yazmayı unutmayın:

```
artelad debug addr <YOUR_ART_ADDRESS>
```

Örneğin, Artela cüzdan adresiniz `art1tl6jr3s64qncn7tg4tdx4wwv92c8ep56ebcdu0` ise, `artelad debug addr art1tl6jr3s64qncn7tg4tdx4wwv92c8ep56ebcdu0` kullanırsınız.

Bu, EVM uyumlu adresinizin çıktısını verecektir. Ardından, Discord'daki **#testnet-faucet** kanalında, `<YOUR_EVM_ADDRESS>` yerine EVM uyumlu adresinizi yazarak testnet token'larını talep etmek için aşağıdaki komutu kullanın:

```
$request <YOUR_EVM_ADDRESS>
```

Örneğin:

```
$request 0x5ff521c61aA87789F938aaFa7AB9CCF2B07C869b
```

Lütfen her 6 saatte 1 ART talep edebileceğinizi unutmayın, bu nedenle talebinizi göndermeden önce ayrıntıların doğru olduğundan emin olun.

Artela Ağını MetaMask'e eklemek istiyorsanız, aşağıdaki adımları izleyin: Ağlar → Ağ Ekle → Ağı Manuel Olarak Ekle'ye gidin ve ayrıntıları aşağıdaki gibi doldurun:

```
Network Name: Artela Testnet
New RPC URL: https://betanet-rpc1.artela.network
ChainID: 11822
Symbol: ART
Block Explorer URL: https://betanet-scan.artela.network/
```

Bu kurulum, Artela Testnet ile doğrudan Metamask'tan etkileşim kurmanızı sağlayacaktır. Son adımda hesabınızı Metamask'a nasıl aktaracağınızı öğreneceksiniz.










