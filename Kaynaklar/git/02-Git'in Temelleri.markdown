# Git'in Temelleri #

Git'i kullanmaya baþlamak için yalnýzca bir bölüm okuyacak kadar zamanýnýz varsa, o bölüm, bu bölüm olmalý. Bu bölüm, Git'i kullanarak yapacaðýnýz þeylerin çok büyük kýsmý için kullanacaðýnýz bütün temel komutlarý içeriyor. Bu bölümün sonunda bir yazýlým havuzunun nasýl yapýlandýrýp, ilkleneceðini (_initialize_), dosyalarýn nasýl izlemeye alýnýp izlemeden çýkarýlacaðýný ve deðiþikliklerin nasýl hazýrlanýp kaydedileceðini öðreneceksiniz. Bunlara ek olarak, Git'i bazý dosyalarý ya da konumlarý belli örüntülere (_pattern_) uyan dosyalarý görmezden gelmesi için nasýl ayarlayacaðýnýzý, hatalarý hýzlýca ve kolayca nasýl geri alabileceðinizi, projenizin tarihçesine nasýl göz gezdirip kayýtlar arasýndaki farklarý nasýl görüntüleyebileceðinizi ve uzak uçbirimlerden nasýl kod çekme iþlemi yapabileceðinizi göstereceðiz.

## Bir Git Yazýlým Havuzu Edinmek ##

Bir Git projesi edinmenin baþlýca iki yolu vardýr. Bunlardan ilki, halihazýrda varolan bir projeyi Git'e aktarmaktýr. Ýkincisi ise bir sunucuda yer alan bir Git yazýlým havuzunu klonlamakdýr.

### Var olan Bir Klasörde Yazýlým Havuzu Oluþturmak ###

Var olan bir projenizi sürüm kontrolü altýna almak istiyorsanýz, projenin bulunduðu klasöre gidip aþaðýdaki komutu çalýþtýrmanýz gerekir:

	$ git init

Bu, gerekli yazýlým havuzu dosyalarýný —Git iskeletini— içeren `.git` adýnda bir klasör oluþturur. Bu noktada, projenizdeki hiçbir þey sürüm kontrolüne girmiþ deðildir. (Oluþturulan `.git` klasöründe tam olarak hangi dosyalarýn bulunduðu hakkýnda daha fazla bilgi edinmek için bkz. _9. Bölüm_.)

Var olan dosyalarýnýzý sürüm kontrolüne almak istiyorsanýz, o dosyalarý hazýrlayýp kayýt etmelisiniz. Bunu, sürüm kontrolüne almak istediðiniz dosyalarý belirleyip kayýt altýna aldýðýnýz birkaç git komutuyla gerçekleþtirebilirsiniz:

	$ git add *.c
	$ git add README
	$ git commit -m 'projenin ilk hali'

Birazdan bu komutlarýn üzerinde duracaðýz. Bu noktada, sürüm kontrolüne aldýðýnýz dosyalarý içeren bir Git yazýlým havuzunuz var.

### Var olan Bir Yazýlým Havuzunu Klonlamak ###

Var olan bir Git yazýlým havuzunu klonlamak istiyorsanýz —söz gelimi, katkýda bulunmak istediðiniz bir proje varsa- ihtiyacýnýz olan komut `git clone`. Subversion gibi baþka SKS'lere aþinaysanýz, komutun `checkout` deðil `clone` olduðunu fark etmiþsinizdir. Bu önemli bir ayrýmdýr —Git, sunucuda bulunan neredeyse bütün veriyi kopyalar. `git clone` komutunu çalýþtýrdýðýnýzda her dosyanýn proje tarihçesinde bulunan her sürümü istemciye indirilir. Hatta, sunucunuzun diski bozulacak olsa, herhangi bir istemcideki herhangi bir klonu, sunucuyu klonlandýðý zamanki haline geri getirmek için kullanabilirsiniz (sunucunuzdaki bazý çengel betikleri (_hook_) kaybedebilirsiniz, ama sürümlenmiþ verinin tamamý elinizin altýnda olacaktýr —daha fazla ayrýntý için bkz. _4. Bölüm_)

Bir yazýlým havuzu `git clone [url]` komutuyla klonlanýr. Örneðin, Grit adlý Ruby Git kütüphanesini klonlamak isterseniz, bunu þu þekilde yapabilirsiniz:

	$ git clone git://github.com/schacon/grit.git

Bu komut `grit` adýnda bir klasör oluþturur, bu klasörün içinde bir `.git` alt dizini oluþturup ilklemesini yapar, söz konusu yazýlým havuzunun bütün verisini indirir ve son sürümünün bir koyasýný seçer (_checkout_). Bu yeni `grit` klasörüne gidecek olursanýz, kullanýlmaya ve üzerinde çalýþýlmaya hazýr proje dosyalarýný görürsünüz. Yazýlým havuzunu adý grit'ten farklý bir klasöre kopyalamak isterseniz, bunu komut satýrý seçeneði olarak vermelisiniz:

	$ git clone git://github.com/schacon/grit.git mygrit

Bu komut da bir öncekiyle ayný þeyleri yapar, fakat oluþturulan klasörün adý `mygrit`'tir.

Git'in bir dizi farklý transfer protokolü vardýr. Yukarýdaki örnek `git://` protokolünü kullanýyor, ama `http(s)://`'in ya da SSH  transfer protokolünü kullanan `user@server:/path.git`'in kullanýldýðýna da tanýk olabilirsiniz. _4. Bölüm_'de Git yazýlým havuzuna eriþmek için sunucunun kullanabileceði bütün geçerli seçenekleri ve bunlarýn olumlu ve olumsuz yanlarýný inceleyeceðiz.

## Deðiþiklikleri Yazýlým Havuzuna Kaydetmek ##

Gerçek bir Git yazýlým havuzuna ve söz konusu proje için gerekli olan bir dosya seçmesine sahipsiniz. Bu proje üzerinde deðiþiklikler yapmanýz ve proje kaydetmek istediðiniz bir seviyeye geldiðinde bu deðiþikliklerin bir bellek kopyasýný kaydetmeniz gerekecek.

Unutmayýn, çalýþma klasörünüzdeki dosyalar iki halden birinde bulunurlar: _izlenenler_ (_tracked_) ve _izlenmeyenler_ (_untracked_). _Ýzlenen_ dosyalar, bir önceki bellek kopyasýnda bulunan dosyalardýr; bunlar _deðiþmemiþ_, _deðiþmiþ_ ya da _hazýrlanmýþ_ olabilirler. Geri kalan her þey —çalýþma klasörünüzde bulunan ve bir önceki bellek kopyasýnda ya da hazýrlama alanýnda bulunmayan dosyalar— _izlenmeyen_ dosyalardýr. Bir yazýlým havuzunu yeni kopyalamýþsanýz, bütün dosyalar, henüz yeni seçme yaptýðýnýz ve hiçbir þeyi deðiþtirmediðiniz için, izlenen ve deðiþmemiþ olacaktýr.

Dosyalarý düzenlemeye baþladýðýnýzda, Git onlarý deðiþmiþ olarak görecektir, çünkü son kaydýnýzdan beri üzerlerinde deðiþiklik yapmýþ olacaksýnýz. Deðiþtirdiðiniz bu dosyalarý önce _hazýrlayýp_ sonra bütün _hazýrlanmýþ_ deðiþiklikleri kaydedeceksiniz ve bu döngü böyle sürüp gidecek. Bu döngü, Figür 2-1'de gösteriliyor.


Insert 18333fig0201.png
Figür 2-1. Dosyalarýnýzýn deðiþik durumlarýnýn döngüsü.

### Dosyalarýn Durumlarýný Kontrol Etmek ###

Hangi dosyanýn hangi durumda olduðunu görmek için kullanýlacak temel araç `git status` komutudur. Bu komutu bir klonlama iþleminin hemen sonrasýnda çalýþtýracak olursanýz, þöyle bir þey görmelisiniz:

	$ git status
	# On branch master
	nothing to commit (working directory clean)

Bu çalýþma klasörünüzün temiz olduðu anlamýna gelir —baþka bir deyiþle, izlenmekte olup da deðiþtirilmiþ herhangi bir dosya yoktur. Git'in saptadýðý herhangi bir izlenmeyen dosya da yok, olsaydý burada listelenmiþ olurdu. Son olarak, bu komut size hangi dal (_branch_) üzerinde olduðunuzu söylüyor. Þimdilik bu, daima varsayýlan dal olan `master` olacaktýr; Þu anda buna kafa yormayýn. Sonraki bölüm de dallar ve referanslar konusu derinlemesine ele alýnacak.

Diyelim ki projenize yeni bir dosya, basit bir README dosyasý eklediniz. Eðer dosya önceden orada bulunmuyorsa, ve `git status` komutunu çalýþtýrýrsanýz, bu izlenmeyen dosyayý þu þekilde görürsünüz:

	$ vim README
	$ git status
	# On branch master
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	README
	nothing added to commit but untracked files present (use "git add" to track)

Yeni README dosyanýzýn izlenmediðini görüyorsunuz, çünkü `status` çýktýsýnda “Untracked files” baþlýðý altýnda listelenmiþtir. Bir dosyanýn izlenmemesi demek, Git'in onu bir önceki bellek kopyasýnda (_commit_) görmemesi demektir; siz açýkça belirtmediðiniz sürece Git bu dosyayý izlemeye baþlamayacaktýr. Bunun nedeni, derleme çýktýsý olan ikili dosyalarýn ya da projeye dahil etmek istemediðiniz dosyalarýn yanlýþlýkla projeye dahil olmasýný engellemektir. README dosyasýný projeye eklemek istiyorsunuz, öyleyse dosyayý izlemeye alalým.

### Yeni Dosyalarý Ýzlemeye Almak ###

Yeni bir dosyayý izlemeye almak için `git add` komutunu kullanmalýsýnýz. README dosyasýný izlemeye almak için komutu þu þekilde çalýþtýrabilirsiniz:

	$ git add README

