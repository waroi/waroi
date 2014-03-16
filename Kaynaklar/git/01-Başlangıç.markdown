# Ba�lang�� #

Bu b�l�mde Git kullan�m� hakk�nda temel bilgileri bulacaks�n�z. ��e, s�r�m kontrol sistemleri hakk�nda a��klamalarla ba�layaca��z; daha sonra Git kurulumunun nas�l yap�laca��n�, en son olarak da arac�n yap�land�rma ve kullan�m�n� a��klayaca��z. Bu b�l�m�n sonunda Git'in varl�k sebebini ve neden onu kullanman�z gerekti�ini anlayacak, Git'i kullanmaya ba�lamak i�in kurulumu tamamlam�� olacaks�n�z.

## S�r�m Kontrol� Hakk�nda ##

S�r�m kontrol� nedir ve ne i�e yarar? S�r�m kontrol�, bir ya da daha fazla dosya �zerinde yap�lan de�i�iklikleri kaydeden ve daha sonra belirli bir s�r�me geri d�nebilmenizi sa�layan bir sistemdir. Bu kitaptaki �rneklerde yaz�l�m kaynak kod dosyalar�n�n s�r�m kontrol�n� yapacaks�n�z, ne var ki ger�ekte s�r�m kontrol�n� neredeyse her t�rden dosya i�in kullanabilirsiniz.

Bir grafik ya da web tasar�mc�s�ysan�z ve bir g�rselin ya da tasar�m�n de�i�ik s�r�mlerini korumak istiyorsan�z (ki muhtemelen bunu yapmak istersiniz), bir S�r�m Kontrol Sistemi (SKS) kullanman�z �ok ak�ll�ca olacakt�r. SKS, dosyalar�n ya da b�t�n projenin ge�mi�teki belirli bir s�r�m�ne eri�menizi, zaman i�inde yap�lan de�i�iklikleri kar��la�t�rman�z�, soruna neden olan �eyde en son kimin de�i�iklik yapt���n�, belirli bir hatay� kimin, ne zaman sisteme dahil etti�ini ve daha ba�ka pek �ok �eyi g�rebilmenizi sa�lar. �te yandan, SKS kullanmak, bir hata yapt���n�zda ya da baz� dosyalar� yanl��l�kla sildi�inizde durumu kolayca tel�fi etmenize yard�mc� olur. �stelik, b�t�n bunlar size kayda de�er bir ek y�k de getirmez.

### Yerel S�r�m Kontrol Sistemleri ###

�o�u insan, dosyalar� bir klas�re (ak�llar� ba�lar�ndaysa tarih ve zaman bilgisini de i�eren bir klas�re) kopyalayarak s�r�m kontrol� yapmay� tercih eder. Bu yakla��m �ok yayg�nd�r, ��nk� �ok kolayd�r; ama ayn� zamanda hatalara da alabildi�ine a��kt�r. Hangi klas�rde oldu�unuzu unutup yanl�� dosyaya yazabilir ya da istemedi�iniz dosyalar�n �st�ne kopyalama yapabilirsiniz.

Bu sorunla ba� edebilmek i�in, programc�lar uzun zaman �nce, dosyalardaki b�t�n de�i�iklikleri s�r�m kontrol�ne alan basit bir veritaban�na sahip olan yerel SKS'ler geli�tirdiler (bkz. Fig�r 1-1).

Insert 18333fig0101.png 
Fig�r 1-1. Yerel s�r�m kontrol diyagram�.

En yayg�n SKS ara�lar�ndan biri, bug�n h�l� pek�ok bilgisayara kurulu olarak da��t�lan, rcs ad�nda bir sistemdi. �nl� Mac OS X i�letim sistemi bile, Developer Tools'u y�kledi�inizde, rcs komutunu kurmaktad�r. Bu ara�, iki s�r�m aras�ndaki yamalar� (yani, dosyalar aras�ndaki farklar�) �zel bir bi�imde diske kaydeder; daha sonra, bu yamalar� birbirine ekleyerek, bir dosyan�n belirli bir s�r�mdeki g�r�n�m�n� yeniden olu�turur.

### Merkezi S�r�m Kontrol Sistemleri ###

