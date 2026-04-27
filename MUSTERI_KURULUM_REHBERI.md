# Insider x ChatGPT Entegrasyon Rehberi

Değerli kullanıcımız, bu rehber size Insider'ın akıllı şablonlarını (widget'larını) ChatGPT içerisinde nasıl canlı kullanıma sunacağınızı açıklamaktadır.

Bu entegrasyon temelde iki aşamadan oluşur: Şablonlara Insider hesap bilgilerinizin girilmesi ve ardından sistemin internet ortamında (Canlı / Production) yayına alınması.

## 1. Insider Hesap Bilgilerinizi Ekleme

Bu şablonların kendi hesabınızla çalışabilmesi için dosyaları sunucuya yüklemeden önce küçük bir ayarlama yapmanız gerekir:
1. İndirdiğiniz klasörün içindeki `widgets` isimli klasörü açın.
2. Klasör içindeki `.html` uzantılı dosyaları (`deals.html`, `notifications.html`, `trend-products.html`) sırayla bir Metin Düzenleyici ile açın.
3. İçerisinde şu satırı bulun:
   `<script async="true" src="https://partnerName.api.useinsider.com/ins.js?id=YOUR_ID"></script>`
4. Buradaki `partnerName` yazan yere kendi Insider Partner adınızı, `YOUR_ID` yazan yere de Insider ID kodunuzu yazıp dosyaları kaydedin. Bunu üç dosya için de yapmayı unutmayın.

## 2. Sistemi Canlıya (Production) Alma

ChatGPT'ye eklenecek olan uygulamanın 7/24 erişilebilir olması ve gerçek müşterilerle çalışabilmesi için dosyaları bilgisayarınız yerine internete açık, canlı bir sunucuya yüklemeniz gerekir. **Bu proje özünde bir Node.js (Express) web uygulamasıdır.** İki farklı şekilde canlıya alabilirsiniz:

### Seçenek A: Kendi IT Altyapınız / AWS / DigitalOcean / Azure vb.
Sunucu işlemlerini eğer ekipleriniz yürütüyorsa:
1. Proje dosyalarını Node.js çalıştırabilen bir sunucuya aktarın.
2. Sunucu terminalinde proje klasörü içindeyken `npm install` komutuyla bağımlılıkları kurun.
3. Uygulamayı `PM2` veya benzeri araçlarla sürekli çalışacak şekilde (`npm start`) başlatın. (*Uygulama varsayılan olarak `8000` portundan ayağa kalkar, bunu `.env` üzerinden değiştirebilirsiniz.*)
4. Bu sunucu adresine web sitenizin bir alt alan adını (örn: `chatgpt-mcp.sirketiniz.com`) ve SSL sertifikasını (HTTPS olmak zorundadır) tanımlayın. Sunucunuz hazır!

### Seçenek B: Render, Railway veya Vercel Gibi Pratik Bulut (Cloud) Sağlayıcılar
Eğer sunucu ayarlamaları ile uğraşmak istemezseniz:
1. Projeyi bir GitHub (veya GitLab vb.) deposu haline getirin.
2. [Render.com](https://render.com) veya [Railway.app](https://railway.app) gibi platformlara giriş yapıp yeni bir *"Web Service"* projesi oluşturup bu kod deponuzu bağlayın.
3. Proje ayarlarınızda "Build Command" kısmına `npm install`, "Start Command" kısmına ise `npm start` yazın.
4. Platform size dakikalar içinde HTTPS uzantılı resmi bir internet adresi (örn: `https://xxxx.onrender.com`) verecektir.

## 3. ChatGPT Taraflı Uygulamanın Bağlanması (Son Aşama)

Sunucuzu canlıya aldıktan sonra elinizdeki HTTPS sunucu adresini ChatGPT'ye tanıtmamız gerekiyor.
1. Tarayıcınızdan **ChatGPT**'yi açın.
2. **Ayarlar (Settings)** > **Uygulamalar (Apps)** bölümüne gidin.
3. (Eğer aktif değilse) **Gelişmiş Ayarlar (Advanced Settings)** altındaki **Geliştirici Modunu (Developer Mode)** açın.
4. Uygulama Listesinin yukarısında bulunan **Uygulama Oluştur (Create App)** butonuna tıklayın.
5. Çıkan formu şu şekilde doldurun:
   - **Name:** *Insider Alışveriş Asistanı* (Markanıza uygun dilediğiniz bir isim girin)
   - **MCP Server URL:** Az önce canlıya aldığınız sunucu adresinizi yapıştırın ve sonuna `/mcp` ekleyin. *(Tam örnek: `https://chatgpt-mcp.sirketiniz.com/mcp`)*
   - **Authentication:** Uygulamanıza dışarıdan erişimi kapamak için bir doğrulama (token vb.) atadıysanız bilgilerinizi girebilirsiniz; böyle bir ayar yapmadıysanız `No Auth` olarak işaretleyin.
6. Kaydet butonuna tıklayın.

Kullanıma hazırsınız! Sınırsız kullanımla ChatGPT ekranında `@` harfiyle uygulamanızı seçip müşterileriniz adına Insider entegrasyonunuzu canlı olarak sorgulayabilirsiniz.

---

### *[Ek Bilgi] Geliştiriciler İçin Sunucusuz Yerel (Local) Test:*
Eğer IT ekipleriniz, canlıya almadan önce sadece kodların çalışıp çalışmadığını lokal bilgisayarlarından test etmek istiyorlarsa:
1. Bilgisayarlarında projeyi açıp `npm install` ve `npm start` desinler.
2. Başka bir terminalde `ssh -R 80:localhost:8000 nokey@localhost.run` komutu ile geçici kısa bir link alsınlar.
3. Bu geçici linki aynı mantıkla ChatGPT'ye ekleyip kendi makinaları üzerinden saniyeler içinde doğrulama yapabilirler. Bilgisayar veya terminal kapandığında entegrasyon da duracaktır.