`status` komutunu yeniden çalýþtýrýrsanýz, README dosyasýnýn artýk izlemeye alýndýðýný ve hazýrlýk alanýnda olduðunu göreceksiniz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#

Hazýrlýk alanýnda olduðunu “Changes to be committed” baþlýðýnýn altýnda olmasýna bakarak söyleyebilirsiniz. Eðer bu noktada bir kayýt (_commit_) yapacak olursanýz, dosyanýn `git add` komutunu çalýþtýrdýðýnýz andaki hali bellek kopyasýna kaydedilecektir. Daha önce `git init` komutunu çalýþtýrdýktan sonra projenize dosya eklemek için `git add (dosya)` komutunu çalýþtýrdýðýnýzý hatýrlayacaksýnýz —bunun amacý klasörünüzdeki dosyalarý izlemeye almaktý. `git add` komutu bir dosya ya da klasörün konumuyla çalýþýr; eðer söz konusu olan bir klasörse, klasördeki bütün dosyalarý tekrarlamalý olarak projeye ekler.

### Deðiþtirilen Dosyalarý Hazýrlamak ###

Gelin þimdi halihazýrda izlenmekte olan bir dosyayý deðiþtirelim. Ýzlenmekte olan `benchmarks.rb` adýndaki bir dosyayý deðiþtirip `status` komutunu çalýþtýrdýðýnýzda þöyle bir ekran çýktýsýyla karþýlaþýrsýnýz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

`benchmarks.rb` dosyasý “Changes not staged for commit” baþlýklý bir bölümün altýnda görünüyor —bu baþlýk izlenmekte olan bir dosyada deðiþiklik yapýlmýþ olduðu fakat dosyanýn henüz hazýrlýk alanýna alýnmadýðý durumlarda kullanýlýr. Dosyayý hazýrlamak için, `git add` komutunu çalýþtýrýn (`git add` çok amaçlý bir komuttur, bir dosyayý izlemeye almak için, kayda hazýrlamak için, ya da birleþtirme uyuþmazlýklarýnýn çözüldüðünü iþaretlemek gibi baþka amaçlarla kullanýlýr). Gelin `benchmarks.rb` dosyasýný kayda hazýrlamak için `git add` komutunu çalýþtýrýp sonra da `git status` komutuyla duruma bakalým:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

Her iki dosya da kayda hazýrlanmýþ durumdadýr ve bir sonraki kaydýnýza dahil edilecektir. Tam da bu noktada, henüz kaydý gerçekleþtirmeden, aklýnýza `benchmarks.rb` dosyasýnda yapmak istediðiniz küçük bir deðiþikliðin geldiðini düþünelim. Dosyayý yeniden açýp deðiþikliði yaptýktan sonra artýk kaydý yapmaya hazýrsýnýz. Fakat, `git status` komutunu bir kez daha çalýþtýralým:

	$ vim benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

Ne oldu? `benchmarks.rb` dosyasý hem kayda hazýrlanmýþ hem de kayda hazýrlanmamýþ görünüyor. Bu nasýl olabiliyor? Git, bir dosyayý `git add` komutunun alýþtýrýldýðý haliyle kayda hazýrlar. Eðer þimdi kayýt yapacak olursanýz, `benchmarks.rb` dosyasý, çalýþma klasöründe göründüðü haliyle deðil, `git add` komutunu son çalýþtýrdýðýnýz haliyle kayýt edilecektir. Bir dosyayý `git add` komutunu çalýþtýrdýktan sonra deðiþtirecek olursanýz, dosyanýn son halini kayda hazýrlamak için `git add` komutunu bir kez daha çalýþtýrmanýz gerekir:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

### Dosyalarý Görmezden Gelmek ###

Çoðu zaman, projenizde Git'in takip etmesini, hatta size izlenmeyenler arasýnda göstermesini bile istemediðiniz bir küme dosya olacaktýr. Bunlar, genellikle otomatik olarak oluþturulan seyir (_log_) dosyalarý ya da yazýlým inþa sisteminin çýktýlarýdýr. Bu durumlarda, bu dosyalarýn konumlarýyla eþleþen örüntüleri listeleyen `.gitignore` adýnda bir dosya oluþturabilirsiniz:

	$ cat .gitignore
	*.[oa]
	*~

Ýlk satýr, Git'e `.o` ya da `.a` ile biten dosyalarý —yazýlým derlemesinin sonucunda ortaya çýkmýþ olabilecek _nesne_ ve _arþiv_ dosyalarýný— görmezden gelmesini söylüyor. Ýkinci satýr, Git'e Emacs gibi pek çok metin editörü tarafýndan geçici dosyalarý iþaretlemek için kullanýlan tilda iþaretiyle (`~`) biten bütün dosyalarý görmezden gelmesini söylüyor. Bu listeye `log`, `tmp` ya da `pid` klasörlerini, otomatik olarak oluþturulan dokümantasyon dosyalarýný ve sair dosyayý ekleyebilirsiniz. Daha projenin baþlangýcýnda bir `.gitignore` dosyasý oluþturmak yazýlým havuzunuzda istemeyeceðiniz dosyalarý yanlýþlýkla kaydetmenize engel olacaðýndan oldukça iyi bir fikirdir.

`.gitignore` dosyanýzda bulundurabileceðiniz örüntüler þu kurallara baðlýdýr:

*	Boþ satýrlar ve `#` ile baþlayan satýrlar görmezden gelinir.
*	Stadart _glob_ örüntüleri ayýrt edilir (Ç.N.: _glob_ \*nix tarafýndan kullanýlan sýnýrlý bir kurallý ifade (_regular expression_) biçimidir).
*	Bir klasörü belirtmek üzere örüntüleri bir eðik çizgi (`/`) ile sonlandýrabilirsiniz.
*	Bir örüntüyü ünlem iþaretiyle (`!`) baþlattýðýnýzda, örüntünün tersi gereçli olur.

_Glob_ örüntüleri _shell_'ler tarafýndan kullanýlan basitleþtirilmiþ kurallý ifadelerdir (_regular expression_). Bir yýldýz iþareti (`*`) sýfýr ya da daha fazla karakterle eþleþir; `[abc]` köþeli parantezin içindeki herhangi bir karakterle eþleþir (buradaki örnekte `a`, `b`, ya da `c` ile); soru iþareti (`?`) bir karakterle eþleþir; tireyle ayrýlmýþ karakterleri içine alan bir köþeli parantez (`[0-9]`) bu aralýktaki bütün karakterlerle eþleþir (bu örnekte 0'dan 9'a kadar olan karakterler).