�nsanlar�n kar��la�t��� ikinci b�y�k sorun, ba�ka sistemlerdeki programc�larla birlikte �al��ma ihtiyac�ndan ileri gelir. Bu sorunla ba�a ��kabilmek i�in, Merkezi S�r�m Kontrol Sistemleri (MSKS) geli�tirilmi�tir. Bu sistemler, mesel� CVS, Subversion ve Perforce, s�r�m kontrol�ne al�nan b�t�n dosyalar� tutan bir sunucu ve bu sunucudan dosyalar� se�erek alan (_check out_) istemcilerden olu�ur. Bu y�ntem, y�llarca, s�r�m kontrol�nde standart y�ntem olarak kabul g�rd� (bkz. Fig�r 1-2).

Insert 18333fig0102.png 
Fig�r 1-2. Merkezi s�r�m kontrol diyagram�.

Bu y�ntemin, �zellikle yerel SKS'lere g�re, �ok say�da avantaj� vard�r. �rne�in, bir projedeki herkes, di�erlerinin ne yapt���ndan belirli �l��de haberdard�r. Sistem y�neticileri kimin hangi yetkilere sahip olaca��n� olduk�a ayr�nt�l� bi�imde d�zenleyebilirler; �stelik bir MSKS'yi y�netmek, her istemcide ayr� ayr� kurulu olan yerel veritabanlar�n� y�netmeye g�re �ok daha kolayd�r.

Ne var ki, bu y�ntemin de ciddi baz� s�k�nt�lar� vard�r. En a�ikar s�k�nt�, merkezi sunucunun ar�zalanmas� durumunda ortaya ��kacak k�r�lma noktas� problemidir. Sunucu bir saatli�ine ��kecek olsa, o bir saat boyunca kullan�c�lar�n �al��malar�n� sisteme aktarmalar� ya da �al��t�klar� �eylerin s�r�mlenmi� kopyalar�n� kaydetmeleri m�mk�n olmayacakt�r. Merkezi veritaban�n�n sabit diski bozulacak olsa, yedekleme de olmas� gerekti�i gibi yap�lmam��sa, elinizdeki her �eyi �projenin, kullan�c�lar�n bilgisayarlar�nda kalan yerel bellek kopyalar� (_snapshot_) d���ndaki b�t�n tarih�esini� kaybedersiniz. Yerel SKS'ler de bu sorundan muzdariptir �projenin b�t�n tarih�esini tek bir yerde tuttu�unuz s�rece her �eyi kaybetme riskiniz vard�r.

### Da��t�k S�r�m Kontrol Sistemleri ###

Bu noktada Da��t�k S�r�m Kontrol Sistemleri (DSKS) devreye girer. Bir DSKS'de (Git, Mercurial, Bazaar ya da Darcs �rneklerinde oldu�u gibi), istemciler (kullan�c�lar) dosyalar�n yaln�zca en son bellek kopyalar�n� almakla kalmazlar: yaz�l�m havuzunu (_repository_) b�t�n�yle yans�larlar (kopyalarlar).

B�ylece, sunuculardan biri ��kerse, ve o sunucu �zerinden ortak �al��ma y�r�ten sistemler varsa, istemcilerden birinin yaz�l�m havuzu sunucuya geri y�klenerek sistem kurtar�labilir. Her se�me (_checkout_) i�lemi esas�nda b�t�n verinin yedeklenmesiyle sonu�lan�r (bkz. Fig�r 1-3).

Insert 18333fig0103.png 
Fig�r 1-3. Da��t�k s�r�m kontrol diyagram�.

Dahas�, bu sistemlerden �o�u birden �ok uzak u�birimdeki yaz�l�m havuzuyla rahatl�kla �al���r, b�ylece, ayn� proje i�in ayn� anda farkl� insan topluluklar�yla farkl� bi�imlerde ortak �al��malar y�r�tebilirsiniz. Bu, birden �ok i� ak��� ile �al��abilmenizi sa�lar, ki bu merkezi sistemlerde (hiyerar�ik modeller gibi) m�mk�n de�ildir.

## Git'in K�sa Bir Tarih�esi ##

