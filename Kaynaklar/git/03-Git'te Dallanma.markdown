# Git'te Dallanma #

Neredeyse her SKS'nin bir dallanma (_branching_) i�levi vard�r. Dallanma, ana geli�tirme �izgisinden sapmak ve i�inizi o ana geli�tirme �izgisine bula�madan devam ettirmek anlam�na gelir. �o�u SKS arac�nda bu pahal� bir s�re�tir; kaynak kod klas�r�n�z�n yeni bir kopyas�n� yapman�z� gerektirir ve b�y�k projelerde �ok zaman al�r.

Baz�lar� Git'in dallanma modelinin onun "en vurucu �zelli�i" oldu�unu s�ylerler; bu �zelli�in SKS toplulu�u i�inde Git'i ayr� bir yere koydu�u do�rudur. Onu bu kadar �zel yapan nedir? Git'te dallanmalar �ok kolay ve neredeyse anl�kt�r, �stelik farkl� dallar aras�nda gidip gelmek de bir o kadar h�zl�d�r. �o�u SKS'den farkl� olarak Git dallanma ve birle�tirmenin (_merge_) s�k (belki de g�nde birka� kez) ger�ekle�ece�i bir i� ak���n� te�vik eder. Bu �zelli�i anlay�p bu konuda ustala�mak size son derece becerikli ve e�siz bir ara� sa�layabilece�i gibi �al��ma bi�iminizi de b�t�n�yle de�i�tirebilir.

## Dal Nedir? ##

Git'in dallanma i�lemini nas�l yapt���n� ger�ekten anlayabilmek i�in geriye do�ru bir ad�m at�p Git'in verilerini nas�l depolad���na bakmam�z gerekiyor. 1. B�l�m'den hat�rlayabilece�iniz �zere, Git verilerini bir dizi de�i�iklik olarak de�il bir dizi bellek kopyas� olarak depolar.

Git'te bir kay�t yapt���n�zda, Git, kayda haz�rlad���n�z i�eri�in bellek kopyas�na i�aret eden imleci, yazar ve mesaj �stverisini ve s�z konusu kayd�n atalar�n� g�steren s�f�r ya da daha fazla imleci (ilk kay�t i�in s�f�r ata, normal bir kay�t i�in bir ata, iki ya da daha fazla dal�n birle�tirilmesinden olu�an bir kay�t i�in birden �ok ata) i�eren bir kay�t nesnesini depolar.

