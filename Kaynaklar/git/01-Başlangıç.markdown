# Baþlangýç #

Bu bölümde Git kullanýmý hakkýnda temel bilgileri bulacaksýnýz. Ýþe, sürüm kontrol sistemleri hakkýnda açýklamalarla baþlayacaðýz; daha sonra Git kurulumunun nasýl yapýlacaðýný, en son olarak da aracýn yapýlandýrma ve kullanýmýný açýklayacaðýz. Bu bölümün sonunda Git'in varlýk sebebini ve neden onu kullanmanýz gerektiðini anlayacak, Git'i kullanmaya baþlamak için kurulumu tamamlamýþ olacaksýnýz.

## Sürüm Kontrolü Hakkýnda ##

Sürüm kontrolü nedir ve ne iþe yarar? Sürüm kontrolü, bir ya da daha fazla dosya üzerinde yapýlan deðiþiklikleri kaydeden ve daha sonra belirli bir sürüme geri dönebilmenizi saðlayan bir sistemdir. Bu kitaptaki örneklerde yazýlým kaynak kod dosyalarýnýn sürüm kontrolünü yapacaksýnýz, ne var ki gerçekte sürüm kontrolünü neredeyse her türden dosya için kullanabilirsiniz.

Bir grafik ya da web tasarýmcýsýysanýz ve bir görselin ya da tasarýmýn deðiþik sürümlerini korumak istiyorsanýz (ki muhtemelen bunu yapmak istersiniz), bir Sürüm Kontrol Sistemi (SKS) kullanmanýz çok akýllýca olacaktýr. SKS, dosyalarýn ya da bütün projenin geçmiþteki belirli bir sürümüne eriþmenizi, zaman içinde yapýlan deðiþiklikleri karþýlaþtýrmanýzý, soruna neden olan þeyde en son kimin deðiþiklik yaptýðýný, belirli bir hatayý kimin, ne zaman sisteme dahil ettiðini ve daha baþka pek çok þeyi görebilmenizi saðlar. Öte yandan, SKS kullanmak, bir hata yaptýðýnýzda ya da bazý dosyalarý yanlýþlýkla sildiðinizde durumu kolayca telâfi etmenize yardýmcý olur. Üstelik, bütün bunlar size kayda deðer bir ek yük de getirmez.

### Yerel Sürüm Kontrol Sistemleri ###

Çoðu insan, dosyalarý bir klasöre (akýllarý baþlarýndaysa tarih ve zaman bilgisini de içeren bir klasöre) kopyalayarak sürüm kontrolü yapmayý tercih eder. Bu yaklaþým çok yaygýndýr, çünkü çok kolaydýr; ama ayný zamanda hatalara da alabildiðine açýktýr. Hangi klasörde olduðunuzu unutup yanlýþ dosyaya yazabilir ya da istemediðiniz dosyalarýn üstüne kopyalama yapabilirsiniz.

Bu sorunla baþ edebilmek için, programcýlar uzun zaman önce, dosyalardaki bütün deðiþiklikleri sürüm kontrolüne alan basit bir veritabanýna sahip olan yerel SKS'ler geliþtirdiler (bkz. Figür 1-1).

Insert 18333fig0101.png 
Figür 1-1. Yerel sürüm kontrol diyagramý.

En yaygýn SKS araçlarýndan biri, bugün hâlâ pekçok bilgisayara kurulu olarak daðýtýlan, rcs adýnda bir sistemdi. Ünlü Mac OS X iþletim sistemi bile, Developer Tools'u yüklediðinizde, rcs komutunu kurmaktadýr. Bu araç, iki sürüm arasýndaki yamalarý (yani, dosyalar arasýndaki farklarý) özel bir biçimde diske kaydeder; daha sonra, bu yamalarý birbirine ekleyerek, bir dosyanýn belirli bir sürümdeki görünümünü yeniden oluþturur.

### Merkezi Sürüm Kontrol Sistemleri ###