Bir `.gitignore` dosyasý örneði daha:

	# bir yorum - bu görmezden gelinir
	# .a dosyalarýný görmezden gel
	*.a
	# ama yukarýda .a dosyalarýný görmezden geliyor olsan bile lib.a dosyasýný izlemeye al
	!lib.a
	# kök dizindeki /TODO dosyasýný (TODO adýndaki alt klasörü deðil) görmezden gel
	/TODO
	# build/ klasöründeki bütün dosyalarý görmezden gel
	build/
	# doc/notes.txt dosyasýný görmezden gel ama doc/server/arch.txt dosyasýný görmezden gelme
	doc/*.txt

### Kayda Hazýrlanmýþ ve Hazýrlanmamýþ Deðiþiklikleri Görüntülemek ###

`git status` komutunu fazla anlaþýlmaz buluyorsanýz —yalnýzca hangi dosyalarýn deðiþtiðini deðil, bu dosyalarda tam olarak nelerin deðiþtiðini görmek istiyorsanýz— `git diff` komutunu kullanabilirsiniz. `git diff` komutunu ileride ayrýntýlý olarak inceleyeceðiz; ama bu komutu muhtemelen en çok þu iki soruya cevap bulmak için kullanacaksýnýz: Deðiþtirip de henüz kayda hazýrlamadýðýnýz neler var? Ve kayda olmak üzere hangi deðiþikliklerin hazýrlýðýný yaptýnýz? `git status` bu sorularý genel biçimde cevaplýyor olsa da `git diff` eklenen ve çýkarýlan bütün dosyalarý —olduðu gibi yamayý— gösterir.

Diyelim `README` dosyasýný düzenleyip kayda hazýrladýnýz, sonra da `benchmarks.rb` dosyasýný düzenlediniz ama kayda hazýrlamadýnýz. `status` komutunu çalýþtýrdýðýnýzda þöyle bir þey görürsünüz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

Henüz kayda hazýrlamadýðýnýz deðiþiklikleri görmek için `git diff` komutunu (baþka hiçbir argüman kullanmadan) çalýþtýrýn:

	$ git diff
	diff --git a/benchmarks.rb b/benchmarks.rb
	index 3cb747f..da65585 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -36,6 +36,10 @@ def main
	           @commit.parents[0].parents[0].parents[0]
	         end

	+        run_code(x, 'commits 1') do
	+          git.commits.size
	+        end
	+
	         run_code(x, 'commits 2') do
	           log = git.commits('master', 15)
	           log.size

Komut, çalýþma klasörünüzün içeriðiyle kayda hazýrlýk alanýnýn içeriðini karþýlaþtýrýr. Sonuç size henüz kayda hazýrlamadýðýnýz deðiþiklikleri gösterir.

Kayda hazýrlamýþ olduðunuz deðiþiklikleri görmek için `git diff --cache` komutunu kullanabilirsiniz. (1.6.1'den sonraki Git sürümlerinde hatýrlamasý daha kolay olabilecek `git diff --staged` komutunu da kullanabilirsiniz.) Bu komut kayda hazýrlanmýþ deðiþikliklerle son kaydý karþýlaþtýrýr.

	$ git diff --cached
	diff --git a/README b/README
	new file mode 100644
	index 0000000..03902a1
	--- /dev/null
	+++ b/README2
	@@ -0,0 +1,5 @@
	+grit
	+ by Tom Preston-Werner, Chris Wanstrath
	+ http://github.com/mojombo/grit
	+
	+Grit is a Ruby library for extracting information from a Git repository

Dikkat edilmesi gereken nokta, `git diff`'in son kayýttan beri yapýlan bütün deðiþiklikleri deðil yalnýzca kayda hazýrlanmamýþ deðiþiklikleri gösteriyor oluþudur. Bu zaman zaman kafa karýþtýrýcý olabilir, zira, bütün deðiþikliklerinizi kayda hazýrladýysanýz, `git diff`'in çýktýsý boþ olacaktýr.

Yine, örnek olarak, `benchmarks.rb` dosyasýný kayda hazýrlayýp daha sonra üzerinde deðiþiklik yaparsanýz, `git diff` komutunu kullanarak hangi deðiþikliklerin kayda hazýrlandýðýný, hangilerinin hazýrlanmadýðýný görüntüleyebilirsiniz:

	$ git add benchmarks.rb
	$ echo '# test line' >> benchmarks.rb
	$ git status
	# On branch master
	#
	# Changes to be committed:
	#
	#	modified:   benchmarks.rb
	#
	# Changes not staged for commit:
	#
	#	modified:   benchmarks.rb
	#

Þimdi `git diff` komutuyla hangi deðiþikliklerin henüz kayda hazýrlanmamýþ olduðunu

	$ git diff
	diff --git a/benchmarks.rb b/benchmarks.rb
	index e445e28..86b2f7c 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -127,3 +127,4 @@ end
	 main()

	 ##pp Grit::GitRuby.cache_client.stats
	+# test line

ve `git diff --cached` komutuyla neleri kayda hazýrlamýþ olduðunuzu görebilirsiniz:

	$ git diff --cached
	diff --git a/benchmarks.rb b/benchmarks.rb
	index 3cb747f..e445e28 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -36,6 +36,10 @@ def main
	          @commit.parents[0].parents[0].parents[0]
	        end

	+        run_code(x, 'commits 1') do
	+          git.commits.size
	+        end
	+
	        run_code(x, 'commits 2') do
	          log = git.commits('master', 15)
	          log.size

### Deðiþiklikleri Kaydetmek ###

Yaptýðýnýz deðiþiklikleri dilediðiniz gibi hazýrlýk alanýna aldýðýnýza göre artýk onlarý kaydedebilirsiniz (_commit_). Unutmayýn, kayda hazýrlanmamýþ —oluþturduðunuz ya da deðiþtirdiðiniz fakat `git add` komutunu kullanarak kayda hazýrlamadýðýnýz— deðiþiklikler kaydedilmeyecektir. Onlar sabit diskinizde deðiþtirilmiþ dosyalar olarak kalacaklar.

Bu örnekte, `git status` komutunu son çalýþtýrdýðýnýzda her þeyin kayda hazýrlanmýþ olduðunu gördünüz, dolayýsýyla deðiþikliklerinizi kaydetmeye hazýrsýnýz. Kayýt yapmanýn en kolay yolu `git commit` komutunu kullanmaktýr:

	$ git commit

Bu komutu çalýþtýrdýðýnýzda sisteminizde seçili bulunan metin editörü açýlacaktýr. (Editörünüz _shell_'inizin `$EDITOR` çevresel deðiþkeni tarafýndan tanýmlanýr —genellikle vim ya da emacs'týr, fakat `git config --global core.editor` komutunu _1. Bölüm_'de gördüðünüz gibi çalýþtýrarak bu ayarý deðiþtirebilirsiniz.)

Metin editörü aþaðýdaki metni görüntüler (bu örnek Vim ekranýndan):

	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   README
	#       modified:   benchmarks.rb
	~
	~
	~
	".git/COMMIT_EDITMSG" 10L, 283C

Gördüðünüz gibi hazýr kayýt mesajý `git status` çýktýsýnýn `#` kullanýlarak devre dýþý býrakýlmýþ haliyle en üstte bir boþ satýrdan oluþur. Bu devre dýþý býrakýlmýþ kayýt mesajýný silip yerine kendi kayýt mesajýnýzý yazabilir, ya da neyi kaydettiðinizi size hatýrlatmasý için orada býrakabilirsiniz. (Neyi deðiþtirdiðinizin daha ayrýntýlý olarak hatýrlatýlmasýný isterseniz, `git commit` mesajýný `-v` seçeneðiyle kullanabilirsiniz. Bu seçenek kaydetmekte olduðunuz deðiþikliðin içeriðini de (_diff_) editörde gösterecektir.) Editörü kapattýðýnýzda Git, yazdýðýnýz mesajý kullanarak deðiþikliði kaydeder (devre dýþý býrakýlmýþ bölümü ve deðiþikliðin içeriðini mesajýn dýþýnda býrakýr).

Bir baþka seçenek de, kayýt mesajýnýzý `commit` komutunu `-m` seçeneðiyle aþaðýdaki gibi kullanmaktýr:

	$ git commit -m "Story 182: Fix benchmarks for speed"
	[master]: created 463dc4f: "Fix benchmarks for speed"
	 2 files changed, 3 insertions(+), 0 deletions(-)
	 create mode 100644 README

Ýlk kaydýnýzý oluþturmuþ oldunuz! Görüldüðü gibi kayýt kendisiyle ilgili bilgiler veriyor: hangi dala kayýt yapmýþ olduðunuzu (`master`), kaydýn SHA-1 sýnama toplamýnýn ne olduðunu (`463dc4f`), kaç dosyada deðiþiklik yaptýðýnýzý ve kayýtta kaç satýr ekleyip çýkardýðýnýza dair istatistiklerin çýktýsýný veriyor.

Unutmayýn, kayýt, hazýrlýk alanýnda kayda hazýrladýðýnýz bellek kopyasýnýn kaydýdýr. Kayda hazýrlamadýðýnýz deðiþiklikler, deðiþiklik olarak duruyor; onlarý da proje tarihçesine eklemek isterseniz yeni bir kayýt yapabilirsiniz. Her kayýt iþleminde projenizin bir bellek kopyasýný kaydediyorsunuz; bu bellek kopyalarýný daha sonra geriye sarabilir ya da birbiriyle karþýlaþtýrabilirsiniz.

### Hazýrlýk Alanýný Atlamak ###

Her ne kadar kayýtlarý tam istediðiniz gibi düzenlemek inanýlmaz derecede yararlý bir þey olsa da, hazýrlýk alaný kimi zaman iþ akýþýnýza fazladan bir yük getirebilir. Git, hazýrlýk alanýný kullanmadan geçmek isteyenler için basit bir kýsayol sunuyor. `git commit` komutunu `-a` seçeneðiyle kullanýrsanýz, Git, halihazýrda izlenmekte olan bütün dosyalarý otomatik olarak kayda hazýrlayýp, `git add` aþamasýný atlamanýzý saðlar:

	$ git status
	# On branch master
	#
	# Changes not staged for commit:
	#
	#	modified:   benchmarks.rb
	#
	$ git commit -a -m 'added new benchmarks'
	[master 83e38c7] added new benchmarks
	 1 files changed, 5 insertions(+), 0 deletions(-)

Gördüðünüz gibi, kayýt iþlemi yapmadan önce `benchmarks.rb` dosyasýný `git add` komutundan geçirmek zorunda kalmadýnýz.

### Dosyalarý Ortadan Kaldýrmak ###

Bir dosyayý Git'ten silmek için, önce izlenen dosyalarý listesinden çýkarmalý (daha doðrusu, kayda hazýrlýk alanýndan kaldýrmalý) sonra da kaydetmelisiniz. `git rm` hem bunu yapar hem de dosyayý çalýþma klasörünüzden siler, böylece dosyayý izlenmeyen dosyalar arasýnda görmezsiniz.

Eðer dosyayý çalýþma klasörünüzden silerseniz, `git status` çýktýsýnýn “Changes not staged for commit” (yani _kayda hazýrlanmamýþ olanlar_) baþlýðý altýnda boy gösterecektir:

	$ rm grit.gemspec
	$ git status
	# On branch master
	#
	# Changes not staged for commit:
	#   (use "git add/rm <file>..." to update what will be committed)
	#
	#       deleted:    grit.gemspec
	#

Sonrasýnda `git rm` komutunu çalýþtýrýrsanýz, dosyanýn ortadan kaldýrýlmasý için kayda hazýrlanmasýný saðlamýþ olursunuz:

	$ git rm grit.gemspec
	rm 'grit.gemspec'
	$ git status
	# On branch master
	#
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       deleted:    grit.gemspec
	#

Bir sonraki kaydýnýzda, dosya silinmiþ olacak ve artýk izlenmeyecektir. Eðer dosyayý deðiþtirmiþ ve halihazýrda indekse eklemiþseniz, ortadan kaldýrma iþlemini `-f` seçeneðini kullanarak zorlamanýz gerekecektir. Bu, herhangi bir bellek kopyasýna kaydedilmemiþ ve Git kullanýlarak kurtarýlamayacak verilerin kaybolmasýný önlemek amacýyla geliþtirilmiþ bir önlemdir.

Yapmak isteyebileceðiniz bir baþka þey de dosyayý çalýþma klasörünüzde tutup, kayda hazýrlýk alanýndan silmek olabilir. Bir baþka deyiþle, dosyayý sabit diskinizde bulundurmak ama Git'in izlenecek dosyalar listesinden çýkarmak isteyebilirsiniz. Bu, özellikle belirli bir dosyayý (büyük bir seyir dosyasýný ya da bir küme derlenmiþ `.a` dosyasýný) `.gitignore` dosyanýza eklemeyi unutup yanlýþlýkla Git indeksine eklediðinizde kullanýþlý olan bir özelliktir. Bunu yapmak için `--cached` seçeneðini kullanmalýsýnýz:

	$ git rm --cached readme.txt

`git rm` komutunu dosyalar, klasörler ya da _glob_ örüntüleri üzerinde kullanabilirsiniz. Yani þöyle þeyler yapabilirsiniz:

	$ git rm log/\*.log

`*`'in önündeki ters eðik çizgi iþaretini gözden kaçýrmayýn. Bu iþaret gereklidir, çünkü _shell_'inizin dosya adý açýmlamasýna ek olarak, Git de kendi dosya adý açýmlamasýný yapar. Yukarýdaki komut, `log/` klasöründeki `.log` eklentili bütün dosyalarý ortadan kaldýracaktýr. Ya da, þöyle bir þey yapabilirsiniz:

	$ git rm \*~

Bu komut `~` ile biten bütün dosyalarý ortadan kaldýracaktýr.

### Dosyalarý Taþýmak ###

Çoðu SKS'nin aksine Git taþýnan dosyalarý takip etmez. Bir dosyanýn adýný deðiþtirirseniz, Git, dosyanýn yeniden adlandýrýldýðýna dair herhangi bir üstveri oluþturmaz. Fakat Git, olay olup bittikten sonra neyin ne olduðunu anlamakta oldukça beceriklidir —dosya hareketlerini keþfetme meselesini birazdan ele alacaðýz.

Bu nedenle Git'in bir `mv` komutu olmasý biraz kafa karýþtýrýcý olabilir. Git'te bir dosyanýn adýný deðiþtirmek istiyorsanýz, þöyle bir komut çalýþtýrabilirsiniz:

	$ git mv eski_dosya yeni_dosya

ve istediðinizi elde edersiniz. Hatta, buna benzer bir komut çalýþtýrdýktan sonra `status` çýktýsýna bakarsanýz Git'in bir dosya adlandýrma iþlemini (_rename_) listelediðini görürsünüz:

	$ git mv README.txt README
	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 1 commit.
	#
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       renamed:    README.txt -> README
	#

Öte yandan bu komut, þu komutlarý arka arkaya çalýþtýrmaya eþdeðerdir:

	$ mv README.txt README
	$ git rm README.txt
	$ git add README

Git dosya taþýma iþlemini dolaylý yollardan anlar, dolayýsýyla dosyayý yeniden adlandýrmayý bu komutlarla mý yaptýðýnýz yoksa `mv` komutunu mu kullandýðýnýz Git açýsýndan önemli deðildir. Tek gerçek fark arka arkaya üç komut kullanmak yerine tek bir komut kullanýyor olmanýzdýr —`mv` bir kullanýcýya kolalýk saðlayan bir komuttur. Daha önemlisi, bir dosyanýn adýný deðiþtirmek için istediðiniz her aracý kullanabilir, `add/rm` iþlemlerini sonraya kayýttan hemen öncesine býrakabilirsiniz.

## Kayýt Tarihçesini Görüntülemek ##

Birkaç kayýt oluþturduktan, ya da halihazýrda kayýt tarihçesi olan bir yazýlým havuzunu klonladýðýnýzda, muhtemelen geçmiþe bakýp neler olduðuna göz atmak isteyeceksiniz. Bunun için kullanabileceðiniz en temel ve becerikli araç `git log` komutudur.

Buradaki örnekler benim çoðunlukla tanýtýmlarda kullandýðým `simplegit` adýnda bir projeyi kullanýyor. Projeyi edinmek için aþaðýdaki komutu çalýþtýrabilirsiniz:

	git clone git://github.com/schacon/simplegit-progit.git

Bu projenin içinde `git log` komutunu çalýþtýrdýðýnýzda þuna benzer bir çýktý göreceksiniz:

	$ git log
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	commit a11bef06a3f659402fe7563abf99ad00de2209e6
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 10:31:28 2008 -0700

	    first commit

Aksi belirtilmedikçe, `git log` bir yazýlým havuzundaki kayýtlarý ters kronolojik sýrada listeler. Yani, en son kayýtlar en üstte görünür. Görüldüðü gibi, bu komut her kaydýn SHA-1 sýnama toplamýný, yazarýnýn adýný ve adresini, kaydedildiði tarihi ve kayýt mesajýný listeler.

`git log` komutunun, size tam olarak aradýðýnýz þeyi göstermek için kullanýlabilecek çok sayýda seçeneði vardýr. Burada, en çok kullanýlan bazý seçenekleri tanýtacaðýz.

En yararlý seçeneklerden biri, kaydýn içeriðini (_diff_) gösteren `-p` seçeneðidir. Ýsterseniz `-2`'yi kullanarak komutun çýktýsýný son iki kayýtla sýnýrlayabilirsiniz:

	$ git log -p -2
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	diff --git a/Rakefile b/Rakefile
	index a874b73..8f94139 100644
	--- a/Rakefile
	+++ b/Rakefile
	@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
	 spec = Gem::Specification.new do |s|
	-    s.version   =   "0.1.0"
	+    s.version   =   "0.1.1"
	     s.author    =   "Scott Chacon"

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	diff --git a/lib/simplegit.rb b/lib/simplegit.rb
	index a0a60ae..47c6340 100644
	--- a/lib/simplegit.rb
	+++ b/lib/simplegit.rb
	@@ -18,8 +18,3 @@ class SimpleGit
	     end

	 end
	-
	-if $0 == __FILE__
	-  git = SimpleGit.new
	-  puts git.show
	-end
	\ No newline at end of file

Bu seçenek daha önceki bilgilere ek olarak kaydýn içeriðini de her gösterir. Bu, yazýlýmý gözden geçirirken ya da belirli bir katýlýmcý tarafýndan yapýlan bir dizi kayýt sýrasýnda nelerin deðiþtiðine hýzlýca göz atarken çok iþe yarar.

Dilerseniz `git log`'u özet bilgiler veren bir dizi seçenekle birlikte kullanabilirsiniz. Örneðin, her kayýtla ilgili özet istatistikler için `--stat` seçeneðini kullanabilirsiniz:

	$ git log --stat
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	 Rakefile |    2 +-
	 1 files changed, 1 insertions(+), 1 deletions(-)

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	 lib/simplegit.rb |    5 -----
	 1 files changed, 0 insertions(+), 5 deletions(-)

	commit a11bef06a3f659402fe7563abf99ad00de2209e6
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 10:31:28 2008 -0700

	    first commit

	 README           |    6 ++++++
	 Rakefile         |   23 +++++++++++++++++++++++
	 lib/simplegit.rb |   25 +++++++++++++++++++++++++
	 3 files changed, 54 insertions(+), 0 deletions(-)

Gördüðünüz gibi `--stat`  seçeneði, her kaydýn altýna o kayýtta deðiþikliðe uðramýþ dosyalarýn listesini, kaç tane dosyanýn deðiþikliðe uðradýðýný ve söz konusu dosyalara kaç satýrýn eklenip çýkarýldýðý bilgisini ekler. Bu bilgilerin bir özetini de kaydýn en altýna yerleþtirir. Oldukça yararlý bir baþka seçenek de `--pretty` seçeneðidir. Bu seçenek `log` çýktýsýnýn biçimini deðiþtirmek için kullanýlýr. Bu seçenekle birlikte kullanacaðýnýz birkaç tane öntanýmlý ek seçenek vardýr. `oneline` ek seçeneði her bir kaydý tek bir satýrda gösterir; bu çok sayýda kayda göz atýyorsanýz yararlý olabilir. Ayrýca `short`, `full` ve `fuller` seçenekleri aþaðý yukarý ayný miktarda bilgiyi —bazý farklarla— gösterir:

	$ git log --pretty=oneline
	ca82a6dff817ec66f44342007202690a93763949 changed the version number
	085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
	a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

En ilginç ek seçenek, istediðiniz log çýktýsýný belirlemenizi saðlayan `format` ek seçeneðidir. Bu, özellikle bilgisayar tarafýndan iþlenecek bir çýktý oluþturmak konusunda elveriþlidir —biçimi açýkça kendiniz belirlediðiniz için farklý Git sürümlerinde farklý sonuçlarla karþýlaþmazsýnýz:

	$ git log --pretty=format:"%h - %an, %ar : %s"
	ca82a6d - Scott Chacon, 11 months ago : changed the version number
	085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
	a11bef0 - Scott Chacon, 11 months ago : first commit

Tablo 2-1 `format` ek seçeneðinin kabul ettiði bazý biçimlendirme seçeneklerini gösteriyor.

	Seçenek	Çýktýnýn Açýklamasý
	%H	Sýnama toplamý
	%h	Kýsaltýlmýþ sýnama toplamý
	%T	Git aðacý sýnama toplamý
	%t	Kýsaltýlmýþ Git aðacý sýnama toplamý
	%P	Ata kayýtlarýn sýnama toplamlarý
	%p	Ata kayýtlarýn kýsaltýlmýþ sýnama toplamlarý
	%an	Yazarýn adý
	%ae	Yazarýn e-posta adresi
	%ad	Yazýlma tarihi (–date= seçeneðiyle uyumludur)
	%ar	Yazýlma tarihi (göreceli tarih)
	%cn	Kaydedenin adý
	%ce	Kaydedenin e-posta adresi
	%cd	Kaydedilme tarihi
	%cr	Kaydedilme tarihi (göreceli tarih)
	%s	Konu

_yazar_'la _kaydeden_ arasýnda ne gibi bir fark olduðunu merak ediyor olabilirsiniz. _yazar_ yamayý oluþturan kiþidir, _kaydeden_'se yamayý projeye uygulayan kiþi. Bir projeye yama gönderdiðinizde, projenin çekirdek üyelerinden biri yamayý projeye uygularsa, her ikinizin de adý kaydedilecektir —sizin adýnýz yazar olarak onun adý kaydeden olarak. Bu farký _5. Bölüm_'de biraz daha ayrýntýlý olarak ele alacaðýz.

`oneline` ve `format` ek seçenekleri özellikle `--graph` ek seçeneðiyle birlikte kullanýldýklarýnda çok iþe yararlar. Bu ek seçenek projenizin dal (_branch_) ve birleþtirme (_merge_) tarihçesini gösteren sevimli bir ASCII grafiði oluþturur. Grit yazýlým havuzunun grafiðine bakalým:

	$ git log --pretty=format:"%h %s" --graph
	* 2d3acf9 ignore errors from SIGCHLD on trap
	*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
	|\
	| * 420eac9 Added a method for getting the current branch.
	* | 30e367c timeout code and tests
	* | 5a09431 add timeout protection to grit
	* | e1193f8 support for heads with slashes in them
	|/
	* d6016bc require time for xmlschema
	*  11d191e Merge branch 'defunkt' into local

Bunlar `git log`'la birlikte kullanabileceðiniz seçeneklerden yalnýzca birkaçý —daha baþka çok sayýda seçenek var. Tablo 2-2 yukarýda incelediðimiz seçeneklerin yaný sýra, yararlý olabilecek baþka seçenekleri `git log` çýktýsýna olan etkileriyle birlikte listeliyor.

	Seçenek	Açýklama
	-p	Kayýtlarýn içeriklerini de göster.
	--stat	Kayýtlarda deðiþikliðe uðrayan dosyalarla ilgili istatistikleri göster.
	--shortstat	Yalnýzca deðiþikliði/eklemeyi/çýkarmayý özetleyen satýrý göster command.
	--name-only	Kayýtlarda deðiþen dosyalarýn yalnýzca adlarýný göster.
	--name-status	Kayýtlarda deðiþen dosyalarýn adlarýyla birlikte deðiþme/eklenme/çýkarýlma bilgisini de göster.
	--abbrev-commit	Sýnama toplamýnýn 40 karakterli tamamý yerine yalnýzca ilk birkaç karakterini göster.
	--relative-date	Tarihi gün, ay, yýl olarak göstermek yerine göreceli olarak göster ("iki hafta önce" gibi).
	--graph	Log tarihçesinin yanýsýra, dal ve birleþtirme tarihçesini ASCII grafiði olarak göster.
	--pretty	Kayýtlarý alternatif bir biçimlendirmeyle göster. `oneline` `short`, `full`, `fuller` ve (kendi istediðiniz biçimi belirleyebildiðiniz) `format` ek seçenekleri kullanýlabilir.

### Log Çýktýsýný Sýnýrlandýrma ###

`git log` komutu, biçimlendirme seçeneklerinin yaný sýra bir dizi sýnýrlandýrma seçeneði de sunar —bu seçenekler kayýtlarýn yalnýzca bir alt kümesini gösterir. Bu seçeneklerden birini yukarýda gördünüz —yalnýzca son iki kaydý gösteren `-2` seçeneðini. Aslýnda, son `n` kaydý görmek için `n` yerine herhangi bir tam sayý koyarak bu seçeneði `-<n>` biçiminde kullanabilirsiniz. Bunu muhtemelen çok sýk kullanmazsýnýz, zira Git `log` çýktýsýný zaten sayfa sayfa gösteriyor, dolayýsýyla `git log` komutunu çalýþtýrdýðýnýzda zaten önce kayýtlarýn birinci sayfasýný göreceksiniz.

Öte yandan `--since` ya da `--until` gibi çýktýyý zamanla sýnýrlayan seçenekler iþinizi kolaylaþtýrabilir. Söz gelimi, þu komut, son iki hafta içinde yapýlmýþ kayýtlarý listeliyor:

	$ git log --since=2.weeks

Bu komut pek çok deðiþik biçimlendirmeyle kullanýlabilir —kesin bir tarih (“2008-01-15”) ya da “2 years 1 day 3 minutes ago” gibi göreli bir tarih kullanabilirsiniz.

Ayrýca listeyi belirli arama ölçütlerine uyacak biçimde filtreleyebilirsiniz. `--author` seçeneði belirli bir yazara aiy kayýtlarý filtrelemenizi saðlar; `--grep` seçeneðiyse kayýt mesajlarýnda anahtar kelimeler aramanýzý saðlar. (Unutmayýn, hem `author` hem de `grep` seçeneklerini kullanmak istiyorsanýz, komuta `--all-match` seçeneðini de eklemelisiniz, aksi takdirde, komut bu iki seçenekten herhangi birine uyan kayýtlarý bulup getirecektir.)

`git log`la kullanýlmasý son derece yararlý olan son seçenek konum seçeneðidir. `git log`'u bir klasör ya da dosya adýyla birlikte kullanýrsanýz, komutun çýktýsýný yalnýzca o dosyalarda deðiþiklik yapan kayýtlarla sýnýrlamýþ olursunuz. Bu, komuta her zaman en son seçenek olarak eklenmelidir ve konumlar iki tireyle (`--`) diðer seçeneklerden ayrýlmalýdýr.

Tablo 2-3, bu seçenekleri ve birkaç baþka yaygýn seçeneði listeliyor.

	Seçenek	Açýklama
	-(n)	Yalnýzca son n kaydý göster.
	--since, --after	Yalnýzca belirli bir tarihten sonra eklenmiþ kayýtllarý göster.
	--until, --before	Yalnýzca belirli bir tarihten önce yapýlmýþ kayýtlarý göster.
	--author	Yalnýzca yazarýn adýnýn belirli bir karakter katarýyla (_string_) eþleþen kayýtlarý göster.
	--committer	Yalnýzca kaydedenin adýnýn belirli bir karakter katarýyla eþleþtiði kayýtlarý göster.

Örneðin, Git kaynak kod yazýlým havuzu tarihçesinde birleþtirme (_merge_) olmayan hangi kayýtlarýn Junio Hamano tarafýndan 2008'in Ekim ayýnda kaydedilmiþ olduðunu görmek için þu komutu çalýþtýrabilirsiniz:

	$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
	   --before="2008-11-01" --no-merges -- t/
	5610e3b - Fix testcase failure when extended attribute
	acd3b9e - Enhance hold_lock_file_for_{update,append}()
	f563754 - demonstrate breakage of detached checkout wi
	d1a43f2 - reset --hard/read-tree --reset -u: remove un
	51a94af - Fix "checkout --track -b newbranch" on detac
	b0ad11e - pull: allow "git pull origin $something:$cur

Bu komut, Git kaynak kodu yazýlým havuzundaki yaklaþýk 20,000 komut arasýndan bu ölçütlere uyan 6 tanesini gösteriyor.

### Tarihçeyi Görselleþtirmek için Grafik Arayüz Kullanýmý ###

Kayýt tarihçenizi görüntülemek için görselliði daha çok ön planda olan bir araç kullanmak isterseniz, Git'le birlikte daðýtýlan bir Tcl/Tk programý olan `gitk`'ya bir göz atmak isteyebilirsiniz. Gitk, temelde `git log`'u görselleþtiren bir araçtýr ve neredeyse `git log`'un kabul ettiði bütün filtreleme seçeneklerini tanýr. Proje klasörünüzde komut satýrýna `gitk` yazacak olursanýz Figür 2-2'deki gibi bir þey görürsünüz.

Insert 18333fig0202.png
Figür 2-2. gitk grafiklse tarihçe görüntüleyicisi.

Pencerenin üst yarýsýnda bir kalýtým grafiðinin yaný sýra kayýt tarihçesini görebilirsiniz. Alttaki kayýt içeriði görüntüleyicisi, týkladýðýnýz herhangi bir kayýttaki deðiþiklikleri gösterecektir.

## Deðiþiklikleri Geri Almak ##

Herhangi bir noktada yaptýðýnýz bir deðiþikliði geri almak isteyebilirsiniz. Burada yapýlan deðiþiklikleri geri almakta kullanýlabilecek bazý araçlarý inceleyeceðiz. Dikkatli olun, zira geri alýnan bu deðiþikliklerden bazýlarýný yeniden gerçekleþtiremeyebilirsiniz. Bu, eðer bir hata yaparsanýz, bunu Git'i kullanarak telafi edemeyeceðiniz, az sayýda alanýndan biridir.

### Son Kayýt Ýþlemini Deðiþtirmek ###

Eðer kaydý çok erken yapmýþsanýz, bazý dosyalarý eklemeyi unutmuþsanýz ya da kayýt mesajýnda hata yapmýþsanýz, sýk rastlanan düzeltme iþlemlerinden birini kullanabilirsiniz. Kaydý deðiþtirmek isterseniz, `commit` komutunu `--amend` seçeneðiyle çalýþtýrabilirsiniz:

	$ git commit --amend

Bu komut, hazýrlýk alanýndaki deðiþiklikleri alýp bunlarý kaydý deðiþtirmek için kullanýr. Eðer son kaydýnýzdan beri hiçbir deðiþiklik yapmamýþsanýz (örneðin, bu komutu yeni bir kayýt yaptýktan hemen sonra çalýþtýrýyorsanýz) o zaman kaydýnýzýn bellek kopyasý ayný kalacak ve deðiþtireceðiniz tek þey kayýt mesajý olacaktýr.

Ayný kayýt mesajý editörü açýlýr, fakat editörde bir önceki kaydýn kayýt mesajý görünür. Mesajý her zamanki gibi deðiþtirebilirsiniz, ama bu yeni kayýt mesajý öncekinin yerine geçecektir.

Söz gelimi, eðer bir kayýt sýrasýnda belirli bir dosyada yaptýðýnýz deðiþiklikleri kayda hazýrlamayý unuttuðunuzu fark ederseniz, þöyle bir þey yapabilirsiniz:

	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend

Bu üç komuttan sonra, tarihçenize tek bir kayýt iþlenmiþtir —son kayýt öncekinin yerine geçer.

### Kayda Hazýrlanmýþ Bir Dosyayý Hazýrlýk Alanýndan Kaldýrmak ###

Bu iki alt bölüm kayda hazýrlýk alanýndaki ve çalýþma klasörünüzdeki deðiþiklikleri nasýl idare edeceðinizi gösteriyor. Ýþin güzel yaný, bu iki alanýn durumunu öðrenmek için kullanacaðýnýz komut ayný zamanda bu alanlardaki deðiþiklikleri nasýl geri alabileceðinizi de hatýrlatýyor. Diyelim ki iki dosyayý deðiþtirdiniz ve bu iki deðiþikliði ayrý birer kayýt olarak iþlemek istiyorsunuz, ama yanlýþlýkla `git add *` komutunu kullanarak ikisini birden hazýrlýk alanýna aldýnýz. Bunlardan birini nasýl hazýrlýk alanýndan çýkarabilirsiniz? `git status` komutu size bunu da anýmsatýyor:

	$ git add .
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#       modified:   benchmarks.rb
	#

“Changes to be committed” yazýsýnýn hemen altýnda "use `git reset HEAD <file>...` to unstage" yazdýðýný görüyoruz. `benchmarks.rb` dosyasýný bu öneriye uygun olarak hazýrlýk alanýndan kaldýralým:

	$ git reset HEAD benchmarks.rb
	benchmarks.rb: locally modified
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   benchmarks.rb
	#

Komut biraz tuhaf, ama iþ görüyor. `benchmarks.rb` dosyasý hazýrlýk alanýndan kaldýrýldý ama hâlâ deðiþmiþ olarak görünüyor.

### Deðiþmiþ Durumdaki Bir Dosyayý Deðiþmemiþ Duruma Geri Getirmek ###

Peki `benchmarks.rb` dosyasýndaki deðiþiklikleri korumak istemiyorsanýz? Yaptýðýnýz deðiþiklikleri kolayca nasýl geri alacaksýnýz —son kayýtta nasýl görünüyorsa o haline (ya da ilk klonlandýðý haline, yahut çalýþma klasörünüze ilk aldýðýnýz haline) nasýl geri getireceksiniz? Neyse ki `git status` komutu bunu nasýl yapacaðýnýzý da söylüyor. Son örnek çýktýda hazýrlýk alaný dýþýndaki deðiþiklikler þöyle görünüyor:

	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   benchmarks.rb
	#

Yaptýðýnýz deðiþiklikleri nasýl çöpe atabileceðinizi açýkça söylüyor (en azýndan Git'in 1.6.1'le baþlayan yeni sürümleri bunu yapýyor —eðer daha eski bir sürümle çalýþýyorsanýz, kolaylýk saðlayan bu özellikleri edinebilmek için programý güncellemenizi öneririz). Gelin, söyleneni yapalým:

	$ git checkout -- benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#

Gördüðünüz gibi deðiþiklikler çöpe atýldý. Bunun tehlikeli bir komut olduðunu aklýnýzdan çýkarmayýn: o dosyaya yaptýðýnýz bütün deðiþiklikler þimdi yok oldu —dosyanýn üstüne yeni bir dosya kopyaladýnýz. Eðer dosyadaki deðiþiklikleri istemediðinizden yüzde yüz emin deðilseniz asla bu komutu kullanmayýn. Eðer sorun bu dosyada yaptýðýnýz deðiþikliklerin baþka iþlemler yapmanýza engel olmasý ise bir sonraki bölümde ele alacaðýmýz zulalama (_stash_) ve dallandýrma (_branch_) iþlemlerini kullanmanýz daha iyi olacaktýr.

Unutmayýn, Git'te kaydedilmiþ her þey neredeyse her zaman kurtarýlabilir. Silinmiþ dallardaki kayýtlar ve hatta `--amend` seçeneðiyle üzerine yazýlmýþ kayýtlar bile kurtarýlabilirler (veri kurtarma konusunda bkz. _9. Bölüm_). Diðer taraftan, kaydedilmemiþ bir deðiþikliði kaybederseniz büyük olasýlýkla onu kurtarmanýz mümkün olmaz.

## Uzak Uçbirimlerle Çalýþmak ##

Bir Git projesine katkýda bulunabilmek için uzaktaki yazýlým havuzlarýný nasýl düzenleyeceðinizi bilmeniz gerekir. Uzaktaki yazýlým havuzlarý, projenizin Ýnternet'te ya da baþka bir aðda barýndýrýlan sürümleridir. Birden fazla uzak yazýlým havuzunuz olabilir, bunlardan her biri sizin için ya salt okunur ya da okunur/yazýlýr durumdadýr. Baþkalarýyla ortak çalýþmak, bu yazýlým havuzlarýný düzenlemeyi, onlardan veri çekip (_pull_) onlara veri iterek (_push_) çalýþmalarýnýzý paylaþmayý gerektirir.

Uzaktaki yazýlým havuzlarýnýzý düzenleyebilmek için, projenize uzak yazýlým havuzlarýnýn nasýl ekleneceðini, kullanýlmayan havuzlarýn nasýl çýkarýlacaðýný, çeþitli uzak dallarý düzenlemeyi ve onlarýn izlenen dallar olarak belirleyip belirlememeyi ve daha baþka þeyleri gerektirir. Bu alt bölümde bu uzaðý yönetme yeteneklerini inceleyeceðiz.

### Uzak Uçbirimleri Görüntüleme ###

Projenizde hangi uzak sunucularý ayarladýðýnýzý görme için `git remote` komutunu kullanabilirsiniz. Bu komut, her bir uzak uçbirimin belirlenmiþ kýsa adýný görüntüler. Eðer yazýlým havuzunuzu bir yerden klonlamýþsanýz, en azýndan _origin_ uzak uçbirimini görmelisiniz —bu Git'in klonlamanýn yapýldýðý sunucuya verdiði öntanýmlý addýr.

	$ git clone git://github.com/schacon/ticgit.git
	Initialized empty Git repository in /private/tmp/ticgit/.git/
	remote: Counting objects: 595, done.
	remote: Compressing objects: 100% (269/269), done.
	remote: Total 595 (delta 255), reused 589 (delta 253)
	Receiving objects: 100% (595/595), 73.31 KiB | 1 KiB/s, done.
	Resolving deltas: 100% (255/255), done.
	$ cd ticgit
	$ git remote
	origin

`-v` seçeneðini kullanarak Git'in bu kýsa ad için depoladýðý URL'yi de görebilirsiniz:

	$ git remote -v
	origin	git://github.com/schacon/ticgit.git

Projenizde birden çok uzak uçbirim varsa, bu komut hepsini listeleyecektir. Örneðin, benim Git yazýlým havuzum þöyle görünüyor:

	$ cd grit
	$ git remote -v
	bakkdoor  git://github.com/bakkdoor/grit.git
	cho45     git://github.com/cho45/grit.git
	defunkt   git://github.com/defunkt/grit.git
	koke      git://github.com/koke/grit.git
	origin    git@github.com:mojombo/grit.git

Bu demek oluyor ki bu kullanýcýlarýn herhangi birinden kolaylýkla çekme iþlemi (_pull_) yapabiliriz. Fakat dikkat ederseniz, yalnýzca _origin_ uçbiriminin SSH URL'si var, yani yalnýzca o havuza kod itebiliriz (_push_) (niye böyle olduðunu _4. Bölüm_'de inceleyeceðiz)

### Uzak Uçbirimler Eklemek ###

Önceki alt bölümlerde uzak uçbirim eklemekten söz ettim ve bazý örnekler verdim, ama bir kez daha konuyu açýkça incelemekte yarar var. Uzaktaki bir yazýlým havuzunu kýsa bir ad vererek eklemek için `git remote add [kisa_ad] [url]` komutunu çalýþtýrýn:

	$ git remote
	origin
	$ git remote add pb git://github.com/paulboone/ticgit.git
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git
	pb	git://github.com/paulboone/ticgit.git

Artýk bütün bir URL yerine `pb`'yi kullanabilirsiniz. Örneðin, Paul'ün yazýlým havuzunda bulunan ama sizde bulunmayan bütün bilgileri getirmek için `git fetch pb` komutunu kullanabilirsiniz:

	$ git fetch pb
	remote: Counting objects: 58, done.
	remote: Compressing objects: 100% (41/41), done.
	remote: Total 44 (delta 24), reused 1 (delta 0)
	Unpacking objects: 100% (44/44), done.
	From git://github.com/paulboone/ticgit
	 * [new branch]      master     -> pb/master
	 * [new branch]      ticgit     -> pb/ticgit

Paul'ün `mastertr` dalý sizin yazýlým havuzunuzda da `pb/master` olarak eriþilebilir durumdadýr —kendi dallarýnýzdan biriyle birleþtirebilir (_merge_) ya da bir yerel dal olarak seçip içeriðini inceleyebilirsiniz.

### Uzak Uçbirimlerden Getirme ve Çekme Ýþlemi Yapmak ###

Biraz önce gördüðünüz gibi, uzaktaki yazýlým havuzlarýndan veri almak için þu komutu kullanabilirsiniz:

	$ git fetch [uzak-sunucu-adý]

Bu komut, söz konusu uzaktaki yazýlým havuzuna gidip orada bulunup da sizin projenizde bulunmayan bütün veriyi getirir. Bunu yaptýktan sonra sizin projenizde o uzak yazýlým havuzundaki bütün dallara referanslar oluþur —ki bunlarý birleþtirme yapmak ya da içeriði incelemek için kullanabilirsiniz. (Dallarýn ne olduðunu ve onlarý nasýl kullanabileceðinizi _3. Bölüm_'de ayrýntýlý biçimde inceleyeceðiz.)

Bir yazýlým havuzunu klonladýðýnýzda, klonlama komutu söz konusu kaynak yazýlým havuzunu _origin_ adýyla uzak uçbirimler arasýna ekler. Dolayýsýyla, `git fetch origin` komutu, klonlamayý yaptýðýnýzdan (ya da en son getirme iþlemini (_fetch_) yatýðýnýzdan) beri sunucuya itilmiþ yeni deðiþiklikleri getirir. Unutmayýn, `fetch` komutu veriyi yeler yazýlým havuzunuza indirir —otomatik olarak sizin yaptýklarýnýzla birleþtirmeye, ya da çalýþtýðýnýz þeyler üzerinde deðiþiklik yapmaya kalkýþmaz. Hazýr olduðunuzda birleþtirme iþlemini sizin yapmanýz gerekir.

Uzaktaki bir dalý izlemek üzere ayarlanmýþ bir dalýnýz varsa (daha fazla bilgi için sonraki alt bölüme ve _3. Bölüm_'e bakýnýz) bu dal üzerinde `git pull` komutunu kullanarak uzaktaki yazýlým havuzundaki veriyi hem getirip hem de mevcut dalýnýzla birleþtirebilirsiniz. Bu çalýþmasý daha kolay bir düzen olabilir; bu arada, `git clone ` komutu, otomatik olarak, yerel yazýlým havuzunuzda, uzaktaki yazýlým havuzunun `master` dalýný takip eden bir `master` dalý oluþturur (uzaktaki yazýlým havuzunun `master` adýnda bir dalý olmasý koþuluyla). `git pull` komutu genellikle yereldeki yazýlým havuzunuza kaynaklýk eden sunucudan veriyi getirip otomatik olarak üzerinde çalýþmakta olduðunuz dalla birleþtirir.

### Uzaktaki Yazýlým Havuzuna Veri Ýtmek ###

Projeniz paylaþmak istediðiniz bir hale geldiðinde, yaptýklarýnýzý kaynaða itmeniz gerekir. Bunun için kullanýlan komut basittir: `git push [uzak-sunucu-adi] [dal-adi]`. Projenizdeki `master` dalýný `origin` sunucunuzdaki `master` dalýna itmek isterseniz (yineleyelim; kolanlama iþlemi genellikle bu isimleri otomatik olarak oluþturur), þu komutu kullanabilirsiniz:

	$ git push origin master

Bu komut, yalnýzca yazma yetkisine sahip olduðunuz bir sunucudan klonlama yapmýþsanýz ve son getirme iþleminizden beri hiç kimse itme iþlemi yapmamýþsa istediðiniz sonucu verir. Eðer sizinle birlikte bir baþkasý daha klonlama yapmýþsa ve o kiþi sizden önce itme yapmýþsa, sizin itme iþleminiz reddedilir. Ýtmeden önce sizden önce itilmiþ deðiþiklikleri çekip kendi çalýþmanýzla birleþtirmeniz gerekir. Uzaktaki yazýlým havuzlarýna itme yapmak konusunda daha ayrýntýlý bilgi için bkz. _3. Bölüm_.

### Uzak Uçbirim Hakkýnda Bilgi Almak ###

Belirli bir uzak uçbirim hakkýnda daha fazla bilgi almak isterseniz `git remote show [ucbirim-adi]` komutunu kullanabilirsiniz. Bu komutu `origin` gibi belirli bir uçbirim kýsa adýyla kullanýrsanýz þöyle bir sonuç alýrsýnýz:

	$ git remote show origin
	* remote origin
	  URL: git://github.com/schacon/ticgit.git
	  Remote branch merged with 'git pull' while on branch master
	    master
	  Tracked remote branches
	    master
	    ticgit

Bu, uçbirimin URL'sini ve dallarýn izlenme durumunu gösterir. Komut, size, eðer `master` dalda iseniz ve `git pull` komutunu çalýþtýrýrsanýz, bütün referanslarý uzak uçbirimden indirip uzaktaki `master` dalýndan yerel `master` dalýna birleþtirme yapacaðýný da söylüyor. Ayrýca, ekmiþ olduðu bütün uzak dallarý da bir liste halinde veriyor.

Yukarýdaki verdiðimiz, basit bir örnekti. Git'i daha yoðun biçimde kullandýðýnýzda, `git remote show` komutu çok daha fazla bilgi içerecektir:

	$ git remote show origin
	* remote origin
	  URL: git@github.com:defunkt/github.git
	  Remote branch merged with 'git pull' while on branch issues
	    issues
	  Remote branch merged with 'git pull' while on branch master
	    master
	  New remote branches (next fetch will store in remotes/origin)
	    caching
	  Stale tracking branches (use 'git remote prune')
	    libwalker
	    walker2
	  Tracked remote branches
	    acl
	    apiv2
	    dashboard2
	    issues
	    master
	    postgres
	  Local branch pushed with 'git push'
	    master:master

Bu çýktý, belirli dallarda `git push` komutunu çalýþtýrdýðýnýzda hangi dallarýn otomatik olarak itileceðini gösteriyor. Buna ek olarak uzak uçbirimde bulunup da sizin projenizde henüz bulunmayan uzak dallarý, uzak uçbirimden silinmiþ olduðu halde sizin projenizde bulunan dallarý ve `git pull` komutunu çalýþtýrdýðýnýzda otomatik olarak birleþtirme iþlemine uðrayacak birden çok dalý gösteriyor.

### Uzan Uçbirimleri Kaldýrmak ve Yeniden Adlandýrmak ###

Bir uçbirimin kýsa adýný deðiþtirmek isterseniz, Git'in yeni sürümlerinde bunu `git remote rename` komutuyla yapabilirsiniz. Örneðin, `pb` uçbirimini `paul` diye yeniden adlandýrmak isterseniz, bunu `git remote rename`'i kullanarak yapabilirsiniz:

	$ git remote rename pb paul
	$ git remote
	origin
	paul

Bu iþlemin uçbirim dal adlarýný da deðiþtirdiðini hatýrlatmakta yarar var. Bu iþlemden önce `pb/master` olan dalýn adý artýk `paul/master` olacaktýr.

Bir uçbirim referansýný herhangi bir nedenle —sunucuyu taþýmýþ ya da belirli bir yansýyý artýk kullanmýyor olabilirsiniz; ya da belki katýlýmcýlardan birisi artýk katkýda bulunmuyordur— kaldýrmak isterseniz `git remote rm` komutunu kullanabilirsiniz:

	$ git remote rm paul
	$ git remote
	origin

## Etiketleme ##

Çoðu SKS gibi Git'in de tarihçedeki belirli noktalarý önemli olarak etiketleyebilme özelliði vardýr. Genellikle insanlar bu iþlevi sürümleri (`v1.0`, vs.) iþaretlemek için kullanýrlar. Bu alt bölümde mevcut etiketleri nasýl listeleyebileceðinizi, nasýl yeni etiketler oluþturabileceðinizi ve deðiþik etiket tiplerini öðreneceksiniz.

### Etiketlerinizi Listeleme ###

Git'te mevcut etiketleri listeleme iþi epeyi kolaydýr. `git tag` yazmanýz yeterlidir:

	$ git tag
	v0.1
	v1.3

Bu komut etiketleri alfabetik biçimde sýralar; etiketlerin sýrasýnýn bir önemi yoktur.

Ýsterseniz belirli bir örüntüyle eþleþen etiketleri de arayabilirsiniz. Git kaynak yazýlým havuzunda 240'tan fazla etiket vardýr. Yalnýzca 1.4.2 serisindeki etiketleri görmek isterseniz þu komutu çalýþtýrmalýsýnýz:

	$ git tag -l 'v1.4.2.*'
	v1.4.2.1
	v1.4.2.2
	v1.4.2.3
	v1.4.2.4

### Etiket Oluþturma ###

Git iki baþlýca etiket tipi kullanýr: hafif ve açýklamalý. Hafif etiketler hiç deðiþmeyen dallar gibidir —belirli bir kaydý iþaret ederler. Öte yandan, açýklamalý etiketler, Git veritabanýnda bütünlüklü nesneler olarak kaydedilirler. Sýnama toplamlarý alýnýr; etiketleyenin adýný ve e-posta adresini içerirler; bir etiket mesajýna sahiptirler ve GNU Privacy Guard (GPG) kullanýlarak imzalanýp doðrulanabilirler. Genellikle bütün bu bilgilere ulaþýlabilmesini olanaklý kýlabilmek için açýklamalý etiketlerin kullanýlmasý önerilir, ama bütün bu bilgileri depolamadan yalnýzca geçici bir etiket oluþturmak istiyorsanýz, hafif etiketleri de kullanabilirsiniz.

### Açýklamalý Etiketler ###

Git'te açýklamalý etiket oluþturmak basittir. En kolayý `tag` komutunu çalýþtýrýrken `-a` seçeneðini kullanmaktýr:

	$ git tag -a v1.4 -m 'sürümüm 1.4'
	$ git tag
	v0.1
	v1.3
	v1.4

`-m` seçeneði etiketle birlikte depolanacak etiketleme mesajýný belirlemek için kullanýlýr. Açýklamalý bir etiket için mesajý bu þekilde belirlemezseniz, Git mesajý yazabilmeniz için bir editör açacaktýr.

`git show` komutunu kullanarak etiketlenen kayýtla birlikte etikete iliþkin verileri de görebilirsiniz:

	$ git show v1.4
	tag v1.4
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 14:45:11 2009 -0800

	my version 1.4
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

Bu, kayýt bilgisinden önce etiketleyenle ilgili bilgileri, kaydýn etiketlendiði tarihi ve açýklama mesajýný gösterir.

### Ýmzalý Etiketler ###

Eðer bir kiþisel anahtarýnýz (_private key_) varsa etiketlerinizi GPG ile imzalayabilirsiniz. Yapmanýz gereken tek þey `-a` yerine `-s` seçeneðini kullanmaktýr:

	$ git tag -s v1.5 -m 'imzalý 1.5 etiketim'
	You need a passphrase to unlock the secret key for
	user: "Scott Chacon <schacon@gee-mail.com>"
	1024-bit DSA key, ID F721C45A, created 2009-02-09

Bu etiket üzerinde `git show` komutunu çalýþtýrýrsanýz, GPG imzasýný da görebilirsiniz:

	$ git show v1.5
	tag v1.5
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 15:22:20 2009 -0800

	imzalý 1.5 etiketim
	-----BEGIN PGP SIGNATURE-----
	Version: GnuPG v1.4.8 (Darwin)

	iEYEABECAAYFAkmQurIACgkQON3DxfchxFr5cACeIMN+ZxLKggJQf0QYiQBwgySN
	Ki0An2JeAVUCAiJ7Ox6ZEtK+NvZAj82/
	=WryJ
	-----END PGP SIGNATURE-----
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

Birazdan imzalý etiketleri nasýl doðrulayabileceðinizi öðreneceksiniz.

### Hafif Etiketler ###

Kayýtlarý etiketlemenin bir yolu da hafif etiketler kullanmaktýr. Bu, kayýt sýnama toplamýnýn bir dosyada depolanmasýndan ibarettir —baþka hiçbir bilgi tutulmaz. Bir hafif etiket oluþtururken `-a`, `-s` ya da `-m` seçeneklerini kullanmamalýsýnýz.

	$ git tag v1.4-lw
	$ git tag
	v0.1
	v1.3
	v1.4
	v1.4-lw
	v1.5

Þimdi etiket üzerinde `git show` komutunu çalýþtýracak olsanýz, etiketle ilgili ek bilgiler görmezsiniz. Komut yalnýzca kaydý gösterir:

	$ git show v1.4-lw
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

### Etiketleri Doðrulamak ###

Ýmzalý bir etiketi doðrulamak için `git tag -v [etiket-adi]` komutu kullanýlýr. Bu komut imzayý doðrulamak için GPG'yi kullanýr. Bunun düzgün çalýþmasý için imza sahibinin kamusal anahtarý (_public key_) anahtar halkanýzda (_keyring_) bulunmalýdýr.

	$ git tag -v v1.4.2.1
	object 883653babd8ee7ea23e6a5c392bb739348b1eb61
	type commit
	tag v1.4.2.1
	tagger Junio C Hamano <junkio@cox.net> 1158138501 -0700

	GIT 1.4.2.1

	Minor fixes since 1.4.2, including git-mv and git-http with alternates.
	gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
	gpg: Good signature from "Junio C Hamano <junkio@cox.net>"
	gpg:                 aka "[jpeg image of size 1513]"
	Primary key fingerprint: 3565 2A26 2040 E066 C9A7  4A7D C0C6 D9A4 F311 9B9A

Eðer imzalayýcýnýn genel anahtarýna sahip deðilseniz, bunun yerine aþaðýdakine benzer bir þey göreceksiniz:

	gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
	gpg: Can't check signature: public key not found
	error: could not verify the tag 'v1.4.2.1'

### Sonradan Etiketleme ###

Geçmiþteki kayýtlarý da etiketleyebilirsiniz. Diyelim ki Git tarihçeniz þöyle olsun:

	$ git log --pretty=oneline
	15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
	a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
	0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
	6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
	0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
	4682c3261057305bdd616e23b64b0857d832627b added a todo file
	166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
	9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
	964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
	8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

Söz gelimi, "updated rakefile" kaydýnda projenizi `v1.2` olarak etiketlemeniz gerekiyordu, ama unuttunuz. Etiketi sonradan da ekleyebilirsiniz. O kaydý etiketlemek için komutun sonuna kaydýn sýnama toplamýný (ya da bir parçasýný) eklemelisiniz:

	$ git tag -a v1.2 9fceb02

Kaydýn etiketlendiðini göreceksiniz:

	$ git tag
	v0.1
	v1.2
	v1.3
	v1.4
	v1.4-lw
	v1.5

	$ git show v1.2
	tag v1.2
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 15:32:16 2009 -0800

	version 1.2
	commit 9fceb02d0ae598e95dc970b74767f19372d61af8
	Author: Magnus Chacon <mchacon@gee-mail.com>
	Date:   Sun Apr 27 20:43:35 2008 -0700

	    updated rakefile
	...

### Etiketleri Paylaþmak ###

Aksi belirtilmedikçe `git push` komutu etiketleri uzak uçbirimlere aktarmaz. Etiketleri belirtik biçimde bir ortak sunucuya itmeniz gerekir. Bu süreç uçbirim dallarýný paylaþmaya benzer —`git push origin [etiket-adi]` komutunu çalýþtýrabilirsiniz.

	$ git push origin v1.5
	Counting objects: 50, done.
	Compressing objects: 100% (38/38), done.
	Writing objects: 100% (44/44), 4.56 KiB, done.
	Total 44 (delta 18), reused 8 (delta 1)
	To git@github.com:schacon/simplegit.git
	* [new tag]         v1.5 -> v1.5

Bir seferde birden çok etiketi paylaþmak isterseniz, `git push` komutuyla birlikte `--tags` seçeneðini de kullanabilirsiniz. Bu, halihazýrda itilmemiþ olan bütün etiketlerinizi uzak uçbirime aktaracaktýr.

	$ git push origin --tags
	Counting objects: 50, done.
	Compressing objects: 100% (38/38), done.
	Writing objects: 100% (44/44), 4.56 KiB, done.
	Total 44 (delta 18), reused 8 (delta 1)
	To git@github.com:schacon/simplegit.git
	 * [new tag]         v0.1 -> v0.1
	 * [new tag]         v1.2 -> v1.2
	 * [new tag]         v1.4 -> v1.4
	 * [new tag]         v1.4-lw -> v1.4-lw
	 * [new tag]         v1.5 -> v1.5

Artýk baþka biri sizin yazýlým havuzunuzdan çekme yaptýðýnda, bütün etiketlerinize de sahip olacaktýr.

## Ýpuçlarý ##

Git'in temelleri hakkýndaki bu bölümü tamamlamadan önce, Git deneyiminizi kolaylaþtýrabilmek için birkaç ipucu vermekte yarar var. Pek çok insan Git'i bu ipuçlarýna baþvurmadan kullanýyor; bu ipuçlarýndan ileride tekrar söz etmeyeceðimiz gibi bunlarý bilmeniz gerektiðini de varsaymýyoruz; ama yine de bilmeniz yararýnýza olacaktýr.

### Otomatik Tamamlama ###

Eðer Bash -shell_'ini kullanýyorsanýz, Git'in otomatik tamamlama betiðini (_script_) kullanabilirsiniz. Git kaynak kodunu indirip `contrib/completion` klasörüne bakýn; orada `git-completion.bash` adýnda bir dosya olmalý. Bu dosyayý ev dizininize (_home_) kopyalayýp `.bashrc` dosyanýza ekleyin:

	source ~/.git-completion.bash

Otomatik tamamlama özelliðinin bütün Git kullanýcýlarý için geçerli olmasýný istiyorsanýz, bu betik dosyasýný Mac sistemler için `/opt/local/etc/bash_completion.d` konumuna Linux sistemlerde `/etc/bash_completion.d/` konumuna kopyalayýn. Bu, Bash'ýn otomatik olarak yükleyeceði betiklerin bulunduðu bir klasördür.

Eðer bir Windows kullanýcýsýysanýz ve Git Bash kullanýyorsanýz- ki bu msysGit'le kurulum yaptýðýnýzdaki öntanýmlý programdýr, otomatik tamamlama kendiliðinden gelecektir.

Bir Git komutu yazarken Sekme tuþuna bastýðýnýzda, karþýnýza bir dizi seçenek getirir:

	$ git co<selme><sekme>
	commit config

Bu örnekte, `git co` yazýp Sekme tuþuna iki kez basmak `commit` ve `config` komutlarýný öneriyor. Komutun devamýnda `m` yazýp bir kez daha Sekme tuþuna basacak olursanýz, komut otomatik olarak `git commit`'e tamamlanýr.

Bu, seçeneklerde de kullanýlabilir, ki muhtemelen daha yararlý olacaktýr. Örneðin, `git log` komutunu çalýþtýrýrken seçeneklerden birisini hatýrlayamadýnýz, seçeneði yazmaya baþlayýp Sekme tuþuna basarak eþleþen seçenekleri görebilirsiniz:

	$ git log --s<sekme>
	--shortstat  --since=  --src-prefix=  --stat   --summary

Bu güzel özellik sizi zaman kazandýrabileceði gibi ikide bir belgelendirmeye bakma gereðini de ortadan kaldýrýr.

### Takma Adlar ###

Bir komutun bir kýsmýný yazdýðýnýzda Git bunu anlamayacaktýr. Komutlarýn uzun adlarýný kullanmak istemezseniz, `git config` komutunu kullanarak bunlarýn yerine daha kýsa takma adlar belirleyebilirsiniz. Kullanmak isteyebileceðiniz bazý takma adlarý buraya aldýk:

	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status

Bu durumda, örneðin,  `git commit` yazmak yerine `git ci` yazmanýz yeterli olacaktýr. Git'i kullandýkça sýk kullandýðýnýz baþka komutlar da olacaktýr, o zaman o komutlar için de takma adlar oluþturabilirsiniz.

Bu teknik, eksikliðini hissettiðiniz komutlarý oluþturmakta da yararlý olabilir. Örneðin, bir dosyayý hazýrlýk alanýndan kaldýrmak için yapýlmasý gerekenleri yeni bir komut olarak tanýmlayabilirsiniz:

	$ git config --global alias.unstage 'reset HEAD --'

Bu durumda þu iki komut eþdeðer olacaktýr:

	$ git unstage fileA
	$ git reset HEAD fileA

Biraz daha temiz deðil mi? Bir `last` komutu eklemek de oldukça yaygýndýr:

	$ git config --global alias.last 'log -1 HEAD'

Böylece son kaydý kolaylýkla görebilirsiniz:

	$ git last
	commit 66938dae3329c7aebe598c2246a8e6af90d04646
	Author: Josh Goebel <dreamer3@example.com>
	Date:   Tue Aug 26 19:48:51 2008 +0800

	    test for current head

	    Signed-off-by: Scott Chacon <schacon@example.com>

Gördüðünüz gibi Git yeni komutu takma ad olarak belirlediðini þeyin yerine kullanýyor. Ama belki de bir Git komutu çalýþtýrmak deðil de baþka bir program kullanmak istiyorsunuz. Bu durumda komutun baþýna `!` karakterini koymalýsýnýz. Bir Git yazýlým havuzu üzerinde çalýþan kendi araçlarýnýzý yazýyorsanýz bu seçenek yararlý olabilir. Bunu göstermek için ,`gitk`'yi çalýþtýrmak için `git visual` diye yeni bir takma ad tanýmlayabiliriz:

	$ git config --global alias.visual '!gitk'

## Özet ##

Bu noktada, bütün temel Git iþlemlerini yapabiliyorsunuz —bir yazýlým havuzunu yaratmak ya da klonlamak, deðiþiklikler yapmak, bu deðiþiklikleri kayda hazýrlamak ve kaydetmek ve yazýlým havuzundaki bütün kayýtlarýn tarihçesini görüntülemek. Sýradaki bölümde Git'in en vurucu özelliðini, dallanma modelini inceleyeceðiz.