Hayattaki pek �ok harika �ey gibi, Git de bir miktar yarat�c� y�k�m ve ate�li tart��mayla ba�lad�. Linux �ekirde�i (_kernel_) olduk�a b�y�k �l�ekli bir a��k kaynak kodlu yaz�l�m projesidir. Linux �ekirdek bak�m ve geli�tirme ya�am s�resinin �o�unda (1991-2002), yaz�l�m de�i�iklikleri yamalar ve ar�iv dosyalar� olarak tutulup ta��nd�. 2002 y�l�nda, Linux �ekirdek projesi, BitKeeper ad�nda tescilli bir DSKS kullanmaya ba�lad�.

2005 y�l�nda, Linux �ekirde�ini geli�tiren toplulukla BitKeeper'� geli�tiren �irket aras�ndaki ili�ki bozuldu ve arac�n topluluk taraf�ndan �cretsiz olarak kullan�labilmesi uygulamas�na son verildi. Bu, Linux geli�tirim toplulu�unu (ve �zellikle Linux'un yarat�c�s� olan Linus Torvalds'�) BitKeeper'� kullan�rken ald�klar� derslerden yola ��karak kendi ara�lar�n� geli�tirme konusunda harekete ge�irdi. Yeni sistemin hedeflerinden baz�lar� �unlard�:

*	H�z
*	Basit tasar�m
*	�izgisel olmayan geli�tirim i�in g��l� destek (binlerce paralel dal (_branch_))
*	B�t�n�yle da��t�k olma
*	Linux �ekirde�i gibi b�y�k projelerle verimli bi�imde ba�a ��kabilme (h�z ve veri boyutu)

2005'teki do�umundan beri, Git kullan�m kolayl�klar�n� geli�tirebilmek i�in evrilip olgunla�t�, ama yine de bu niteliklerini korudu. Git, inan�lmaz �l��de h�zl�, b�y�k �l�ekli projelerde alabildi�ine verimli ve �izgisel olmayan geli�tirim (bkz. 3. B�l�m) i�in inan�lmaz bir dallanma (_branching_) sistemine sahip.

## Git'in Temelleri ##

Peki Git �z�nde nedir? Bu, �z�msenmesi gereken �nemli bir alt b�l�m, ��nk� Git'in ne oldu�unu ve temel �al��ma ilkelerini anlarsan�z, Git'i etkili bi�imde kullanman�z �ok daha kolay olacakt�r. Git'i ��renirken, Subversion ve Perforce gibi di�er SKS'ler hakk�nda bildiklerinizi akl�n�zdan ��karmaya �al���n; bu arac� kullan�rken ya�anabilecek kafa kar���kl�klar�n� �nlemenize yard�mc� olacakt�r. Git'in, kullan�c� aray�z� s�z konusu sistemlerle benzerlik g�sterse de, bilgiyi depolama ve yorumlama bi�imi �ok farkl�d�r; bu farkl�l�klar� anlamak, arac� kullan�rken kafa kar���kl���na d��menizi engellemekte yard�mc� olacakt�r.

### Farklar De�il, Bellek Kopyalar� ###

Git ile di�er SKS'ler (Subversion ve ahbaplar� dahil) aras�ndaki esas fark, Git'in bilgiyi yorumlay�� bi�imiyle ilgilidir. Kavramsal olarak, di�er sistemlerin �o�u, bilgiyi dosya-tabanl� bir dizi de�i�iklik olarak depolar. Bu sistemler (CVS, Subversion, Perforce, Bazaar ve saire) bilgiyi, kay�t alt�nda tuttuklar� bir dosya k�mesi ve zamanla her bir dosya �zerinde yap�lan de�i�ikliklerin listesi olarak yorumlarlar (bkz. Fig�r 1-4).

Insert 18333fig0104.png 
Fig�r 1-4. Di�er sistemler veriyi her bir dosyan�n ilk s�r�mu �zerinde yap�lan de�i�iklikler olarak depolama e�ilimindedir.

Git, veriyi b�yle yorumlay�p depolamaz. Bunun yerine, Git, veriyi, bir mini dosya sisteminin bellek kopyalar� olarak yorumlar. Her kay�t i�leminde (_commit_), ya da projenizin konumunu her kaydedi�inizde, Git o anda dosyalar�n�z�n nas�l g�r�nd���n�n bir foto�raf�n� �ekip o bellek kopyas�na bir referans� depolar. Verimli olabilmek i�in, de�i�meyen dosyalar� yeniden depolamaz, yaln�zca halihaz�rda depolanm�� olan bir �nceki �zde� kopyaya bir ba�lant� kurar. Git'in veriyi yorumlay��� daha �ok Fig�r 1-5'teki gibidir.

