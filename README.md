## Survival and Cohort Analysis in Retail
kali ini saya akan share hasil pembelajaran saya dari workshop yang di selenggarakan oleh **PACMAN AI** bersama Ka SABRINA selaku mentor saya, mengenai survival dan cohort analysis pada retail.

Saya ucapkan Terimakasih pada pihak PACMAN AI dan KA SABRINA atas kesempatan dan ilmu nya yang sangat bermanfaat bagi saya
## Dataset dan tools
#### Dataset
dataset yang digunakan didapat dari kaggle https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci
####  Attribute dataset
dataset terdiri dari 8 kolom atribut yang terdiri dari Country, Customer ID, Description, Invoice Date, Stock Code, Invoice, Price, dan Quantity
#### Aplikasi yang digunakan
Menggunakan microsoft excel untuk analisis datanya

#Survival Analysis

### Tahapan Survival Analisa : Mencari Rentang Waktu pada kolom ***first_invoice_date***
##### 1. Membuat sheet baru untuk lookup first_invoice_date sebagai data referensi
membuat sheet baru dengan nama first_invoice_lookup, kemudian membuat attribute dengan nama ***customer ID*** yang didapat dari sheet online_retail_II_germany_and_ireland dan attribute* **first_invoice_date***

[![sheet](https://user-images.githubusercontent.com/55653494/198643057-40e5de3e-53ed-4fa6-8ce3-ba2ade085f37.png "sheet")](http:/https://user-images.githubusercontent.com/55653494/198643057-40e5de3e-53ed-4fa6-8ce3-ba2ade085f37.png/ "sheet")

##### 2. menggunakan fungsi MINIFS untuk mendapatkan data pada attribute ***first_invoice_date***

![image](https://user-images.githubusercontent.com/55653494/198646036-3aeac1ab-b849-43f5-a497-7d711869c219.png)

menggunakan fungsi MINIFS dengan ketentuan :
- seluruh kolom ** *Invoice date * ** sebagai minimum range nya
- seluruh kolom ** *Customer ID* ** sebagai criteria range
- data pada kolom  ** *Customer ID* ** sebagai criteria

![image](https://user-images.githubusercontent.com/55653494/198643950-b77a99be-084b-42cc-b39c-a44adfed81c2.png)

kemudian dari hasil value yang didapat ubah kedalam format tanggal

![image](https://user-images.githubusercontent.com/55653494/198647023-9ca9c560-bfdb-4eca-b3c5-a375539083e9.png)

lakukan hal yang sama untuk data yang selanjutnya pada kolom first_invoice_date

sampai sini data referensi first_invoice_date sudah berhasil didapat, langkah selanjutnya yauda memindahkan data referensi ke data yang akan kita olah

##### 3. Memindahkan data referensi ke data yang akan diolah
menggunakan fungsi xlookup
![image](https://user-images.githubusercontent.com/55653494/198655609-eda9377e-1adc-4d4e-927a-a2b8730e2451.png)
dengan ketentuan :
- lookup valuenya yaitu data pada kolom ***Customer ID***
- lookup arraynya yaitu pada sheet yang lain pada data referensi yaitu kolom ***Customer ID***
- lalu return array nya pada sheet yang lain pada data referensi yaitu kolom ***first_invoice_date***

### Tahapan Survival Analisa :  Membuat Observasion Group pada kolom ***month_group* ** dan ***month_age* **
##### 4. Mencari nilai ***month_group* ** dengan mengambil bulan dan tahun dari kolom ***invoice_date***
![image](https://user-images.githubusercontent.com/55653494/198812312-00b36c13-34d1-4556-b93e-a0a228f13cd0.png)
menggunakan fungsi **DATE** dengan ketentuan, value dari Year dan Month merujuk pada data kolom ***invoice_date***

##### 5. Mencari nilai*** month_age*** untuk membentuk group dalam bentuk bulanan
![image](https://user-images.githubusercontent.com/55653494/198812738-5d480eb0-e7ee-4456-bdeb-a3f6c91423ad.png)
menggunakan fungsi **ROUND** dengan ketentuan value1 dikurang value2 dibagi 30. value1 yaitu data kolom ***invoice_date*** dan value2 merujuk pada data pada kolom ***first_invoice_date* **, dan nilai 0 adalah tidak ada digit dibelakang koma

### Tahapan Survival Analisa : Membuat summary dengan menggunakan table pivot
##### 6. buat table pivot berdasarkan dataset utama, dengan mengambil kolom ***month_group*** sebagai kolom dan ***month_age*** sebagai baris attribute nya dan nilai tablenya yaitu pada kolom ***Customer ID***
![image](https://user-images.githubusercontent.com/55653494/198813633-94dfe4c9-2c25-4718-8e3c-fa1aa9ef4ec4.png)
##### 7. membuat sheet baru untuk menghitung user profile
pada langkah ini duplikasi nilai table dari pivot sebelumnya, kemudian beri label untuk tiap table tersebut yaitu user profile dan number of user loss.
![image](https://user-images.githubusercontent.com/55653494/198815108-63d010e6-b54f-4927-8d81-3d8be64a4374.png)
<br>
untuk mendapatan nilai user profie, cara nya yaitu data grand total dikurangi data tiap bulan. kemudian lakukan hal yang sama untuk tiap datanya
![image](https://user-images.githubusercontent.com/55653494/198815175-8ea35bd7-79c0-456a-978e-8e6e804a9ed0.png)
<br>
maka nanti hasilnya akan seperti ini untuk table user profile nya
![image](https://user-images.githubusercontent.com/55653494/198816097-4d6abbbf-929d-47b1-85c4-f787a51e606c.png)

##### 8. Menghitung Retention Rate per age
Retention rate merupakan persentase customer yang terus berlangganan pada Anda dibandingkan dengan persentase customer yang berhenti menjadi langganan.

ketika membuat kalkulasi customer retention rate, kita tidak perlu menghitung berapa jumlah customer baru yang didapatkan dalam satu kurun waktu tersebut.

Jadi, kita membutuhkan jumlah customer baru tersebut untuk dikurangi total customer yang ada di akhir kurun waktu. Kemudian, kita membagi selisihnya dengan jumlah customer di awal kurun waktu. Terakhir, kita mengalikannya dengan 100 untuk menemukan angka retention rate dalam persentase.

berikut ini merupakan cara mendapatkan nilai retention ratenya:
duplikasi tabel lagi kemudian ubah grand total menjadi bernilai 1. kemudian untuk mendapatkan nilai retention rate caranya, nilai pada data table user profile dibagi dengan grand total
![image](https://user-images.githubusercontent.com/55653494/198816406-52087d90-77bd-4df8-8772-b4a3945dba11.png)
<br>
maka nanti hasil retention rate per age akan menjadi seperti ini
![image](https://user-images.githubusercontent.com/55653494/198816474-eba45f92-169d-4091-bcd9-56463d379587.png)

##### 8. Menghitung Retention Rate through period
perhitugan Retention Rate through period menghitung data berdasarkan dari periode sebelumnya, berbeda dengan perhitungan retention rate per age, yang melakukan perhitungan berdasarkan grand total.

untuk cara mendapatkan nilai Retention Rate through period caranya yaitu nilai pada data retention per age * dengan bulan sebelumnya (untuk data bulan pertama dikali dengan 100%)
![image](https://user-images.githubusercontent.com/55653494/198818016-01fb0263-8884-494c-b13f-0e832ba0793a.png)

setelah itu kemudian ubah kolom grand total pada retention Rate through period menjadi First_Transaction dan ubah nilai datanya menjadi 100%

maka nanti hasilnya akan menjadi seperti ini :
![image](https://user-images.githubusercontent.com/55653494/198818597-94488597-b007-4873-88f8-de74fb35e28e.png)

#Cohort Analysis
Cohort analysis berfokus pada survival anlysis per group. untuk contoh kasus kali ini group yang akan dipilih berdasarkan pada country yaitu ada data EIRE (Ireland / Irlandia) dan Germany (jerman). tujuan nya yaitu untuk mengetahui distribusi data  customer ID dari data country ini seperti apa.

## Tahapan Cohort Analysis
##### 9. Survival Analysis berdasarkan country irland dan Germany
untuk mendapatkan data customer berdasarkan negara Irlandia, maka kita terlebih dahulu harus melakukan filter pada pivot. caranya yaitu pada sheet **PIVOT** duplikasi table dan ubah filter nya menjadi country = EIRE
![image](https://user-images.githubusercontent.com/55653494/198819417-1130a18b-75f2-440d-beeb-03e4f3099ea7.png)

kemudian duplikasi sheet survival analysis yang telah kita lakukan sebelumnya dan beri nama sheet menjadi **Analysis_Ireland** dan copy data pada **table pivot ireland** ke sheet **Analysis_ireland** table **num_of_user_loss**
![image](https://user-images.githubusercontent.com/55653494/198819561-213fd2f5-afce-485d-a574-88743ff8ca71.png)

seluruh table untuk survival analysis pada sheet Analysis_ireland akan otomatis terbentuk sendiri, karna kita telah membuat formulanya terlebih dahulu

***untuk survival analysis ireland sudah berhasil terbuat, lakukan langkah ini juga untuk analisa Germany***

##### 10.Membuat Cohort Analysis
buat sheet baru kemudian copy paste table **Retention % throught period** pada masing-masing analysis (sheet Analysis_All, Analysis_Ireland, Analsis_Germany)
![image](https://user-images.githubusercontent.com/55653494/198820500-b2bfbed0-0561-481b-95a8-b37b3da34a33.png)

**PENJELASAN**
Dari gambar diatas dapat disimpulkan bahwa untuk negara GERMANY jumlah customer menurun drastis tiap periode dibandingkan dengan negara EIRE (Irlandia). untuk bulan pertama, negara GERMANY kehilangan 18% customer dibandingkan dengan negara IRLANDIA yang hanya 1% saja.

adapun berbagai faktor yang menyebabkan hilangnya pelanggan, diantaranya seperti :
- Respons Penjual Lambat.
- Penawaran Harga Tidak Sesuai.
- Kualitas Produk yang Buruk.
- Proses Belanja Tidak Efisien.
- Tidak Ada Inovasi.
- ataupun campign yang kurang baik

Chart
![image](https://user-images.githubusercontent.com/55653494/198821123-7e3aecd6-6637-4e64-af6d-c64588bb5baa.png)