Bunu g�rselle�tirmek i�in, �� dosyadan olu�an bir klas�r�n�z�n oldu�unu ve bu �� dosyay� da kay�t i�in haz�rlad���n�z� varsayal�m. Dosyalar� kayda haz�rlamak her bir dosyan�n s�nama toplam�n� al�r (1. B�l�m'de s�z etti�imiz SHA-1 �zeti), dosyan�n o s�r�m�n� Git yaz�l�m havuzunda depolar (Git'te bunlara _blob_ denir (�.N. _blob_ T�rk�e'ye damla ya da topak diye �evrilebilir, fakat kelimeyi oldu�u gibi kullanman�n daha anla��l�r olaca��n� d���nd�k.)) ve s�nama toplam�n� haz�rl�k alan�na ekler:

	$ git add README test.rb LICENSE
	$ git commit -m 'initial commit of my project'

`git commit` komutunu �al��t�rarak bir kay�t olu�turdu�unuzda, Git her bir alt klas�r�n (bu �rnekte yaln�zca k�k klas�r�n) s�nama toplam�n� al�r ve bu a�a� yap�s�ndaki bu nesneleri yaz�l�m havuzunda depolar. Git, daha sonra, �st veriyi ve ihtiya� duyuldu�unda bellek kopyas�n�n yeniden yaratabilmek i�in a�a� yap�s�ndaki nesneyi g�steren bir imleci i�eren bir kay�t nesneyi yarat�r.

�imdi, Git yaz�l�m havuzunuzda be� nesne bulunuyor: �� dosyan�z�n her biri i�in bir i�erik _blob_'u, klas�r�n i�eri�ini listeleyen ve hangi dosyan�n hangi _blob_'da depoland��� bilgisini i�eren bir a�a� nesnesi ve o a�a� nesnesini g�steren bir imleci ve b�t�n kay�t �stverisini i�eren bir kay�t nesnesi. Kavramsal olarak, Git yaz�l�m havuzunuzdaki veri Fig�r 3-1'deki gibi g�r�n�r.

Insert 18333fig0301.png 
Fig�r 3-1. Tek kay�tl� yaz�l�m havuzundaki veri.

Yeniden de�i�iklik yap�p kaydederseniz, yeni kay�t kendisinden hemen �nce gelen kayd� g�steren bir imleci de depolar. �ki ya da daha fazla kayd�n sonunda tarih�eniz Fig�r 3-2'deki gibi g�r�n�r.

Insert 18333fig0302.png 
Fig�r 3-2. Birden �ok kay�t sonunda Git nesne verisi.

Git'te bir dal, bu kay�tlardan birine i�aret eden, yer de�i�tirebilen k�vrak bir imle�ten ibarettir. Git'teki varsay�lan dal ad� `master`'d�r. �lk kayd� yapt���n�zda, son yapt���n�z kayd� g�steren bir `master` dal�na sahip olursunuz. Her kay�t yapt���n�zda dal otomatik olarak son kayd� g�stermek �zere hareket eder.

Insert 18333fig0303.png 
Fig�r 3-3. Dal kay�t verisinin tarih�esini g�steriyor.

Yeni bir dal olu�turdu�unuzda ne olur? Yeni kay�tlarla ilerlemenizi sa�layan yeni bir imle� yarat�l�r. S�z gelimi, `testing` ad�nda yeni bir dal olu�tural�m. Bunu, `git branch` komutuyla yapabilirsiniz:

	$ git branch testing

Bu, �u an bulundu�unuz kay�ttan hareketle yeni bir imle� yarat�r (bkz. Fig�r 3-4).


Insert 18333fig0304.png 
Fig�r 3-4. Birden �ok dal kay�t verisinin tarih�esini g�steriyor.

Git �u an hangi dal�n �zerinde oldu�unuzu nereden biliyor? `HEAD` ad�nda �zel bir imle� tutuyor. Unutmay�n, buradaki `HEAD` Subversion ya da CVS gibi ba�ka SKS'lerden al���k oldu�unuz `HEAD`'den �ok farkl�d�r. Git'te bu, �zerinde bulundu�unuz yerel dal� g�sterir. Bu �rnekte h�l� `master` dal�ndas�n�z. `git branch` komutu yaln�zca yeni bir dal yaratt� �o dala atlamad� (bkz. Fig�r 3-5).

Insert 18333fig0305.png 
Fig�r 3-5. HEAD dosyas� �zerinde bulundu�unuz dal� g�steriyor.

Varolan bir dala atlamak i�in `git checkout` komutunu �al��t�rmal�s�n�z. Gelin, `testing` dal�na atlayal�m:

	$ git checkout testing

Bu, `HEAD`'in `testing` dal�n� g�stermesiyle sonu�lan�r (bkz. Fig�r 3-6).

Insert 18333fig0306.png
Fig�r 3-6. Dal de�i�tirdi�inizde HEAD �zerinde oldu�unuz dal� g�sterir.

Bunun ne �nemi var? Gelin bir kay�t daha yapal�m:

	$ vim test.rb
	$ git commit -a -m 'made a change'

Fig�r 3-7 sonucu resmediyor.

Insert 18333fig0307.png 
Fig�r 3-7. HEAD'in g�sterdi�i dal her kay�tla ileri do�ru hareket eder.

Burada ilgin� olan `testing` dal� ilerledi�i halde `master` dal� h�l� dal de�i�tirmek i�in `git checkout` komutunu �al��t�rd���n�z zamanki yerinde duruyor. Gelin yeniden `master` dal�na d�nelim.

	$ git checkout master

Fig�r 3-8 sonucu g�steriyor.

Insert 18333fig0308.png 
Fig�r 3-8. Se�me (checkout) i�lemi yap�ld���nda HEAD ba�ka bir dal� g�sterir.

�rnekteki komut iki �ey yapt�. `HEAD` imlecini yeniden `master` dal�n� g�sterecek �ekilde hareket ettirdi ve �al��ma klas�r�n�zdeki dosyalar� `master`'�n g�sterdi�i bellek kopyas�ndaki hallerine getirdi. Bu demek oluyor ki, bu noktada yapaca��n�z de�i�iklikler projenin daha eski bir s�r�m�n� baz alacak. �z�nde, ba�ka bir y�ne gidebilmek i�in `testing` dal�nda yapt���n�z de�i�iklikleri ge�ici olarak geri alm�� oldunuz.

Gelin bir de�i�iklik daha yap�p kaydedelim:

	$ vim test.rb
	$ git commit -a -m 'made other changes'

�imdi proje tarih�eniz iki ayr� dala �raksad� (bkz. Fig�r 3-9). Yeni bir dal yarat�p ona ge�tiniz, baz� de�i�iklikler yapt�n�z; sonra ana dal�n�za geri d�nd�n�z ve ba�ka baz� de�i�iklikler yapt�n�z. Bu iki de�i�iklik iki ayr� dalda birbirinden yal�t�k durumdalar: iki dal aras�nda gidip gelebilir, haz�r oldu�unuzda bu iki dal� birle�tirebilirsiniz. B�t�n bunlar� yaln�zca `branch` ve `checkout` komutlar�n� kullanarak yapt�n�z.

Insert 18333fig0309.png 
Fig�r 3-9. Dal tarih�eleri birbirinden �raksad�.

Git'te bir dal i�aret etti�i kayd�n 40 karakterlik SHA-1 s�nama toplam�n� i�eren basit bir dosyadan ibaret oldu�u i�in dallar� yaratmak ve yok etmek olduk�a masrafs�zd�r. Yeni bir dal yaratmak bir dosyaya 41 karakter (40 karakter ve bir sat�r sonu) yazmak kadar h�zl�d�r.

Bu, �o�u SKS'nin b�t�n proje dosyalar�n� yeni bir klas�re kopyalamay� gerektiren dallanma yakla��m�yla keskin bir kar��tl�k i�indedir. S�z konusu yakla��mda projenin boyutlar�na ba�l� olarak dallanma saniyeler, hatta dakikalar s�rebilir; Git'te ise bu s�re� her zaman anl�kt�r. Ayr�ca, kay�t yaparken ata kay�tlar� da kaydetti�imiz i�in birle�tirme s�ras�nda uygun bir ortak payda bulma i�i de otomatik olarak ve genellikle olduk�a kolayca halledilir. Bu �zellikler yaz�l�mc�lar� s�k s�k dal yarat�p kullanmaya te�vik eder.

Neden b�yle olmas� gerekti�ine yak�ndan bakal�m.

## Dallanma ve Birle�tirmenin Temelleri ##

Gelin, basit bir �rnekle, ger�ek hayatta kullanaca��n�z bir dallanma ve birle�tirme i�leyi�inin �st�nden ge�elim. �u ad�mlar� izleyeceksiniz:

1. Bir web sitesi �zerine �al���yor olun.
2. �zerinde �al��t���n�z yeni bir i� par�as� i�in bir dal yarat�n.
3. �al��malar�n�z� bu dalda ger�ekle�tirin.

Bu noktada, sizden kritik �nemde ba�ka sorun �zerinde �al���p h�zl�ca bir yama haz�rlaman�z istensin. Bu durumda �unlar� yapacaks�n�z:

1. Ana dal�n�za geri d�n�n.
2. Yamay� eklemek i�in yeni bir dal olu�turun.
3. Testleri tamamland�ktan sonra yama dal�n� ana dalla birle�tirip yay�na verin.
4. �al��makta oldu�unuz i� par�as� dal�na geri d�n�p �al��maya devam edin.

### Dallanman�n Temelleri ###

�nce, diyelim ki bir projede �al���yorsunuz ve halihaz�rda birka� tane kayd�n�z var (bkz. Fig�r 3-10).

Insert 18333fig0310.png 
Fig�r 3-10. K�sa ve basit bir kay�t tarih�esi.

�irketinizin kulland��� sorun izleme program�ndaki #53 numaral� sorun �zerinde �al��maya karar verdiniz. A��kl��a kavu�turmak i�in s�yleyelim: Git herhangi bir sorun izleme program�na ba�l� de�ildir; ama #53 numaral� sorun �zerinde �al��mak istedi�iniz ba�� sonu belli bir konu oldu�u i�in, �al��man�z� bir dal �zerinde yapacaks�n�z. Bir dal� yarat�r yaratmaz hemen ona ge�i� yapmak i�in `git checout` komutunu `-b` se�ene�iyle birlikte kullanabilirsiniz:

	$ git checkout -b iss53
	Switched to a new branch "iss53"

Bu, a�a��daki iki komutun yerine kullanabilece�iniz bir k�sayoldur:

	$ git branch iss53
	$ git checkout iss53

Fig�r 3-11 sonucu resmediyor.

Insert 18333fig0311.png 
Fig�r 3-11. Yeni bir dal imleci yaratmak.

Web sitesi �zerinde �al���p baz� kay�tlar yap�yorsunuz. Bunu yapt���n�zda `iss53` dal� ilerliyor, ��nk� se�ti�iniz dal o (yani `HEAD` onu g�steriyor; bkz. Fig�r 3-12).

	$ vim index.html
	$ git commit -a -m 'added a new footer [issue 53]'

Insert 18333fig0312.png 
Fig�r 3-12. �al��am�z sonucunda iss53 dal� ilerledi.

�imdi, sizden web sitesindeki bir sorun i�in acilen bir yama haz�rlaman�z istensin. Git kullan�yorsan�z, yamay� daha �nce `iss53` dal�nda yapt���n�z yapt���n�z de�i�ikliklerle birlikte yay�na sokman�z gerekmez; yama �zerinde �al��maya ba�lamadan �nce s�z konusu de�i�iklikleri geri al�p yay�ndaki web sitesini kaynak koduna ula�abilmek i�in fazla �abalaman�za da gerek yok. Tek yapman�z gereken `master` dal�na geri d�nmek.

Ama, bunu yapmadan �nce �unu belirtmekte yarar var: e�er �al��ma klas�r�n�zde ya da kayda haz�rl�k alan�nda se�mek (_checkout_) istedi�iniz dalla uyu�mazl�k g�steren kaydedilmemi� de�i�iklikler varsa, Git dal de�i�tirmenize izin vermeyecektir. Dal de�i�tirirken �al��ma alan�n�z� temiz olmas� en iyisidir. Bunun �stesinden gelmek i�in ba�vurulabilecek yollar� (zulalama ve kay�t de�i�tirme gibi) daha sonra inceleyece�iz. �imdilik, b�t�n de�i�ikliklerinizi kaydettiniz, dolay�s�yla `master` dal�na ge�i� yapabilirsiniz.

	$ git checkout master
	Switched to branch "master"

Bu noktada, �al��ma klas�r�n�z #53 numaral� sorun �zerinde �al��maya ba�lamadan hemen �nceki halindedir ve yamay� haz�rlamaya odaklanabilirsiniz. Buras� �nemli: Git, �al��ma klas�r�n�z� se�ti�iniz dal�n g�sterdi�i kayd�n bellek kopyas�yla ayn� olacak �ekilde ayarlar. Dal, son kayd�n�zda nas�l g�r�n�yorsa �al��ma klas�r�n� o hale getirebilmek i�in otomatik olarak dosyalar� ekler, siler ve de�i�tirir.

S�rada, haz�rlanacak yama var. �imdi yama �zerinde �al��mak i�in bir `hotfix` dal� olu�tural�m (bkz. Fig�r 3-13):

	$ git checkout -b 'hotfix'
	Switched to a new branch "hotfix"
	$ vim index.html
	$ git commit -a -m 'fixed the broken email address'
	[hotfix]: created 3a0874c: "fixed the broken email address"
	 1 files changed, 0 insertions(+), 1 deletions(-)

Insert 18333fig0313.png 
Fig�r 3-13. hotfix dal� master dal�n� baz al�yor.

Testlerinizi uygulayabilir, yaman�z�n istedi�iniz gibi oldu�undan emin olduktan sonra yay�na sokabilmek i�in `master` dal�yla birle�tirebilirsiniz. Bunun i�in `git merge` komutu kullan�l�r:

	$ git checkout master
	$ git merge hotfix
	Updating f42c576..3a0874c
	Fast forward
	 README |    1 -
	 1 files changed, 0 insertions(+), 1 deletions(-)

Birle�tirme ��kt�s�ndaki "Fast forward" ifadesine dikkat. Birle�tirdi�iniz dal�n g�sterdi�i kay�t, �st�nde bulundu�unuz dal�n do�rudan devam� oldu�undan, Git yaln�zca imleci ileri al�r. Ba�ka bir deyi�le, bir kayd�, kendi tarih�esinde geri giderek ula��labilecek bir ba�ka kay�tla birle�tiriyorsan�z, Git �raksayan ve birle�tirilmesi gereken herhangi bir �ey olmad��� i�in i�leri kolayla�t�r�p imleci ileri al�r �buna "fast forward" (h�zl� ileri alma) denir.

Yapt���n�z de�i�iklik art�k `master` dal� taraf�ndan i�aret edilen kayd�n bellek kopyas�ndad�r ve yay�mlanabilir (bkz. Fig�r 3-14).

Insert 18333fig0314.png 
Fig�r 3-14. Birle�tirmeden sonra master dal�n�z hotfix dal�n�zla ayn� yeri g�sterir.

Bu �ok �nemli yama yay�mland�ktan sonra, kald���n�z yere geri d�nebilirsiniz. Fakat �nce `hotfix` dal�n� sileceksiniz, ��nk� art�k ona ihtiyac�n�z kalmad� �`master` dal� ayn� yeri g�steriyor. `git branch` komutunu `-d` se�ene�iyle birlikte kullanarak silme i�lemini yapabilirsiniz:

	$ git branch -d hotfix
	Deleted branch hotfix (3a0874c).

�imdi kald���n�z yere geri d�nebilir ve #53 numaral� sorun �zerinde �al��maya devam edebilirsiniz (bkz. 3-15).

	$ git checkout iss53
	Switched to branch "iss53"
	$ vim index.html
	$ git commit -a -m 'finished the new footer [issue 53]'
	[iss53]: created ad82d7a: "finished the new footer [issue 53]"
	 1 files changed, 1 insertions(+), 0 deletions(-)

Insert 18333fig0315.png 
Fig�r 3-15. iss53 dal�n�z ba��ms�z olarak ilerleyebilir.

�unu belirtmekte yarar var: `hotfix` dal�nda yapt���n�z d�zeltme `iss53` dal�ndaki dosyalarda bulunmuyor. E�er bu de�i�ikli�i �ekmek isterseniz, `git merge master` komutunu �al��t�rarak `master` dal�n�z� `iss53` dal�n�zla birle�tirebilirsiniz; alternatif olarak `iss53` dal�ndaki de�i�iklikleri `master`dal�yla birle�tirmeye haz�r hale getirene kadar bekleyebilirsiniz.

### Birle�tirmenin Temelleri ###

Diyelim ki #53 numaral� sorunla ilgili �al��man�z� tamamlad�n�z ve `master` dal�yla birle�tirmeye haz�rs�n�z. Bunu yapabilmek i�in `iss53` dal�n�z�, ayn� `hotfix` dal�n� yapt���n�z gibi birle�tireceksiniz. B�t�n yapman�z gereken birle�tirmeyi ger�ekle�tirmek istedi�iniz dal� se�mek (_checkout_) ve `git merge` komutunu �al��t�rmak:

	$ git checkout master
	$ git merge iss53
	Merge made by recursive.
	 README |    1 +
	 1 files changed, 1 insertions(+), 0 deletions(-)

Bu daha �nce yapt���n�z `hotfix` birle�tirmesinden biraz farkl� g�r�n�yor. Burada, kay�t tarih�eniz daha eski bir noktadan �raksam��t�. �zerinde bulundu�unuz dal�n g�sterdi�i kay�t birle�tirmekte oldu�unuz dal�n do�rudan atas� olmad���ndan Git'in biraz i� yapmas� gerekiyor. Bu �rnekte Git, iki dal�n en u� noktas� ve ikisinin ortak atas�n�n kullan�ld��� �� tarafl� basit bir birle�tirme yap�yor. Fig�r 3-16, bu birle�tirmede kullan�lan �� farkl� bellek kopyas�n� vurguluyor.

Insert 18333fig0316.png 
Fig�r 3-16. Git, dallar� birle�tirmek i�in en uygun ortak atay� buluyor.

Git, yaln�zca dal imlecini ileri kayd�rmak yerine �� tarafl� birle�tirmenin sonucunda ortaya ��kan bellek kopyas� i�in otomatik bir kay�t olu�turuyor (bkz. Fig�r 3-17). Buna birle�tirme kayd� denir ve �zelli�i birden �ok atas�n�n olmas�d�r.

Git'in en uygun ortak atay� otomatik olarak buldu�unu vurgulamakta yarar var; bu kullan�c�n�n en uygun ortak payday� bulmak zorunda oldu�u CVS ve Subversion'daki durumdan (1.5 s�r�m�nden �nceki haliyle) farkl�d�r. Bu Git kullanarak birle�tirme yapmay� s�z konusu di�er sistemlere g�re �ok daha kolay bir hale getirir.

Insert 18333fig0317.png 
Fig�r 3-17. Git, otomatik olarak, birle�tirilmi� �al��may� i�eren yeni bir kay�t nesnesi yarat�r.

�al��man�z birle�tirildi�ine g�re, art�k `iss53` dal�na ihtiyac�n�z kalmad�. Dal� silip, sorun izleme sisteminizdeki sorunu da kapatabilirsiniz:

	$ git branch -d iss53

### Temel Birle�tirme Uyu�mazl�klar� ###

Zaman zaman bu s�re� o kadar da p�r�zs�z ilerlemez. E�er ayn� dosyan�n ayn� b�l�m�n� her iki dalda da de�i�tirmi�seniz, Git temiz bir birle�tirme yapamaz. #53 numaralar� sorun i�in haz�rlad���n�z d�zeltme `hotfix`le ayn� yaz�l�m par�as�n� de�i�tiriyorsa, �una benzer bir birle�tirme uyu�mazl���yla kar��la��rs�n�z:

	$ git merge iss53
	Auto-merging index.html
	CONFLICT (content): Merge conflict in index.html
	Automatic merge failed; fix conflicts and then commit the result.

Burada Git otomatik olarak yeni bir birle�tirme kayd� olu�turmad�. Sizin uyu�mazl��� ��zmenizi beklemek i�in s�rece ara verdi. Bir birle�tirme uyu�mazl���ndan sonra hangi dosyalar�n birle�tirilmemi� oldu�unu g�rmek i�in `git status` komutunu �al��t�rabilirsiniz.

	[master*]$ git status
	index.html: needs merge
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#	unmerged:   index.html
	#

Birle�tirme uyu�mazl��� hen�z ��z�mlenmemi� her �ey _unmerged_ (birle�tirilmemi�) olarak g�sterilecektir. Git, dosyalar� a��p uyu�mazl�klar� ��z�mleyebilmeniz i�in standart uyu�mazl�k ��z�mleme i�aret�ileri koyar. Dosyan�zda �una benzer bir b�l�mle kar��la��rs�n�z:

	<<<<<<< HEAD:index.html
	<div id="footer">contact : email.support@github.com</div>
	=======
	<div id="footer">
	  please contact us at support@github.com
	</div>
	>>>>>>> iss53:index.html

Burada , `HEAD`deki s�r�m (ki bu `master` dal�ndaki s�r�md�r ��nk� birle�tirme komutunu bu daldan �al��t�rd�n�z) �stte, (`=======` i�aretinin �st�ndeki her �ey), `iss53` dal�ndaki s�r�m ise altta g�sterilmektedir. Uyu�mazl��� ��z�mleyebilmek i�in bu ikisinden birini se�meli, ya da birle�tirmeyi istedi�iniz gibi kendiniz d�zenlemelisiniz. S�z gelimi, uyu�mazl��� ��zmek i�in b�t�n bu kod blo�unun yerine �unu yerle�tirebilirsiniz:

	<div id="footer">
	please contact us at email.support@github.com
	</div>

��z�mlemede iki taraftan da bir �eyler var ve `<<<<<<<`, `=======`, ve `>>>>>>>` i�aretlerini i�eren sat�rlar tamamen silinmi� durumda. Uyu�mazl�k olan her bir dosyadaki her bir uyu�mazl�k blo�unu ��z�mledikten sonra her dosyan�n �zerinde `git add` komutunu �al��t�rarak, uyu�mazl���n o dosya i�in ��z�lm�� oldu�unu belirtebilirsiniz. Bir dosyay� ayda haz�rlamak o dosyay� uyu�mazl��� ��z�mlenmi� olarak i�aretler.
Uyu�mazl�klar� ��z�mlemek i�in g�rsel bir ara� kullanmak isterseniz `git mergetool` komutunu �al��t�rabilirsiniz; bu komut size tek tek herbir uyu�mazl��� g�sterecek uygun bir birle�tirme arac�n� �al��t�r�r:

	$ git mergetool
	merge tool candidates: kdiff3 tkdiff xxdiff meld gvimdiff opendiff emerge vimdiff
	Merging the files: index.html

	Normal merge conflict for 'index.html':
	  {local}: modified
	  {remote}: modified
	Hit return to start merge resolution tool (opendiff):

Varsay�lan arac�n d���nda bir ara� kullanmak isterseniz (Git, Mac'te �al��t���m i�in bu �rnekte `opendiff`'i se�ti), Git'in destekledi�i b�t�n birle�tirme ara�lar�n�n listesini en �stte �merge tool candidates� yaz�s�ndan hemen sonra g�rebilirsiniz. Kullanmak istedi�iniz arac�n ad�n� yaz�n. 7. B�l�m'de kendi �al��ma ortam�n�z i�in varsay�lan de�eri nas�l de�i�tirebilece�inizi inceleyece�iz.

Birle�tirme arac�n� kapatt�ktan sonra, Git size birle�tirmenin ba�ar�l� olup olmad���n� soracakt�r. E�er ba�ar�l� oldu�unu s�ylerseniz, sizin yerinize dosyay� kayda haz�rlay�p ��z�mlenmi� olarak i�aretler.

B�t�n uyu�mazl�klar�n ��z�mlendi�inden emin olmak i�in tekrar `git status` komutunu �al��t�rabilirsiniz:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   index.html
	#

Durumdan memnunsan�z ve uyu�mazl��� olan b�t�n dosyalar�n kayda haz�rland���ndan eminseniz, `git commit`'i kullanarak birle�tirme kayd�n� tamamlayabilirsiniz. �ntan�ml� kay�t mesaj� ��yle g�r�n�r:

	Merge branch 'iss53'

	Conflicts:
	  index.html
	#
	# It looks like you may be committing a MERGE.
	# If this is not correct, please remove the file
	# .git/MERGE_HEAD
	# and try again.
	#

�leride bu birle�tirme i�lemini inceleyecek olanlar i�in yararl� olaca��n� d���n�yorsan�z bu kay�t mesaj�n� ayr�nt�land�rabilirsiniz �e�er a�ik�r de�ilse, birle�tirmeyi neden yapt���n�z�, ve birle�tirmede neler yapt���n�z� a��klayabilirsiniz.

## Dal Y�netimi ##

Dal yaratma, birle�tirme ve silme i�lemlerini yapt���m�za g�re, gelin �imdi de dallar �zerinde �al���rken i�imize yarayacak kimi dal y�netim ara�lar�na g�z atal�m.

`git branch` komutu dal yaratmak ve silmekten fazlas�n� yapar. Bu komutu hi�bir se�enek kullanmadan �al��t�r�rsan�z, mevcut dallar�n�z�n bir listesini g�r�rs�n�z:

	$ git branch
	  iss53
	* master
	  testing

`master` dal�n�n �n�ndeki `*` karakterine dikkatinizi �ekmi�tir: bu, o dal� se�mi� oldu�unuzu (_checkout_) g�steriyor. Yani, bu noktada bir kay�t yapacak olursan�z, yeni de�i�ikli�iniz `master` dal�n� ileri g�t�recek. Her bir dal�n en son kayd�n�n ne oldu�unu g�rmek isterseniz `git branch -v` komutunu �al��t�rabilirsiniz:

	$ git branch -v
	  iss53   93b412c fix javascript issue
	* master  7a98805 Merge branch 'iss53'
	  testing 782fd34 add scott to the author list in the readmes

Dallar�n�z�n ne durumda oldu�unu incelerken yararl� olacak bir ba�ka �ey de, hangi dallar�n �zerinde bulundu�unuz dalla birle�tirilip hangisinin birle�tirilmedi�ini g�rmek olabilir. `--merged` ve `--no-merge` se�enekleri Git'in 1.5.6 s�r�m�nden itibaren kullan�ma sunulmu�tur. Hangi dallar�n �zerinde bulundu�unuz dalla birle�tirilmi� oldu�unu g�rmek i�in `git branch --merged` komutunu kullanabilirsiniz:

	$ git branch --merged
	  iss53
	* master

`iss53` dal�n� daha �nce birle�tirdi�iniz i�in listede g�r�yorsunuz. Bu listede �n�nde `*` olmayan dallar� `git branch -d` komutuyla silebilirsiniz; onlardaki de�i�iklikleri zaten ba�ka bir dalla birle�tirdi�iniz i�in, herhangi bir kayb�n�z olmaz.

Hen�z birle�tirmedi�iniz de�i�ikliklerin bulundu�u dallar� g�rmek i�in `git branch --no-merged` komutunu �al��t�rabilirsiniz:

	$ git branch --no-merged
	  testing
Burada di�er dal� g�r�yorsunuz. Bu dalda hen�z birle�tirmedi�iniz de�i�iklikler bulundu�u i�in `git branch -d` komutu hata verecektir:

	$ git branch -d testing
	error: The branch 'testing' is not an ancestor of your current HEAD.
	If you are sure you want to delete it, run 'git branch -D testing'.

Oradaki de�i�iklikleri kaybetmeyi g�ze alarak dal� her �eye ra�men silmek istiyorsan�z, yukar�daki ��kt�da da belirtildi�i gibi, `-D` se�ene�iyle �steleyebilirsiniz.

## Dallanma �� Ak��lar� ##

Dallanma ve birle�tirmenin temellerine hakim oldu�unuza g�re, �imdi bu bilgiyi kullanarak neler yapabilece�imize bakal�m. Bu alt b�l�mde masrafs�z dallanman�n olanakl� k�ld��� baz� yayg�n i� ak��lar� �zerinde duraca��z, b�ylece bunlar� kendi geli�tirme d�ng�n�zde kullan�p kullanmamaya karar verebilirsiniz.

### Uzun S�reli Dallar ###

Git, basit �� tarafl� birle�tirme yapt��� i�in uzun bir zaman dilimi boyunca bir daldan di�erine �ok say�da birle�tirme yapmak genellikle kolayd�r. Yani, s�rekli a��k olan ve geli�tirme d�ng�n�z�n de�i�ik a�amalar�nda kullanabilece�iniz birka� dal bulundurabilirsiniz; d�zenli olarak baz�lar�ndan di�erlerine birle�tirme yapabilirsiniz.

Git'i kullanan pek �ok yaz�l�mc� bu yakla��m� benimser, `master` dal�nda yaln�zca kararl� (_stable_) durumdaki kod bulunur �yaln�zca yay�mlanm�� olan ya da yay�mlanacak kod. `develop` ya da `next` ad�nda, kararl�l�k testlerinin y�r�t�ld��� bir paralel dallar� daha vard�r �bu dal o kadar kararl� olmayabilir, fakat kararl� duruma getirildi�inde `master` dal�na birle�tirilir. K�sa �m�rl�, belirli bir i�levin geli�tirilmesine ayr�lm�� dallar�n (sizin `iss53` adl� dal�n�z gibi) haz�r olduklar�nda birle�tirilmeleri i�in �b�t�n testlerden ge�tiklerinden ve yeni hatalara kap� aralamad�klar�ndan emin olmak amac�yla� kullan�l�r.

Ger�ekte, yaz�l�m tarih�esinde ileri do�ru hareket eden imle�lerden s�z ediyoruz. Kararl� dallar eski kay�tlar�, g�ncel dallar �ok daha yenilerini g�sterir (bkz. Fig�r 3-18).

Insert 18333fig0318.png 
Fig�r 3-18. Daha kararl� dallar genellikle kay�t tarih�esinde daha geride bulunurlar.

Bu dallar�, �al��ma ambarlar� olarak hayal ediliriz, bir dizi kay�t b�t�n�yle test ediltikten sonra daha kararl� ba�ka br ambara konulurlar (bkz. Fig�r 3-19).

Insert 18333fig0319.png 
Figure 3-19. Dallar�n ambarlar gibi oldu�unu d���nebilirsiniz.

�e�itli kararl�l�k seviyeleri tan�mlay�p bu i�leyi�i o �ekilde kullanabilirsiniz. B�y�k projelerde `proposed` (�nerilen) ya da `pu` (proposed updates - �nerilen g�ncellemeler) ad�nda bir dal daha olabilir. Bu dala, `next` ya da `master` dal�na birle�tirilecek kadar kararl� a�amada bulunmayan dallar birle�tirilir. Sonu�ta, dallar farkl� kararl�l�k seviyelerinde bulunurlar; daha kararl� bir seviyeye ula�t�klar�nda, bir �stlerindeki dala birle�tirilirler.
Tekrarlayal�m: birden �ok uzun �m�rl� dal bulundurmak zorunlu de�ildir, ama, �zellikle �ok b�y�k ya da karma��k projelerde �al���yorsan�z �o�unlukla yararl�d�r.

### ��lev Dallar� ###

��lev dallar�, her �l�ekte proje i�in yararl�d�r. ��lev dallar�, belirli bir �zellikle ilgili de�i�ikliklerin geli�tirilmesi i�in kullan�lan k�sa �m�rl� dallard�r. Ba�ka SKS'lerde bu �ok masrafl� oldu�u i�in, muhtemelen bu yakla��m� daha �nce benimsemediniz. Ama Git'te dal yaratmak, o dal �zerinde �al��mak, dal� birle�tirmek ve daha sonra silmek, g�nde birka� kez yap�lan yayg�n bir y�ntemdir.

Bunu bir �nceki alt b�l�mde `iss53` ve `hotfix` dallar� �zerinde �al���rken g�rd�n�z. Bu dallarda birka� de�i�iklik yapt�n�z ve bu de�i�iklikleri `master` dal�na birle�tirdikten hemen sonra bu dallar� sildiniz. Bu teknik sayesinde, ba�lamlar aras�nda h�zl� ve b�t�nl�kl� ge�i�ler yapabilirsiniz ��al��malar�n�z belirli bir i�levin geli�tirilmesine adanm�� farkl� ambarlara ayr�lm�� oldu�undan, ge�en s�re zarf�nda, diyelim kod g�zden ge�irmesi s�ras�nda neler oldu�unu kolayl�kla g�rebilirsiniz. De�i�ikliklerinizi i�lev dallar�nda dakikalarca, g�nlerce ya da aylarca tutabilir, haz�r olduklar� zaman, hangisinin dal�n daha �nce olu�turuldu�una ald�rmadan birle�tirebilirsiniz.

Diyelim ki `master` dal�nda �al���yorsunuz, sonra bir hatay� gidermek i�in yeni bir dal olu�turuyorsunuz (`iss91`), derken ayn� hatay� ba�ka t�rl� gidermek i�in yeni bir dal olu�turuyorsunuz (`iss91v2`), sonra `master`'a geri d�n�p biraz daha �al���yorsunuz, sonra akl�n�za gelen ama �ok da gerekli olmad���n� d���nd���n�z bir �eyle ilgili �al��mak i�in yeni bir dal olu�turuyorsunuz (`dumbidea`)... Kay�t tarih�eniz Fig�r 3-20'deki gibi g�r�necektir.

Insert 18333fig0320.png 
Fig�r 3-20. Birden �ok i�lev dal�n�n bulundu�u kay�t tarih�eniz.

�imdi diyelim ki, hatan�n giderilmesinde ikinci ��z�m� (`iss91v2`) kullanmaya karar veriyorsunuz ve i� arkada�lar�n�z `dumbidea` dal�nda yapt�klar�n�z� dahice buluyor. `iss91` dal�n�z� ��pe atabilir (C5 ve C6 kay�tlar�n� kaybedeceksiniz) di�er iki dal� birle�tirebilirsiniz. Bu durumda tarih�eniz Fig�r 3-21'deki gibi g�r�necektir.

Insert 18333fig0321.png 
Fig�r 3-21. dumbidea ve iss91v2'yi birle�tirdikten sonra kay�t tarih�eniz.

Unutmay�n, b�t�n bunlar� yerel dallarda yap�yorsunuz. Dal yarat�rken ve birle�tirme yaparken her �ey yaln�zca yerel yaz�l�m havuzunda ger�ekle�iyor �hi�bir sunucu ileti�imi olmuyor.

## Uzak U�birim Dallar� ##

Yerel yaz�l�m havuzunuzdaki uzak u�birim dallar�, uzak u�birimlerdeki yaz�l�m havuzlar�n�z�n durumlar�n� g�steren imle�lerdir. Bunlar, hareket ettiremedi�iniz yerel dallard�r; yaln�zca sunucuyla ileti�im kurdu�unuzda hareket ederler. Bu dallar, son ba�land���n�zda sunucudaki yaz�l�m havuzunun ne durumda oldu�unu hat�rlatan i�aret�ilerdir.

`(remote)`/`(dal)` bi�imindedirler. �rne�in, sunucuya son ba�land���n�zda `origin` uzak u�birimindeki `master` dal�n�n nas�l oldu�unu g�rmek isterseniz, `origin/master` dal�na bakmal�s�n�z. Bir hatay� bir i� orta��yla birlikte ��z�yorsan�z ve onlar `iss53` ad�nda bir dal� sunucuya itmi�lerse, sizin yerel dal�n�z�n ad� `iss53` iken, sunucuya itilmi� olan dal�n ad� `origin/iss53` olacakt�r.

Bu biraz kafa kar��t�r�c� olabilir, gelin bir �rnekle a��klayal�m. Diyelim ki `git.�irketimiz.com` adresinde bir Git sunucunuz var. Buradan klonlama yaparsan�z, Git bu yaz�l�m havuzunu otomatik olarak `origin` olarak adland�racak, b�t�n veriyi indirecek, onun `master` dal�n�n g�sterdi�i kayd� g�steren `origin/master` ad�nda hareket ettiremeyece�iniz bir yerel dal olu�turacakt�r. Git ayr�ca,  �zerinde �al��abilmeniz i�in `origin`in `master` dal�n�n oldu�u yeri g�steren `master` ad�nda yerel bir dal da olu�turacakt�r (bkz. Fig�r 3-22).

Insert 18333fig0322.png 
Fig�r 3-22. Bir Git klonlad���n�zda hem yerel bir master dal�n�z hem de origin'in master dal�n� g�steren origin/master ad�nda bir dal�n�z olur.

E�er siz kendi master dal�n�zda �al���rken biir ba�kas� `git.�irketimiz.com`'a itme yap�p `master` dal�n� g�ncellerse, tarih�eleriniz birbirinden farkl�la�acakt�r. �stelik, `origin` sunucusuyla ileti�ime ge�medi�iniz s�rece sizin `origin/master` dal�n�z hareket etmeyecektir (bkz. Fig�r 3-23).

Insert 18333fig0323.png 
Fig�r 3-23. Siz yerelde �al���yorken bir ba�kas� sunucuya itme yaparsa, tarih�eleriniz birbirinden farkl� hareket etmeye ba�lar.

�al��malar�n�z� e�itlemek i�in `git fetch origin` komutunu �al��t�rabilirsiniz. Bu komut `origin` sunucusunun hangisi oldu�una bakar (bu �rnekte `git.�irketimiz.com`), orada bulunup da sizde olmayan her t�rl� veriyi indirir, yerel veritaban�n�z� g�ncelleyip yerelinizdeki `origin/master` dal�n� yeni, g�ncel konumuna ta��r (bkz. Fig�r 3-24).

Insert 18333fig0324.png 
Fig�r 3-24. git fetch komutu uzak u�birim imle�lerinizi g�nceller.

Birden �ok uzak u�birime sahip bir projede uzak u�birim imle�lerinin nas�l g�r�nece�ini incelemek i�in, Scrum tak�mlar�n�zdan birisi taraf�ndan kullan�lan ba�ka bir sunucunuzun daha oldu�unu varsayal�m. Bu sunucunun adresi `git.team1.�irketimiz.com` olsun. 2. B�l�m'de inceledi�imiz gibi, bu sunucuyu projenize uzak u�birim olarak eklemek i�in `git remote add` komutunu kullanabilirsiniz. Bu u�birimin ad� `teamone` olsun, ki bu ad� daha sonra b�t�n URL yerine k�saltma olarak kullanacaks�n�z (bkz. Fig�r 3-25).

Insert 18333fig0325.png 
Fig�r 3-25. Ba�ka bir sunucuyu uzak u�birim olarak eklemek.

`teamone` uzak u�biriminde bulunup da sizde bulunmayan �eyleri getirmek i�in `git fetch teamone` komutunu �al��t�rabilirsiniz. O sunucuda bulunan veriler `origin` sunucusunda bulunanlar�n alt k�mesi oldu�undan, Git herhangi bir veri �ekmez, ama `teamone/master` ad�nda, `teamone` sunucusunun `master` dal�n�n g�sterdi�i kayd� g�steren bir uzak u�birim dal� olu�turur (bkz. Fig�r 3-26).

Insert 18333fig0326.png 
Fig�r 3-26. teamone'nin master dal�n�n pozisyonunu g�steren bir yerel imleciniz oluyor.

### �tme ��lemi ###

Bir daldaki �al��malar�n�z� ba�kalar�yla payla�mak istedi�inizde, onu yazma yetkinizin oldu�u bir uzak u�birime itmelisiniz (_push_). yerel dallar�n�z otomatik olarak sunucuyla e�itlenmez �payla�mak istedi�iniz dallar� a��k �ekilde itmelisiniz. B�ylece, payla�mak istemedi�iniz dallar i�in �zel yerel dallar kullan�p, yaln�zca payla�mak istedi�iniz i�lev dallar�n� iteblirsiniz.

Ba�kalar�yla ortakla�a �al��mak istedi�iniz `serverfix` ad�nda bir dal�n�z varsa, onu da ilk dal�n�z� itti�iniz gibi itebilirsiniz. `git push (remote) (branch)` komutunu �al��t�r�n.

	$ git push origin serverfix
	Counting objects: 20, done.
	Compressing objects: 100% (14/14), done.
	Writing objects: 100% (15/15), 1.74 KiB, done.
	Total 15 (delta 5), reused 0 (delta 0)
	To git@github.com:schacon/simplegit.git
	 * [new branch]      serverfix -> serverfix

Bu bir t�r k�sayol say�labilir. Git `serverfix` dal ad�n� otomatik olarak `refs/heads/serverfix:refs/heads/serverfix` bi�iminde a��mlar, bu �u demektir: �yerel `serverfix` dal�m� al�p uzak u�birimin `serverfix` dal�n� g�ncellemek i�in kullan.� `refs/heads/` k�sm�nz 9. B�l�m'de ayr�nt�s�yla de�inece�iz, ama genellikle bu k�sm� kullanmasan�z da olur. Ayn� ama�la `git push origin serverfix:serverfix` komutunu da �al��t�rabilirsiniz �bu da �u demektir: �Yereldeki serverfix'i al, bunu uzak u�birimin serverfix'i yap.� Bu bi�imi, yereldeki dal ad�yla uzak u�birimdeki dal ad� farkl� ise kullanabilirsiniz. Dal ad�n�n uzak u�birimde `serferfix` olmas�n� istemezseniz `git push origin serverfix:awesomebranch` komutunu �al��t�rarak yereldeki `serverfix` dal�n� uzak u�birimdeki `awesomebranch` dal�na itebilirsiniz.

Birlikte �al��t���n�z insanlar sunucudan getirme i�lemi (_fetch_) yapt�klar�nda,  sunucudaki `serverfix` s�r�m�n�n bulundu�u yeri g�steren `origin/serverfix` ad�nda bir imlece sahip olacaklar.

	$ git fetch origin
	remote: Counting objects: 20, done.
	remote: Compressing objects: 100% (14/14), done.
	remote: Total 15 (delta 5), reused 0 (delta 0)
	Unpacking objects: 100% (15/15), done.
	From git@github.com:schacon/simplegit
	 * [new branch]      serverfix    -> origin/serverfix

Unutmay�n, getirme (_fetch_) komutuyla yeni uzak u�birim dallar�n� indirdi�inizde, yerelde otomatik olarak de�i�tirilebilir dallar olu�turulmaz. Ba�ka bir deyi�le, bu �rnekte, `serverfix` ad�nda bir dal�n�z olmaz, de�i�tiremeyece�iniz `origin/serverfix` ad�nda bir imleciniz olur.

Oradaki de�i�iklikleri �zerinde �al��makta oldu�unuz dala birle�tirmek isterseniz, `git merge origin/serverfix` komutunu �al��t�rabilirsiniz. �zerinde �al��mak �zere kendinize ait bir `serverfix` dal�n�z olmas�n� isterseniz, uzak u�birim dal�n� temel alabilirsiniz:

	$ git checkout -b serverfix origin/serverfix
	Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "serverfix"

Bu, �zerinde �al��abilece�iniz ve `origin/serverfix`in g�sterdi�i yerden ba�layan bir yerel dal yarat�r.

### �zleme Dallar� ###

Bir uzak u�birim dal�ndan yerel bir dal se�ti�inizde (_checkout_), bu i�lem otomatik olarak bir _izleme dal�_ (_tracking branch_) olu�turur. �zleme dallar�, uzak u�birim dallar�yla do�rudan ili�kileri bulunan yerel dallard�r. Bir izleme dal�ndan `git push` komutunu �al��t�rd���n�zda , Git hangi sunucudaki hangi dala itme i�lemi yapmas� gerekti�ini bilir. Ayr�ca, bu dallardan birinden `git pull` komutunu �al��t�rd���n�zda, b�t�n imle�ler indirilece�i gibi, bu izleme dal�na kar��l�k gelen uzak u�birim dal� da otomatik olarak bu dalla birle�tirilir.

Bir yaz�l�m havuzunu klonlad���n�zda, genellikle `origin/master` dal�n� izleyen bir `master` dal� yarat�l�r. Bu nedenle `git push` ve `git pull` komutlar� bu durumlarda ek arg�manlara gerek kalmadan �al���rlar. �te yandan, isterseniz ba�ka izleme dallar� da �`origin`'i ya da `master` dal�n�z izlemeyen dallar� olu�turabilirsiniz. Yukar�da basit bir �rne�ini g�rd�k: `git checkout -b [dal] [uzak_ucbirim]/[dal]`. Git'in 1.6.2'den itibaren olan s�r�mlerinde `--track` k�sayolunu da kullanabilirsiniz:

	$ git checkout --track origin/serverfix
	Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "serverfix"

Uzak u�birim dal�n�n ad�ndan ba�ka bir adla yerel dal olu�turmak isterseniz, yukar�daki komutu farkl� bir yerel dal ad�yla kullanabilirsiniz:

	$ git checkout -b sf origin/serverfix
	Branch sf set up to track remote branch refs/remotes/origin/serverfix.
	Switched to a new branch "sf"

�imdi, yereldeki sf dal�, otomatik olarak `origin/serverfix` dal�na itme ve �ekme i�lemi yapabilecek.

### Uzak U�birim Dallar�n� Silmek ###

Suppose you�re done with a remote branch � say, you and your collaborators are finished with a feature and have merged it into your remote�s `master` branch (or whatever branch your stable codeline is in). You can delete a remote branch using the rather obtuse syntax `git push [remotename] :[branch]`. If you want to delete your `serverfix` branch from the server, you run the following:

	$ git push origin :serverfix
	To git@github.com:schacon/simplegit.git
	 - [deleted]         serverfix

Boom. No more branch on your server. You may want to dog-ear this page, because you�ll need that command, and you�ll likely forget the syntax. A way to remember this command is by recalling the `git push [remotename] [localbranch]:[remotebranch]` syntax that we went over a bit earlier. If you leave off the `[localbranch]` portion, then you�re basically saying, �Take nothing on my side and make it be `[remotebranch]`.�

## Rebasing ## Zemin, K�k, Temel

In Git, there are two main ways to integrate changes from one branch into another: the `merge` and the `rebase`. In this section you�ll learn what rebasing is, how to do it, why it�s a pretty amazing tool, and in what cases you won�t want to use 