Ýnsanlarýn karþýlaþtýðý ikinci büyük sorun, baþka sistemlerdeki programcýlarla birlikte çalýþma ihtiyacýndan ileri gelir. Bu sorunla baþa çýkabilmek için, Merkezi Sürüm Kontrol Sistemleri (MSKS) geliþtirilmiþtir. Bu sistemler, meselâ CVS, Subversion ve Perforce, sürüm kontrolüne alýnan bütün dosyalarý tutan bir sunucu ve bu sunucudan dosyalarý seçerek alan (_check out_) istemcilerden oluþur. Bu yöntem, yýllarca, sürüm kontrolünde standart yöntem olarak kabul gördü (bkz. Figür 1-2).

Insert 18333fig0102.png 
Figür 1-2. Merkezi sürüm kontrol diyagramý.

Bu yöntemin, özellikle yerel SKS'lere göre, çok sayýda avantajý vardýr. Örneðin, bir projedeki herkes, diðerlerinin ne yaptýðýndan belirli ölçüde haberdardýr. Sistem yöneticileri kimin hangi yetkilere sahip olacaðýný oldukça ayrýntýlý biçimde düzenleyebilirler; üstelik bir MSKS'yi yönetmek, her istemcide ayrý ayrý kurulu olan yerel veritabanlarýný yönetmeye göre çok daha kolaydýr.

Ne var ki, bu yöntemin de ciddi bazý sýkýntýlarý vardýr. En aþikar sýkýntý, merkezi sunucunun arýzalanmasý durumunda ortaya çýkacak kýrýlma noktasý problemidir. Sunucu bir saatliðine çökecek olsa, o bir saat boyunca kullanýcýlarýn çalýþmalarýný sisteme aktarmalarý ya da çalýþtýklarý þeylerin sürümlenmiþ kopyalarýný kaydetmeleri mümkün olmayacaktýr. Merkezi veritabanýnýn sabit diski bozulacak olsa, yedekleme de olmasý gerektiði gibi yapýlmamýþsa, elinizdeki her þeyi —projenin, kullanýcýlarýn bilgisayarlarýnda kalan yerel bellek kopyalarý (_snapshot_) dýþýndaki bütün tarihçesini— kaybedersiniz. Yerel SKS'ler de bu sorundan muzdariptir —projenin bütün tarihçesini tek bir yerde tuttuðunuz sürece her þeyi kaybetme riskiniz vardýr.

### Daðýtýk Sürüm Kontrol Sistemleri ###

Bu noktada Daðýtýk Sürüm Kontrol Sistemleri (DSKS) devreye girer. Bir DSKS'de (Git, Mercurial, Bazaar ya da Darcs örneklerinde olduðu gibi), istemciler (kullanýcýlar) dosyalarýn yalnýzca en son bellek kopyalarýný almakla kalmazlar: yazýlým havuzunu (_repository_) bütünüyle yansýlarlar (kopyalarlar).

Böylece, sunuculardan biri çökerse, ve o sunucu üzerinden ortak çalýþma yürüten sistemler varsa, istemcilerden birinin yazýlým havuzu sunucuya geri yüklenerek sistem kurtarýlabilir. Her seçme (_checkout_) iþlemi esasýnda bütün verinin yedeklenmesiyle sonuçlanýr (bkz. Figür 1-3).

Insert 18333fig0103.png 
Figür 1-3. Daðýtýk sürüm kontrol diyagramý.

Dahasý, bu sistemlerden çoðu birden çok uzak uçbirimdeki yazýlým havuzuyla rahatlýkla çalýþýr, böylece, ayný proje için ayný anda farklý insan topluluklarýyla farklý biçimlerde ortak çalýþmalar yürütebilirsiniz. Bu, birden çok iþ akýþý ile çalýþabilmenizi saðlar, ki bu merkezi sistemlerde (hiyerarþik modeller gibi) mümkün deðildir.

## Git'in Kýsa Bir Tarihçesi ##

