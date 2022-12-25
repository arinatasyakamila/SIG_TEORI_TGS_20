# SIG_TEORI_TGS_20
 Calculating Raster Area 
 Cara Membuatnya!

1. Buka zip semua file yang diunduh. Di Browser, cari folder yang berisi file batas WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons.shp dan drag-and-drop ke kanvas QGIS.

2. Sekarang cari petak raster penutup dunia ESA_WorldCover_10m_2020_v100_N24_E093_Map.tif dan jatuhkan ke kanvas QGIS.

3. Anda sekarang akan memiliki layer vektor batas dan raster tutupan lahan dimuat di panel Layers. Mari potong raster tutupan lahan ke batas taman nasional. Pergi ke Processing ‣ Toolbox untuk membuka Processing toolbox. Cari dan temukan GDAL ‣ Ekstraksi raster ‣ Klip raster dengan algoritma lapisan topeng. Klik dua kali untuk meluncurkannya.

4. Dalam dialog Clip Raster by Mask Layer, pilih layer ESA_WorldCover_10m_2020_v100_N24_E093_Map sebagai layer Input dan layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai Layer Mask. Masukkan -9999 di Tetapkan nilai nodata yang ditentukan ke bagian pita keluaran.

5. Sekarang buka bagian Advanced Parameters dan pilih High Compression in Profile. Sekarang di bawah Clipped (mask), klik pada ... dan pilih Save To File…. Masukkan nama file sebagai worldcover_cliped.tif. Klik Jalankan.

6. Sekarang layer worldcover_cliped akan dimuat di kanvas QGIS. Klik kanan layer ESA_WorldCover_10m_2020_v100_N24_E093_Map dan pilih Remove Layer…

7. Kedua layer kami hadir dalam Geographic CRS EPSG:4326. CRS ini memiliki satuan derajat dan tidak cocok untuk menghitung luas. Pertama-tama kita harus memproyeksikan ulang layer ke Projected CRS. untuk analisis regional seperti ini, UTM adalah pilihan yang baik untuk proyeksi CRS. Kami akan memproyeksikan ulang layer ke CRS untuk zona UTM lokal. Buka kotak alat Pemrosesan dan cari Vector general ‣ Algoritma lapisan proyeksi ulang. Klik dua kali untuk meluncurkannya.

8. Dalam dialog Reproject Layer, pilih layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons sebagai layer Input, klik tombol Select CRS di bawah Target CRS.

9. Area minat kami termasuk dalam Zona UTM 46N. Cari 46 N dan pilih zona WGS 84 / UTM 46N CRS.

Catatan

Untuk mengetahui zona UTM mana untuk wilayah Anda, lihat Zona UTM Apa Saya di situs web.

10. Di bagian Diproyeksikan ulang, klik ... dan pilih Simpan Ke File…. Masukkan nama sebagai nationalpark_reprojected.gpkg. Klik Jalankan.

11. Sekarang layer nationalpark_reprojected akan dimuat di kanvas. Klik kanan layer WDPA_WDOECM_Apr2022_Publicc_10744_shp-polygons dan pilih Remove Layer… untuk menghapusnya. Sekarang kita akan memproyeksikan ulang layer raster. Di Kotak Alat Pemrosesan, cari dan temukan GDAL ‣ Proyeksi raster ‣ Warp (proyeksi ulang)

12. Dalam dialog Warp (Reproject) pilih worldcover_cliped sebagai Input layer, pilih WGS 84 / UTM zone 46N CRS di Target CRS. Buka Parameter Lanjutan dan pilih Kompresi Tinggi di Profil.

13. Sekarang di bawah Diproyeksikan ulang, klik ... dan pilih Simpan Ke File…. Masukkan nama sebagai worldcover_reprojected.tif. Klik Jalankan.

14. Sekarang lapisan worldcover_reprojected akan dimuat di kanvas, hapus lapisan worldcover_cliped. Mari atur layer proyek ke zona UTM. Klik pada layer mana saja dan pilih Layer CRS ‣ Set Project CRS from Layer.

15. Sekarang proyek CRS akan diperbarui. Mari atur simbologi lapisan raster sesuai nama kelas dan warna kumpulan data ESA WorldCover. Klik kanan pada layer worldcover_reprojected dan klik Properties…

16. Dalam dialog Layer Properties, pilih Symbology. Anda dapat melihat warna Layer divisualisasikan dalam nada putih-hitam. Untuk memperbaikinya, klik Style ‣ Load Style…. Telusuri dan pilih file ESAWorldCover_ColorLegend.qml.

17. Sekarang Anda dapat melihat warna simbol yang diperbarui dan deskripsi kelas. Klik Oke.

