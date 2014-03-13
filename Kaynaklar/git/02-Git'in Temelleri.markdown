# Git'in Temelleri #

Git'i kullanmaya ba�lamak i�in yaln�zca bir b�l�m okuyacak kadar zaman�n�z varsa, o b�l�m, bu b�l�m olmal�. Bu b�l�m, Git'i kullanarak yapaca��n�z �eylerin �ok b�y�k k�sm� i�in kullanaca��n�z b�t�n temel komutlar� i�eriyor. Bu b�l�m�n sonunda bir yaz�l�m havuzunun nas�l yap�land�r�p, ilklenece�ini (_initialize_), dosyalar�n nas�l izlemeye al�n�p izlemeden ��kar�laca��n� ve de�i�ikliklerin nas�l haz�rlan�p kaydedilece�ini ��reneceksiniz. Bunlara ek olarak, Git'i baz� dosyalar� ya da konumlar� belli �r�nt�lere (_pattern_) uyan dosyalar� g�rmezden gelmesi i�in nas�l ayarlayaca��n�z�, hatalar� h�zl�ca ve kolayca nas�l geri alabilece�inizi, projenizin tarih�esine nas�l g�z gezdirip kay�tlar aras�ndaki farklar� nas�l g�r�nt�leyebilece�inizi ve uzak u�birimlerden nas�l kod �ekme i�lemi yapabilece�inizi g�sterece�iz.

## Bir Git Yaz�l�m Havuzu Edinmek ##

Bir Git projesi edinmenin ba�l�ca iki yolu vard�r. Bunlardan ilki, halihaz�rda varolan bir projeyi Git'e aktarmakt�r. �kincisi ise bir sunucuda yer alan bir Git yaz�l�m havuzunu klonlamakd�r.

### Var olan Bir Klas�rde Yaz�l�m Havuzu Olu�turmak ###

Var olan bir projenizi s�r�m kontrol� alt�na almak istiyorsan�z, projenin bulundu�u klas�re gidip a�a��daki komutu �al��t�rman�z gerekir:

	$ git init

Bu, gerekli yaz�l�m havuzu dosyalar�n� �Git iskeletini� i�eren `.git` ad�nda bir klas�r olu�turur. Bu noktada, projenizdeki hi�bir �ey s�r�m kontrol�ne girmi� de�ildir. (Olu�turulan `.git` klas�r�nde tam olarak hangi dosyalar�n bulundu�u hakk�nda daha fazla bilgi edinmek i�in bkz. _9. B�l�m_.)

Var olan dosyalar�n�z� s�r�m kontrol�ne almak istiyorsan�z, o dosyalar� haz�rlay�p kay�t etmelisiniz. Bunu, s�r�m kontrol�ne almak istedi�iniz dosyalar� belirleyip kay�t alt�na ald���n�z birka� git komutuyla ger�ekle�tirebilirsiniz:

	$ git add *.c
	$ git add README
	$ git commit -m 'projenin ilk hali'

Birazdan bu komutlar�n �zerinde duraca��z. Bu noktada, s�r�m kontrol�ne ald���n�z dosyalar� i�eren bir Git yaz�l�m havuzunuz var.

### Var olan Bir Yaz�l�m Havuzunu Klonlamak ###

Var olan bir Git yaz�l�m havuzunu klonlamak istiyorsan�z �s�z gelimi, katk�da bulunmak istedi�iniz bir proje varsa- ihtiyac�n�z olan komut `git clone`. Subversion gibi ba�ka SKS'lere a�inaysan�z, komutun `checkout` de�il `clone` oldu�unu fark etmi�sinizdir. Bu �nemli bir ayr�md�r �Git, sunucuda bulunan neredeyse b�t�n veriyi kopyalar. `git clone` komutunu �al��t�rd���n�zda her dosyan�n proje tarih�esinde bulunan her s�r�m� istemciye indirilir. Hatta, sunucunuzun diski bozulacak olsa, herhangi bir istemcideki herhangi bir klonu, sunucuyu klonland��� zamanki haline geri getirmek i�in kullanabilirsiniz (sunucunuzdaki baz� �engel betikleri (_hook_) kaybedebilirsiniz, ama s�r�mlenmi� verinin tamam� elinizin alt�nda olacakt�r �daha fazla ayr�nt� i�in bkz. _4. B�l�m_)

Bir yaz�l�m havuzu `git clone [url]` komutuyla klonlan�r. �rne�in, Grit adl� Ruby Git k�t�phanesini klonlamak isterseniz, bunu �u �ekilde yapabilirsiniz:

	$ git clone git://github.com/schacon/grit.git

Bu komut `grit` ad�nda bir klas�r olu�turur, bu klas�r�n i�inde bir `.git` alt dizini olu�turup ilklemesini yapar, s�z konusu yaz�l�m havuzunun b�t�n verisini indirir ve son s�r�m�n�n bir koyas�n� se�er (_checkout_). Bu yeni `grit` klas�r�ne gidecek olursan�z, kullan�lmaya ve �zerinde �al���lmaya haz�r proje dosyalar�n� g�r�rs�n�z. Yaz�l�m havuzunu ad� grit'ten farkl� bir klas�re kopyalamak isterseniz, bunu komut sat�r� se�ene�i olarak vermelisiniz:

	$ git clone git://github.com/schacon/grit.git mygrit

Bu komut da bir �ncekiyle ayn� �eyleri yapar, fakat olu�turulan klas�r�n ad� `mygrit`'tir.

Git'in bir dizi farkl� transfer protokol� vard�r. Yukar�daki �rnek `git://` protokol�n� kullan�yor, ama `http(s)://`'in ya da SSH  transfer protokol�n� kullanan `user@server:/path.git`'in kullan�ld���na da tan�k olabilirsiniz. _4. B�l�m_'de Git yaz�l�m havuzuna eri�mek i�in sunucunun kullanabilece�i b�t�n ge�erli se�enekleri ve bunlar�n olumlu ve olumsuz yanlar�n� inceleyece�iz.

## De�i�iklikleri Yaz�l�m Havuzuna Kaydetmek ##

Ger�ek bir Git yaz�l�m havuzuna ve s�z konusu proje i�in gerekli olan bir dosya se�mesine sahipsiniz. Bu proje �zerinde de�i�iklikler yapman�z ve proje kaydetmek istedi�iniz bir seviyeye geldi�inde bu de�i�ikliklerin bir bellek kopyas�n� kaydetmeniz gerekecek.

Unutmay�n, �al��ma klas�r�n�zdeki dosyalar iki halden birinde bulunurlar: _izlenenler_ (_tracked_) ve _izlenmeyenler_ (_untracked_). _�zlenen_ dosyalar, bir �nceki bellek kopyas�nda bulunan dosyalard�r; bunlar _de�i�memi�_, _de�i�mi�_ ya da _haz�rlanm��_ olabilirler. Geri kalan her �ey ��al��ma klas�r�n�zde bulunan ve bir �nceki bellek kopyas�nda ya da haz�rlama alan�nda bulunmayan dosyalar� _izlenmeyen_ dosyalard�r. Bir yaz�l�m havuzunu yeni kopyalam��san�z, b�t�n dosyalar, hen�z yeni se�me yapt���n�z ve hi�bir �eyi de�i�tirmedi�iniz i�in, izlenen ve de�i�memi� olacakt�r.

Dosyalar� d�zenlemeye ba�lad���n�zda, Git onlar� de�i�mi� olarak g�recektir, ��nk� son kayd�n�zdan beri �zerlerinde de�i�iklik yapm�� olacaks�n�z. De�i�tirdi�iniz bu dosyalar� �nce _haz�rlay�p_ sonra b�t�n _haz�rlanm��_ de�i�iklikleri kaydedeceksiniz ve bu d�ng� b�yle s�r�p gidecek. Bu d�ng�, Fig�r 2-1'de g�steriliyor.


Insert 18333fig0201.png
Fig�r 2-1. Dosyalar�n�z�n de�i�ik durumlar�n�n d�ng�s�.

### Dosyalar�n Durumlar�n� Kontrol Etmek ###

Hangi dosyan�n hangi durumda oldu�unu g�rmek i�in kullan�lacak temel ara� `git status` komutudur. Bu komutu bir klonlama i�leminin hemen sonras�nda �al��t�racak olursan�z, ��yle bir �ey g�rmelisiniz:

	$ git status
	# On branch master
	nothing to commit (working directory clean)

Bu �al��ma klas�r�n�z�n temiz oldu�u anlam�na gelir �ba�ka bir deyi�le, izlenmekte olup da de�i�tirilmi� herhangi bir dosya yoktur. Git'in saptad��� herhangi bir izlenmeyen dosya da yok, olsayd� burada listelenmi� olurdu. Son olarak, bu komut size hangi dal (_branch_) �zerinde oldu�unuzu s�yl�yor. �imdilik bu, daima varsay�lan dal olan `master` olacakt�r; �u anda buna kafa yormay�n. Sonraki b�l�m de dallar ve referanslar konusu derinlemesine ele al�nacak.

Diyelim ki projenize yeni bir dosya, basit bir README dosyas� eklediniz. E�er dosya �nceden orada bulunmuyorsa, ve `git status` komutunu �al��t�r�rsan�z, bu izlenmeyen dosyay� �u �ekilde g�r�rs�n�z:

	$ vim README
	$ git status
	# On branch master
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	README
	nothing added to commit but untracked files present (use "git add" to track)

