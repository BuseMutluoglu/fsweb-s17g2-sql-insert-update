# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
    insert into yazar (yazarad, yazarsoyad) values ('KEMAL', 'UYUMAZ')


    2) Biyografi türünü tür tablosuna ekleyiniz.
    insert into tur (turadi) values ('Biyografi')


    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.
    INSERT INTO ogrenci (ograd, ogrsoyad, sinif, cinsiyet) VALUES
    ('Çağlar', 'Üzümcü', '10A', 'E'),
    ('Leyla', 'Alagöz', '9B', 'K'),
    ('Ayşe', 'Bektaş', '11C', 'K');


    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
    INSERT INTO yazar (yazarad, yazarsoyad)
    SELECT ograd, ogrsoyad
    FROM ogrenci
    ORDER BY rand()
    LIMIT 1;

    INSERT INTO yazar (yazarad, yazarsoyad)
    SELECT ograd, ogrsoyad
    FROM ogrenci
    where rand()
    LIMIT 1;

    INSERT INTO yazar (yazarad, yazarsoyad)
    SELECT ograd, ogrsoyad
    FROM ogrenci
    where rand()
    LIMIT 1;
    select @@identity

    INSERT INTO yazar (yazarad, yazarsoyad)

SELECT ograd, ogrsoyad
FROM ogrenci
where rand()
LIMIT 1;
select LAST_INSERT_ID()

    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
    insert into yazar(yazarad,yazarsoyad)
    select ograd, ogrsoyad from ogrenci where ogrno between 10 and 30

    insert into yazar (yazarad,yazarsoyad)
    select ograd, concat (TRIM(ogrsoyad),'-',ogrno) from ogrenci where ogrno between 10 and 30

    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)


    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
    update ogrenci set sinif='10C' where ogrno='3' and sinif='10B'

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
    update ogrenci set sinif='10A' where sinif='9A'

    9) Tüm öğrencilerin puanını 5 puan arttırın.
    update ogrenci set puan=puan+5 where puan is not null
    select * from ogrenci
    update ogrenci set puan=puan+15 ,ograd=CONCAT(ograd,'(*****)')

where ogrno IN (Select o.ogrno from ogrenci as o
Inner Join islem as i On i.ogrno=o.ogrno
Inner Join kitap as k On k.kitapno = i.kitapno
group by o.ogrno
order by Sum(k.sayfasayisi) DESC
Limit 5)

    10) 25 numaralı yazarı silin.
    delete from yazar where yazarno=25

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    select * from ogrenci where dtarih is null

    12) Doğum tarihi null olan öğrencileri silin.
    delete from ogrenci where dtarih is null

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
    update ditap set puan=puan+2 where kitapadi like 'a%'

    14) Kişisel Gelişim isimli bir tür oluşturun.
    insert into tur (turadi) values ('Kişisel Gelişim')

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
    update kitap set turno=(select min(turno) from tur where turadi="Kişisel Gelişim") where kitapadi="Başarı Rehberi";

    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
    create procedure ogrencilistesi as select * from ogrenci;
    exec ogrencilistesi;

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
    CREATE PROCEDURE Ekle(
    IN name VARCHAR(50),
    IN surname VARCHAR(50)
    )
    BEGIN
    INSERT INTO ogrenci(ograd, ogrsoyad)
    VALUES (name, surname);
    END;
    CALL Ekle('Buse','Mutluoglu')


    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
    create procedure sil @id numeric(10) as delete from ogrenci where ogrno=@id;

    exec sil 32

    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
     create procedure sinifdegis @no numeric(10), @sinif nvarchar(25) as update ogrenci set sinif=@sinif where ogrno=@no;

    exec sinifdegis 32, "10A";

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
    create procedure adsoyadbirlesiklistele as select concat(ograd, ogrsoyad) as adsoyad from ogrenci;

    exec adsoyadbirlesiklistele

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.
    drop procedure [procedurler];

    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
     select tur.*,kitap.* from tur, kitap where tur.turno=(select tur.turno from tur where tur.turadi="Dram");

    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
    select kitap.*,yazar.yazarad from kitap,yazar where yazar.yazarno in (select yazar.yazarno from yazar where yazar.yazarad like "E%");

    24) Kitap okumayan öğrencileri listeleyiniz.
    select * from ogrenci,islem where ogrenci.ogrno not in (select islem.ogrno from islem);

    25) Okunmayan kitapları listeleyiniz
    select * from kitap where kitapno not in (select kitapno from islem);


    26) Mayıs ayında okunmayan kitapları listeleyiniz.