Insert 18333fig0105.png 
Fig�r 1-5. Git veriyi projenin zaman i�indeki bellek kopyalar� olarak depolar.

Bu, Git'le neredeyse b�t�n di�er SKS'ler aras�nda ciddi bir ayr�md�r. Bu ayr�m nedeniyle Git, s�r�m kontrol�n�n, di�er s�r�m kontrol sistemlerinin �o�u taraf�ndan �nceki ku�aklardan devral�nan neredeyse b�t�n y�nlerini yeniden g�zden ge�irmek zorunda b�rak�r. Bu ayr�m Git'i basit bir SKS olman�n �tesinde, etkili ara�lara sahip bir mini dosya sistemi gibi olmaya iter. Veriyi bu �ekilde yorumlaman�n yararlar�ndan baz�lar�n� dallanmay� i�leyece�imiz 3. B�l�m'de ele alaca��z.

### Neredeyse Her ��lem Yerel ###

Git'teki i�lemlerin �o�u, yaln�zca yerel dosyalara ve kaynaklara ihtiya� duyar �genellikle bilgisayar a��ndaki ba�ka bir bilgisayardaki bilgilere ihtiya� yoktur. E�er �o�u i�lemin a� gecikmesi maliyetiyle ger�ekle�ti�i bir MSKS kullanm��san�z, Git'in bu y�n�n� g�r�nce, onun h�z tanr�lar� taraf�ndan kutsanm�� oldu�unu d���nebilirsiniz. ��nk� projenin b�t�n tarih�esi orada, yerel diskinde bulunmaktad�r, i�lemlerin �o�u anl�k ger�ekle�iyor gibi g�r�n�r.

�rne�in, projenin tarih�esini taramak i�in Git bir sunucuya ba�lan�p oradan tarih�eyi indirdikten sonra g�r�nt�lemekle u�ra�maz �yerel veritaban�n� okumak yeterlidir. Bu da proje terih�esini neredeyse an�nda g�r�nteleyebilmeniz anlam�na gelir. Bir dosyan�n �imdiki haliyle bir ay �nceki hali aras�ndaki farklar� g�rmek isterseniz, Git, bir sunucudan fark hesaplamas� yapmas�n� talep etmek ya da kar��la�t�rmay� yerelde yapabilmek i�in dosyan�n bir ay �nceki halini indirmek zorunda kalmak yerine, dosyan�n bir ay �nceki halini yerelde bulup fark hesaplamas�n� yerelde yapar.

Bu ayn� zamanda, e�er ba�lant�n�z kopmu�sa, ya da VPN ba�lant�n� yoksa yapamayaca��n�z �eylerin de say�ca olduk�a s�n�rl� oldu�u anlam�na geliyor. U�a�a ya da trene binmi� oldu�unuz halde biraz �al��mak istiyorsan�z, y�kleme yapabilece�iniz bir a� ba�lant�s�na kavu�ana kadar g�le oynaya kay�t yapabilirsiniz. Eve vard���n�zda VPN istemcinizin olmas� gerekti�i gibi �al��m�yorsa, yine de �al��maya devam edebilirsiniz. pek �ok ba�ka sistemde bunlar� yapmak ya imk^ans�z ya da zahmetlidir. S�z gelimi Perforce'ta, bir sunucuya ba�l� de�ilseniz fazlaca bir �ey yapamazs�n�z; Subversion ve CVS'te dosyalar� de�i�tirebilirsiniz, ama veritaban�na kay�t yapamazs�n�z (��nk� veritaban�na ba�lant�n�z yoktur). Bu, �ok �nemli bir sorun gibi g�r�nmeyebilir, ama ne kadar fark yaratabilece�ini g�rd���n�zde �a��rabilirsiniz.

### Git B�t�nl�kl�d�r ###