Hayattaki pek çok harika þey gibi, Git de bir miktar yaratýcý yýkým ve ateþli tartýþmayla baþladý. Linux çekirdeði (_kernel_) oldukça büyük ölçekli bir açýk kaynak kodlu yazýlým projesidir. Linux çekirdek bakým ve geliþtirme yaþam süresinin çoðunda (1991-2002), yazýlým deðiþiklikleri yamalar ve arþiv dosyalarý olarak tutulup taþýndý. 2002 yýlýnda, Linux çekirdek projesi, BitKeeper adýnda tescilli bir DSKS kullanmaya baþladý.

2005 yýlýnda, Linux çekirdeðini geliþtiren toplulukla BitKeeper'ý geliþtiren þirket arasýndaki iliþki bozuldu ve aracýn topluluk tarafýndan ücretsiz olarak kullanýlabilmesi uygulamasýna son verildi. Bu, Linux geliþtirim topluluðunu (ve özellikle Linux'un yaratýcýsý olan Linus Torvalds'ý) BitKeeper'ý kullanýrken aldýklarý derslerden yola çýkarak kendi araçlarýný geliþtirme konusunda harekete geçirdi. Yeni sistemin hedeflerinden bazýlarý þunlardý:

*	Hýz
*	Basit tasarým
*	Çizgisel olmayan geliþtirim için güçlü destek (binlerce paralel dal (_branch_))
*	Bütünüyle daðýtýk olma
*	Linux çekirdeði gibi büyük projelerle verimli biçimde baþa çýkabilme (hýz ve veri boyutu)

2005'teki doðumundan beri, Git kullaným kolaylýklarýný geliþtirebilmek için evrilip olgunlaþtý, ama yine de bu niteliklerini korudu. Git, inanýlmaz ölçüde hýzlý, büyük ölçekli projelerde alabildiðine verimli ve çizgisel olmayan geliþtirim (bkz. 3. Bölüm) için inanýlmaz bir dallanma (_branching_) sistemine sahip.

## Git'in Temelleri ##

Peki Git özünde nedir? Bu, özümsenmesi gereken önemli bir alt bölüm, çünkü Git'in ne olduðunu ve temel çalýþma ilkelerini anlarsanýz, Git'i etkili biçimde kullanmanýz çok daha kolay olacaktýr. Git'i öðrenirken, Subversion ve Perforce gibi diðer SKS'ler hakkýnda bildiklerinizi aklýnýzdan çýkarmaya çalýþýn; bu aracý kullanýrken yaþanabilecek kafa karýþýklýklarýný önlemenize yardýmcý olacaktýr. Git'in, kullanýcý arayüzü söz konusu sistemlerle benzerlik gösterse de, bilgiyi depolama ve yorumlama biçimi çok farklýdýr; bu farklýlýklarý anlamak, aracý kullanýrken kafa karýþýklýðýna düþmenizi engellemekte yardýmcý olacaktýr.

### Farklar Deðil, Bellek Kopyalarý ###

Git ile diðer SKS'ler (Subversion ve ahbaplarý dahil) arasýndaki esas fark, Git'in bilgiyi yorumlayýþ biçimiyle ilgilidir. Kavramsal olarak, diðer sistemlerin çoðu, bilgiyi dosya-tabanlý bir dizi deðiþiklik olarak depolar. Bu sistemler (CVS, Subversion, Perforce, Bazaar ve saire) bilgiyi, kayýt altýnda tuttuklarý bir dosya kümesi ve zamanla her bir dosya üzerinde yapýlan deðiþikliklerin listesi olarak yorumlarlar (bkz. Figür 1-4).

Insert 18333fig0104.png 
Figür 1-4. Diðer sistemler veriyi her bir dosyanýn ilk sürümu üzerinde yapýlan deðiþiklikler olarak depolama eðilimindedir.

Git, veriyi böyle yorumlayýp depolamaz. Bunun yerine, Git, veriyi, bir mini dosya sisteminin bellek kopyalarý olarak yorumlar. Her kayýt iþleminde (_commit_), ya da projenizin konumunu her kaydediþinizde, Git o anda dosyalarýnýzýn nasýl göründüðünün bir fotoðrafýný çekip o bellek kopyasýna bir referansý depolar. Verimli olabilmek için, deðiþmeyen dosyalarý yeniden depolamaz, yalnýzca halihazýrda depolanmýþ olan bir önceki özdeþ kopyaya bir baðlantý kurar. Git'in veriyi yorumlayýþý daha çok Figür 1-5'teki gibidir.

