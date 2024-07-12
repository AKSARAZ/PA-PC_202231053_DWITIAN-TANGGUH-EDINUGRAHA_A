# PA-PC_202231053_DWITIAN-TANGGUH-EDINUGRAHA_A

## UAS PRAKTIKUM PENGOLAHAN CITRA DIGITAL KELAS A
DWITIAN TANGGUH EDINUGRAHA\
202231053


### Teori Pendukung
- Filtering Citra    : Filtering Citra merupakan prooses mengolah gambar untuk memperbaiki kualitas maupun mengekstrak informasi tertencu dari suatu citra dengan berbagai tujuan seperti penghapusan noise, menghaluskan gambar, dan peningkatan kontras.

- Median Filtering   : adalah suatu teknik pemrosesan gambar yang digunakan untuk menghilangkan noise dengan mempertahankan detail tepi dalam gambar dengan cara mengganti setiap piksel dalam gamabar menggunakan mesian dari piksel-piksel ketetanggannya.

- Mean Filtering     : adalah teknik pemrosesan gambar yang digunakan untuk menghaluskan gambar dan mengurangi noise dengan mengganti piksel dalam gambar dengan nilai rata-rata piksel dari piksel ketetanggaannya

- Library OpenCV     : (Open Source Computer Vision Library) merupakan library open source untuk pemrosesan gambar dan menyediakan berbagai fungsi untuk memproses dan menganalisa gambar.

- matplotlib.pyplot  : modul dalam library matlotlib yang menyediakan interface untuk membuat plot dan visualisasi data dengan sederhana.

- Library Numpy      : merupakan library komputasi ilmiah di python yang menyediakan struktur data array yang kuat, fungsi matematika, dan algoritma untuk operasi numerik.


### Tahapan Menyelesaikan Program

1. Mengimport Library
```python
import cv2
import matplotlib.pyplot as plt
import numpy as np
```

library openCV untuk pemrosesan gambar\
modul matplotlib.pyplot untuk membuat tampilan plot\
library numpy operasi numerik

2. Membaca Image atau Citra
```python
img = cv2.imread("foto_diri.jpg")
```
membaca gambar dari folder menggunakan fungsi cv2.imread dari library OpenCV dan menyimpanya dalam variabel img

3. Membuat Baris dan Kolom
```python
img.shape
```
img.shape digunakan untuk mendapatkan dimensi dari gambar yang telah dibaca menggunakan OpenCV dimana dimensi tersebut adalah jumlah baris, kolom, dan jumlah saluran warna atau channels dari gambar tersebut.

4. Membuat variabel baris dan kolom
```python
[baris, kolom] = img.shape[:2]
```
script tersebut digunakan untuk mengambil hanya dua komponen pertama dari hasil img.shape, yaitu jumlah baris dan kolom dari gambar img

5. Mengkonversi image dalam variabel img dari format BGR ke RGB
```python
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
```
script tersebut digunakan untuk mengubah format warna gambar dari BGR (Blue-Green-Red) ke RGB (Red-Green-Blue) menggunakan fungsi dari pustaka OpenCV (cv2) yang digunakan untuk mengubah format warna gambar dengan parameter img yaitu image dalam variabel img.

6. Membuat Median Filter Menggunakan OpenCV
```python
img_median = img.copy()
img_median_after = cv2.medianBlur(img_median, 11)

fig, axis = plt.subplots(1, 2, figsize = (15, 15))
ax = axis.ravel()

ax[0].imshow(img_median, cmap='gray')
ax[0].set_title('citra asli')

ax[1].imshow(img_median_after, cmap='gray')
ax[1].set_title('after filter median')
plt.show()
```
membuat salinan gambar menggunakan fungsi img.copy() dan menyimpanya ke dalam variabel img_median agar image atau citra asli tidak terpengaruhi dalam proses kemudian filter median diterapkan menggunakan fungsi cv2.medianBlur dengan kernel berukuran 11x11 untuk mengurangi noise pada salinan gambar dan hasil akan ditampilkan dalam sebuah plot menggunakan matplotlib, menampilkan gambar asli sebelah kiri dan gambar yang telah dihaluskan dengan filter median sebelah kanan.