Git'te her �ey depolanmadan �nce s�nama toplam�ndan ge�irilir (_checksum_) ve daha sonra bu s�nama toplam� kullan�larak ifade edilir. Bu da demek oluyor ki, Git fark etmeden bir dosyan�n ya da klas�r�n i�eri�ini de�i�tirmek m�mk�n de�ildir. Bu i�lev Git'in merkezi i�levlerinden biridir ve felsefesiyle bir b�t�nl�k olu�turur. Transfer s�ras�nda veri kayb� ya da doysa ar�zas� olmu�sa, Git bunu mutlaka fark edecektir.

Git'in s�nama toplam� i�in kulland��� mekanizmaya SHA-1 �zeti denir. Bu, on alt�l� say� sisteminin (_hexadecimal_) sembolleriyle g�sterilen (0-9 ve A-F) ve dosya ve klas�r d�zenini temel alan bir hesaplamayla elde denilen 40 karakterlik bir karakter dizisidir. Bir SHA-1 �zeti �una benzer:

	24b9da6552252987aa493b52f8696cd6d3b00373

Bu �zetler s�kl�kla kar��n�za ��kacak, ��nk� Git onlar� yayg�n bi�imde kullanyor. Hatta, Git her �eyi dosya ad�yla de�il, i�eri�inin �zet de�eriyle adreslenen veritaban�nda tutar.

### Git Genellikle Yaln�zca Veri Ekler ###

Git'te i�lem yapt���n�zda neredeyse bu i�lemlerin tamam� Git veritaban�na veri ekler. Sistemin geri d�nd�r�lemez bir �ey yapmas�n� ya da veri silmesini sa�lamak �ok zordur. Her SKS'de oldu�u gibi hen�z kaydetmedi�iniz de�i�iklikleri kaybedebilir ya da bozabilirsiniz; ama Git'e bir bellek kopyas�n� kaydettikten sonra veri kaybetmek �ok zordur, �zellikle de veritaban�n�z� d�zenli olarak ba�ka bir yaz�l�m havuzuna itiyorsan�z (_push_).

Bu Git kullanmay� keyifli hale getirir, ��nk� i�leri ciddi bi�imde s�k�nt�ya sokmadan denemeler yapabilece�imizi biliriz. Git'in veriyi nas�l depolad��� ve kaybolmu� g�r�nen veriyi nas�l kurtarabilece�iniz hakk�nda daha derinlikli bir inceleme i�in bkz. 9. B�l�m.

### �� A�ama ###

�imdi dikkat! ��renme s�recinizin p�r�zs�z ilerlemesini istiyorsan�z, akl�n�zda bulundurman�z gereken esas �ey bu. Git'te, dosyalar�n�z�n i�inde bulunabilece�i �� a�ama (_state_) vard�r: kaydedilmi�, de�i�tirilmi� ve haz�rlanm��. Kaydedilmi�, verinin g�venli bi�imde veritaban�nda depolanm�� oldu�u anlam�na gelir. De�i�tirilmi�, dosyay� de�i�tirmi� oldu�unuz fakat hen�z veritaban�na kaydetmedi�iniz anlam�na gelir. Haz�rlanm�� ise, de�i�tirilmi� bir dosyay� bir sonraki kay�t i�leminde bellek kopyas�na al�nmak �zere i�aretledi�iniz anlam�na gelir.

Bu da bizi bir Git projesinin �� ana b�l�m�ne getiriyor: Git klas�r�, �al��ma klas�r� ve haz�rl�k alan�.

Insert 18333fig0106.png 
Fig�r 1-6. �al��ma klas�r�, haz�rl�k alan� ve Git klas�r�.

Git klas�r�, Git'in �stverileri (_metadata_) ve nesne veritaban�n� depolad��� yerdir. Bu, Git'in en �nemli par�as�d�r ve bir yaz�l�m havuzunu bir bilgisayardan bir ba�kas�na klonlad���n�zda kopyalanan �eydir.

�al��ma klas�r� projenin bir s�r�m�nden yap�lan tek bir se�medir. Bu dosyalar Git klas�r�ndeki s�k��t�r�lm�� veritaban�ndan ��kart�l�p sizin kullan�m�n�z i�in sabit diske yerle�tirilir.

Haz�rl�k alan� (_staging area_), genellikle Git klas�r�n�zde bulunan ve bir sonraki kay�t i�lemine hangi de�i�ikliklerin dahil olaca��n� tutan sade bir dosyad�r. Buna bazen indeks dendi�i de olur, ama haz�rl�k alan� ifadesi giderek daha standart hale geliyor.