18. Perluas layer worldcover_reprojected di panel Layers untuk melihat legenda dengan deskripsi kelas yang benar.

19. Sekarang mari kita hitung luas untuk setiap kelas. Di kotak alat pemrosesan, cari dan temukan alat laporan nilai unik lapisan Raster. Klik dua kali untuk membukanya.

20. Dalam dialog Raster Layer Unique Values Report, pilih layer Input sebagai worldcover_reprojected. Di bawah tabel Unique values klik ... dan pilih Save to File…. Masukkan nama sebagai class_areas. gpkg. Klik Jalankan.

21. Sekarang layer class_areas akan ditambahkan ke panel Layers. Klik kanan pada layer dan klik Open Attribute Table. Kolom m2 memuat luas untuk setiap kelas dalam meter persegi.

22. Mari ubah luas menjadi kilometer persegi. Di Processing Toolbox, cari dan pilih tabel Vektor ‣ Kalkulator Bidang.

23. Dalam dialog Field Calculator, pilih layer class_areas di Layer Input. Masukkan nama Bidang sebagai area_sqkm. Di bidang Hasil, pilih Float. Di jendela Ekspresi, masukkan ekspresi di bawah ini. Ini akan mengubah sqmt menjadi sqkm dan membulatkan hasilnya menjadi 2 tempat desimal. Di bawah Terhitung klik pada ... dan pilih Simpan Ke File… . Masukkan nama sebagai class_area_sqkm.gpkg. Klik Jalankan.

24. Sekarang lapisan class_area_sqkm akan dimuat di kanvas. Buka tabel Atribut dan periksa kolom area_sqkm yang baru ditambahkan. Anda akan melihat bahwa kolom Nilai berisi angka untuk setiap kelas. Agar hasilnya lebih mudah diinterpretasikan, Mari tambahkan juga deskripsi untuk setiap nomor kelas. Deskripsi kelas tersedia di Panduan Pengguna Produk ESA.

25. Buka Field Calculator, dan pilih layer class_areas_sqkm di Input Layer. Masukkan Nama bidang sebagai tutupan lahan, pada jenis bidang Hasil, pilih String. Di jendela Expression masukkan ekspresi di bawah ini. Ekspresi ini menggunakan pernyataan CASE untuk menetapkan nilai berdasarkan beberapa kondisi. Di bawah Terhitung klik pada ... dan pilih Simpan Ke File… . Masukkan nama sebagai class_area_with_landcover.gpkg. Klik Jalankan.

KASUS
KETIKA "nilai" = 10 MAKA 'Tutup pohon'
KETIKA "nilai" = 20 MAKA 'Semak'
KETIKA "nilai" = 30 LALU 'Padang Rumput'
KETIKA "nilai" = 40 MAKA 'Lahan Pertanian'
KETIKA "nilai" = 50 MAKA 'Dibangun'
KETIKA "nilai" = 60 LALU 'Vegetasi gundul / jarang'
KETIKA "nilai" = 70 LALU 'Salju dan Es'
KAPAN "nilai" = 80 MAKA 'Badan air permanen'
KETIKA "nilai" = 90 LALU 'Lahan basah herba'
KETIKA "nilai" = 95 LALU 'Lumut dan lumut'
KETIKA "nilai" = 100 MAKA 'Mangrove'
AKHIR

26. Sekarang lapisan class_area_with_landcover akan dimuat di kanvas. Buka tabel Atribut. Kolom tutupan lahan akan berisi nama tutupan lahan terhadap setiap nilai tutupan lahan.

27. Mari ekspor hasil ini sebagai file excel. Sebelum mengekspor, kami juga akan mengatur tabel dan menghapus kolom yang tidak diinginkan. Di Processing Toolbox, cari dan pilih Vector table ‣ Refactor field.

28. Dalam dialog Refactor Fields, pilih layer class_area_with_landcover di Layer Input. Pilih semua kolom kecuali area_sqkm dan tutupan lahan, lalu klik Hapus kolom yang dipilih.

29. Anda juga dapat mengubah urutan bidang dalam tabel menggunakan tombol Pindahkan Bidang yang Dipilih. Setelah Anda selesai mengedit, klik tombol ... di sebelah Refactored dan pilih Save To File…. Pilih File XLSX (*.xlsx) sebagai formatnya. Masukkan nama file sebagai park_area_by_landcover.xlsx dan klik Save. Dalam dialog Bidang Refaktor, klik Jalankan untuk menerapkan perubahan Anda.

30. Hasilnya akan berupa spreadsheet dengan kolom landcover dan area_sqkm.