Yeni README dosyan�z�n izlenmedi�ini g�r�yorsunuz, ��nk� `status` ��kt�s�nda �Untracked files� ba�l��� alt�nda listelenmi�tir. Bir dosyan�n izlenmemesi demek, Git'in onu bir �nceki bellek kopyas�nda (_commit_) g�rmemesi demektir; siz a��k�a belirtmedi�iniz s�rece Git bu dosyay� izlemeye ba�lamayacakt�r. Bunun nedeni, derleme ��kt�s� olan ikili dosyalar�n ya da projeye dahil etmek istemedi�iniz dosyalar�n yanl��l�kla projeye dahil olmas�n� engellemektir. README dosyas�n� projeye eklemek istiyorsunuz, �yleyse dosyay� izlemeye alal�m.

### Yeni Dosyalar� �zlemeye Almak ###

Yeni bir dosyay� izlemeye almak i�in `git add` komutunu kullanmal�s�n�z. README dosyas�n� izlemeye almak i�in komutu �u �ekilde �al��t�rabilirsiniz:

	$ git add README

`status` komutunu yeniden �al��t�r�rsan�z, README dosyas�n�n art�k izlemeye al�nd���n� ve haz�rl�k alan�nda oldu�unu g�receksiniz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#

Haz�rl�k alan�nda oldu�unu �Changes to be committed� ba�l���n�n alt�nda olmas�na bakarak s�yleyebilirsiniz. E�er bu noktada bir kay�t (_commit_) yapacak olursan�z, dosyan�n `git add` komutunu �al��t�rd���n�z andaki hali bellek kopyas�na kaydedilecektir. Daha �nce `git init` komutunu �al��t�rd�ktan sonra projenize dosya eklemek i�in `git add (dosya)` komutunu �al��t�rd���n�z� hat�rlayacaks�n�z �bunun amac� klas�r�n�zdeki dosyalar� izlemeye almakt�. `git add` komutu bir dosya ya da klas�r�n konumuyla �al���r; e�er s�z konusu olan bir klas�rse, klas�rdeki b�t�n dosyalar� tekrarlamal� olarak projeye ekler.

### De�i�tirilen Dosyalar� Haz�rlamak ###

Gelin �imdi halihaz�rda izlenmekte olan bir dosyay� de�i�tirelim. �zlenmekte olan `benchmarks.rb` ad�ndaki bir dosyay� de�i�tirip `status` komutunu �al��t�rd���n�zda ��yle bir ekran ��kt�s�yla kar��la��rs�n�z:

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

`benchmarks.rb` dosyas� �Changes not staged for commit� ba�l�kl� bir b�l�m�n alt�nda g�r�n�yor �bu ba�l�k izlenmekte olan bir dosyada de�i�iklik yap�lm�� oldu�u fakat dosyan�n hen�z haz�rl�k alan�na al�nmad��� durumlarda kullan�l�r. Dosyay� haz�rlamak i�in, `git add` komutunu �al��t�r�n (`git add` �ok ama�l� bir komuttur, bir dosyay� izlemeye almak i�in, kayda haz�rlamak i�in, ya da birle�tirme uyu�mazl�klar�n�n ��z�ld���n� i�aretlemek gibi ba�ka ama�larla kullan�l�r). Gelin `benchmarks.rb` dosyas�n� kayda haz�rlamak i�in `git add` komutunu �al��t�r�p sonra da `git status` komutuyla duruma bakal�m:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

Her iki dosya da kayda haz�rlanm�� durumdad�r ve bir sonraki kayd�n�za dahil edilecektir. Tam da bu noktada, hen�z kayd� ger�ekle�tirmeden, akl�n�za `benchmarks.rb` dosyas�nda yapmak istedi�iniz k���k bir de�i�ikli�in geldi�ini d���nelim. Dosyay� yeniden a��p de�i�ikli�i yapt�ktan sonra art�k kayd� yapmaya haz�rs�n�z. Fakat, `git status` komutunu bir kez daha �al��t�ral�m:

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

Ne oldu? `benchmarks.rb` dosyas� hem kayda haz�rlanm�� hem de kayda haz�rlanmam�� g�r�n�yor. Bu nas�l olabiliyor? Git, bir dosyay� `git add` komutunun al��t�r�ld��� haliyle kayda haz�rlar. E�er �imdi kay�t yapacak olursan�z, `benchmarks.rb` dosyas�, �al��ma klas�r�nde g�r�nd��� haliyle de�il, `git add` komutunu son �al��t�rd���n�z haliyle kay�t edilecektir. Bir dosyay� `git add` komutunu �al��t�rd�ktan sonra de�i�tirecek olursan�z, dosyan�n son halini kayda haz�rlamak i�in `git add` komutunu bir kez daha �al��t�rman�z gerekir:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

### Dosyalar� G�rmezden Gelmek ###

�o�u zaman, projenizde Git'in takip etmesini, hatta size izlenmeyenler aras�nda g�stermesini bile istemedi�iniz bir k�me dosya olacakt�r. Bunlar, genellikle otomatik olarak olu�turulan seyir (_log_) dosyalar� ya da yaz�l�m in�a sisteminin ��kt�lar�d�r. Bu durumlarda, bu dosyalar�n konumlar�yla e�le�en �r�nt�leri listeleyen `.gitignore` ad�nda bir dosya olu�turabilirsiniz:

	$ cat .gitignore
	*.[oa]
	*~

�lk sat�r, Git'e `.o` ya da `.a` ile biten dosyalar� �yaz�l�m derlemesinin sonucunda ortaya ��km�� olabilecek _nesne_ ve _ar�iv_ dosyalar�n�� g�rmezden gelmesini s�yl�yor. �kinci sat�r, Git'e Emacs gibi pek �ok metin edit�r� taraf�ndan ge�ici dosyalar� i�aretlemek i�in kullan�lan tilda i�aretiyle (`~`) biten b�t�n dosyalar� g�rmezden gelmesini s�yl�yor. Bu listeye `log`, `tmp` ya da `pid` klas�rlerini, otomatik olarak olu�turulan dok�mantasyon dosyalar�n� ve sair dosyay� ekleyebilirsiniz. Daha projenin ba�lang�c�nda bir `.gitignore` dosyas� olu�turmak yaz�l�m havuzunuzda istemeyece�iniz dosyalar� yanl��l�kla kaydetmenize engel olaca��ndan olduk�a iyi bir fikirdir.

`.gitignore` dosyan�zda bulundurabilece�iniz �r�nt�ler �u kurallara ba�l�d�r:

*	Bo� sat�rlar ve `#` ile ba�layan sat�rlar g�rmezden gelinir.
*	Stadart _glob_ �r�nt�leri ay�rt edilir (�.N.: _glob_ \*nix taraf�ndan kullan�lan s�n�rl� bir kurall� ifade (_regular expression_) bi�imidir).
*	Bir klas�r� belirtmek �zere �r�nt�leri bir e�ik �izgi (`/`) ile sonland�rabilirsiniz.
*	Bir �r�nt�y� �nlem i�aretiyle (`!`) ba�latt���n�zda, �r�nt�n�n tersi gere�li olur.

_Glob_ �r�nt�leri _shell_'ler taraf�ndan kullan�lan basitle�tirilmi� kurall� ifadelerdir (_regular expression_). Bir y�ld�z i�areti (`*`) s�f�r ya da daha fazla karakterle e�le�ir; `[abc]` k��eli parantezin i�indeki herhangi bir karakterle e�le�ir (buradaki �rnekte `a`, `b`, ya da `c` ile); soru i�areti (`?`) bir karakterle e�le�ir; tireyle ayr�lm�� karakterleri i�ine alan bir k��eli parantez (`[0-9]`) bu aral�ktaki b�t�n karakterlerle e�le�ir (bu �rnekte 0'dan 9'a kadar olan karakterler).