Git i�leyi�i temelde ��yledir:

1. �al��ma klas�r�n�zdeki dosyalar �zerinde de�i�iklik yapars�n�z.
2. Dosyalar� bellek kopyalar�n� haz�rl�k alan�na ekleyerek haz�rlars�n�z.
3. Dosyalar�n haz�rl�k alan�ndaki hallerini al�p oradaki bellek kopyas�n� kal�c� olarak Git klas�r�ne depolayan bir kay�t i�lemi yapars�n�z.

Bir dosyan�n belirli bir s�r�m� Git klas�r�ndeyse, onun kaydedilmi� oldu�u kabul edilir. E�er �zerinde de�i�iklik yap�lm�� fakat haz�rl�k alan�na eklenmi�se, haz�rlanm�� oldu�u s�ylenir. Ve se�me i�leminden sonra �zerinde de�i�iklik yap�lm�� fakat kay�t i�in haz�rlanmam��sa, de�i�tirilmi� olarak nitelenir.

## Git'in Kurulumu ##

Gelin Git'i kullanmaya ba�layal�m. Her �eyden �nce, Git'i kurman�z gerekiyor. Bunu iki ba�l�ca bi�imde ger�ekle�tirebilirsiniz: kaynak kodundan ya da platformunuz i�in halihaz�rda varolan paketi kullanarak kurabilirsiniz.

### Kaynak Kodundan Kurulum ###

Yapabiliyorsan�z, Git'i kaynak kodundan kurmak kullan��l�d�r, ��nk� b�ylece en yeni versiyonunu edinebilirsiniz. Git'in her yeni versiyonu yararl� kullan�c� aray�z� g�ncellemeleri i�erir, dolay�s�yla en son versiyonu kurmak, e�er yaz�l�m derlemek konusunda s�k�nt� ya�amayaca��n�z� d���n�yorsan�z, en iyi yoldur. Ayr�ca kimi zaman, Linux da��t�mlar� yaz�l�mlar�n �ok eski paketlerini i�erirler; dolay�s�yla, �ok g�ncel bir da��t�ma sahip de�ilseniz ya da tersta��malar (_backport_) kullanm�yorsan�z, kaynak koddan kurulum en mant�kl� se�enek olabilir.

Git'i kurmak i�in, Git'in ba��ml� oldu�u �u k�t�phanelerin sisteminizde bulunmas� gerekiyor: curl, zlib, openssl, expat, ve libiconv. �rne�in, (Fedora gibi) yum arac�na ya da (Debian tabanl� sistemler gibi) apt-get arac�na sahip bir sistemdeyseniz, ba��ml�l�klar� kurmak i�in �u komutlardan birini kullanabilirsiniz:

	$ yum install curl-devel expat-devel gettext-devel \
	  openssl-devel zlib-devel

	$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
	  libz-dev libssl-dev
	
B�t�n gerekli ba��ml�l�klar� y�kledikten sonra Git web sitesinden en son bellek kopyas�n� edinebilirsiniz:

	http://git-scm.com/download

Sonra derleyip kurabilirsiniz:

	$ tar -zxf git-1.7.2.2.tar.gz
	$ cd git-1.7.2.2
	$ make prefix=/usr/local all
	$ sudo make prefix=/usr/local install

Bu ad�mdan sonra, Git'teki yeni g�ncellemeleri Git'in kendisini kullanarak edinebilirsiniz:

	$ git clone git://git.kernel.org/pub/scm/git/git.git
	
### Linux'ta Kurulum ###

Git'i Linux sisteminize paket kurucu yard�m�yla kurmak istiyorsan�z, bunu genellikle da��t�m�n�zla birlikte gelen temel paket y�netim arac�yla yapabilirsiniz. Fedora kullan�c�s�ysan�z, yum'u kullanabilirsiniz:

	$ yum install git-core

Ubuntu gibi Debian-tabanl� bir sistemdeyseniz, apt-get'i kullanabilirsiniz:

	$ apt-get install git
	
Pisi Linux veya pisi tabanlı bir sistemde iseniz:

	$ pisi it git

### Mac'te Kurulum ###

