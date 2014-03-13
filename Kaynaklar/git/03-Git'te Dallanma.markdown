# Git'te Dallanma #

Neredeyse her SKS'nin bir dallanma (_branching_) iþlevi vardýr. Dallanma, ana geliþtirme çizgisinden sapmak ve iþinizi o ana geliþtirme çizgisine bulaþmadan devam ettirmek anlamýna gelir. Çoðu SKS aracýnda bu pahalý bir süreçtir; kaynak kod klasörünüzün yeni bir kopyasýný yapmanýzý gerektirir ve büyük projelerde çok zaman alýr.

Bazýlarý Git'in dallanma modelinin onun "en vurucu özelliði" olduðunu söylerler; bu özelliðin SKS topluluðu içinde Git'i ayrý bir yere koyduðu doðrudur. Onu bu kadar özel yapan nedir? Git'te dallanmalar çok kolay ve neredeyse anlýktýr, üstelik farklý dallar arasýnda gidip gelmek de bir o kadar hýzlýdýr. Çoðu SKS'den farklý olarak Git dallanma ve birleþtirmenin (_merge_) sýk (belki de günde birkaç kez) gerçekleþeceði bir iþ akýþýný teþvik eder. Bu özelliði anlayýp bu konuda ustalaþmak size son derece becerikli ve eþsiz bir araç saðlayabileceði gibi çalýþma biçiminizi de bütünüyle deðiþtirebilir.

## Dal Nedir? ##

Git'in dallanma iþlemini nasýl yaptýðýný gerçekten anlayabilmek için geriye doðru bir adým atýp Git'in verilerini nasýl depoladýðýna bakmamýz gerekiyor. 1. Bölüm'den hatýrlayabileceðiniz üzere, Git verilerini bir dizi deðiþiklik olarak deðil bir dizi bellek kopyasý olarak depolar.

Git'te bir kayýt yaptýðýnýzda, Git, kayda hazýrladýðýnýz içeriðin bellek kopyasýna iþaret eden imleci, yazar ve mesaj üstverisini ve söz konusu kaydýn atalarýný gösteren sýfýr ya da daha fazla imleci (ilk kayýt için sýfýr ata, normal bir kayýt için bir ata, iki ya da daha fazla dalýn birleþtirilmesinden oluþan bir kayýt için birden çok ata) içeren bir kayýt nesnesini depolar.