Bir `.gitignore` dosyas� �rne�i daha:

	# bir yorum - bu g�rmezden gelinir
	# .a dosyalar�n� g�rmezden gel
	*.a
	# ama yukar�da .a dosyalar�n� g�rmezden geliyor olsan bile lib.a dosyas�n� izlemeye al
	!lib.a
	# k�k dizindeki /TODO dosyas�n� (TODO ad�ndaki alt klas�r� de�il) g�rmezden gel
	/TODO
	# build/ klas�r�ndeki b�t�n dosyalar� g�rmezden gel
	build/
	# doc/notes.txt dosyas�n� g�rmezden gel ama doc/server/arch.txt dosyas�n� g�rmezden gelme
	doc/*.txt

### Kayda Haz�rlanm�� ve Haz�rlanmam�� De�i�iklikleri G�r�nt�lemek ###

`git status` komutunu fazla anla��lmaz buluyorsan�z �yaln�zca hangi dosyalar�n de�i�ti�ini de�il, bu dosyalarda tam olarak nelerin de�i�ti�ini g�rmek istiyorsan�z� `git diff` komutunu kullanabilirsiniz. `git diff` komutunu ileride ayr�nt�l� olarak inceleyece�iz; ama bu komutu muhtemelen en �ok �u iki soruya cevap bulmak i�in kullanacaks�n�z: De�i�tirip de hen�z kayda haz�rlamad���n�z neler var? Ve kayda olmak �zere hangi de�i�ikliklerin haz�rl���n� yapt�n�z? `git status` bu sorular� genel bi�imde cevapl�yor olsa da `git diff` eklenen ve ��kar�lan b�t�n dosyalar� �oldu�u gibi yamay�� g�sterir.

Diyelim `README` dosyas�n� d�zenleyip kayda haz�rlad�n�z, sonra da `benchmarks.rb` dosyas�n� d�zenlediniz ama kayda haz�rlamad�n�z. `status` komutunu �al��t�rd���n�zda ��yle bir �ey g�r�rs�n�z:

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

Hen�z kayda haz�rlamad���n�z de�i�iklikleri g�rmek i�in `git diff` komutunu (ba�ka hi�bir arg�man kullanmadan) �al��t�r�n:

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

Komut, �al��ma klas�r�n�z�n i�eri�iyle kayda haz�rl�k alan�n�n i�eri�ini kar��la�t�r�r. Sonu� size hen�z kayda haz�rlamad���n�z de�i�iklikleri g�sterir.

Kayda haz�rlam�� oldu�unuz de�i�iklikleri g�rmek i�in `git diff --cache` komutunu kullanabilirsiniz. (1.6.1'den sonraki Git s�r�mlerinde hat�rlamas� daha kolay olabilecek `git diff --staged` komutunu da kullanabilirsiniz.) Bu komut kayda haz�rlanm�� de�i�ikliklerle son kayd� kar��la�t�r�r.

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

Dikkat edilmesi gereken nokta, `git diff`'in son kay�ttan beri yap�lan b�t�n de�i�iklikleri de�il yaln�zca kayda haz�rlanmam�� de�i�iklikleri g�steriyor olu�udur. Bu zaman zaman kafa kar��t�r�c� olabilir, zira, b�t�n de�i�ikliklerinizi kayda haz�rlad�ysan�z, `git diff`'in ��kt�s� bo� olacakt�r.

Yine, �rnek olarak, `benchmarks.rb` dosyas�n� kayda haz�rlay�p daha sonra �zerinde de�i�iklik yaparsan�z, `git diff` komutunu kullanarak hangi de�i�ikliklerin kayda haz�rland���n�, hangilerinin haz�rlanmad���n� g�r�nt�leyebilirsiniz:

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

�imdi `git diff` komutuyla hangi de�i�ikliklerin hen�z kayda haz�rlanmam�� oldu�unu

	$ git diff
	diff --git a/benchmarks.rb b/benchmarks.rb
	index e445e28..86b2f7c 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -127,3 +127,4 @@ end
	 main()

	 ##pp Grit::GitRuby.cache_client.stats
	+# test line

ve `git diff --cached` komutuyla neleri kayda haz�rlam�� oldu�unuzu g�rebilirsiniz:

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

### De�i�iklikleri Kaydetmek ###

Yapt���n�z de�i�iklikleri diledi�iniz gibi haz�rl�k alan�na ald���n�za g�re art�k onlar� kaydedebilirsiniz (_commit_). Unutmay�n, kayda haz�rlanmam�� �olu�turdu�unuz ya da de�i�tirdi�iniz fakat `git add` komutunu kullanarak kayda haz�rlamad���n�z� de�i�iklikler kaydedilmeyecektir. Onlar sabit diskinizde de�i�tirilmi� dosyalar olarak kalacaklar.

Bu �rnekte, `git status` komutunu son �al��t�rd���n�zda her �eyin kayda haz�rlanm�� oldu�unu g�rd�n�z, dolay�s�yla de�i�ikliklerinizi kaydetmeye haz�rs�n�z. Kay�t yapman�n en kolay yolu `git commit` komutunu kullanmakt�r:

	$ git commit

Bu komutu �al��t�rd���n�zda sisteminizde se�ili bulunan metin edit�r� a��lacakt�r. (Edit�r�n�z _shell_'inizin `$EDITOR` �evresel de�i�keni taraf�ndan tan�mlan�r �genellikle vim ya da emacs't�r, fakat `git config --global core.editor` komutunu _1. B�l�m_'de g�rd���n�z gibi �al��t�rarak bu ayar� de�i�tirebilirsiniz.)

Metin edit�r� a�a��daki metni g�r�nt�ler (bu �rnek Vim ekran�ndan):

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

G�rd���n�z gibi haz�r kay�t mesaj� `git status` ��kt�s�n�n `#` kullan�larak devre d��� b�rak�lm�� haliyle en �stte bir bo� sat�rdan olu�ur. Bu devre d��� b�rak�lm�� kay�t mesaj�n� silip yerine kendi kay�t mesaj�n�z� yazabilir, ya da neyi kaydetti�inizi size hat�rlatmas� i�in orada b�rakabilirsiniz. (Neyi de�i�tirdi�inizin daha ayr�nt�l� olarak hat�rlat�lmas�n� isterseniz, `git commit` mesaj�n� `-v` se�ene�iyle kullanabilirsiniz. Bu se�enek kaydetmekte oldu�unuz de�i�ikli�in i�eri�ini de (_diff_) edit�rde g�sterecektir.) Edit�r� kapatt���n�zda Git, yazd���n�z mesaj� kullanarak de�i�ikli�i kaydeder (devre d��� b�rak�lm�� b�l�m� ve de�i�ikli�in i�eri�ini mesaj�n d���nda b�rak�r).

Bir ba�ka se�enek de, kay�t mesaj�n�z� `commit` komutunu `-m` se�ene�iyle a�a��daki gibi kullanmakt�r:

	$ git commit -m "Story 182: Fix benchmarks for speed"
	[master]: created 463dc4f: "Fix benchmarks for speed"
	 2 files changed, 3 insertions(+), 0 deletions(-)
	 create mode 100644 README

�lk kayd�n�z� olu�turmu� oldunuz! G�r�ld��� gibi kay�t kendisiyle ilgili bilgiler veriyor: hangi dala kay�t yapm�� oldu�unuzu (`master`), kayd�n SHA-1 s�nama toplam�n�n ne oldu�unu (`463dc4f`), ka� dosyada de�i�iklik yapt���n�z� ve kay�tta ka� sat�r ekleyip ��kard���n�za dair istatistiklerin ��kt�s�n� veriyor.

Unutmay�n, kay�t, haz�rl�k alan�nda kayda haz�rlad���n�z bellek kopyas�n�n kayd�d�r. Kayda haz�rlamad���n�z de�i�iklikler, de�i�iklik olarak duruyor; onlar� da proje tarih�esine eklemek isterseniz yeni bir kay�t yapabilirsiniz. Her kay�t i�leminde projenizin bir bellek kopyas�n� kaydediyorsunuz; bu bellek kopyalar�n� daha sonra geriye sarabilir ya da birbiriyle kar��la�t�rabilirsiniz.

### Haz�rl�k Alan�n� Atlamak ###

Her ne kadar kay�tlar� tam istedi�iniz gibi d�zenlemek inan�lmaz derecede yararl� bir �ey olsa da, haz�rl�k alan� kimi zaman i� ak���n�za fazladan bir y�k getirebilir. Git, haz�rl�k alan�n� kullanmadan ge�mek isteyenler i�in basit bir k�sayol sunuyor. `git commit` komutunu `-a` se�ene�iyle kullan�rsan�z, Git, halihaz�rda izlenmekte olan b�t�n dosyalar� otomatik olarak kayda haz�rlay�p, `git add` a�amas�n� atlaman�z� sa�lar:

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

G�rd���n�z gibi, kay�t i�lemi yapmadan �nce `benchmarks.rb` dosyas�n� `git add` komutundan ge�irmek zorunda kalmad�n�z.

### Dosyalar� Ortadan Kald�rmak ###

Bir dosyay� Git'ten silmek i�in, �nce izlenen dosyalar� listesinden ��karmal� (daha do�rusu, kayda haz�rl�k alan�ndan kald�rmal�) sonra da kaydetmelisiniz. `git rm` hem bunu yapar hem de dosyay� �al��ma klas�r�n�zden siler, b�ylece dosyay� izlenmeyen dosyalar aras�nda g�rmezsiniz.

E�er dosyay� �al��ma klas�r�n�zden silerseniz, `git status` ��kt�s�n�n �Changes not staged for commit� (yani _kayda haz�rlanmam�� olanlar_) ba�l��� alt�nda boy g�sterecektir:

	$ rm grit.gemspec
	$ git status
	# On branch master
	#
	# Changes not staged for commit:
	#   (use "git add/rm <file>..." to update what will be committed)
	#
	#       deleted:    grit.gemspec
	#

Sonras�nda `git rm` komutunu �al��t�r�rsan�z, dosyan�n ortadan kald�r�lmas� i�in kayda haz�rlanmas�n� sa�lam�� olursunuz:

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

Bir sonraki kayd�n�zda, dosya silinmi� olacak ve art�k izlenmeyecektir. E�er dosyay� de�i�tirmi� ve halihaz�rda indekse eklemi�seniz, ortadan kald�rma i�lemini `-f` se�ene�ini kullanarak zorlaman�z gerekecektir. Bu, herhangi bir bellek kopyas�na kaydedilmemi� ve Git kullan�larak kurtar�lamayacak verilerin kaybolmas�n� �nlemek amac�yla geli�tirilmi� bir �nlemdir.

Yapmak isteyebilece�iniz bir ba�ka �ey de dosyay� �al��ma klas�r�n�zde tutup, kayda haz�rl�k alan�ndan silmek olabilir. Bir ba�ka deyi�le, dosyay� sabit diskinizde bulundurmak ama Git'in izlenecek dosyalar listesinden ��karmak isteyebilirsiniz. Bu, �zellikle belirli bir dosyay� (b�y�k bir seyir dosyas�n� ya da bir k�me derlenmi� `.a` dosyas�n�) `.gitignore` dosyan�za eklemeyi unutup yanl��l�kla Git indeksine ekledi�inizde kullan��l� olan bir �zelliktir. Bunu yapmak i�in `--cached` se�ene�ini kullanmal�s�n�z:

	$ git rm --cached readme.txt

`git rm` komutunu dosyalar, klas�rler ya da _glob_ �r�nt�leri �zerinde kullanabilirsiniz. Yani ��yle �eyler yapabilirsiniz:

	$ git rm log/\*.log

`*`'in �n�ndeki ters e�ik �izgi i�aretini g�zden ka��rmay�n. Bu i�aret gereklidir, ��nk� _shell_'inizin dosya ad� a��mlamas�na ek olarak, Git de kendi dosya ad� a��mlamas�n� yapar. Yukar�daki komut, `log/` klas�r�ndeki `.log` eklentili b�t�n dosyalar� ortadan kald�racakt�r. Ya da, ��yle bir �ey yapabilirsiniz:

	$ git rm \*~

Bu komut `~` ile biten b�t�n dosyalar� ortadan kald�racakt�r.

### Dosyalar� Ta��mak ###

�o�u SKS'nin aksine Git ta��nan dosyalar� takip etmez. Bir dosyan�n ad�n� de�i�tirirseniz, Git, dosyan�n yeniden adland�r�ld���na dair herhangi bir �stveri olu�turmaz. Fakat Git, olay olup bittikten sonra neyin ne oldu�unu anlamakta olduk�a beceriklidir �dosya hareketlerini ke�fetme meselesini birazdan ele alaca��z.

Bu nedenle Git'in bir `mv` komutu olmas� biraz kafa kar��t�r�c� olabilir. Git'te bir dosyan�n ad�n� de�i�tirmek istiyorsan�z, ��yle bir komut �al��t�rabilirsiniz:

	$ git mv eski_dosya yeni_dosya

ve istedi�inizi elde edersiniz. Hatta, buna benzer bir komut �al��t�rd�ktan sonra `status` ��kt�s�na bakarsan�z Git'in bir dosya adland�rma i�lemini (_rename_) listeledi�ini g�r�rs�n�z:

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

�te yandan bu komut, �u komutlar� arka arkaya �al��t�rmaya e�de�erdir:

	$ mv README.txt README
	$ git rm README.txt
	$ git add README

Git dosya ta��ma i�lemini dolayl� yollardan anlar, dolay�s�yla dosyay� yeniden adland�rmay� bu komutlarla m� yapt���n�z yoksa `mv` komutunu mu kulland���n�z Git a��s�ndan �nemli de�ildir. Tek ger�ek fark arka arkaya �� komut kullanmak yerine tek bir komut kullan�yor olman�zd�r �`mv` bir kullan�c�ya kolal�k sa�layan bir komuttur. Daha �nemlisi, bir dosyan�n ad�n� de�i�tirmek i�in istedi�iniz her arac� kullanabilir, `add/rm` i�lemlerini sonraya kay�ttan hemen �ncesine b�rakabilirsiniz.

## Kay�t Tarih�esini G�r�nt�lemek ##

Birka� kay�t olu�turduktan, ya da halihaz�rda kay�t tarih�esi olan bir yaz�l�m havuzunu klonlad���n�zda, muhtemelen ge�mi�e bak�p neler oldu�una g�z atmak isteyeceksiniz. Bunun i�in kullanabilece�iniz en temel ve becerikli ara� `git log` komutudur.

Buradaki �rnekler benim �o�unlukla tan�t�mlarda kulland���m `simplegit` ad�nda bir projeyi kullan�yor. Projeyi edinmek i�in a�a��daki komutu �al��t�rabilirsiniz:

	git clone git://github.com/schacon/simplegit-progit.git

Bu projenin i�inde `git log` komutunu �al��t�rd���n�zda �una benzer bir ��kt� g�receksiniz:

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

Aksi belirtilmedik�e, `git log` bir yaz�l�m havuzundaki kay�tlar� ters kronolojik s�rada listeler. Yani, en son kay�tlar en �stte g�r�n�r. G�r�ld��� gibi, bu komut her kayd�n SHA-1 s�nama toplam�n�, yazar�n�n ad�n� ve adresini, kaydedildi�i tarihi ve kay�t mesaj�n� listeler.

`git log` komutunun, size tam olarak arad���n�z �eyi g�stermek i�in kullan�labilecek �ok say�da se�ene�i vard�r. Burada, en �ok kullan�lan baz� se�enekleri tan�taca��z.

En yararl� se�eneklerden biri, kayd�n i�eri�ini (_diff_) g�steren `-p` se�ene�idir. �sterseniz `-2`'yi kullanarak komutun ��kt�s�n� son iki kay�tla s�n�rlayabilirsiniz:

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

Bu se�enek daha �nceki bilgilere ek olarak kayd�n i�eri�ini de her g�sterir. Bu, yaz�l�m� g�zden ge�irirken ya da belirli bir kat�l�mc� taraf�ndan yap�lan bir dizi kay�t s�ras�nda nelerin de�i�ti�ine h�zl�ca g�z atarken �ok i�e yarar.

Dilerseniz `git log`'u �zet bilgiler veren bir dizi se�enekle birlikte kullanabilirsiniz. �rne�in, her kay�tla ilgili �zet istatistikler i�in `--stat` se�ene�ini kullanabilirsiniz:

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

G�rd���n�z gibi `--stat`  se�ene�i, her kayd�n alt�na o kay�tta de�i�ikli�e u�ram�� dosyalar�n listesini, ka� tane dosyan�n de�i�ikli�e u�rad���n� ve s�z konusu dosyalara ka� sat�r�n eklenip ��kar�ld��� bilgisini ekler. Bu bilgilerin bir �zetini de kayd�n en alt�na yerle�tirir. Olduk�a yararl� bir ba�ka se�enek de `--pretty` se�ene�idir. Bu se�enek `log` ��kt�s�n�n bi�imini de�i�tirmek i�in kullan�l�r. Bu se�enekle birlikte kullanaca��n�z birka� tane �ntan�ml� ek se�enek vard�r. `oneline` ek se�ene�i her bir kayd� tek bir sat�rda g�sterir; bu �ok say�da kayda g�z at�yorsan�z yararl� olabilir. Ayr�ca `short`, `full` ve `fuller` se�enekleri a�a�� yukar� ayn� miktarda bilgiyi �baz� farklarla� g�sterir:

	$ git log --pretty=oneline
	ca82a6dff817ec66f44342007202690a93763949 changed the version number
	085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
	a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

En ilgin� ek se�enek, istedi�iniz log ��kt�s�n� belirlemenizi sa�layan `format` ek se�ene�idir. Bu, �zellikle bilgisayar taraf�ndan i�lenecek bir ��kt� olu�turmak konusunda elveri�lidir �bi�imi a��k�a kendiniz belirledi�iniz i�in farkl� Git s�r�mlerinde farkl� sonu�larla kar��la�mazs�n�z:

	$ git log --pretty=format:"%h - %an, %ar : %s"
	ca82a6d - Scott Chacon, 11 months ago : changed the version number
	085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
	a11bef0 - Scott Chacon, 11 months ago : first commit

Tablo 2-1 `format` ek se�ene�inin kabul etti�i baz� bi�imlendirme se�eneklerini g�steriyor.

	Se�enek	��kt�n�n A��klamas�
	%H	S�nama toplam�
	%h	K�salt�lm�� s�nama toplam�
	%T	Git a�ac� s�nama toplam�
	%t	K�salt�lm�� Git a�ac� s�nama toplam�
	%P	Ata kay�tlar�n s�nama toplamlar�
	%p	Ata kay�tlar�n k�salt�lm�� s�nama toplamlar�
	%an	Yazar�n ad�
	%ae	Yazar�n e-posta adresi
	%ad	Yaz�lma tarihi (�date= se�ene�iyle uyumludur)
	%ar	Yaz�lma tarihi (g�receli tarih)
	%cn	Kaydedenin ad�
	%ce	Kaydedenin e-posta adresi
	%cd	Kaydedilme tarihi
	%cr	Kaydedilme tarihi (g�receli tarih)
	%s	Konu

_yazar_'la _kaydeden_ aras�nda ne gibi bir fark oldu�unu merak ediyor olabilirsiniz. _yazar_ yamay� olu�turan ki�idir, _kaydeden_'se yamay� projeye uygulayan ki�i. Bir projeye yama g�nderdi�inizde, projenin �ekirdek �yelerinden biri yamay� projeye uygularsa, her ikinizin de ad� kaydedilecektir �sizin ad�n�z yazar olarak onun ad� kaydeden olarak. Bu fark� _5. B�l�m_'de biraz daha ayr�nt�l� olarak ele alaca��z.

`oneline` ve `format` ek se�enekleri �zellikle `--graph` ek se�ene�iyle birlikte kullan�ld�klar�nda �ok i�e yararlar. Bu ek se�enek projenizin dal (_branch_) ve birle�tirme (_merge_) tarih�esini g�steren sevimli bir ASCII grafi�i olu�turur. Grit yaz�l�m havuzunun grafi�ine bakal�m:

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

Bunlar `git log`'la birlikte kullanabilece�iniz se�eneklerden yaln�zca birka�� �daha ba�ka �ok say�da se�enek var. Tablo 2-2 yukar�da inceledi�imiz se�eneklerin yan� s�ra, yararl� olabilecek ba�ka se�enekleri `git log` ��kt�s�na olan etkileriyle birlikte listeliyor.

	Se�enek	A��klama
	-p	Kay�tlar�n i�eriklerini de g�ster.
	--stat	Kay�tlarda de�i�ikli�e u�rayan dosyalarla ilgili istatistikleri g�ster.
	--shortstat	Yaln�zca de�i�ikli�i/eklemeyi/��karmay� �zetleyen sat�r� g�ster command.
	--name-only	Kay�tlarda de�i�en dosyalar�n yaln�zca adlar�n� g�ster.
	--name-status	Kay�tlarda de�i�en dosyalar�n adlar�yla birlikte de�i�me/eklenme/��kar�lma bilgisini de g�ster.
	--abbrev-commit	S�nama toplam�n�n 40 karakterli tamam� yerine yaln�zca ilk birka� karakterini g�ster.
	--relative-date	Tarihi g�n, ay, y�l olarak g�stermek yerine g�receli olarak g�ster ("iki hafta �nce" gibi).
	--graph	Log tarih�esinin yan�s�ra, dal ve birle�tirme tarih�esini ASCII grafi�i olarak g�ster.
	--pretty	Kay�tlar� alternatif bir bi�imlendirmeyle g�ster. `oneline` `short`, `full`, `fuller` ve (kendi istedi�iniz bi�imi belirleyebildi�iniz) `format` ek se�enekleri kullan�labilir.

### Log ��kt�s�n� S�n�rland�rma ###

`git log` komutu, bi�imlendirme se�eneklerinin yan� s�ra bir dizi s�n�rland�rma se�ene�i de sunar �bu se�enekler kay�tlar�n yaln�zca bir alt k�mesini g�sterir. Bu se�eneklerden birini yukar�da g�rd�n�z �yaln�zca son iki kayd� g�steren `-2` se�ene�ini. Asl�nda, son `n` kayd� g�rmek i�in `n` yerine herhangi bir tam say� koyarak bu se�ene�i `-<n>` bi�iminde kullanabilirsiniz. Bunu muhtemelen �ok s�k kullanmazs�n�z, zira Git `log` ��kt�s�n� zaten sayfa sayfa g�steriyor, dolay�s�yla `git log` komutunu �al��t�rd���n�zda zaten �nce kay�tlar�n birinci sayfas�n� g�receksiniz.

�te yandan `--since` ya da `--until` gibi ��kt�y� zamanla s�n�rlayan se�enekler i�inizi kolayla�t�rabilir. S�z gelimi, �u komut, son iki hafta i�inde yap�lm�� kay�tlar� listeliyor:

	$ git log --since=2.weeks

Bu komut pek �ok de�i�ik bi�imlendirmeyle kullan�labilir �kesin bir tarih (�2008-01-15�) ya da �2 years 1 day 3 minutes ago� gibi g�reli bir tarih kullanabilirsiniz.

Ayr�ca listeyi belirli arama �l��tlerine uyacak bi�imde filtreleyebilirsiniz. `--author` se�ene�i belirli bir yazara aiy kay�tlar� filtrelemenizi sa�lar; `--grep` se�ene�iyse kay�t mesajlar�nda anahtar kelimeler araman�z� sa�lar. (Unutmay�n, hem `author` hem de `grep` se�eneklerini kullanmak istiyorsan�z, komuta `--all-match` se�ene�ini de eklemelisiniz, aksi takdirde, komut bu iki se�enekten herhangi birine uyan kay�tlar� bulup getirecektir.)

`git log`la kullan�lmas� son derece yararl� olan son se�enek konum se�ene�idir. `git log`'u bir klas�r ya da dosya ad�yla birlikte kullan�rsan�z, komutun ��kt�s�n� yaln�zca o dosyalarda de�i�iklik yapan kay�tlarla s�n�rlam�� olursunuz. Bu, komuta her zaman en son se�enek olarak eklenmelidir ve konumlar iki tireyle (`--`) di�er se�eneklerden ayr�lmal�d�r.

Tablo 2-3, bu se�enekleri ve birka� ba�ka yayg�n se�ene�i listeliyor.

	Se�enek	A��klama
	-(n)	Yaln�zca son n kayd� g�ster.
	--since, --after	Yaln�zca belirli bir tarihten sonra eklenmi� kay�tllar� g�ster.
	--until, --before	Yaln�zca belirli bir tarihten �nce yap�lm�� kay�tlar� g�ster.
	--author	Yaln�zca yazar�n ad�n�n belirli bir karakter katar�yla (_string_) e�le�en kay�tlar� g�ster.
	--committer	Yaln�zca kaydedenin ad�n�n belirli bir karakter katar�yla e�le�ti�i kay�tlar� g�ster.

�rne�in, Git kaynak kod yaz�l�m havuzu tarih�esinde birle�tirme (_merge_) olmayan hangi kay�tlar�n Junio Hamano taraf�ndan 2008'in Ekim ay�nda kaydedilmi� oldu�unu g�rmek i�in �u komutu �al��t�rabilirsiniz:

	$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
	   --before="2008-11-01" --no-merges -- t/
	5610e3b - Fix testcase failure when extended attribute
	acd3b9e - Enhance hold_lock_file_for_{update,append}()
	f563754 - demonstrate breakage of detached checkout wi
	d1a43f2 - reset --hard/read-tree --reset -u: remove un
	51a94af - Fix "checkout --track -b newbranch" on detac
	b0ad11e - pull: allow "git pull origin $something:$cur

Bu komut, Git kaynak kodu yaz�l�m havuzundaki yakla��k 20,000 komut aras�ndan bu �l��tlere uyan 6 tanesini g�steriyor.

### Tarih�eyi G�rselle�tirmek i�in Grafik Aray�z Kullan�m� ###

Kay�t tarih�enizi g�r�nt�lemek i�in g�rselli�i daha �ok �n planda olan bir ara� kullanmak isterseniz, Git'le birlikte da��t�lan bir Tcl/Tk program� olan `gitk`'ya bir g�z atmak isteyebilirsiniz. Gitk, temelde `git log`'u g�rselle�tiren bir ara�t�r ve neredeyse `git log`'un kabul etti�i b�t�n filtreleme se�eneklerini tan�r. Proje klas�r�n�zde komut sat�r�na `gitk` yazacak olursan�z Fig�r 2-2'deki gibi bir �ey g�r�rs�n�z.

Insert 18333fig0202.png
Fig�r 2-2. gitk grafiklse tarih�e g�r�nt�leyicisi.

Pencerenin �st yar�s�nda bir kal�t�m grafi�inin yan� s�ra kay�t tarih�esini g�rebilirsiniz. Alttaki kay�t i�eri�i g�r�nt�leyicisi, t�klad���n�z herhangi bir kay�ttaki de�i�iklikleri g�sterecektir.

## De�i�iklikleri Geri Almak ##

Herhangi bir noktada yapt���n�z bir de�i�ikli�i geri almak isteyebilirsiniz. Burada yap�lan de�i�iklikleri geri almakta kullan�labilecek baz� ara�lar� inceleyece�iz. Dikkatli olun, zira geri al�nan bu de�i�ikliklerden baz�lar�n� yeniden ger�ekle�tiremeyebilirsiniz. Bu, e�er bir hata yaparsan�z, bunu Git'i kullanarak telafi edemeyece�iniz, az say�da alan�ndan biridir.

### Son Kay�t ��lemini De�i�tirmek ###

E�er kayd� �ok erken yapm��san�z, baz� dosyalar� eklemeyi unutmu�san�z ya da kay�t mesaj�nda hata yapm��san�z, s�k rastlanan d�zeltme i�lemlerinden birini kullanabilirsiniz. Kayd� de�i�tirmek isterseniz, `commit` komutunu `--amend` se�ene�iyle �al��t�rabilirsiniz:

	$ git commit --amend

Bu komut, haz�rl�k alan�ndaki de�i�iklikleri al�p bunlar� kayd� de�i�tirmek i�in kullan�r. E�er son kayd�n�zdan beri hi�bir de�i�iklik yapmam��san�z (�rne�in, bu komutu yeni bir kay�t yapt�ktan hemen sonra �al��t�r�yorsan�z) o zaman kayd�n�z�n bellek kopyas� ayn� kalacak ve de�i�tirece�iniz tek �ey kay�t mesaj� olacakt�r.

Ayn� kay�t mesaj� edit�r� a��l�r, fakat edit�rde bir �nceki kayd�n kay�t mesaj� g�r�n�r. Mesaj� her zamanki gibi de�i�tirebilirsiniz, ama bu yeni kay�t mesaj� �ncekinin yerine ge�ecektir.

S�z gelimi, e�er bir kay�t s�ras�nda belirli bir dosyada yapt���n�z de�i�iklikleri kayda haz�rlamay� unuttu�unuzu fark ederseniz, ��yle bir �ey yapabilirsiniz:

	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend

Bu �� komuttan sonra, tarih�enize tek bir kay�t i�lenmi�tir �son kay�t �ncekinin yerine ge�er.

### Kayda Haz�rlanm�� Bir Dosyay� Haz�rl�k Alan�ndan Kald�rmak ###

Bu iki alt b�l�m kayda haz�rl�k alan�ndaki ve �al��ma klas�r�n�zdeki de�i�iklikleri nas�l idare edece�inizi g�steriyor. ��in g�zel yan�, bu iki alan�n durumunu ��renmek i�in kullanaca��n�z komut ayn� zamanda bu alanlardaki de�i�iklikleri nas�l geri alabilece�inizi de hat�rlat�yor. Diyelim ki iki dosyay� de�i�tirdiniz ve bu iki de�i�ikli�i ayr� birer kay�t olarak i�lemek istiyorsunuz, ama yanl��l�kla `git add *` komutunu kullanarak ikisini birden haz�rl�k alan�na ald�n�z. Bunlardan birini nas�l haz�rl�k alan�ndan ��karabilirsiniz? `git status` komutu size bunu da an�msat�yor:

	$ git add .
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#       modified:   benchmarks.rb
	#

�Changes to be committed� yaz�s�n�n hemen alt�nda "use `git reset HEAD <file>...` to unstage" yazd���n� g�r�yoruz. `benchmarks.rb` dosyas�n� bu �neriye uygun olarak haz�rl�k alan�ndan kald�ral�m:

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

Komut biraz tuhaf, ama i� g�r�yor. `benchmarks.rb` dosyas� haz�rl�k alan�ndan kald�r�ld� ama h�l� de�i�mi� olarak g�r�n�yor.

### De�i�mi� Durumdaki Bir Dosyay� De�i�memi� Duruma Geri Getirmek ###

Peki `benchmarks.rb` dosyas�ndaki de�i�iklikleri korumak istemiyorsan�z? Yapt���n�z de�i�iklikleri kolayca nas�l geri alacaks�n�z �son kay�tta nas�l g�r�n�yorsa o haline (ya da ilk klonland��� haline, yahut �al��ma klas�r�n�ze ilk ald���n�z haline) nas�l geri getireceksiniz? Neyse ki `git status` komutu bunu nas�l yapaca��n�z� da s�yl�yor. Son �rnek ��kt�da haz�rl�k alan� d���ndaki de�i�iklikler ��yle g�r�n�yor:

	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   benchmarks.rb
	#

Yapt���n�z de�i�iklikleri nas�l ��pe atabilece�inizi a��k�a s�yl�yor (en az�ndan Git'in 1.6.1'le ba�layan yeni s�r�mleri bunu yap�yor �e�er daha eski bir s�r�mle �al���yorsan�z, kolayl�k sa�layan bu �zellikleri edinebilmek i�in program� g�ncellemenizi �neririz). Gelin, s�yleneni yapal�m:

	$ git checkout -- benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#

G�rd���n�z gibi de�i�iklikler ��pe at�ld�. Bunun tehlikeli bir komut oldu�unu akl�n�zdan ��karmay�n: o dosyaya yapt���n�z b�t�n de�i�iklikler �imdi yok oldu �dosyan�n �st�ne yeni bir dosya kopyalad�n�z. E�er dosyadaki de�i�iklikleri istemedi�inizden y�zde y�z emin de�ilseniz asla bu komutu kullanmay�n. E�er sorun bu dosyada yapt���n�z de�i�ikliklerin ba�ka i�lemler yapman�za engel olmas� ise bir sonraki b�l�mde ele alaca��m�z zulalama (_stash_) ve dalland�rma (_branch_) i�lemlerini kullanman�z daha iyi olacakt�r.

Unutmay�n, Git'te kaydedilmi� her �ey neredeyse her zaman kurtar�labilir. Silinmi� dallardaki kay�tlar ve hatta `--amend` se�ene�iyle �zerine yaz�lm�� kay�tlar bile kurtar�labilirler (veri kurtarma konusunda bkz. _9. B�l�m_). Di�er taraftan, kaydedilmemi� bir de�i�ikli�i kaybederseniz b�y�k olas�l�kla onu kurtarman�z m�mk�n olmaz.

## Uzak U�birimlerle �al��mak ##

Bir Git projesine katk�da bulunabilmek i�in uzaktaki yaz�l�m havuzlar�n� nas�l d�zenleyece�inizi bilmeniz gerekir. Uzaktaki yaz�l�m havuzlar�, projenizin �nternet'te ya da ba�ka bir a�da bar�nd�r�lan s�r�mleridir. Birden fazla uzak yaz�l�m havuzunuz olabilir, bunlardan her biri sizin i�in ya salt okunur ya da okunur/yaz�l�r durumdad�r. Ba�kalar�yla ortak �al��mak, bu yaz�l�m havuzlar�n� d�zenlemeyi, onlardan veri �ekip (_pull_) onlara veri iterek (_push_) �al��malar�n�z� payla�may� gerektirir.

Uzaktaki yaz�l�m havuzlar�n�z� d�zenleyebilmek i�in, projenize uzak yaz�l�m havuzlar�n�n nas�l eklenece�ini, kullan�lmayan havuzlar�n nas�l ��kar�laca��n�, �e�itli uzak dallar� d�zenlemeyi ve onlar�n izlenen dallar olarak belirleyip belirlememeyi ve daha ba�ka �eyleri gerektirir. Bu alt b�l�mde bu uza�� y�netme yeteneklerini inceleyece�iz.

### Uzak U�birimleri G�r�nt�leme ###

Projenizde hangi uzak sunucular� ayarlad���n�z� g�rme i�in `git remote` komutunu kullanabilirsiniz. Bu komut, her bir uzak u�birimin belirlenmi� k�sa ad�n� g�r�nt�ler. E�er yaz�l�m havuzunuzu bir yerden klonlam��san�z, en az�ndan _origin_ uzak u�birimini g�rmelisiniz �bu Git'in klonlaman�n yap�ld��� sunucuya verdi�i �ntan�ml� add�r.

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

`-v` se�ene�ini kullanarak Git'in bu k�sa ad i�in depolad��� URL'yi de g�rebilirsiniz:

	$ git remote -v
	origin	git://github.com/schacon/ticgit.git

Projenizde birden �ok uzak u�birim varsa, bu komut hepsini listeleyecektir. �rne�in, benim Git yaz�l�m havuzum ��yle g�r�n�yor:

	$ cd grit
	$ git remote -v
	bakkdoor  git://github.com/bakkdoor/grit.git
	cho45     git://github.com/cho45/grit.git
	defunkt   git://github.com/defunkt/grit.git
	koke      git://github.com/koke/grit.git
	origin    git@github.com:mojombo/grit.git

Bu demek oluyor ki bu kullan�c�lar�n herhangi birinden kolayl�kla �ekme i�lemi (_pull_) yapabiliriz. Fakat dikkat ederseniz, yaln�zca _origin_ u�biriminin SSH URL'si var, yani yaln�zca o havuza kod itebiliriz (_push_) (niye b�yle oldu�unu _4. B�l�m_'de inceleyece�iz)

### Uzak U�birimler Eklemek ###

�nceki alt b�l�mlerde uzak u�birim eklemekten s�z ettim ve baz� �rnekler verdim, ama bir kez daha konuyu a��k�a incelemekte yarar var. Uzaktaki bir yaz�l�m havuzunu k�sa bir ad vererek eklemek i�in `git remote add [kisa_ad] [url]` komutunu �al��t�r�n:

	$ git remote
	origin
	$ git remote add pb git://github.com/paulboone/ticgit.git
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git
	pb	git://github.com/paulboone/ticgit.git

Art�k b�t�n bir URL yerine `pb`'yi kullanabilirsiniz. �rne�in, Paul'�n yaz�l�m havuzunda bulunan ama sizde bulunmayan b�t�n bilgileri getirmek i�in `git fetch pb` komutunu kullanabilirsiniz:

	$ git fetch pb
	remote: Counting objects: 58, done.
	remote: Compressing objects: 100% (41/41), done.
	remote: Total 44 (delta 24), reused 1 (delta 0)
	Unpacking objects: 100% (44/44), done.
	From git://github.com/paulboone/ticgit
	 * [new branch]      master     -> pb/master
	 * [new branch]      ticgit     -> pb/ticgit

Paul'�n `mastertr` dal� sizin yaz�l�m havuzunuzda da `pb/master` olarak eri�ilebilir durumdad�r �kendi dallar�n�zdan biriyle birle�tirebilir (_merge_) ya da bir yerel dal olarak se�ip i�eri�ini inceleyebilirsiniz.

### Uzak U�birimlerden Getirme ve �ekme ��lemi Yapmak ###

Biraz �nce g�rd���n�z gibi, uzaktaki yaz�l�m havuzlar�ndan veri almak i�in �u komutu kullanabilirsiniz:

	$ git fetch [uzak-sunucu-ad�]

Bu komut, s�z konusu uzaktaki yaz�l�m havuzuna gidip orada bulunup da sizin projenizde bulunmayan b�t�n veriyi getirir. Bunu yapt�ktan sonra sizin projenizde o uzak yaz�l�m havuzundaki b�t�n dallara referanslar olu�ur �ki bunlar� birle�tirme yapmak ya da i�eri�i incelemek i�in kullanabilirsiniz. (Dallar�n ne oldu�unu ve onlar� nas�l kullanabilece�inizi _3. B�l�m_'de ayr�nt�l� bi�imde inceleyece�iz.)

Bir yaz�l�m havuzunu klonlad���n�zda, klonlama komutu s�z konusu kaynak yaz�l�m havuzunu _origin_ ad�yla uzak u�birimler aras�na ekler. Dolay�s�yla, `git fetch origin` komutu, klonlamay� yapt���n�zdan (ya da en son getirme i�lemini (_fetch_) yat���n�zdan) beri sunucuya itilmi� yeni de�i�iklikleri getirir. Unutmay�n, `fetch` komutu veriyi yeler yaz�l�m havuzunuza indirir �otomatik olarak sizin yapt�klar�n�zla birle�tirmeye, ya da �al��t���n�z �eyler �zerinde de�i�iklik yapmaya kalk��maz. Haz�r oldu�unuzda birle�tirme i�lemini sizin yapman�z gerekir.

Uzaktaki bir dal� izlemek �zere ayarlanm�� bir dal�n�z varsa (daha fazla bilgi i�in sonraki alt b�l�me ve _3. B�l�m_'e bak�n�z) bu dal �zerinde `git pull` komutunu kullanarak uzaktaki yaz�l�m havuzundaki veriyi hem getirip hem de mevcut dal�n�zla birle�tirebilirsiniz. Bu �al��mas� daha kolay bir d�zen olabilir; bu arada, `git clone ` komutu, otomatik olarak, yerel yaz�l�m havuzunuzda, uzaktaki yaz�l�m havuzunun `master` dal�n� takip eden bir `master` dal� olu�turur (uzaktaki yaz�l�m havuzunun `master` ad�nda bir dal� olmas� ko�uluyla). `git pull` komutu genellikle yereldeki yaz�l�m havuzunuza kaynakl�k eden sunucudan veriyi getirip otomatik olarak �zerinde �al��makta oldu�unuz dalla birle�tirir.

### Uzaktaki Yaz�l�m Havuzuna Veri �tmek ###

Projeniz payla�mak istedi�iniz bir hale geldi�inde, yapt�klar�n�z� kayna�a itmeniz gerekir. Bunun i�in kullan�lan komut basittir: `git push [uzak-sunucu-adi] [dal-adi]`. Projenizdeki `master` dal�n� `origin` sunucunuzdaki `master` dal�na itmek isterseniz (yineleyelim; kolanlama i�lemi genellikle bu isimleri otomatik olarak olu�turur), �u komutu kullanabilirsiniz:

	$ git push origin master

Bu komut, yaln�zca yazma yetkisine sahip oldu�unuz bir sunucudan klonlama yapm��san�z ve son getirme i�leminizden beri hi� kimse itme i�lemi yapmam��sa istedi�iniz sonucu verir. E�er sizinle birlikte bir ba�kas� daha klonlama yapm��sa ve o ki�i sizden �nce itme yapm��sa, sizin itme i�leminiz reddedilir. �tmeden �nce sizden �nce itilmi� de�i�iklikleri �ekip kendi �al��man�zla birle�tirmeniz gerekir. Uzaktaki yaz�l�m havuzlar�na itme yapmak konusunda daha ayr�nt�l� bilgi i�in bkz. _3. B�l�m_.

### Uzak U�birim Hakk�nda Bilgi Almak ###

Belirli bir uzak u�birim hakk�nda daha fazla bilgi almak isterseniz `git remote show [ucbirim-adi]` komutunu kullanabilirsiniz. Bu komutu `origin` gibi belirli bir u�birim k�sa ad�yla kullan�rsan�z ��yle bir sonu� al�rs�n�z:

	$ git remote show origin
	* remote origin
	  URL: git://github.com/schacon/ticgit.git
	  Remote branch merged with 'git pull' while on branch master
	    master
	  Tracked remote branches
	    master
	    ticgit

Bu, u�birimin URL'sini ve dallar�n izlenme durumunu g�sterir. Komut, size, e�er `master` dalda iseniz ve `git pull` komutunu �al��t�r�rsan�z, b�t�n referanslar� uzak u�birimden indirip uzaktaki `master` dal�ndan yerel `master` dal�na birle�tirme yapaca��n� da s�yl�yor. Ayr�ca, ekmi� oldu�u b�t�n uzak dallar� da bir liste halinde veriyor.

Yukar�daki verdi�imiz, basit bir �rnekti. Git'i daha yo�un bi�imde kulland���n�zda, `git remote show` komutu �ok daha fazla bilgi i�erecektir:

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

Bu ��kt�, belirli dallarda `git push` komutunu �al��t�rd���n�zda hangi dallar�n otomatik olarak itilece�ini g�steriyor. Buna ek olarak uzak u�birimde bulunup da sizin projenizde hen�z bulunmayan uzak dallar�, uzak u�birimden silinmi� oldu�u halde sizin projenizde bulunan dallar� ve `git pull` komutunu �al��t�rd���n�zda otomatik olarak birle�tirme i�lemine u�rayacak birden �ok dal� g�steriyor.

### Uzan U�birimleri Kald�rmak ve Yeniden Adland�rmak ###

Bir u�birimin k�sa ad�n� de�i�tirmek isterseniz, Git'in yeni s�r�mlerinde bunu `git remote rename` komutuyla yapabilirsiniz. �rne�in, `pb` u�birimini `paul` diye yeniden adland�rmak isterseniz, bunu `git remote rename`'i kullanarak yapabilirsiniz:

	$ git remote rename pb paul
	$ git remote
	origin
	paul

Bu i�lemin u�birim dal adlar�n� da de�i�tirdi�ini hat�rlatmakta yarar var. Bu i�lemden �nce `pb/master` olan dal�n ad� art�k `paul/master` olacakt�r.

Bir u�birim referans�n� herhangi bir nedenle �sunucuyu ta��m�� ya da belirli bir yans�y� art�k kullanm�yor olabilirsiniz; ya da belki kat�l�mc�lardan birisi art�k katk�da bulunmuyordur� kald�rmak isterseniz `git remote rm` komutunu kullanabilirsiniz:

	$ git remote rm paul
	$ git remote
	origin

## Etiketleme ##

�o�u SKS gibi Git'in de tarih�edeki belirli noktalar� �nemli olarak etiketleyebilme �zelli�i vard�r. Genellikle insanlar bu i�levi s�r�mleri (`v1.0`, vs.) i�aretlemek i�in kullan�rlar. Bu alt b�l�mde mevcut etiketleri nas�l listeleyebilece�inizi, nas�l yeni etiketler olu�turabilece�inizi ve de�i�ik etiket tiplerini ��reneceksiniz.

### Etiketlerinizi Listeleme ###

Git'te mevcut etiketleri listeleme i�i epeyi kolayd�r. `git tag` yazman�z yeterlidir:

	$ git tag
	v0.1
	v1.3

Bu komut etiketleri alfabetik bi�imde s�ralar; etiketlerin s�ras�n�n bir �nemi yoktur.

�sterseniz belirli bir �r�nt�yle e�le�en etiketleri de arayabilirsiniz. Git kaynak yaz�l�m havuzunda 240'tan fazla etiket vard�r. Yaln�zca 1.4.2 serisindeki etiketleri g�rmek isterseniz �u komutu �al��t�rmal�s�n�z:

	$ git tag -l 'v1.4.2.*'
	v1.4.2.1
	v1.4.2.2
	v1.4.2.3
	v1.4.2.4

### Etiket Olu�turma ###

Git iki ba�l�ca etiket tipi kullan�r: hafif ve a��klamal�. Hafif etiketler hi� de�i�meyen dallar gibidir �belirli bir kayd� i�aret ederler. �te yandan, a��klamal� etiketler, Git veritaban�nda b�t�nl�kl� nesneler olarak kaydedilirler. S�nama toplamlar� al�n�r; etiketleyenin ad�n� ve e-posta adresini i�erirler; bir etiket mesaj�na sahiptirler ve GNU Privacy Guard (GPG) kullan�larak imzalan�p do�rulanabilirler. Genellikle b�t�n bu bilgilere ula��labilmesini olanakl� k�labilmek i�in a��klamal� etiketlerin kullan�lmas� �nerilir, ama b�t�n bu bilgileri depolamadan yaln�zca ge�ici bir etiket olu�turmak istiyorsan�z, hafif etiketleri de kullanabilirsiniz.

### A��klamal� Etiketler ###

Git'te a��klamal� etiket olu�turmak basittir. En kolay� `tag` komutunu �al��t�r�rken `-a` se�ene�ini kullanmakt�r:

	$ git tag -a v1.4 -m 's�r�m�m 1.4'
	$ git tag
	v0.1
	v1.3
	v1.4

`-m` se�ene�i etiketle birlikte depolanacak etiketleme mesaj�n� belirlemek i�in kullan�l�r. A��klamal� bir etiket i�in mesaj� bu �ekilde belirlemezseniz, Git mesaj� yazabilmeniz i�in bir edit�r a�acakt�r.

`git show` komutunu kullanarak etiketlenen kay�tla birlikte etikete ili�kin verileri de g�rebilirsiniz:

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

Bu, kay�t bilgisinden �nce etiketleyenle ilgili bilgileri, kayd�n etiketlendi�i tarihi ve a��klama mesaj�n� g�sterir.

### �mzal� Etiketler ###

E�er bir ki�isel anahtar�n�z (_private key_) varsa etiketlerinizi GPG ile imzalayabilirsiniz. Yapman�z gereken tek �ey `-a` yerine `-s` se�ene�ini kullanmakt�r:

	$ git tag -s v1.5 -m 'imzal� 1.5 etiketim'
	You need a passphrase to unlock the secret key for
	user: "Scott Chacon <schacon@gee-mail.com>"
	1024-bit DSA key, ID F721C45A, created 2009-02-09

Bu etiket �zerinde `git show` komutunu �al��t�r�rsan�z, GPG imzas�n� da g�rebilirsiniz:

	$ git show v1.5
	tag v1.5
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 15:22:20 2009 -0800

	imzal� 1.5 etiketim
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

Birazdan imzal� etiketleri nas�l do�rulayabilece�inizi ��reneceksiniz.

### Hafif Etiketler ###

Kay�tlar� etiketlemenin bir yolu da hafif etiketler kullanmakt�r. Bu, kay�t s�nama toplam�n�n bir dosyada depolanmas�ndan ibarettir �ba�ka hi�bir bilgi tutulmaz. Bir hafif etiket olu�tururken `-a`, `-s` ya da `-m` se�eneklerini kullanmamal�s�n�z.

	$ git tag v1.4-lw
	$ git tag
	v0.1
	v1.3
	v1.4
	v1.4-lw
	v1.5

�imdi etiket �zerinde `git show` komutunu �al��t�racak olsan�z, etiketle ilgili ek bilgiler g�rmezsiniz. Komut yaln�zca kayd� g�sterir:

	$ git show v1.4-lw
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

### Etiketleri Do�rulamak ###

�mzal� bir etiketi do�rulamak i�in `git tag -v [etiket-adi]` komutu kullan�l�r. Bu komut imzay� do�rulamak i�in GPG'yi kullan�r. Bunun d�zg�n �al��mas� i�in imza sahibinin kamusal anahtar� (_public key_) anahtar halkan�zda (_keyring_) bulunmal�d�r.

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

E�er imzalay�c�n�n genel anahtar�na sahip de�ilseniz, bunun yerine a�a��dakine benzer bir �ey g�receksiniz:

	gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
	gpg: Can't check signature: public key not found
	error: could not verify the tag 'v1.4.2.1'

### Sonradan Etiketleme ###

Ge�mi�teki kay�tlar� da etiketleyebilirsiniz. Diyelim ki Git tarih�eniz ��yle olsun:

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

S�z gelimi, "updated rakefile" kayd�nda projenizi `v1.2` olarak etiketlemeniz gerekiyordu, ama unuttunuz. Etiketi sonradan da ekleyebilirsiniz. O kayd� etiketlemek i�in komutun sonuna kayd�n s�nama toplam�n� (ya da bir par�as�n�) eklemelisiniz:

	$ git tag -a v1.2 9fceb02

Kayd�n etiketlendi�ini g�receksiniz:

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

### Etiketleri Payla�mak ###

Aksi belirtilmedik�e `git push` komutu etiketleri uzak u�birimlere aktarmaz. Etiketleri belirtik bi�imde bir ortak sunucuya itmeniz gerekir. Bu s�re� u�birim dallar�n� payla�maya benzer �`git push origin [etiket-adi]` komutunu �al��t�rabilirsiniz.

	$ git push origin v1.5
	Counting objects: 50, done.
	Compressing objects: 100% (38/38), done.
	Writing objects: 100% (44/44), 4.56 KiB, done.
	Total 44 (delta 18), reused 8 (delta 1)
	To git@github.com:schacon/simplegit.git
	* [new tag]         v1.5 -> v1.5

Bir seferde birden �ok etiketi payla�mak isterseniz, `git push` komutuyla birlikte `--tags` se�ene�ini de kullanabilirsiniz. Bu, halihaz�rda itilmemi� olan b�t�n etiketlerinizi uzak u�birime aktaracakt�r.

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

Art�k ba�ka biri sizin yaz�l�m havuzunuzdan �ekme yapt���nda, b�t�n etiketlerinize de sahip olacakt�r.

## �pu�lar� ##

Git'in temelleri hakk�ndaki bu b�l�m� tamamlamadan �nce, Git deneyiminizi kolayla�t�rabilmek i�in birka� ipucu vermekte yarar var. Pek �ok insan Git'i bu ipu�lar�na ba�vurmadan kullan�yor; bu ipu�lar�ndan ileride tekrar s�z etmeyece�imiz gibi bunlar� bilmeniz gerekti�ini de varsaym�yoruz; ama yine de bilmeniz yarar�n�za olacakt�r.

### Otomatik Tamamlama ###

E�er Bash -shell_'ini kullan�yorsan�z, Git'in otomatik tamamlama beti�ini (_script_) kullanabilirsiniz. Git kaynak kodunu indirip `contrib/completion` klas�r�ne bak�n; orada `git-completion.bash` ad�nda bir dosya olmal�. Bu dosyay� ev dizininize (_home_) kopyalay�p `.bashrc` dosyan�za ekleyin:

	source ~/.git-completion.bash

Otomatik tamamlama �zelli�inin b�t�n Git kullan�c�lar� i�in ge�erli olmas�n� istiyorsan�z, bu betik dosyas�n� Mac sistemler i�in `/opt/local/etc/bash_completion.d` konumuna Linux sistemlerde `/etc/bash_completion.d/` konumuna kopyalay�n. Bu, Bash'�n otomatik olarak y�kleyece�i betiklerin bulundu�u bir klas�rd�r.

E�er bir Windows kullan�c�s�ysan�z ve Git Bash kullan�yorsan�z- ki bu msysGit'le kurulum yapt���n�zdaki �ntan�ml� programd�r, otomatik tamamlama kendili�inden gelecektir.

Bir Git komutu yazarken Sekme tu�una bast���n�zda, kar��n�za bir dizi se�enek getirir:

	$ git co<selme><sekme>
	commit config

Bu �rnekte, `git co` yaz�p Sekme tu�una iki kez basmak `commit` ve `config` komutlar�n� �neriyor. Komutun devam�nda `m` yaz�p bir kez daha Sekme tu�una basacak olursan�z, komut otomatik olarak `git commit`'e tamamlan�r.

Bu, se�eneklerde de kullan�labilir, ki muhtemelen daha yararl� olacakt�r. �rne�in, `git log` komutunu �al��t�r�rken se�eneklerden birisini hat�rlayamad�n�z, se�ene�i yazmaya ba�lay�p Sekme tu�una basarak e�le�en se�enekleri g�rebilirsiniz:

	$ git log --s<sekme>
	--shortstat  --since=  --src-prefix=  --stat   --summary

Bu g�zel �zellik sizi zaman kazand�rabilece�i gibi ikide bir belgelendirmeye bakma gere�ini de ortadan kald�r�r.

### Takma Adlar ###

Bir komutun bir k�sm�n� yazd���n�zda Git bunu anlamayacakt�r. Komutlar�n uzun adlar�n� kullanmak istemezseniz, `git config` komutunu kullanarak bunlar�n yerine daha k�sa takma adlar belirleyebilirsiniz. Kullanmak isteyebilece�iniz baz� takma adlar� buraya ald�k:

	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status

Bu durumda, �rne�in,  `git commit` yazmak yerine `git ci` yazman�z yeterli olacakt�r. Git'i kulland�k�a s�k kulland���n�z ba�ka komutlar da olacakt�r, o zaman o komutlar i�in de takma adlar olu�turabilirsiniz.

Bu teknik, eksikli�ini hissetti�iniz komutlar� olu�turmakta da yararl� olabilir. �rne�in, bir dosyay� haz�rl�k alan�ndan kald�rmak i�in yap�lmas� gerekenleri yeni bir komut olarak tan�mlayabilirsiniz:

	$ git config --global alias.unstage 'reset HEAD --'

Bu durumda �u iki komut e�de�er olacakt�r:

	$ git unstage fileA
	$ git reset HEAD fileA

Biraz daha temiz de�il mi? Bir `last` komutu eklemek de olduk�a yayg�nd�r:

	$ git config --global alias.last 'log -1 HEAD'

B�ylece son kayd� kolayl�kla g�rebilirsiniz:

	$ git last
	commit 66938dae3329c7aebe598c2246a8e6af90d04646
	Author: Josh Goebel <dreamer3@example.com>
	Date:   Tue Aug 26 19:48:51 2008 +0800

	    test for current head

	    Signed-off-by: Scott Chacon <schacon@example.com>

G�rd���n�z gibi Git yeni komutu takma ad olarak belirledi�ini �eyin yerine kullan�yor. Ama belki de bir Git komutu �al��t�rmak de�il de ba�ka bir program kullanmak istiyorsunuz. Bu durumda komutun ba��na `!` karakterini koymal�s�n�z. Bir Git yaz�l�m havuzu �zerinde �al��an kendi ara�lar�n�z� yaz�yorsan�z bu se�enek yararl� olabilir. Bunu g�stermek i�in ,`gitk`'yi �al��t�rmak i�in `git visual` diye yeni bir takma ad tan�mlayabiliriz:

	$ git config --global alias.visual '!gitk'

## �zet ##

Bu noktada, b�t�n temel Git i�lemlerini yapabiliyorsunuz �bir yaz�l�m havuzunu yaratmak ya da klonlamak, de�i�iklikler yapmak, bu de�i�iklikleri kayda haz�rlamak ve kaydetmek ve yaz�l�m havuzundaki b�t�n kay�tlar�n tarih�esini g�r�nt�lemek. S�radaki b�l�mde Git'in en vurucu �zelli�ini, dallanma modelini inceleyece�iz.