7. Mengkonversi image dari RGB ke GRAY untuk penghitungan mean
```python
img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)
```
gambar dikonversi ke dalam format GRAY dikarenakan dalam penghitungan ketenggaan piksel untuk selanjutnya penghitungan mean ditak mengambil dimensi warna dan akan merubah image menjadi tipe float.

8. Memproses Ketetanggaan piksel untuk mencari mean secara manual
```python
img_mean = img_gray.copy().astype(float)

m1, n1 = img_mean.shape
img_mean_after = np.empty([m1, n1])

print("shape image median : ", img_mean.shape)
print("shape image median after : ", img_mean_after.shape)

print("m1 : ", m1)
print("n1 : ", n1)
print()
```
Program ini dimulai dengan mengambil citra dalam format grayscale dari variabel img_gray dan membuat salinannya ke dalam variabel img_mean yang diubah ke tipe data float untuk memproses filter rata-rata. Langkah ini akan memastikan bahwa perubahan nilai piksel dapat dilakukan secara tepat dalam operasi matematis selanjutnya. Selanjutnya, variabel m1 dan n1 diinisialisasi dengan dimensi citra menggunakan img_mean.shape, untuk mengambil jumlah baris dan kolom dari citra tersebut. Kemudian, sebuah array kosong img_mean_after dibuat dengan dimensi yang sama dengan citra img_mean untuk menyimpan hasil dari filter rata-rata yang akan diterapkan. Informasi tentang dimensi citra sebelum dan setelah proses juga dicetak menggunakan perintah print, yang berguna untuk memantau proses pengolahan data. Dengan demikian, program ini mempersiapkan citra untuk proses filter rata-rata, menginisialisasi variabel, dan mempersiapkan struktur data yang diperlukan untuk menyimpan hasil akhir dari operasi pengolahan citra ini.

9. Membuat filter rata-rata secara manual
```python
for baris in range (0, m1-1) :
    for kolom in range (0, n1-1) :
        a1 = baris
        b1 = kolom
        jumlah = img_mean[a1-1, b1-1] + img_mean[a1-1, b1] + img_mean[a1-1, b1+1] +\
            img_mean[a1, b1-1] + img_mean[a1, b1] + img_mean[a1, b1+1] +\
            img_mean[a1+1, b1-1] + img_mean[a1+1, b1] + img_mean[a1+1, b1+1]
        img_mean_after[a1, b1] = 1/9 * jumlah
```
script tersebut digunakan untuk menerapkan filter rata-rata atau mean filter secara manual pada citra grayscale. Dengan menggunakan nested loop, program mengiterasi setiap piksel dalam citra img_mean yang merupakan salinan dari citra grayscale asli. Setiap piksel diproses dengan menghitung nilai rata-rata dari nilai piksel di sekitarnya menggunakan kernel 3x3. Proses ini dilakukan dengan cara menjumlahkan nilai piksel dari baris dan kolom sebelumnya, saat ini, dan berikutnya, kemudian hasilnya dibagi dengan 9 untuk mendapatkan nilai rata-ratanya. Hasil dari proses ini disimpan dalam matriks img_mean_after, yang akhirnya akan menampilkan gambar yang telah dihaluskan menggunakan filter rata-rata.

10. Menampilkan image asli dan image setelah menggunakan filter mean
```python
fig, axis = plt.subplots(1, 2, figsize = (15, 15))
ax = axis.ravel()

ax[0].imshow(img_mean, cmap='gray')
ax[0].set_title('Original Image')

ax[1].imshow(img_mean_after, cmap='gray')
ax[1].set_title('Mean Filtered Image')
plt.show()
```
script tersebut akan menampilkan gambar img_mean disebelah kiri dengan format cmap gray untuk skema warna grayscale dengan title Original Image dan menampilkan gambar img_mean_after disebelah kanan dengan format cmap gray juga untuk skema warna grayscale dengan title Mean Filtered Image menggunakan fungssi plot dari matplotlib


### Jurnal Terkait
1. https://elib.unikom.ac.id/download.php?id=6964#:~:text=Filtering%20citra%20merupakan%20salah%20satu,linear%20maupun%20secara%20non%2Dlinear.
2. https://media.neliti.com/media/publications/104120-ID-analisis-perbandingan-metode-2d-median-f.pdf
3. https://publikasiilmiah.ums.ac.id/handle/11617/4925

