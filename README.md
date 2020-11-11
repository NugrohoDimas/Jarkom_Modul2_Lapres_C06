# Jarkom_Modul2_Lapres_C06

### Soal 1. Membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw <br>
Sebelumnya lakukan konfigurasi seperti pada modul pengenalan uml. <br>
Langkah pertama adalah konfigurasi pada file /etc/bind/named.conf.local sesuai dengan gambar dibawah. 
![soal1conflocal](https://user-images.githubusercontent.com/49650266/98755261-ce028580-23fa-11eb-8e9c-2f8e2ac4fe6c.png) <br>
Lalu buat sebuah direktori dengan nama jarkom. <br>
```diff
nano /etc/bind/jarkom
```
Setelahnya salin db.local ke semeruC06.pw .
```diff
cp /etc/bind/db.local /etc/bind/jarkom/semeruC06.pw
```
Lalu konfigurasi pada file /etc/bind/jarkom/semeruC06.pw .<br>
![soal1semeruC06](https://user-images.githubusercontent.com/49650266/98757570-66026e00-23ff-11eb-9139-bb9d1bf40fb8.png) <br>
Lalu kita konfigurasi beberapa file di uml GRESIK. <br>
Comment (tambahkan # di depan) 2 nameserver di atas. <br>
![soal1resolv](https://user-images.githubusercontent.com/49650266/98757869-f9d43a00-23ff-11eb-96c8-53bfb83a2c8b.png) <br>
Lalu coba ping semeruC06.pw. <br>
![pingsoal1](https://user-images.githubusercontent.com/49650266/98758108-8da60600-2400-11eb-8258-b43f22392401.png)

### Soal 2. Buat Alias www.semeruC06.pw
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw sesuai gambar di bawah. <br>
![soal2semeruc06](https://user-images.githubusercontent.com/49650266/98758356-2a68a380-2401-11eb-9007-55678723f426.png) <br>
Coba ping pada UML Gresik. <br>
![soal2ping](https://user-images.githubusercontent.com/49650266/98758419-54ba6100-2401-11eb-8a9f-fc99fb38fae1.png) <br>

### Soal 3. Membuat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw seperti gambar dibawah. <br>
![soal3semeruc06](https://user-images.githubusercontent.com/49650266/98758605-c1cdf680-2401-11eb-9543-d95cee120ba9.png) <br>
Coba ping www.penanjakan.semeruC06.pw pada UML GRESIK. <br>
![soal3ping](https://user-images.githubusercontent.com/49650266/98758705-00fc4780-2402-11eb-9700-1c08ac9c99fb.png) <br>

### Soal 4. Membuat reverse domain untuk domain utama.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local. <br>
![soal4conflocal](https://user-images.githubusercontent.com/49650266/98761348-cd242080-2407-11eb-86ba-8875f011f3a6.png) <br>
Salin file /etc/bind/db.local ke /etc/bind/jarkom/77.151.10.in-addr.arpa dan konfigurasi file tersebut sesuai gambar di bawah. <br>
![soal4arpa](https://user-images.githubusercontent.com/49650266/98761471-12e0e900-2408-11eb-9a82-79bada8e0f6e.png) <br>
Lalu pindah ke UML GRESIK dan konfigurasi file /etc/resolv.conf. Pada file tersebut uncomment nameserver dua atas dan comment yg IP Malang. <br>
Setelah melakukan konfigurasi update dan install dnsutils. <br>
```diff
apt-get update 
apt-get install dnsutils
```
Lalu konfigurasi file /etc/resolv.conf, comment nameserver dua atas, uncomment yg IP Malang. <br>
Setelah itu jalankan host -t PTR 10.151.77.58. <br>
![soal4ptr](https://user-images.githubusercontent.com/49650266/98761797-ccd85500-2408-11eb-8b47-4bd862804ef2.png)

### Soal 5. Membuat DNS Server Slave pada MOJOKERTO.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local seperti gambar dibawah. <br>
![soal5conflocal](https://user-images.githubusercontent.com/49650266/98762480-4b81c200-240a-11eb-9e48-1de362d06079.png) <br>
Lalu pindah ke UML MOJOKERTO, update UML dan install aplikasi bind9. Konfigurasi file /etc/bind/named.conf.local seperti gambar dibawah. <br>
![soal5conflocalmojo](https://user-images.githubusercontent.com/49650266/98762632-ad422c00-240a-11eb-8a77-d5d9cd3f25d0.png) <br>
Sebelum melakukan ping pada UML GRESIK, hentikan dulu bind9 di UML MALANG. <br>
![soal5ping](https://user-images.githubusercontent.com/49650266/98762731-e4184200-240a-11eb-9f0f-87e0fe1da766.png) <br>

### Soal 6. Membuat subdomain gunung.semeruC06.pw delegasi pada MOJOKERTO dan mengarah ke IP PROBOLINGGO.
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw seperti gambar dibawah. <br>
```diff
@	IN	SOA	semeruC06.pw. root.semeruC06.pw. (
			2020110901		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		semeruC06.pw.
@		IN	A		10.151.77.58	; IP MALANG
www		IN	CNAME	semeruC06.pw.
penanjakan	IN	A		10.151.77.60	; IP PROBO
ns1		IN	A		10.151.77.59	; IP MOJO
gunung	IN	NS		ns1
```
Konfigurasi file /etc/bind/named.conf.options dengan melakukan comment dnssec-validation auto menambahkan allow-query{any;}; <br>
Lalu tambahkan konfigurasi file /etc/bind/named.conf.local seperti dibawah ini. <br>
```diff
zone "semeruC06.pw" {
    type master;
    file "/etc/bind/jarkom/semeruC06.pw";
    allow-transfer { 10.151.77.59; }; // IP MOJOKERTO
};
```
Pindah ke UML MOJOKERTO. <br>
Konfigurasi file /etc/bind/named.conf.options dengan melakukan comment dnssec-validation auto dan menambahkan allow-query{any;}; <br>
Setelah itu menambahkan konfigurasi file /etc/bind/named.conf.local <br>
```diff
zone "semeruC06.pw" {
    type master;
    file "/etc/bind/delegasi/semeruC06.pw";
    allow-transfer { any; }; // IP MOJOKERTO
};
```
Lalu buat folder delegasimkdir. <br>
```diff
mkdir /etc/bind/delegasi
```
Setelah itu salin file db.local ke /delegasi/gunung.semeruC06.pw. <br>
```diff
cp /etc/bind/db.local /etc/bind/delegasi/gunung.semeruC06.pw
```
Konfigurasi file gunung.semeruC06.pw seperti dibawah ini. <br>
```diff
@	IN	SOA	gunung.semeruC06.pw. root.gunung.semeruC06.pw. (
			2020110903		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semeruC06.pw.
@		IN	A		10.151.77.60	; IP PROBO
```
Lalu melakukan ping di UML GRESIK <br>
![soal6ping](https://user-images.githubusercontent.com/49650266/98763686-23e02900-240d-11eb-8910-d5c3376e15a2.png)

### Soal 7. Membuat subdomain naik.gunung.semeruC06.pw diarahkan ke Probo.
Konfigurasi pada UML MOJOKERTO di file /etc/bind/delegasi/gunung.semeruC06.pw .<br>
```diff
@	IN	SOA	gunung.semeruC06.pw. root.gunung.semeruC06.pw. (
			2020110903		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semeruC06.pw.
@		IN	A		10.151.77.60	; IP PROBO
naik		IN	A		10.151.77.60	; IP PROBO
```
Lalu coba ping dari UML GRESIK. <br>
![soal7ping](https://user-images.githubusercontent.com/49650266/98764476-70783400-240e-11eb-9c3f-990808b9eea1.png) <br>

### Soal 8. Membuat domain semeruC06.pw DocumentRoot /var/www/semeruC06.pw dapat diakses pada semeruC06.pw/index.php/home.
Konfigurasi pada UML PROBOLINGGO :
* Membuat file semeruC06.pw.conf di sites-available, salin dari default
* Tambahkan ServerName semeruC06.pw pada file tersebut
* Ubah DocumentRoot seperti yg diminta
* Download file semru.xip, unzip, copy ke /var/ww/semeruC06.pw <br>
Berikut ini konfigurasinya : <br>
![soal8conf](https://user-images.githubusercontent.com/49650266/98770482-ef706b00-2414-11eb-9ce0-df48440e3e4b.png) <br>
Dan ini merupakan hasil yang diminta : <br>
![soal8hasil](https://user-images.githubusercontent.com/49650266/98770485-f0a19800-2414-11eb-8101-efab536959fc.png) <br>

### Soal 9. Mengaktifkan mod rewrite semeruC06.pw/index.php/home -> semeruC06.pw/home

Langkah menyelesaikan soal 9 :
* Nyalakan mod rewrite dengan a2enmod mod_rewrite
* Konfigurasi semeruC06 agar bisa pake mod_rewrite kyk di modul
* Buat .htaccess di /var/www/semeruC06.pw dengan rule berikut :
![soal9conf](https://user-images.githubusercontent.com/49650266/98770859-bf759780-2415-11eb-8849-4b27874807c2.png) <br>
Dibawah ini merupakan hasilnya :
![soal9hasil](https://user-images.githubusercontent.com/49650266/98770849-b97fb680-2415-11eb-9457-f6180e88d12c.png) <br>

### Soal 10. 





















