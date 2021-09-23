# Laporan Resmi Modul 1 Jarkom 2021


**1. Sebutkan webserver yang digunakan pada "ichimarumaru.tech"!**  
cara pengerjaan :
```
http.request and http.host eq "ichimarumaru.tech"
```
perintah diatas untuk mendapatkan packet yang berhubungan dengan ichimarumaru.tect  
  
![image](https://user-images.githubusercontent.com/55046884/134530664-a36595a3-3f95-4c7f-b737-6e56092cd689.png)  

setelah mendapatkan packet yang difilter maka follow -> tcp stream  
  
![image](https://user-images.githubusercontent.com/55046884/134531670-5d2f3ad9-5115-4976-a69c-cd2dd9c64eef.png)  
begitu juga dengan packet ke 2  
  
![image](https://user-images.githubusercontent.com/55046884/134531785-10ef1282-0c99-41b4-80a9-164eb0ecde83.png)  

jawaban : Nginx/1.18.0 (Ubuntu)

**2. Temukan paket dari web-web yang menggunakan basic authentication method!**  
```
http.authbasic
```
![image](https://user-images.githubusercontent.com/55046884/134533001-486318ef-ca39-4961-8e83-b7dd36ff0247.png)  

**3. Ikuti perintah di basic.ichimarumaru.tech! Username dan password bisa didapatkan dari file .pcapng!**  
```
http.host == basic.ichimarumaru.tech
```
![image](https://user-images.githubusercontent.com/55046884/134533211-6800ab55-2d76-4efd-bd3e-9aa2d5b6d612.png)  

follow -> tcp stream  pada packet favicon.ico  
![image](https://user-images.githubusercontent.com/55046884/134533388-888a105b-70bf-40f3-a9f1-434f3395152f.png)  
karena menggunakan basic authorization maka user dan password menggunakan base64. Maka tinggal decode ke https://www.base64decode.org/  

hasil decode :  kuncimenujulautan:tQKEJFbgNGC1NCZlWAOjhyCOm6o3xEbPkJhTciZN  
user : kuncimenujulautan  
pass : tQKEJFbgNGC1NCZlWAOjhyCOm6o3xEbPkJhTciZN  

tinggal login deh :)  
![image](https://user-images.githubusercontent.com/55046884/134533762-82847ec6-0113-4fda-b16b-c8550202b1a2.png)  


**4. Temukan paket mysql yang mengandung perintah query select!**  
```
mysql.command == 3 and frame matches "SELECT"
```
command diatas untuk mendapatkan mysql yang melakukan command SELECT  
![image](https://user-images.githubusercontent.com/55046884/134534386-7f9474ae-3eaa-412e-936e-ad4bd3a7aeb5.png)  

**5. Login ke portal.ichimarumaru.tech kemudian ikuti perintahnya! Username dan password bisa didapat dari query insert pada table users dari file .pcap!**  
```
mysql.command == 3 and frame matches "INSERT"
```
command diatas untuk mendapatkan mysql yang melakukan command INSERT  
![image](https://user-images.githubusercontent.com/55046884/134534508-93b5e02f-f049-4f46-8f81-f9cbd0a6547b.png)  

kemudian follow -> tcp stream untuk mendapatkan user dan passwordnya  
![image](https://user-images.githubusercontent.com/55046884/134534660-ba4aa202-6012-4bb0-8177-9c79a1e8f12c.png)  
Ketemu user = akakanomi dan password = pemisah4lautan  

tinggal login deh :)  
![image](https://user-images.githubusercontent.com/55046884/134534711-68fb133e-48c1-41d3-8717-b2c72aba9877.png)  


**6. Cari username dan password ketika melakukan login ke FTP Server!**  
```
ftp.request.command == USER or ftp.request.command == PASS
```
command diatas untuk memfilter proses FTP yang menginput user dan pass pada prosesnya  
![image](https://user-images.githubusercontent.com/55046884/134534930-2e9847b4-4e15-4dac-9f72-5f5c5c81e8b4.png)  

**7. Ada 500 file zip yang disimpan ke FTP Server dengan nama 0.zip, 1.zip, 2.zip, ..., 499.zip. Simpan dan Buka file pdf tersebut. (Hint = nama pdf-nya "Real.pdf")**  
karena udah dikasih nama filenya tinggal filter berdasarkan namanya aja :)  
```
frame contains Real.pdf
```
![image](https://user-images.githubusercontent.com/55046884/134535062-ca52d601-3541-4729-856a-da4223a397e6.png)
setelah itu follow -> tcp stream  
file zip yang dicari ada di stream 128. kemudian save as raw dan kasih nama sesuai yang diinginkan. disini saya memberikan nama no7.zip  
![image](https://user-images.githubusercontent.com/55046884/134535341-62f2bd2a-7060-431c-8785-958b61f6e984.png)  
extract zip tersebut  
![image](https://user-images.githubusercontent.com/55046884/134535401-8541089f-c430-46bc-9fd5-1dd2f7111dda.png)


**8. Cari paket yang menunjukan pengambilan file dari FTP tersebut!**  
```
ftp.request.command == STOR
```
![image](https://user-images.githubusercontent.com/55046884/134535854-6f8be435-9e4c-4a8d-9c4b-da96f59f32ea.png)  

**9. Dari paket-paket yang menuju FTP terdapat inidkasi penyimpanan beberapa file. Salah satunya adalah sebuah file berisi data rahasia dengan nama "secret.zip". Simpan dan buka file tersebut!**  
```
ftp.request.command == STOR and frame contains secret.zip
```
dengan command diatas akan memfilter ftp yang melakukan penyimpanan file secret.zip  
![image](https://user-images.githubusercontent.com/55046884/134536063-7576dac9-e746-4e3f-85de-b1ccf5b8d001.png)  

sesuai dengan file signature dari zip (depannya PK) maka naikkan filter sampai ke stream 10 dimana disitu ada file zip  
![image](https://user-images.githubusercontent.com/55046884/134536315-99a263d8-bb89-490c-ae63-87d184976b67.png)  

show data as raw -> save as -> beri nama  
![image](https://user-images.githubusercontent.com/55046884/134536443-796b52c7-523d-442d-b1fc-55ebd2a4fa40.png)  


**10. Selain itu terdapat "history.txt" yang kemungkinan berisi history bash server tersebut! Gunakan isi dari "history.txt" untuk menemukan password untuk membuka file rahasia yang ada di "secret.zip"!**  
dari file no 9 diperlukan password untuk membuka file zip
![image](https://user-images.githubusercontent.com/55046884/134536559-c9ed9ea8-7816-4e9a-aeb5-0547165b0190.png)  
di salah satu packet ada history.txt  
![image](https://user-images.githubusercontent.com/55046884/134536646-41bfecad-4e18-4792-85a2-1c8ea98acd0f.png)  
dan waktu saya naikkan stream ketemu isi dari history.txt  
![image](https://user-images.githubusercontent.com/55046884/134536741-2f6c5a41-26f6-4eb0-ac97-78982d24fb7b.png)  
tinggal masukkan passnya deh :)  
![image](https://user-images.githubusercontent.com/55046884/134536803-994db093-7bac-48d1-baf6-5a72bfebfa82.png)  

**11. Filter sehingga wireshark hanya mengambil paket yang berasal dari port 80!**  
```
tcp.srcport == 80
```
**12. Filter sehingga wireshark hanya mengambil paket yang mengandung port 21!**  
```
tcp.port == 21
```
**13. Filter sehingga wireshark hanya menampilkan paket yang menuju port 443!**  
```
tcp.dstport == 443
```
**14. Filter sehingga wireshark hanya mengambil paket yang tujuannya ke kemenag.go.id!**  
```
ping kemenag.go.id
ip.dst == 103.7.13.247
```
**15. Filter sehingga wireshark hanya mengambil paket yang berasal dari ip kalian!**  
```
ip.src == 192.168.1.6
```