Insert 18333fig0105.png 
Figür 1-5. Git veriyi projenin zaman içindeki bellek kopyalarý olarak depolar.

Bu, Git'le neredeyse bütün diðer SKS'ler arasýnda ciddi bir ayrýmdýr. Bu ayrým nedeniyle Git, sürüm kontrolünün, diðer sürüm kontrol sistemlerinin çoðu tarafýndan önceki kuþaklardan devralýnan neredeyse bütün yönlerini yeniden gözden geçirmek zorunda býrakýr. Bu ayrým Git'i basit bir SKS olmanýn ötesinde, etkili araçlara sahip bir mini dosya sistemi gibi olmaya iter. Veriyi bu þekilde yorumlamanýn yararlarýndan bazýlarýný dallanmayý iþleyeceðimiz 3. Bölüm'de ele alacaðýz.

### Neredeyse Her Ýþlem Yerel ###

Git'teki iþlemlerin çoðu, yalnýzca yerel dosyalara ve kaynaklara ihtiyaç duyar —genellikle bilgisayar aðýndaki baþka bir bilgisayardaki bilgilere ihtiyaç yoktur. Eðer çoðu iþlemin að gecikmesi maliyetiyle gerçekleþtiði bir MSKS kullanmýþsanýz, Git'in bu yönünü görünce, onun hýz tanrýlarý tarafýndan kutsanmýþ olduðunu düþünebilirsiniz. Çünkü projenin bütün tarihçesi orada, yerel diskinde bulunmaktadýr, iþlemlerin çoðu anlýk gerçekleþiyor gibi görünür.

Örneðin, projenin tarihçesini taramak için Git bir sunucuya baðlanýp oradan tarihçeyi indirdikten sonra görüntülemekle uðraþmaz —yerel veritabanýný okumak yeterlidir. Bu da proje terihçesini neredeyse anýnda görünteleyebilmeniz anlamýna gelir. Bir dosyanýn þimdiki haliyle bir ay önceki hali arasýndaki farklarý görmek isterseniz, Git, bir sunucudan fark hesaplamasý yapmasýný talep etmek ya da karþýlaþtýrmayý yerelde yapabilmek için dosyanýn bir ay önceki halini indirmek zorunda kalmak yerine, dosyanýn bir ay önceki halini yerelde bulup fark hesaplamasýný yerelde yapar.

Bu ayný zamanda, eðer baðlantýnýz kopmuþsa, ya da VPN baðlantýný yoksa yapamayacaðýnýz þeylerin de sayýca oldukça sýnýrlý olduðu anlamýna geliyor. Uçaða ya da trene binmiþ olduðunuz halde biraz çalýþmak istiyorsanýz, yükleme yapabileceðiniz bir að baðlantýsýna kavuþana kadar güle oynaya kayýt yapabilirsiniz. Eve vardýðýnýzda VPN istemcinizin olmasý gerektiði gibi çalýþmýyorsa, yine de çalýþmaya devam edebilirsiniz. pek çok baþka sistemde bunlarý yapmak ya imk^ansýz ya da zahmetlidir. Söz gelimi Perforce'ta, bir sunucuya baðlý deðilseniz fazlaca bir þey yapamazsýnýz; Subversion ve CVS'te dosyalarý deðiþtirebilirsiniz, ama veritabanýna kayýt yapamazsýnýz (çünkü veritabanýna baðlantýnýz yoktur). Bu, çok önemli bir sorun gibi görünmeyebilir, ama ne kadar fark yaratabileceðini gördüðünüzde þaþýrabilirsiniz.

### Git Bütünlüklüdür ###

Git'te her þey depolanmadan önce sýnama toplamýndan geçirilir (_checksum_) ve daha sonra bu sýnama toplamý kullanýlarak ifade edilir. Bu da demek oluyor ki, Git fark etmeden bir dosyanýn ya da klasörün içeriðini deðiþtirmek mümkün deðildir. Bu iþlev Git'in merkezi iþlevlerinden biridir ve felsefesiyle bir bütünlük oluþturur. Transfer sýrasýnda veri kaybý ya da doysa arýzasý olmuþsa, Git bunu mutlaka fark edecektir.