Bunu görselleþtirmek için, üç dosyadan oluþan bir klasörünüzün olduðunu ve bu üç dosyayý da kayýt için hazýrladýðýnýzý varsayalým. Dosyalarý kayda hazýrlamak her bir dosyanýn sýnama toplamýný alýr (1. Bölüm'de söz ettiðimiz SHA-1 özeti), dosyanýn o sürümünü Git yazýlým havuzunda depolar (Git'te bunlara _blob_ denir (Ç.N. _blob_ Türkçe'ye damla ya da topak diye çevrilebilir, fakat kelimeyi olduðu gibi kullanmanýn daha anlaþýlýr olacaðýný düþündük.)) ve sýnama toplamýný hazýrlýk alanýna ekler:

	$ git add README test.rb LICENSE
	$ git commit -m 'initial commit of my project'

`git commit` komutunu çalýþtýrarak bir kayýt oluþturduðunuzda, Git her bir alt klasörün (bu örnekte yalnýzca kök klasörün) sýnama toplamýný alýr ve bu aðaç yapýsýndaki bu nesneleri yazýlým havuzunda depolar. Git, daha sonra, üst veriyi ve ihtiyaç duyulduðunda bellek kopyasýnýn yeniden yaratabilmek için aðaç yapýsýndaki nesneyi gösteren bir imleci içeren bir kayýt nesneyi yaratýr.

Þimdi, Git yazýlým havuzunuzda beþ nesne bulunuyor: üç dosyanýzýn her biri için bir içerik _blob_'u, klasörün içeriðini listeleyen ve hangi dosyanýn hangi _blob_'da depolandýðý bilgisini içeren bir aðaç nesnesi ve o aðaç nesnesini gösteren bir imleci ve bütün kayýt üstverisini içeren bir kayýt nesnesi. Kavramsal olarak, Git yazýlým havuzunuzdaki veri Figür 3-1'deki gibi görünür.

Insert 18333fig0301.png 
Figür 3-1. Tek kayýtlý yazýlým havuzundaki veri.

Yeniden deðiþiklik yapýp kaydederseniz, yeni kayýt kendisinden hemen önce gelen kaydý gösteren bir imleci de depolar. Ýki ya da daha fazla kaydýn sonunda tarihçeniz Figür 3-2'deki gibi görünür.

Insert 18333fig0302.png 
Figür 3-2. Birden çok kayýt sonunda Git nesne verisi.

Git'te bir dal, bu kayýtlardan birine iþaret eden, yer deðiþtirebilen kývrak bir imleçten ibarettir. Git'teki varsayýlan dal adý `master`'dýr. Ýlk kaydý yaptýðýnýzda, son yaptýðýnýz kaydý gösteren bir `master` dalýna sahip olursunuz. Her kayýt yaptýðýnýzda dal otomatik olarak son kaydý göstermek üzere hareket eder.

Insert 18333fig0303.png 
Figür 3-3. Dal kayýt verisinin tarihçesini gösteriyor.

Yeni bir dal oluþturduðunuzda ne olur? Yeni kayýtlarla ilerlemenizi saðlayan yeni bir imleç yaratýlýr. Söz gelimi, `testing` adýnda yeni bir dal oluþturalým. Bunu, `git branch` komutuyla yapabilirsiniz:

	$ git branch testing

Bu, þu an bulunduðunuz kayýttan hareketle yeni bir imleç yaratýr (bkz. Figür 3-4).


Insert 18333fig0304.png 
Figür 3-4. Birden çok dal kayýt verisinin tarihçesini gösteriyor.

Git þu an hangi dalýn üzerinde olduðunuzu nereden biliyor? `HEAD` adýnda özel bir imleç tutuyor. Unutmayýn, buradaki `HEAD` Subversion ya da CVS gibi baþka SKS'lerden alýþýk olduðunuz `HEAD`'den çok farklýdýr. Git'te bu, üzerinde bulunduðunuz yerel dalý gösterir. Bu örnekte hâlâ `master` dalýndasýnýz. `git branch` komutu yalnýzca yeni bir dal yarattý —o dala atlamadý (bkz. Figür 3-5).

Insert 18333fig0305.png 
Figür 3-5. HEAD dosyasý üzerinde bulunduðunuz dalý gösteriyor.

Varolan bir dala atlamak için `git checkout` komutunu çalýþtýrmalýsýnýz. Gelin, `testing` dalýna atlayalým:

	$ git checkout testing

Bu, `HEAD`'in `testing` dalýný göstermesiyle sonuçlanýr (bkz. Figür 3-6).

Insert 18333fig0306.png
Figür 3-6. Dal deðiþtirdiðinizde HEAD üzerinde olduðunuz dalý gösterir.

Bunun ne önemi var? Gelin bir kayýt daha yapalým:

	$ vim test.rb
	$ git commit -a -m 'made a change'

Figür 3-7 sonucu resmediyor.

Insert 18333fig0307.png 
Figür 3-7. HEAD'in gösterdiði dal her kayýtla ileri doðru hareket eder.

Burada ilginç olan `testing` dalý ilerlediði halde `master` dalý hâlâ dal deðiþtirmek için `git checkout` komutunu çalýþtýrdýðýnýz zamanki yerinde duruyor. Gelin yeniden `master` dalýna dönelim.

	$ git checkout master

Figür 3-8 sonucu gösteriyor.

Insert 18333fig0308.png 
Figür 3-8. Seçme (checkout) iþlemi yapýldýðýnda HEAD baþka bir dalý gösterir.

Örnekteki komut iki þey yaptý. `HEAD` imlecini yeniden `master` dalýný gösterecek þekilde hareket ettirdi ve çalýþma klasörünüzdeki dosyalarý `master`'ýn gösterdiði bellek kopyasýndaki hallerine getirdi. Bu demek oluyor ki, bu noktada yapacaðýnýz deðiþiklikler projenin daha eski bir sürümünü baz alacak. Özünde, baþka bir yöne gidebilmek için `testing` dalýnda yaptýðýnýz deðiþiklikleri geçici olarak geri almýþ oldunuz.

Gelin bir deðiþiklik daha yapýp kaydedelim:

	$ vim test.rb
	$ git commit -a -m 'made other changes'

Þimdi proje tarihçeniz iki ayrý dala ýraksadý (bkz. Figür 3-9). Yeni bir dal yaratýp ona geçtiniz, bazý deðiþiklikler yaptýnýz; sonra ana dalýnýza geri döndünüz ve baþka bazý deðiþiklikler yaptýnýz. Bu iki deðiþiklik iki ayrý dalda birbirinden yalýtýk durumdalar: iki dal arasýnda gidip gelebilir, hazýr olduðunuzda bu iki dalý birleþtirebilirsiniz. Bütün bunlarý yalnýzca `branch` ve `checkout` komutlarýný kullanarak yaptýnýz.

Insert 18333fig0309.png 
Figür 3-9. Dal tarihçeleri birbirinden ýraksadý.

Git'te bir dal iþaret ettiði kaydýn 40 karakterlik SHA-1 sýnama toplamýný içeren basit bir dosyadan ibaret olduðu için dallarý yaratmak ve yok etmek oldukça masrafsýzdýr. Yeni bir dal yaratmak bir dosyaya 41 karakter (40 karakter ve bir satýr sonu) yazmak kadar hýzlýdýr.

Bu, çoðu SKS'nin bütün proje dosyalarýný yeni bir klasöre kopyalamayý gerektiren dallanma yaklaþýmýyla keskin bir karþýtlýk içindedir. Söz konusu yaklaþýmda projenin boyutlarýna baðlý olarak dallanma saniyeler, hatta dakikalar sürebilir; Git'te ise bu süreç her zaman anlýktýr. Ayrýca, kayýt yaparken ata kayýtlarý da kaydettiðimiz için birleþtirme sýrasýnda uygun bir ortak payda bulma iþi de otomatik olarak ve genellikle oldukça kolayca halledilir. Bu özellikler yazýlýmcýlarý sýk sýk dal yaratýp kullanmaya teþvik eder.

Neden böyle olmasý gerektiðine yakýndan bakalým.

## Dallanma ve Birleþtirmenin Temelleri ##

Gelin, basit bir örnekle, gerçek hayatta kullanacaðýnýz bir dallanma ve birleþtirme iþleyiþinin üstünden geçelim. Þu adýmlarý izleyeceksiniz:

1. Bir web sitesi üzerine çalýþýyor olun.
2. Üzerinde çalýþtýðýnýz yeni bir iþ parçasý için bir dal yaratýn.
3. Çalýþmalarýnýzý bu dalda gerçekleþtirin.

Bu noktada, sizden kritik önemde baþka sorun üzerinde çalýþýp hýzlýca bir yama hazýrlamanýz istensin. Bu durumda þunlarý yapacaksýnýz:

1. Ana dalýnýza geri dönün.
2. Yamayý eklemek için yeni bir dal oluþturun.
3. Testleri tamamlandýktan sonra yama dalýný ana dalla birleþtirip yayýna verin.
4. Çalýþmakta olduðunuz iþ parçasý dalýna geri dönüp çalýþmaya devam edin.

### Dallanmanýn Temelleri ###

Önce, diyelim ki bir projede çalýþýyorsunuz ve halihazýrda birkaç tane kaydýnýz var (bkz. Figür 3-10).

Insert 18333fig0310.png 
Figür 3-10. Kýsa ve basit bir kayýt tarihçesi.

Þirketinizin kullandýðý sorun izleme programýndaki #53 numaralý sorun üzerinde çalýþmaya karar verdiniz. Açýklýða kavuþturmak için söyleyelim: Git herhangi bir sorun izleme programýna baðlý deðildir; ama #53 numaralý sorun üzerinde çalýþmak istediðiniz baþý sonu belli bir konu olduðu için, çalýþmanýzý bir dal üzerinde yapacaksýnýz. Bir dalý yaratýr yaratmaz hemen ona geçiþ yapmak için `git checout` komutunu `-b` seçeneðiyle birlikte kullanabilirsiniz:

	$ git checkout -b iss53
	Switched to a new branch "iss53"

Bu, aþaðýdaki iki komutun yerine kullanabileceðiniz bir kýsayoldur:

	$ git branch iss53
	$ git checkout iss53

Figür 3-11 sonucu resmediyor.

Insert 18333fig0311.png 
Figür 3-11. Yeni bir dal imleci yaratmak.

Web sitesi üzerinde çalýþýp bazý kayýtlar yapýyorsunuz. Bunu yaptýðýnýzda `iss53` dalý ilerliyor, çünkü seçtiðiniz dal o (yani `HEAD` onu gösteriyor; bkz. Figür 3-12).

	$ vim index.html
	$ git commit -a -m 'added a new footer [issue 53]'

Insert 18333fig0312.png 
Figür 3-12. Çalýþamýz sonucunda iss53 dalý ilerledi.

Þimdi, sizden web sitesindeki bir sorun için acilen bir yama hazýrlamanýz istensin. Git kullanýyorsanýz, yamayý daha önce `iss53` dalýnda yaptýðýnýz yaptýðýnýz deðiþikliklerle birlikte yayýna sokmanýz gerekmez; yama üzerinde çalýþmaya baþlamadan önce söz konusu deðiþiklikleri geri alýp yayýndaki web sitesini kaynak koduna ulaþabilmek için fazla çabalamanýza da gerek yok. Tek yapmanýz gereken `master` dalýna geri dönmek.

Ama, bunu yapmadan önce þunu belirtmekte yarar var: eðer çalýþma klasörünüzde ya da kayda hazýrlýk alanýnda seçmek (_checkout_) istediðiniz dalla uyuþmazlýk gösteren kaydedilmemiþ deðiþiklikler varsa, Git dal deðiþtirmenize izin vermeyecektir. Dal deðiþtirirken çalýþma alanýnýzý temiz olmasý en iyisidir. Bunun üstesinden gelmek için baþvurulabilecek yollarý (zulalama ve kayýt deðiþtirme gibi) daha sonra inceleyeceðiz. Þimdilik, bütün deðiþikliklerinizi kaydettiniz, dolayýsýyla `master` dalýna geçiþ yapabilirsiniz.

	$ git checkout master
	Switched to branch "master"

Bu noktada, çalýþma klasörünüz #53 numaralý sorun üzerinde çalýþmaya baþlamadan hemen önceki halindedir ve yamayý hazýrlamaya odaklanabilirsiniz. Burasý önemli: Git, çalýþma klasörünüzü seçtiðiniz dalýn gösterdiði kaydýn bellek kopyasýyla ayný olacak þekilde ayarlar. Dal, son kaydýnýzda nasýl görünüyorsa çalýþma klasörünü o hale getirebilmek için otomatik olarak dosyalarý ekler, siler ve deðiþtirir.

Sýrada, hazýrlanacak yama var. Þimdi yama üzerinde çalýþmak için bir `hotfix` dalý oluþturalým (bkz. Figür 3-13):

	$ git checkout -b 'hotfix'
	Switched to a new branch "hotfix"
	$ vim index.html
	$ git commit -a -m 'fixed the broken email address'
	[hotfix]: created 3a0874c: "fixed the broken email address"
	 1 files changed, 0 insertions(+), 1 deletions(-)

Insert 18333fig0313.png 
Figür 3-13. hotfix dalý master dalýný baz alýyor.

Testlerinizi uygulayabilir, yamanýzýn istediðiniz gibi olduðundan emin olduktan sonra yayýna sokabilmek için `master` dalýyla birleþtirebilirsiniz. Bunun için `git merge` komutu kullanýlýr:

	$ git checkout master
	$ git merge hotfix
	Updating f42c576..3a0874c
	Fast forward
	 README |    1 -
	 1 files changed, 0 insertions(+), 1 deletions(-)

Birleþtirme çýktýsýndaki "Fast forward" ifadesine dikkat. Birleþtirdiðiniz dalýn gösterdiði kayýt, üstünde bulunduðunuz dalýn doðrudan devamý olduðundan, Git yalnýzca imleci ileri alýr. Baþka bir deyiþle, bir kaydý, kendi tarihçesinde geri giderek ulaþýlabilecek bir baþka kayýtla birleþtiriyorsanýz, Git ýraksayan ve birleþtirilmesi gereken herhangi bir þey olmadýðý için iþleri kolaylaþtýrýp imleci ileri alýr —buna "fast forward" (hýzlý ileri alma) denir.

Yaptýðýnýz deðiþiklik artýk `master` dalý tarafýndan iþaret edilen kaydýn bellek kopyasýndadýr ve yayýmlanabilir (bkz. Figür 3-14).

Insert 18333fig0314.png 
Figür 3-14. Birleþtirmeden sonra master dalýnýz hotfix dalýnýzla ayný yeri gösterir.

Bu çok önemli yama yayýmlandýktan sonra, kaldýðýnýz yere geri dönebilirsiniz. Fakat önce `hotfix` dalýný sileceksiniz, çünkü artýk ona ihtiyacýnýz kalmadý —`master` dalý ayný yeri gösteriyor. `git branch` komutunu `-d` seçeneðiyle birlikte kullanarak silme iþlemini yapabilirsiniz:

	$ git branch -d hotfix
	Deleted branch hotfix (3a0874c).

Þimdi kaldýðýnýz yere geri dönebilir ve #53 numaralý sorun üzerinde çalýþmaya devam edebilirsiniz (bkz. 3-15).

	$ git checkout iss53
	Switched to branch "iss53"
	$ vim index.html
	$ git commit -a -m 'finished the new footer [issue 53]'
	[iss53]: created ad82d7a: "finished the new footer [issue 53]"
	 1 files changed, 1 insertions(+), 0 deletions(-)

Insert 18333fig0315.png 
Figür 3-15. iss53 dalýnýz baðýmsýz olarak ilerleyebilir.

Þunu belirtmekte yarar var: `hotfix` dalýnda yaptýðýnýz düzeltme `iss53` dalýndaki dosyalarda bulunmuyor. Eðer bu deðiþikliði çekmek isterseniz, `git merge master` komutunu çalýþtýrarak `master` dalýnýzý `iss53` dalýnýzla birleþtirebilirsiniz; alternatif olarak `iss53` dalýndaki deðiþiklikleri `master`dalýyla birleþtirmeye hazýr hale getirene kadar bekleyebilirsiniz.

### Birleþtirmenin Temelleri ###

Diyelim ki #53 numaralý sorunla ilgili çalýþmanýzý tamamladýnýz ve `master` dalýyla birleþtirmeye hazýrsýnýz. Bunu yapabilmek için `iss53` dalýnýzý, ayný `hotfix` dalýný yaptýðýnýz gibi birleþtireceksiniz. Bütün yapmanýz gereken birleþtirmeyi gerçekleþtirmek istediðiniz dalý seçmek (_checkout_) ve `git merge` komutunu çalýþtýrmak:

	$ git checkout master
	$ git merge iss53
	Merge made by recursive.
	 README |    1 +
	 1 files changed, 1 insertions(+), 0 deletions(-)

Bu daha önce yaptýðýnýz `hotfix` birleþtirmesinden biraz farklý görünüyor. Burada, kayýt tarihçeniz daha eski bir noktadan ýraksamýþtý. Üzerinde bulunduðunuz dalýn gösterdiði kayýt birleþtirmekte olduðunuz dalýn doðrudan atasý olmadýðýndan Git'in biraz iþ yapmasý gerekiyor. Bu örnekte Git, iki dalýn en uç noktasý ve ikisinin ortak atasýnýn kullanýldýðý üç taraflý basit bir birleþtirme yapýyor. Figür 3-16, bu birleþtirmede kullanýlan üç farklý bellek kopyasýný vurguluyor.

Insert 18333fig0316.png 
Figür 3-16. Git, dallarý birleþtirmek için en uygun ortak atayý buluyor.

Git, yalnýzca dal imlecini ileri kaydýrmak yerine üç taraflý birleþtirmenin sonucunda ortaya çýkan bellek kopyasý için otomatik bir kayýt oluþturuyor (bkz. Figür 3-17). Buna birleþtirme kaydý denir ve özelliði birden çok atasýnýn olmasýdýr.

Git'in en uygun ortak atayý otomatik olarak bulduðunu vurgulamakta yarar var; bu kullanýcýnýn en uygun ortak paydayý bulmak zorunda olduðu CVS ve Subversion'daki durumdan (1.5 sürümünden önceki haliyle) farklýdýr. Bu Git kullanarak birleþtirme yapmayý söz konusu diðer sistemlere göre çok daha kolay bir hale getirir.

Insert 18333fig0317.png 
Figür 3-17. Git, otomatik olarak, birleþtirilmiþ çalýþmayý içeren yeni bir kayýt nesnesi yaratýr.

Çalýþmanýz birleþtirildiðine göre, artýk `iss53` dalýna ihtiyacýnýz kalmadý. Dalý silip, sorun izleme sisteminizdeki sorunu da kapatabilirsiniz:

	$ git branch -d iss53

### Temel Birleþtirme Uyuþmazlýklarý ###

Zaman zaman bu süreç o kadar da pürüzsüz ilerlemez. Eðer ayný dosyanýn ayný bölümünü her iki dalda da deðiþtirmiþseniz, Git temiz bir birleþtirme yapamaz. #53 numaralarý sorun için hazýrladýðýnýz düzeltme `hotfix`le ayný yazýlým parçasýný deðiþtiriyorsa, þuna benzer bir birleþtirme uyuþmazlýðýyla karþýlaþýrsýnýz:

	$ git merge iss53
	Auto-merging index.html
	CONFLICT (content): Merge conflict in index.html
	Automatic merge failed; fix conflicts and then commit the result.

Burada Git otomatik olarak yeni bir birleþtirme kaydý oluþturmadý. Sizin uyuþmazlýðý çözmenizi beklemek için sürece ara verdi. Bir birleþtirme uyuþmazlýðýndan sonra hangi dosyalarýn birleþtirilmemiþ olduðunu görmek için `git status` komutunu çalýþtýrabilirsiniz.

	[master*]$ git status
	index.html: needs merge
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	unmerged:   index.html
	#

Birleþtirme uyuþmazlýðý henüz çözümlenmemiþ her þey _unmerged_ (birleþtirilmemiþ) olarak gösterilecektir. Git, dosyalarý açýp uyuþmazlýklarý çözümleyebilmeniz için standart uyuþmazlýk çözümleme iþaretçileri koyar. Dosyanýzda þuna benzer bir bölümle karþýlaþýrsýnýz:

	<<<<<<< HEAD:index.html
	<div id="footer">contact : email.support@github.com</div>
	=======
	<div id="footer">
	  please contact us at support@github.com
	</div>
	>>>>>>> iss53:index.html

Burada , `HEAD`deki sürüm (ki bu `master` dalýndaki sürümdür çünkü birleþtirme komutunu bu daldan çalýþtýrdýnýz) üstte, (`=======` iþaretinin üstündeki her þey), `iss53` dalýndaki sürüm ise altta gösterilmektedir. Uyuþmazlýðý çözümleyebilmek için bu ikisinden birini seçmeli, ya da birleþtirmeyi istediðiniz gibi kendiniz düzenlemelisiniz. Söz gelimi, uyuþmazlýðý çözmek için bütün bu kod bloðunun yerine þunu yerleþtirebilirsiniz:

	<div id="footer">
	please contact us at email.support@github.com
	</div>

Çözümlemede iki taraftan da bir þeyler var ve `<<<<<<<`, `=======`, ve `>>>>>>>` iþaretlerini içeren satýrlar tamamen silinmiþ durumda. Uyuþmazlýk olan her bir dosyadaki her bir uyuþmazlýk bloðunu çözümledikten sonra her dosyanýn üzerinde `git add` komutunu çalýþtýrarak, uyuþmazlýðýn o dosya için çözülmüþ olduðunu belirtebilirsiniz. Bir dosyayý ayda hazýrlamak o dosyayý uyuþmazlýðý çözümlenmiþ olarak iþaretler.
Uyuþmazlýklarý çözümlemek için görsel bir araç kullanmak isterseniz `git mergetool` komutunu çalýþtýrabilirsiniz; bu komut size tek tek herbir uyuþmazlýðý gösterecek uygun bir birleþtirme aracýný çalýþtýrýr:

	$ git mergetool
	merge tool candidates: kdiff3 tkdiff xxdiff meld gvimdiff opendiff emerge vimdiff
	Merging the files: index.html

	Normal merge conflict for 'index.html':
	  {local}: modified
	  {remote}: modified
	Hit return to start merge resolution tool (opendiff):

Varsayýlan aracýn dýþýnda bir araç kullanmak isterseniz (Git, Mac'te çalýþtýðým için bu örnekte `opendiff`'i seçti), Git'in desteklediði bütün birleþtirme araçlarýnýn listesini en üstte “merge tool candidates” yazýsýndan hemen sonra görebilirsiniz. Kullanmak istediðiniz aracýn adýný yazýn. 7. Bölüm'de kendi çalýþma ortamýnýz için varsayýlan deðeri nasýl deðiþtirebileceðinizi inceleyeceðiz.

Birleþtirme aracýný kapattýktan sonra, Git size birleþtirmenin baþarýlý olup olmadýðýný soracaktýr. Eðer baþarýlý olduðunu söylerseniz, sizin yerinize dosyayý kayda hazýrlayýp çözümlenmiþ olarak iþaretler.

Bütün uyuþmazlýklarýn çözümlendiðinden emin olmak için tekrar `git status` komutunu çalýþtýrabilirsiniz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   index.html
	#

Durumdan memnunsanýz ve uyuþmazlýðý olan bütün dosyalarýn kayda hazýrlandýðýndan eminseniz, `git commit`'i kullanarak birleþtirme kaydýný tamamlayabilirsiniz. Öntanýmlý kayýt mesajý þöyle görünür:

	Merge branch 'iss53'

	Conflicts:
	  index.html
	#
	# It looks like you may be committing a MERGE.
	# If this is not correct, please remove the file
	# .git/MERGE_HEAD
	# and try again.
	#

Ýleride bu birleþtirme iþlemini inceleyecek olanlar için yararlý olacaðýný düþünüyorsanýz bu kayýt mesajýný ayrýntýlandýrabilirsiniz —eðer aþikâr deðilse, birleþtirmeyi neden yaptýðýnýzý, ve birleþtirmede neler yaptýðýnýzý açýklayabilirsiniz.

## Dal Yönetimi ##

Dal yaratma, birleþtirme ve silme iþlemlerini yaptýðýmýza göre, gelin þimdi de dallar üzerinde çalýþýrken iþimize yarayacak kimi dal yönetim araçlarýna göz atalým.

`git branch` komutu dal yaratmak ve silmekten fazlasýný yapar. Bu komutu hiçbir seçenek kullanmadan çalýþtýrýrsanýz, mevcut dallarýnýzýn bir listesini görürsünüz:

	$ git branch
	  iss53
	* master
	  testing

`master` dalýnýn önündeki `*` karakterine dikkatinizi çekmiþtir: bu, o dalý seçmiþ olduðunuzu (_checkout_) gösteriyor. Yani, bu noktada bir kayýt yapacak olursanýz, yeni deðiþikliðiniz `master` dalýný ileri götürecek. Her bir dalýn en son kaydýnýn ne olduðunu görmek isterseniz `git branch -v` komutunu çalýþtýrabilirsiniz:

	$ git branch -v
	  iss53   93b412c fix javascript issue
	* master  7a98805 Merge branch 'iss53'
	  testing 782fd34 add scott to the author list in the readmes

Dallarýnýzýn ne durumda olduðunu incelerken yararlý olacak bir baþka þey de, hangi dallarýn üzerinde bulunduðunuz dalla birleþtirilip hangisinin birleþtirilmediðini görmek olabilir. `--merged` ve `--no-merge` seçenekleri Git'in 1.5.6 sürümünden itibaren kullanýma sunulmuþtur. Hangi dallarýn üzerinde bulunduðunuz dalla birleþtirilmiþ olduðunu görmek için `git branch --merged` komutunu kullanabilirsiniz:

	$ git branch --merged
	  iss53
	* master

`iss53` dalýný daha önce birleþtirdiðiniz için listede görüyorsunuz. Bu listede önünde `*` olmayan dallarý `git branch -d` komutuyla silebilirsiniz; onlardaki deðiþiklikleri zaten baþka bir dalla birleþtirdiðiniz için, herhangi bir kaybýnýz olmaz.

Henüz birleþtirmediðiniz deðiþikliklerin bulunduðu dallarý görmek için `git branch --no-merged` komutunu çalýþtýrabilirsiniz:

	$ git branch --no-merged
	  testing
Burada diðer dalý görüyorsunuz. Bu dalda henüz birleþtirmediðiniz deðiþiklikler bulunduðu için `git branch -d` komutu hata verecektir:

	$ git branch -d testing
	error: The branch 'testing' is not an ancestor of your current HEAD.
	If you are sure you want to delete it, run 'git branch -D testing'.

Oradaki deðiþiklikleri kaybetmeyi göze alarak dalý her þeye raðmen silmek istiyorsanýz, yukarýdaki çýktýda da belirtildiði gibi, `-D` seçeneðiyle üsteleyebilirsiniz.

## Dallanma Ýþ Akýþlarý ##

Dallanma ve birleþtirmenin temellerine hakim olduðunuza göre, þimdi bu bilgiyi kullanarak neler yapabileceðimize bakalým. Bu alt bölümde masrafsýz dallanmanýn olanaklý kýldýðý bazý yaygýn iþ akýþlarý üzerinde duracaðýz, böylece bunlarý kendi geliþtirme döngünüzde kullanýp kullanmamaya karar verebilirsiniz.

### Uzun Süreli Dallar ###

Git, basit üç taraflý birleþtirme yaptýðý için uzun bir zaman dilimi boyunca bir daldan diðerine çok sayýda birleþtirme yapmak genellikle kolaydýr. Yani, sürekli açýk olan ve geliþtirme döngünüzün deðiþik aþamalarýnda kullanabileceðiniz birkaç dal bulundurabilirsiniz; düzenli olarak bazýlarýndan diðerlerine birleþtirme yapabilirsiniz.

Git'i kullanan pek çok yazýlýmcý bu yaklaþýmý benimser, `master` dalýnda yalnýzca kararlý (_stable_) durumdaki kod bulunur —yalnýzca yayýmlanmýþ olan ya da yayýmlanacak kod. `develop` ya da `next` adýnda, kararlýlýk testlerinin yürütüldüðü bir paralel dallarý daha vardýr —bu dal o kadar kararlý olmayabilir, fakat kararlý duruma getirildiðinde `master` dalýna birleþtirilir. Kýsa ömürlü, belirli bir iþlevin geliþtirilmesine ayrýlmýþ dallarýn (sizin `iss53` adlý dalýnýz gibi) hazýr olduklarýnda birleþtirilmeleri için —bütün testlerden geçtiklerinden ve yeni hatalara kapý aralamadýklarýndan emin olmak amacýyla— kullanýlýr.

Gerçekte, yazýlým tarihçesinde ileri doðru hareket eden imleçlerden söz ediyoruz. Kararlý dallar eski kayýtlarý, güncel dallar çok daha yenilerini gösterir (bkz. Figür 3-18).

Insert 18333fig0318.png 
Figür 3-18. Daha kararlý dallar genellikle kayýt tarihçesinde daha geride bulunurlar.

Bu dallarý, çalýþma ambarlarý olarak hayal ediliriz, bir dizi kayýt bütünüyle test ediltikten sonra daha kararlý baþka br ambara konulurlar (bkz. Figür 3-19).

Insert 18333fig0319.png 
Figure 3-19. Dallarýn ambarlar gibi olduðunu düþünebilirsiniz.

Çeþitli kararlýlýk seviyeleri tanýmlayýp bu iþleyiþi o þekilde kullanabilirsiniz. Büyük projelerde `proposed` (önerilen) ya da `pu` (proposed updates - önerilen güncellemeler) adýnda bir dal daha olabilir. Bu dala, `next` ya da `master` dalýna birleþtirilecek kadar kararlý aþamada bulunmayan dallar birleþtirilir. Sonuçta, dallar farklý kararlýlýk seviyelerinde bulunurlar; daha kararlý bir seviyeye ulaþtýklarýnda, bir üstlerindeki dala birleþtirilirler.
Tekrarlayalým: birden çok uzun ömürlü dal bulundurmak zorunlu deðildir, ama, özellikle çok büyük ya da karmaþýk projelerde çalýþýyorsanýz çoðunlukla yararlýdýr.

### Ýþlev Dallarý ###

Ýþlev dallarý, her ölçekte proje için yararlýdýr. Ýþlev dallarý, belirli bir özellikle ilgili deðiþikliklerin geliþtirilmesi için kullanýlan kýsa ömürlü dallardýr. Baþka SKS'lerde bu çok masraflý olduðu için, muhtemelen bu yaklaþýmý daha önce benimsemediniz. Ama Git'te dal yaratmak, o dal üzerinde çalýþmak, dalý birleþtirmek ve daha sonra silmek, günde birkaç kez yapýlan yaygýn bir yöntemdir.

Bunu bir önceki alt bölümde `iss53` ve `hotfix` dallarý üzerinde çalýþýrken gördünüz. Bu dallarda birkaç deðiþiklik yaptýnýz ve bu deðiþiklikleri `master` dalýna birleþtirdikten hemen sonra bu dallarý sildiniz. Bu teknik sayesinde, baðlamlar arasýnda hýzlý ve bütünlüklü geçiþler yapabilirsiniz —çalýþmalarýnýz belirli bir iþlevin geliþtirilmesine adanmýþ farklý ambarlara ayrýlmýþ olduðundan, geçen süre zarfýnda, diyelim kod gözden geçirmesi sýrasýnda neler olduðunu kolaylýkla görebilirsiniz. Deðiþikliklerinizi iþlev dallarýnda dakikalarca, günlerce ya da aylarca tutabilir, hazýr olduklarý zaman, hangisinin dalýn daha önce oluþturulduðuna aldýrmadan birleþtirebilirsiniz.

Diyelim ki `master` dalýnda çalýþýyorsunuz, sonra bir hatayý gidermek için yeni bir dal oluþturuyorsunuz (`iss91`), derken ayný hatayý baþka türlü gidermek için yeni bir dal oluþturuyorsunuz (`iss91v2`), sonra `master`'a geri dönüp biraz daha çalýþýyorsunuz, sonra aklýnýza gelen ama çok da gerekli olmadýðýný düþündüðünüz bir þeyle ilgili çalýþmak için yeni bir dal oluþturuyorsunuz (`dumbidea`)... Kayýt tarihçeniz Figür 3-20'deki gibi görünecektir.

Insert 18333fig0320.png 
Figür 3-20. Birden çok iþlev dalýnýn bulunduðu kayýt tarihçeniz.

Þimdi diyelim ki, hatanýn giderilmesinde ikinci çözümü (`iss91v2`) kullanmaya karar veriyorsunuz ve iþ arkadaþlarýnýz `dumbidea` dalýnda yaptýklarýnýzý dahice buluyor. `iss91` dalýnýzý çöpe atabilir (C5 ve C6 kayýtlarýný kaybedeceksiniz) diðer iki dalý birleþtirebilirsiniz. Bu durumda tarihçeniz Figür 3-21'deki gibi görünecektir.

Insert 18333fig0321.png 
Figür 3-21. dumbidea ve iss91v2'yi birleþtirdikten sonra kayýt tarihçeniz.

Unutmayýn, bütün bunlarý yerel dallarda yapýyorsunuz. Dal yaratýrken ve birleþtirme yaparken her þey yalnýzca yerel yazýlým havuzunda gerçekleþiyor —hiçbir sunucu iletiþimi olmuyor.

## Uzak Uçbirim Dallarý ##

Yerel yazýlým havuzunuzdaki uzak uçbirim dallarý, uzak uçbirimlerdeki yazýlým havuzlarýnýzýn durumlarýný gösteren imleçlerdir. Bunlar, hareket ettiremediðiniz yerel dallardýr; yalnýzca sunucuyla iletiþim kurduðunuzda hareket ederler. Bu dallar, son baðlandýðýnýzda sunucudaki yazýlým havuzunun ne durumda olduðunu hatýrlatan iþaretçilerdir.

`(remote)`/`(dal)` biçimindedirler. Örneðin, sunucuya son baðlandýðýnýzda `origin` uzak uçbirimindeki `master` dalýnýn nasýl olduðunu görmek isterseniz, `origin/master` dalýna bakmalýsýnýz. Bir hatayý bir iþ ortaðýyla birlikte çözüyorsanýz ve onlar `iss53` adýnda bir dalý sunucuya itmiþlerse, sizin yerel dalýnýzýn adý `iss53` iken, sunucuya itilmiþ olan dalýn adý `origin/iss53` olacaktýr.

Bu biraz kafa karýþtýrýcý olabilir, gelin bir örnekle açýklayalým. Diyelim ki `git.þirketimiz.com` adresinde bir Git sunucunuz var. Buradan klonlama yaparsanýz, Git bu yazýlým havuzunu otomatik olarak `origin` olarak adlandýracak, bütün veriyi indirecek, onun `master` dalýnýn gösterdiði kaydý gösteren `origin/master` adýnda hareket ettiremeyeceðiniz bir yerel dal oluþturacaktýr. Git ayrýca,  üzerinde çalýþabilmeniz için `origin`in `master` dalýnýn olduðu yeri gösteren `master` adýnda yerel bir dal da oluþturacaktýr (bkz. Figür 3-22).

Insert 18333fig0322.png 
Figür 3-22. Bir Git klonladýðýnýzda hem yerel bir master dalýnýz hem de origin'in master dalýný gösteren origin/master adýnda bir dalýnýz olur.

Eðer siz kendi master dalýnýzda çalýþýrken biir baþkasý `git.þirketimiz.com`'a itme yapýp `master` dalýný güncellerse, tarihçeleriniz birbirinden farklýlaþacaktýr. Üstelik, `origin` sunucusuyla iletiþime geçmediðiniz sürece sizin `origin/master` dalýnýz hareket etmeyecektir (bkz. Figür 3-23).

Insert 18333fig0323.png 
Figür 3-23. Siz yerelde çalýþýyorken bir baþkasý sunucuya itme yaparsa, tarihçeleriniz birbirinden farklý hareket etmeye baþlar.

Çalýþmalarýnýzý eþitlemek için `git fetch origin` komutunu çalýþtýrabilirsiniz. Bu komut `origin` sunucusunun hangisi olduðuna bakar (bu örnekte `git.þirketimiz.com`), orada bulunup da sizde olmayan her türlü veriyi indirir, yerel veritabanýnýzý güncelleyip yerelinizdeki `origin/master` dalýný yeni, güncel konumuna taþýr (bkz. Figür 3-24).

Insert 18333fig0324.png 
Figür 3-24. git fetch komutu uzak uçbirim imleçlerinizi günceller.

Birden çok uzak uçbirime sahip bir projede uzak uçbirim imleçlerinin nasýl görüneceðini incelemek için, Scrum takýmlarýnýzdan birisi tarafýndan kullanýlan baþka bir sunucunuzun daha olduðunu varsayalým. Bu sunucunun adresi `git.team1.þirketimiz.com` olsun. 2. Bölüm'de incelediðimiz gibi, bu sunucuyu projenize uzak uçbirim olarak eklemek için `git remote add` komutunu kullanabilirsiniz. Bu uçbirimin adý `teamone` olsun, ki bu adý daha sonra bütün URL yerine kýsaltma olarak kullanacaksýnýz (bkz. Figür 3-25).

Insert 18333fig0325.png 
Figür 3-25. Baþka bir sunucuyu uzak uçbirim olarak eklemek.

`teamone` uzak uçbiriminde bulunup da sizde bulunmayan þeyleri getirmek için `git fetch teamone` komutunu çalýþtýrabilirsiniz. O sunucuda bulunan veriler `origin` sunucusunda bulunanlarýn alt kümesi olduðundan, Git herhangi bir veri çekmez, ama `teamone/master` adýnda, `teamone` sunucusunun `master` dalýnýn gösterdiði kaydý gösteren bir uzak uçbirim dalý oluþturur (bkz. Figür 3-26).

Insert 18333fig0326.png 
Figür 3-26. teamone'nin master dalýnýn pozisyonunu gösteren bir yerel imleciniz oluyor.

### Ýtme Ýþlemi ###

Bir daldaki çalýþmalarýnýzý baþkalarýyla paylaþmak istediðinizde, onu yazma yetkinizin olduðu bir uzak uçbirime itmelisiniz (_push_). yerel dallarýnýz otomatik olarak sunucuyla eþitlenmez —paylaþmak istediðiniz dallarý açýk þekilde itmelisiniz. Böylece, paylaþmak istemediðiniz dallar için özel yerel dallar kullanýp, yalnýzca paylaþmak istediðiniz iþlev dallarýný iteblirsiniz.

Baþkalarýyla ortaklaþa çalýþmak istediðiniz `serverfix` adýnda bir dalýnýz varsa, onu da ilk dalýnýzý ittiðiniz gibi itebilirsiniz. `git push (remote) (branch)` komutunu çalýþtýrýn.

	$ git push origin serverfix
	Counting objects: 20, done.
	Compressing objects: 100% (14/14), done.
	Writing objects: 100% (15/15), 1.74 KiB, done.
	Total 15 (delta 5), reused 0 (delta 0)
	To git@github.com:schacon/simplegit.git
	 * [new branch]      serverfix -> serverfix

Bu bir tür kýsayol sayýlabilir. Git `serverfix` dal adýný otomatik olarak `refs/heads/serverfix:refs/heads/serverfix` biçiminde açýmlar, bu þu demektir: “yerel `serverfix` dalýmý alýp uzak uçbirimin `serverfix` dalýný güncellemek için kullan.” `refs/heads/` kýsmýnz 9. Bölüm'de ayrýntýsýyla deðineceðiz, ama genellikle bu kýsmý kullanmasanýz da olur. Ayný amaçla `git push origin serverfix:serverfix` komutunu da çalýþtýrabilirsiniz —bu da þu demektir: “Yereldeki serverfix'i al, bunu uzak uçbirimin serverfix'i yap.” Bu biçimi, yereldeki dal adýyla uzak uçbirimdeki dal adý farklý ise kullanabilirsiniz. Dal adýnýn uzak uçbirimde `serferfix` olmasýný istemezseniz `git push origin serverfix:awesomebranch` komutunu çalýþtýrarak yereldeki `serverfix` dalýný uzak uçbirimdeki `awesomebranch` dalýna itebilirsiniz.

Birlikte çalýþtýðýnýz insanlar sunucudan getirme iþlemi (_fetch_) yaptýklarýnda,  sunucudaki `serverfix` sürümünün bulunduðu yeri gösteren `origin/serverfix` adýnda bir imlece sahip olacaklar.

	$ git fetch origin
	remote: Counting objects: 20, done.
	remote: Compressing objects: 100% (14/14), done.
	remote: Total 15 (delta 5), reused 0 (delta 0)
	Unpacking objects: 100% (15/15), done.
	From git@github.com:schacon/simplegit
	 * [new branch]      serverfix    -> origin/serverfix

Unutmayýn, getirme (_fetch_) komutuyla yeni uzak uçbirim dallarýný indirdiðinizde, yerelde otomatik olarak deðiþtirilebilir dallar oluþturulmaz. Baþka bir deyiþle, bu örnekte, `serverfix` adýnda bir dalýnýz olmaz, deðiþtiremeyeceðiniz `origin/serverfix` adýnda bir imleciniz olur.

Oradaki deðiþiklikleri üzerinde çalýþmakta olduðunuz dala birleþtirmek isterseniz, `git merge origin/serverfix` komutunu çalýþtýrabilirsiniz. Üzerinde çalýþmak üzere kendinize ait bir `serverfix` dalýnýz olmasýný isterseniz, uzak uçbirim dalýný temel alabilirsiniz:

	$ git checkout -b serverfix origin/serverfix
	Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "serverfix"

Bu, üzerinde çalýþabileceðiniz ve `origin/serverfix`in gösterdiði yerden baþlayan bir yerel dal yaratýr.

### Ýzleme Dallarý ###

Bir uzak uçbirim dalýndan yerel bir dal seçtiðinizde (_checkout_), bu iþlem otomatik olarak bir _izleme dalý_ (_tracking branch_) oluþturur. Ýzleme dallarý, uzak uçbirim dallarýyla doðrudan iliþkileri bulunan yerel dallardýr. Bir izleme dalýndan `git push` komutunu çalýþtýrdýðýnýzda , Git hangi sunucudaki hangi dala itme iþlemi yapmasý gerektiðini bilir. Ayrýca, bu dallardan birinden `git pull` komutunu çalýþtýrdýðýnýzda, bütün imleçler indirileceði gibi, bu izleme dalýna karþýlýk gelen uzak uçbirim dalý da otomatik olarak bu dalla birleþtirilir.

Bir yazýlým havuzunu klonladýðýnýzda, genellikle `origin/master` dalýný izleyen bir `master` dalý yaratýlýr. Bu nedenle `git push` ve `git pull` komutlarý bu durumlarda ek argümanlara gerek kalmadan çalýþýrlar. Öte yandan, isterseniz baþka izleme dallarý da —`origin`'i ya da `master` dalýnýz izlemeyen dallar— oluþturabilirsiniz. Yukarýda basit bir örneðini gördük: `git checkout -b [dal] [uzak_ucbirim]/[dal]`. Git'in 1.6.2'den itibaren olan sürümlerinde `--track` kýsayolunu da kullanabilirsiniz:

	$ git checkout --track origin/serverfix
	Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "serverfix"

Uzak uçbirim dalýnýn adýndan baþka bir adla yerel dal oluþturmak isterseniz, yukarýdaki komutu farklý bir yerel dal adýyla kullanabilirsiniz:

	$ git checkout -b sf origin/serverfix
	Branch sf set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "sf"

Þimdi, yereldeki sf dalý, otomatik olarak `origin/serverfix` dalýna itme ve çekme iþlemi yapabilecek.

### Uzak Uçbirim Dallarýný Silmek ###

Suppose you’re done with a remote branch — say, you and your collaborators are finished with a feature and have merged it into your remote’s `master` branch (or whatever branch your stable codeline is in). You can delete a remote branch using the rather obtuse syntax `git push [remotename] :[branch]`. If you want to delete your `serverfix` branch from the server, you run the following:

	$ git push origin :serverfix
	To git@github.com:schacon/simplegit.git
	 - [deleted]         serverfix

Boom. No more branch on your server. You may want to dog-ear this page, because you’ll need that command, and you’ll likely forget the syntax. A way to remember this command is by recalling the `git push [remotename] [localbranch]:[remotebranch]` syntax that we went over a bit earlier. If you leave off the `[localbranch]` portion, then you’re basically saying, “Take nothing on my side and make it be `[remotebranch]`.”

## Rebasing ## Zemin, Kök, Temel

In Git, there are two main ways to integrate changes from one branch into another: the `merge` and the `rebase`. In this section you’ll learn what rebasing is, how to do it, why it’s a pretty amazing tool, and in what cases you won’t want to use it.

### The Basic Rebase ###

If you go back to an earlier example from the Merge section (see Figure 3-27), you can see that you diverged your work and made commits on two different branches.

Insert 18333fig0327.png 
Figure 3-27. Your initial diverged commit history.

The easiest way to integrate the branches, as we’ve already covered, is the `merge` command. It performs a three-way merge between the two latest branch snapshots (C3 and C4) and the most recent common ancestor of the two (C2), creating a new snapshot (and commit), as shown in Figure 3-28.

Insert 18333fig0328.png 
Figure 3-28. Merging a branch to integrate the diverged work history.

However, there is another way: you can take the patch of the change that was introduced in C3 and reapply it on top of C4. In Git, this is called _rebasing_. With the `rebase` command, you can take all the changes that were committed on one branch and replay them on another one.

In this example, you’d run the following:

	$ git checkout experiment
	$ git rebase master
	First, rewinding head to replay your work on top of it...
	Applying: added staged command

It works by going to the common ancestor of the two branches (the one you’re on and the one you’re rebasing onto), getting the diff introduced by each commit of the branch you’re on, saving those diffs to temporary files, resetting the current branch to the same commit as the branch you are rebasing onto, and finally applying each change in turn. Figure 3-29 illustrates this process.

Insert 18333fig0329.png 
Figure 3-29. Rebasing the change introduced in C3 onto C4.

At this point, you can go back to the master branch and do a fast-forward merge (see Figure 3-30).

Insert 18333fig0330.png 
Figure 3-30. Fast-forwarding the master branch.

Now, the snapshot pointed to by C3' is exactly the same as the one that was pointed to by C5 in the merge example. There is no difference in the end product of the integration, but rebasing makes for a cleaner history. If you examine the log of a rebased branch, it looks like a linear history: it appears that all the work happened in series, even when it originally happened in parallel.

Often, you’ll do this to make sure your commits apply cleanly on a remote branch — perhaps in a project to which you’re trying to contribute but that you don’t maintain. In this case, you’d do your work in a branch and then rebase your work onto `origin/master` when you were ready to submit your patches to the main project. That way, the maintainer doesn’t have to do any integration work — just a fast-forward or a clean apply.

Note that the snapshot pointed to by the final commit you end up with, whether it’s the last of the rebased commits for a rebase or the final merge commit after a merge, is the same snapshot — it’s only the history that is different. Rebasing replays changes from one line of work onto another in the order they were introduced, whereas merging takes the endpoints and merges them together.

### More Interesting Rebases ###

You can also have your rebase replay on something other than the rebase branch. Take a history like Figure 3-31, for example. You branched a topic branch (`server`) to add some server-side functionality to your project, and made a commit. Then, you branched off that to make the client-side changes (`client`) and committed a few times. Finally, you went back to your server branch and did a few more commits.

Insert 18333fig0331.png 
Figure 3-31. A history with a topic branch off another topic branch.

Suppose you decide that you want to merge your client-side changes into your mainline for a release, but you want to hold off on the server-side changes until it’s tested further. You can take the changes on client that aren’t on server (C8 and C9) and replay them on your master branch by using the `--onto` option of `git rebase`:

	$ git rebase --onto master server client

This basically says, “Check out the client branch, figure out the patches from the common ancestor of the `client` and `server` branches, and then replay them onto `master`.” It’s a bit complex; but the result, shown in Figure 3-32, is pretty cool.

Insert 18333fig0332.png 
Figure 3-32. Rebasing a topic branch off another topic branch.

Now you can fast-forward your master branch (see Figure 3-33):

	$ git checkout master
	$ git merge client

Insert 18333fig0333.png 
Figure 3-33. Fast-forwarding your master branch to include the client branch changes.

Let’s say you decide to pull in your server branch as well. You can rebase the server branch onto the master branch without having to check it out first by running `git rebase [basebranch] [topicbranch]` — which checks out the topic branch (in this case, `server`) for you and replays it onto the base branch (`master`):

	$ git rebase master server

This replays your `server` work on top of your `master` work, as shown in Figure 3-34.

Insert 18333fig0334.png 
Figure 3-34. Rebasing your server branch on top of your master branch.

Then, you can fast-forward the base branch (`master`):

	$ git checkout master
	$ git merge server

You can remove the `client` and `server` branches because all the work is integrated and you don’t need them anymore, leaving your history for this entire process looking like Figure 3-35:

	$ git branch -d client
	$ git branch -d server

Insert 18333fig0335.png 
Figure 3-35. Final commit history.

### The Perils of Rebasing ###

Ahh, but the bliss of rebasing isn’t without its drawbacks, which can be summed up in a single line:

**Do not rebase commits that you have pushed to a public repository.**

If you follow that guideline, you’ll be fine. If you don’t, people will hate you, and you’ll be scorned by friends and family.

When you rebase stuff, you’re abandoning existing commits and creating new ones that are similar but different. If you push commits somewhere and others pull them down and base work on them, and then you rewrite those commits with `git rebase` and push them up again, your collaborators will have to re-merge their work and things will get messy when you try to pull their work back into yours.

Let’s look at an example of how rebasing work that you’ve made public can cause problems. Suppose you clone from a central server and then do some work off that. Your commit history looks like Figure 3-36.

Insert 18333fig0336.png 
Figure 3-36. Clone a repository, and base some work on it.

Now, someone else does more work that includes a merge, and pushes that work to the central server. You fetch them and merge the new remote branch into your work, making your history look something like Figure 3-37.

Insert 18333fig0337.png 
Figure 3-37. Fetch more commits, and merge them into your work.

Next, the person who pushed the merged work decides to go back and rebase their work instead; they do a `git push --force` to overwrite the history on the server. You then fetch from that server, bringing down the new commits.

Insert 18333fig0338.png 
Figure 3-38. Someone pushes rebased commits, abandoning commits you’ve based your work on.

At this point, you have to merge this work in again, even though you’ve already done so. Rebasing changes the SHA-1 hashes of these commits so to Git they look like new commits, when in fact you already have the C4 work in your history (see Figure 3-39).

Insert 18333fig0339.png 
Figure 3-39. You merge in the same work again into a new merge commit.

You have to merge that work in at some point so you can keep up with the other developer in the future. After you do that, your commit history will contain both the C4 and C4' commits, which have different SHA-1 hashes but introduce the same work and have the same commit message. If you run a `git log` when your history looks like this, you’ll see two commits that have the same author date and message, which will be confusing. Furthermore, if you push this history back up to the server, you’ll reintroduce all those rebased commits to the central server, which can further confuse people.

If you treat rebasing as a way to clean up and work with commits before you push them, and if you only rebase commits that have never been available publicly, then you’ll be fine. If you rebase commits that have already been pushed publicly, and people may have based work on those commits, then you may be in for some frustrating trouble.

## Summary ##

We’ve covered basic branching and merging in Git. You should feel comfortable creating and switching to new branches, switching between branches and merging local branches together.  You should also be able to share your branches by pushing them to a shared server, working with others on shared branches and rebasing your branches before they are shared.