Git'i Mac'te kurmak i�in iki kolay yol vard�r. En kolay�, Google Code sayfas�ndan indirebilece�iniz g�rsel Git y�kleyicisini kullanmakt�r (bkz. Fig�r 1-7).

	http://code.google.com/p/git-osx-installer

Insert 18333fig0107.png 
Fig�r 1-7. Git OS X y�kleyicisi.

Di�er ba�l�ca yol, Git'i MacPorts (`http://www.macports.org`) vas�tas�yla kurmakt�r. MacPorts halihaz�rda kurulu bulunuyorsa Git'i �u komutla kurabilirsiniz:

	$ sudo port install git-core +svn +doc +bash_completion +gitweb

B�t�n ek paketleri kurman�z �art de�il, ama Git'i Subversion yaz�l�m havuzlar�yla kullanman�z gerekecekse en az�ndan +svn'i edinmelisiniz.

### Windows'ta Kurulum ###

Git'i Windows'da kurmak olduk�a kolayd�r. mysysGit projesi en basit kurulum y�ntemlerinden birine sahip. �al��t�r�labilir kurulum dosyas�n� GitHub sayfas�ndan indirip �al��t�rman�z yeterli:

	http://msysgit.github.com/

Kurulum tamamland���nda hem (daha sonra i�e yarayacak olan SSH istemcisini de i�eren) komut sat�r� n�shas�na hem de standart kullan�c� aray�z�ne sahip olacaks�n�z.

## �lk Ayarlamalar ##

Art�k Git sisteminizde kurulu oldu�una g�re, onu ihtiyac�n�za g�re uyarlamak i�in baz� d�zenlemeler yapabilirsiniz. Bunlar� yaln�zca bir kere yapman�z yeterli olacakt�r: g�ncellemelerden etkilenmeyeceklerdir. Ayr�ca istedi�iniz zaman komutlar� yeniden �al��t�rarak ayarlar� de�i�tirebilirsiniz.

Git, Git'in nas�l g�r�nece�ini ve �al��aca��n� belirleyen b�t�n konfig�rasyon de�i�kenlerini g�rmenizi ve de�i�tirmenizi sa�layan git config ad�nda bir ara�la birlikte gelir. Bu de�i�kenler �� farkl� yerde depolanabilirler:

*	`/etc/gitconfig` dosyas�: Sistemdeki b�t�n kullan�c�lar ve onlar�n b�t�n yaz�l�m havuzlar� i�in ge�erli olan de�erleri i�erir. `git config` komutunu `--system` se�ene�iyle kullan�rsan�z, ara� bu dosyadan okuyup de�i�iklikleri bu dosyaya kaydedecektir.
*	`~/.gitconfig` dosyas�: Kullan�c�ya �zeldir. `--global` se�ene�iyle Git'in bu dosyadan okuyup de�i�iklikleri bu dosyaya kaydetmesini sa�layabilirsiniz.
*	kullanmakta oldu�unuz yaz�l�m havuzundaki git klas�r�nde bulunan config dosyas� (yani `.git/config`): S�z konusu yaz�l�m havuzuna �zeldir. Her d�zeydeki ayarlar kendisinden �nce gelen d�zeydeki ayarlar� g�lgede b�rak�r (_override_), dolay�s�yla `.git/config`'deki de�erler `/etc/gitconfig`'deki de�erlerden daha bask�nd�r.

Git, Windows sistemlerde `$HOME` klas�r�ndeki (�o�u kullan�c� i�in `C:\Documents and Settings\$USER` klas�r�d�r) `.gitconfig` dosyas�na bakar (�.N.: Windows kullan�c� klas�r�ne %HOMEPATH% �evresel de�i�kenini kullanarak ula�abilirsiniz). Git, Windows sistemlerde de /etc/gitconfig dosyas�n� arar fakat bu konum, Git kurulumu s�ras�nda se�ti�iniz MSys k�k dizinine g�redir.

### Kimli�iniz ###

Git'i kurdu�unuzda yapman�z gereken ilk �ey ad�n�z� ve e-posta adresinizi ayarlamakt�r. Bunun �nemli olmas�n�n nedeni her bir Git kayd�n�n bu bilgiyi kullan�yor olmas� ve bu bilgilerin dola��ma soktu�unuz kay�tlara de�i�mez bi�imde i�lenmesidir.

	$ git config --global user.name "John Doe"
	$ git config --global user.email johndoe@example.com