Git'in sýnama toplamý için kullandýðý mekanizmaya SHA-1 özeti denir. Bu, on altýlý sayý sisteminin (_hexadecimal_) sembolleriyle gösterilen (0-9 ve A-F) ve dosya ve klasör düzenini temel alan bir hesaplamayla elde denilen 40 karakterlik bir karakter dizisidir. Bir SHA-1 özeti þuna benzer:

	24b9da6552252987aa493b52f8696cd6d3b00373

Bu özetler sýklýkla karþýnýza çýkacak, çünkü Git onlarý yaygýn biçimde kullanyor. Hatta, Git her þeyi dosya adýyla deðil, içeriðinin özet deðeriyle adreslenen veritabanýnda tutar.

### Git Genellikle Yalnýzca Veri Ekler ###

Git'te iþlem yaptýðýnýzda neredeyse bu iþlemlerin tamamý Git veritabanýna veri ekler. Sistemin geri döndürülemez bir þey yapmasýný ya da veri silmesini saðlamak çok zordur. Her SKS'de olduðu gibi henüz kaydetmediðiniz deðiþiklikleri kaybedebilir ya da bozabilirsiniz; ama Git'e bir bellek kopyasýný kaydettikten sonra veri kaybetmek çok zordur, özellikle de veritabanýnýzý düzenli olarak baþka bir yazýlým havuzuna itiyorsanýz (_push_).

Bu Git kullanmayý keyifli hale getirir, çünkü iþleri ciddi biçimde sýkýntýya sokmadan denemeler yapabileceðimizi biliriz. Git'in veriyi nasýl depoladýðý ve kaybolmuþ görünen veriyi nasýl kurtarabileceðiniz hakkýnda daha derinlikli bir inceleme için bkz. 9. Bölüm.

### Üç Aþama ###

Þimdi dikkat! Öðrenme sürecinizin pürüzsüz ilerlemesini istiyorsanýz, aklýnýzda bulundurmanýz gereken esas þey bu. Git'te, dosyalarýnýzýn içinde bulunabileceði üç aþama (_state_) vardýr: kaydedilmiþ, deðiþtirilmiþ ve hazýrlanmýþ. Kaydedilmiþ, verinin güvenli biçimde veritabanýnda depolanmýþ olduðu anlamýna gelir. Deðiþtirilmiþ, dosyayý deðiþtirmiþ olduðunuz fakat henüz veritabanýna kaydetmediðiniz anlamýna gelir. Hazýrlanmýþ ise, deðiþtirilmiþ bir dosyayý bir sonraki kayýt iþleminde bellek kopyasýna alýnmak üzere iþaretlediðiniz anlamýna gelir.

Bu da bizi bir Git projesinin üç ana bölümüne getiriyor: Git klasörü, çalýþma klasörü ve hazýrlýk alaný.

Insert 18333fig0106.png 
Figür 1-6. Çalýþma klasörü, hazýrlýk alaný ve Git klasörü.

Git klasörü, Git'in üstverileri (_metadata_) ve nesne veritabanýný depoladýðý yerdir. Bu, Git'in en önemli parçasýdýr ve bir yazýlým havuzunu bir bilgisayardan bir baþkasýna klonladýðýnýzda kopyalanan þeydir.

Çalýþma klasörü projenin bir sürümünden yapýlan tek bir seçmedir. Bu dosyalar Git klasöründeki sýkýþtýrýlmýþ veritabanýndan çýkartýlýp sizin kullanýmýnýz için sabit diske yerleþtirilir.

Hazýrlýk alaný (_staging area_), genellikle Git klasörünüzde bulunan ve bir sonraki kayýt iþlemine hangi deðiþikliklerin dahil olacaðýný tutan sade bir dosyadýr. Buna bazen indeks dendiði de olur, ama hazýrlýk alaný ifadesi giderek daha standart hale geliyor.

