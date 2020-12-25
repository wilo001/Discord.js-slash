# Discord.js'de "/" Komutları Ekleme  / Örnek Oto Rol Komutu

## Genel Anlatım

---
"/" komutları Discord'da Aralık 2020 tarihinde güncellenip botlar için daha kapsamlı hale getirilen bir özelliktir bu özelliği kullanmak için yapmanız gereken tek şey bir metin kanalına "/" yazmak.

![/ komutlarının görseli](https://media.discordapp.net/attachments/791312887992811542/791749243546501140/unknown.png)

---

Artık bu / özelliğine botlar kendi komutlarını ekleyebiliyorlar fakat bunun için sunucudaki bir yetkilinin botunuzu onaylaması gerekiyor, alt resimde gördüğünüz gibi ben bir otorol komutu yaptım ve komut bu şekilde gözükmekte.

![/ komutlarının görseli](https://media.discordapp.net/attachments/791312887992811542/791753191837794304/unknown.png)

---

## Yapım

"/" komutlarının 2 türü vardır:

1. Global Komutlar
2. Sunucuya Özel Komutlar

## 1. Global Komutlar

Bir global komut oluşturduğunuzda botun olduğu tüm sunuculara bu komutu uygularsınız. Fakat hepsinde göremezsiniz çünkü botunuza sunucu içinde özel ve gözükmeyen bir yetki vermelisiniz.

### Bota yetki nasıl verilir?

* Yetki vermek için [Discord Developers](https://discord.com/developers/applications) sayfasına giriyoruz.
* Ardından botumuzu seçiyoruz ([Görsel](https://media.discordapp.net/attachments/791312887992811542/791756364468584448/unknown.png?width=1080&height=430))
* Daha sonra sol taraftan OAuth2 kısmına giriyoruz ([Görsel](https://media.discordapp.net/attachments/791312887992811542/791756893789356072/unknown.png))
* Biraz aşağıya inip sunulan seçeneklerden **applications.commands** seçeneğini işaretliyoruz ve alt tarafta oluşan linke giriyoruz. ([Görsel](https://media.discordapp.net/attachments/791312887992811542/791757731048325150/unknown.png?width=1078&height=559))
* Girdiğimiz linkte alt taraftaki sunucu seçme butonundan hangi sunucuda yetki vermek istiyorsak onu seçiyoruz ve yetkilendir butonuna basıyoruz. ([Görsel](https://media.discordapp.net/attachments/791312887992811542/791758192476291112/unknown.png?width=321&height=559))

Ve artık botumuz yetkilendirildi. Artık komutlarımızı oluşturabiliriz.

---

## Global Komut Oluşturma

Global komut oluşturmak için aşağıdaki kodu yazmanız yeterli. Fakat bu komutu botunuza bir kere ekleyin ve başlatın, başlattıktan sonra eğer bir hata vermezse kodu silmenizi tavsiye ediyorum.

```javascript
client.on("ready", () => {
  client.api.applications(client.user.id).commands.post({data: {
      name: 'ping',
      description: 'ping pong!'
  }})
});
```

---

Komutumuzu oluşturduktan sonra "/" yazınca karşımıza ping komutu çıkıyor. Ben daha önce otorol komutunu yaptığım için otorol de gözüküyor, sizde sadece ping komutu gözükür.

![Görsel](https://media.discordapp.net/attachments/791312887992811542/791772556239306762/unknown.png)

---
Komutumuzu oluşturduk fakat komutumuzu yazınca karşımıza hiç bir şey çıkmıyor. Bunun için bir event oluşturmamız gerekiyor.

```javascript
client.ws.on('INTERACTION_CREATE', async interaction => {
  const command = interaction.data.name.toLowerCase();
  
  if (command == 'ping'){
    client.api.interactions(interaction.id, interaction.token).callback.post({
      data: {
        type: 4,
        data: {
          content: "Pong!"
        }
      }
    })
  }
});
```

Eventimizi ekledikten sonra artık ping komutunu girince botumuz cevap veriyor. 

![Görsel](https://media.discordapp.net/attachments/791312887992811542/791774575293759528/unknown.png)
---
## 2. Sunucuya Özel Komutlar

Sunucuya özel komut oluşturduğunuzda isminden belli olduğu gibi sunucuya özel bir komut oluşturursunuz Global komutlar ile tek farkı sadece bir sunucu için geçerli olması. Sadece oluşturması farklı diğer tüm her şeyi Global ile aynı.

```javascript
client.on("ready", () => {
  client.api.applications(client.user.id)..guilds("SUNUCU ID")commands.post({data: {
      name: 'ping',
      description: 'ping pong!'
  }})
});
```

## Oto Rol Komutu

* Oto Rol komutu kurulumu için ilk öncelikle botumuza yetki veriyoruz (Global komutlar kısmında anlattığım gibi).
* Daha sonra (bot.js)[https://github.com/CodAre-Development/Discord.js-slash/bot.js] dosyasındaki ilk parçayı alıyoruz ve çalıştırıyoruz. Çalıştırdıktan sonra hatasız bir şekilde ekranda "hazır" yazarsa botu kapatıp kodu siliyoruz.
* Ardından (bot.js)[https://github.com/CodAre-Development/Discord.js-slash/bot.js] dosyasındaki ikinci parçayı alıyoruz ve çalıştırıyoruz.
* Ve botunuz artık hazır!

#### Ek notlar;

* Eğer botunuz çalışmazsa ve hata verirse dert etmeyin çünkü bu daha yeni gelmiş bir özellik ve tam çalışmayan bir özellik.
* Bu bilgilerin hepsini internetten araştırdım daha fazla bilgi istiyorsanız [buradan](https://discord.com/developers/docs/interactions/slash-commands) ve ya internette araştırarak bulabilirsiniz.
* V12 için geçerlidir v11'de çalışmamaktadır.