Yinelemek gerekirse, `--global` se�ene�ini kulland���n�zda bunu bir kez yapman�z yeterli olacakt�r, ��nk� Git, o sistemde yapaca��n�z her i�lem i�in bu bilgileri kullanacakt�r. �smi ya da e-posta adresini projeden projeye de�i�tirmek isterseniz, komutu de�i�iklik yapmak istedi�iniz proje klas�r�n�n i�inde `--global` se�ene�i olmadan �al��t�rabilirsiniz.

### Edit�r�n�z ###

Kimlik ayarlar�n�z� yapt���n�za g�re, Git sizden bir mesaj yazman�z� istedi�inde kullanaca��n�z edit�rle ilgili d�zenlemeyi yapabilirsiniz. Aksi belirtilmedik�e Git sisteminizdeki �ntan�ml� (_default_) edit�r� kullan�r, bu da genellikle Vi ya da Vim'dir. Emacs gibi ba�ka bir metin edit�r� kullanmak isterseniz, �u komutu kullanabilirsiniz:

	$ git config --global core.editor emacs
	
### Dosya Kar��la�t�rma Arac�n�z ###

D�zenlemek isteyece�iniz bir di�er yararl� ayar da birle�tirme (_merge_) uyu�mazl�klar�n� gidermek i�in kullanaca��n�z ara�la ilgilidir. vimdiff arac�n� se�mek i�in �u komutu kullanabilirsiniz:

	$ git config --global merge.tool vimdiff

Git kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge ve opendiff ara�lar�n� kabul eder. Dilerseniz �zel bir ara� i�in de ayarlamalar yapabilirsiniz (bununla ilgili daha fazla bilgi i�in bkz. 7. B�l�m).

### Ayarlar�n�z� G�zden Ge�irmek ###

Ayarlar�n�z� g�zden ge�irmek isterseniz, Git'in bulabildi�i b�t�n ayarlar� listelemek i�in `git config --list` komutunu kullanabilirsiniz.

	$ git config --list
	user.name=Scott Chacon
	user.email=schacon@gmail.com
	color.status=auto
	color.branch=auto
	color.interactive=auto
	color.diff=auto
	...

Bir ayar� birden �ok kez g�rebilirsiniz; bunun nedeni Git'in ayn� ayar� de�i�ik dosyalardan (�rne�in `etc/gitconfig` ve `~/.gitconfig`'den) okumu� olmas�d�r. Bu durumda Git g�rd��� her bir tekil ayar i�in en son buldu�u de�eri kullan�r.

`git config {ayar}` komutunu kullanarak Git'ten bir ayar�n de�erini g�r�nt�lemesini de isteyebilirsiniz:

	$ git config user.name
	Scott Chacon

## Yard�m Almak ##

Git'i kullan�rken yard�ma ihtiyac�n�z olursa, herhangi bir Git komutunun yard�m k�lavuzu sayfas�n� (_manpage_) �� de�i�ik bi�imde g�r�nt�leyebilirsiniz:

	$ git help <eylem>
	$ git <eylem> --help
	$ man git-<eylem>

�rne�in, config komutu i�in k�lavuzu sayfas�n� g�r�nt�lemek i�in �u komutu �al��t�rabilirsiniz:

	$ git help config

Bu komutlar�n g�zel taraf� onlara her an, a� ba�lant�n�z olmasa bile ula�abiliyor olman�zd�r. E�er k�lavuz sayfalar� ve bu kitap yeterli olmazsa ve ki�isel yard�ma ihtiya� duyacak olursan�z, Freenode IRC sunucusundaki (irc.freenode.net) `#git` ya da `#github` kanallar�na ba�lanmay� deneyebilirsiniz. Bu kanallar Git hakk�nda derin bilgiye sahip y�zlerce ki�i taraf�ndan d�zenli olarak ziyaret edilmektedir.

## �zet ##

Art�k Git'in ne oldu�u ve kullanm�� olabilece�iniz MSKS'den hangi a��lardan farkl� oldu�u konusunda temel bilgilere sahipsiniz. Ayr�ca sisteminizde, sizin kimlik bilgilerinize g�re ayarlanm�� bir Git kurulumu bulunuyor. �imdi Git'in temellerini ��renme zaman�.