Git iþleyiþi temelde þöyledir:

1. Çalýþma klasörünüzdeki dosyalar üzerinde deðiþiklik yaparsýnýz.
2. Dosyalarý bellek kopyalarýný hazýrlýk alanýna ekleyerek hazýrlarsýnýz.
3. Dosyalarýn hazýrlýk alanýndaki hallerini alýp oradaki bellek kopyasýný kalýcý olarak Git klasörüne depolayan bir kayýt iþlemi yaparsýnýz.

Bir dosyanýn belirli bir sürümü Git klasöründeyse, onun kaydedilmiþ olduðu kabul edilir. Eðer üzerinde deðiþiklik yapýlmýþ fakat hazýrlýk alanýna eklenmiþse, hazýrlanmýþ olduðu söylenir. Ve seçme iþleminden sonra üzerinde deðiþiklik yapýlmýþ fakat kayýt için hazýrlanmamýþsa, deðiþtirilmiþ olarak nitelenir.

## Git'in Kurulumu ##

Gelin Git'i kullanmaya baþlayalým. Her þeyden önce, Git'i kurmanýz gerekiyor. Bunu iki baþlýca biçimde gerçekleþtirebilirsiniz: kaynak kodundan ya da platformunuz için halihazýrda varolan paketi kullanarak kurabilirsiniz.

### Kaynak Kodundan Kurulum ###

Yapabiliyorsanýz, Git'i kaynak kodundan kurmak kullanýþlýdýr, çünkü böylece en yeni versiyonunu edinebilirsiniz. Git'in her yeni versiyonu yararlý kullanýcý arayüzü güncellemeleri içerir, dolayýsýyla en son versiyonu kurmak, eðer yazýlým derlemek konusunda sýkýntý yaþamayacaðýnýzý düþünüyorsanýz, en iyi yoldur. Ayrýca kimi zaman, Linux daðýtýmlarý yazýlýmlarýn çok eski paketlerini içerirler; dolayýsýyla, çok güncel bir daðýtýma sahip deðilseniz ya da terstaþýmalar (_backport_) kullanmýyorsanýz, kaynak koddan kurulum en mantýklý seçenek olabilir.

Git'i kurmak için, Git'in baðýmlý olduðu þu kütüphanelerin sisteminizde bulunmasý gerekiyor: curl, zlib, openssl, expat, ve libiconv. Örneðin, (Fedora gibi) yum aracýna ya da (Debian tabanlý sistemler gibi) apt-get aracýna sahip bir sistemdeyseniz, baðýmlýlýklarý kurmak için þu komutlardan birini kullanabilirsiniz:

	$ yum install curl-devel expat-devel gettext-devel \
	  openssl-devel zlib-devel

	$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
	  libz-dev libssl-dev
	
Bütün gerekli baðýmlýlýklarý yükledikten sonra Git web sitesinden en son bellek kopyasýný edinebilirsiniz:

	http://git-scm.com/download

Sonra derleyip kurabilirsiniz:

	$ tar -zxf git-1.7.2.2.tar.gz
	$ cd git-1.7.2.2
	$ make prefix=/usr/local all
	$ sudo make prefix=/usr/local install

Bu adýmdan sonra, Git'teki yeni güncellemeleri Git'in kendisini kullanarak edinebilirsiniz:

	$ git clone git://git.kernel.org/pub/scm/git/git.git
	
### Linux'ta Kurulum ###

Git'i Linux sisteminize paket kurucu yardýmýyla kurmak istiyorsanýz, bunu genellikle daðýtýmýnýzla birlikte gelen temel paket yönetim aracýyla yapabilirsiniz. Fedora kullanýcýsýysanýz, yum'u kullanabilirsiniz:

	$ yum install git-core

Ubuntu gibi Debian-tabanlý bir sistemdeyseniz, apt-get'i kullanabilirsiniz:

	$ apt-get install git

### Mac'te Kurulum ###

Git'i Mac'te kurmak için iki kolay yol vardýr. En kolayý, Google Code sayfasýndan indirebileceðiniz görsel Git yükleyicisini kullanmaktýr (bkz. Figür 1-7).

	http://code.google.com/p/git-osx-installer

Insert 18333fig0107.png 
Figür 1-7. Git OS X yükleyicisi.

Diðer baþlýca yol, Git'i MacPorts (`http://www.macports.org`) vasýtasýyla kurmaktýr. MacPorts halihazýrda kurulu bulunuyorsa Git'i þu komutla kurabilirsiniz:

	$ sudo port install git-core +svn +doc +bash_completion +gitweb

Bütün ek paketleri kurmanýz þart deðil, ama Git'i Subversion yazýlým havuzlarýyla kullanmanýz gerekecekse en azýndan +svn'i edinmelisiniz.

### Windows'ta Kurulum ###

Git'i Windows'da kurmak oldukça kolaydýr. mysysGit projesi en basit kurulum yöntemlerinden birine sahip. Çalýþtýrýlabilir kurulum dosyasýný GitHub sayfasýndan indirip çalýþtýrmanýz yeterli:

	http://msysgit.github.com/

Kurulum tamamlandýðýnda hem (daha sonra iþe yarayacak olan SSH istemcisini de içeren) komut satýrý nüshasýna hem de standart kullanýcý arayüzüne sahip olacaksýnýz.

## Ýlk Ayarlamalar ##

Artýk Git sisteminizde kurulu olduðuna göre, onu ihtiyacýnýza göre uyarlamak için bazý düzenlemeler yapabilirsiniz. Bunlarý yalnýzca bir kere yapmanýz yeterli olacaktýr: güncellemelerden etkilenmeyeceklerdir. Ayrýca istediðiniz zaman komutlarý yeniden çalýþtýrarak ayarlarý deðiþtirebilirsiniz.

Git, Git'in nasýl görüneceðini ve çalýþacaðýný belirleyen bütün konfigürasyon deðiþkenlerini görmenizi ve deðiþtirmenizi saðlayan git config adýnda bir araçla birlikte gelir. Bu deðiþkenler üç farklý yerde depolanabilirler:

*	`/etc/gitconfig` dosyasý: Sistemdeki bütün kullanýcýlar ve onlarýn bütün yazýlým havuzlarý için geçerli olan deðerleri içerir. `git config` komutunu `--system` seçeneðiyle kullanýrsanýz, araç bu dosyadan okuyup deðiþiklikleri bu dosyaya kaydedecektir.
*	`~/.gitconfig` dosyasý: Kullanýcýya özeldir. `--global` seçeneðiyle Git'in bu dosyadan okuyup deðiþiklikleri bu dosyaya kaydetmesini saðlayabilirsiniz.
*	kullanmakta olduðunuz yazýlým havuzundaki git klasöründe bulunan config dosyasý (yani `.git/config`): Söz konusu yazýlým havuzuna özeldir. Her düzeydeki ayarlar kendisinden önce gelen düzeydeki ayarlarý gölgede býrakýr (_override_), dolayýsýyla `.git/config`'deki deðerler `/etc/gitconfig`'deki deðerlerden daha baskýndýr.

Git, Windows sistemlerde `$HOME` klasöründeki (çoðu kullanýcý için `C:\Documents and Settings\$USER` klasörüdür) `.gitconfig` dosyasýna bakar (Ç.N.: Windows kullanýcý klasörüne %HOMEPATH% çevresel deðiþkenini kullanarak ulaþabilirsiniz). Git, Windows sistemlerde de /etc/gitconfig dosyasýný arar fakat bu konum, Git kurulumu sýrasýnda seçtiðiniz MSys kök dizinine göredir.

### Kimliðiniz ###

Git'i kurduðunuzda yapmanýz gereken ilk þey adýnýzý ve e-posta adresinizi ayarlamaktýr. Bunun önemli olmasýnýn nedeni her bir Git kaydýnýn bu bilgiyi kullanýyor olmasý ve bu bilgilerin dolaþýma soktuðunuz kayýtlara deðiþmez biçimde iþlenmesidir.

	$ git config --global user.name "John Doe"
	$ git config --global user.email johndoe@example.com

Yinelemek gerekirse, `--global` seçeneðini kullandýðýnýzda bunu bir kez yapmanýz yeterli olacaktýr, çünkü Git, o sistemde yapacaðýnýz her iþlem için bu bilgileri kullanacaktýr. Ýsmi ya da e-posta adresini projeden projeye deðiþtirmek isterseniz, komutu deðiþiklik yapmak istediðiniz proje klasörünün içinde `--global` seçeneði olmadan çalýþtýrabilirsiniz.

### Editörünüz ###

Kimlik ayarlarýnýzý yaptýðýnýza göre, Git sizden bir mesaj yazmanýzý istediðinde kullanacaðýnýz editörle ilgili düzenlemeyi yapabilirsiniz. Aksi belirtilmedikçe Git sisteminizdeki öntanýmlý (_default_) editörü kullanýr, bu da genellikle Vi ya da Vim'dir. Emacs gibi baþka bir metin editörü kullanmak isterseniz, þu komutu kullanabilirsiniz:

	$ git config --global core.editor emacs
	
### Dosya Karþýlaþtýrma Aracýnýz ###

Düzenlemek isteyeceðiniz bir diðer yararlý ayar da birleþtirme (_merge_) uyuþmazlýklarýný gidermek için kullanacaðýnýz araçla ilgilidir. vimdiff aracýný seçmek için þu komutu kullanabilirsiniz:

	$ git config --global merge.tool vimdiff

Git kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge ve opendiff araçlarýný kabul eder. Dilerseniz özel bir araç için de ayarlamalar yapabilirsiniz (bununla ilgili daha fazla bilgi için bkz. 7. Bölüm).

### Ayarlarýnýzý Gözden Geçirmek ###

Ayarlarýnýzý gözden geçirmek isterseniz, Git'in bulabildiði bütün ayarlarý listelemek için `git config --list` komutunu kullanabilirsiniz.

	$ git config --list
	user.name=Scott Chacon
	user.email=schacon@gmail.com
	color.status=auto
	color.branch=auto
	color.interactive=auto
	color.diff=auto
	...

Bir ayarý birden çok kez görebilirsiniz; bunun nedeni Git'in ayný ayarý deðiþik dosyalardan (örneðin `etc/gitconfig` ve `~/.gitconfig`'den) okumuþ olmasýdýr. Bu durumda Git gördüðü her bir tekil ayar için en son bulduðu deðeri kullanýr.

`git config {ayar}` komutunu kullanarak Git'ten bir ayarýn deðerini görüntülemesini de isteyebilirsiniz:

	$ git config user.name
	Scott Chacon

## Yardým Almak ##

Git'i kullanýrken yardýma ihtiyacýnýz olursa, herhangi bir Git komutunun yardým kýlavuzu sayfasýný (_manpage_) üç deðiþik biçimde görüntüleyebilirsiniz:

	$ git help <eylem>
	$ git <eylem> --help
	$ man git-<eylem>

Örneðin, config komutu için kýlavuzu sayfasýný görüntülemek için þu komutu çalýþtýrabilirsiniz:

	$ git help config

Bu komutlarýn güzel tarafý onlara her an, að baðlantýnýz olmasa bile ulaþabiliyor olmanýzdýr. Eðer kýlavuz sayfalarý ve bu kitap yeterli olmazsa ve kiþisel yardýma ihtiyaç duyacak olursanýz, Freenode IRC sunucusundaki (irc.freenode.net) `#git` ya da `#github` kanallarýna baðlanmayý deneyebilirsiniz. Bu kanallar Git hakkýnda derin bilgiye sahip yüzlerce kiþi tarafýndan düzenli olarak ziyaret edilmektedir.

## Özet ##

Artýk Git'in ne olduðu ve kullanmýþ olabileceðiniz MSKS'den hangi açýlardan farklý olduðu konusunda temel bilgilere sahipsiniz. Ayrýca sisteminizde, sizin kimlik bilgilerinize göre ayarlanmýþ bir Git kurulumu bulunuyor. Þimdi Git'in temellerini öðrenme zamaný.