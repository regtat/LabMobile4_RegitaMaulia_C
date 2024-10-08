Nama  : Regita Maulia Gani N.R.

NIM   : H1D022021

Shift : C

## 1. Proses Login
![Halaman Pertama - Login](https://github.com/user-attachments/assets/4fac8523-754a-4816-b1f0-5b460273a3e2)

Jika belum memiliki akun, maka harus registrasi akun dengan klik 'Registrasi'
## a. Registrasi
Pada halaman Registrasi fungsinya untuk membuat akun. Halaman ini menggunakan StatefulWidget yang mendukung penyimpanan informasi (inputan user) serta validasi. TextFormField untuk memasukkan nama, email, password, dan konfirmasi password seperti berikut. 
![Registrasi](https://github.com/user-attachments/assets/5a4d8ca9-1be2-42de-9034-c91d5f648a92)

Tiap field terdapat validasi agar memenuhi kriteria tertentu seperti berikut:
![Validasi inputan user](https://github.com/user-attachments/assets/02d69665-2a92-448b-8ad6-6c170320e0f8)

Ketika user klik tombol registrasi, method _submit() memvalidasi inputan user berikut. Jika valid, data akan dikirim ke backend melalui RegistrasiBloc.registrasi()

![Input Usn & Pwd](https://github.com/user-attachments/assets/4b1ed238-5a5c-428f-9df3-88fdd7f21bd3)
## b. PopUP
Jika registrasi berhasil, PopUp Sukses akan muncul seperti berikut:
![PopUp Berhasil](https://github.com/user-attachments/assets/ce9bd59a-1d35-4757-be65-a9f1ab843c44)

PopUp tersebut menggunakan showDialog

void _submit() {

    _formKey.currentState!.save();
    
    setState(() {
    
      _isLoading = true;
      
    });
    
    RegistrasiBloc.registrasi(
    
....

.then((value) {

      showDialog(
      
          ....
          
          builder: (BuildContext context) => SuccessDialog(
          
                description: "Registrasi berhasil, silahkan login",
                
                okClick: () {
                
                  Navigator.pop(context);
                  
                },
                
              ));
              
    }, onError: (error) {
    
      showDialog(
      
          ....
          
          builder: (BuildContext context) => const WarningDialog(
          
                description: "Registrasi gagal, silahkan coba lagi",
                
              ));
    });
  }
}

## c. Login
Setelah Berhasil Registrasi dan muncul PopUp Sukses, jika klik 'ok' maka akan diarahkan ke halaman login dengan TextFormField untuk memasukkan email dan password. Ketika tombol 'Login' diklik, method _submit() akan memvalidasi inputan. Jika valid, data login dikirim ke backend melalui LoginBloc.login(). Jika login berhasil, token dan ID pengguna akan disimpan menggunakan UserInfo, lalu user diarahkan ke halaman ProdukPage.

![Login - inputan user baru](https://github.com/user-attachments/assets/439c1f94-ad97-4a9e-8041-f8e7d8ea59a1)

Jika tidak berhasil login (akun tidak terdaftar atau inputan tidak sesuai), muncul PopUp seperti berikut, lalu diarahkan kembali ke halaman Login jika klik 'OK'

![Gagal Login](https://github.com/user-attachments/assets/b18491ee-a3fa-48a9-8498-6c4051e1380e)

## 2. Tambah Data
Setelah berhasil login, user diarahkan ke halaman Produk (ProdukPage). Pada bagian atas (header) terdapat AppBar dengan title 'List Produk' dan tombol '+' untuk menambahkan produk.

![List Produk Awal](https://github.com/user-attachments/assets/fa555f06-e60b-45ed-a2d0-e472037dd116)
## a. Side Menu
Halaman tersebut dilengkapi dengan Drawer (panel samping (Hamburger Icon)), yang terdapat menu Logout seperti berikut. Jika diklik akan keluar dari akun user dan kembali ke LoginPage

![Drawer, Side Menu, Logout](https://github.com/user-attachments/assets/ca0d2209-dd3f-4d8a-abb3-a3bfa057209e)
## b. CREATE
Saat tombol '+' diklik, user diarahkan ke halaman produk_page yang diatur dalam class ProdukForm. Terdapat tiga TextFormField untuk input kode produk, nama produk, dan harga produk (terdapat validasi field tidak boleh kosong). Ketika tombol SIMPAN diklik, form divalidasi, jika sukses maka fungsi simpan() berjalan, yang akan membuat objek produk baru berdasarkan inputan user dan mengirim ke backend melalui ProdukBloc.addProduk(). 

![Menambahkan data](https://github.com/user-attachments/assets/e79b64d1-fd65-430d-86f0-935bf49b025f)
## b. Data berhasil ditambahkan
Jika berhasil, user diarahkan ke halaman List Produk.

![List Produk Setelah Create](https://github.com/user-attachments/assets/f9893ea8-0710-4cd0-a890-bd3b676e1a7e)

## 3. Detail Produk
Jika salah satu card produk diklik, maka user diarahkan ke halaman ProdukDetail. Menampilkan informasi produk seperti berikut. 

![Detail Produk Buku Sebelum Diedit](https://github.com/user-attachments/assets/9c8d7618-3213-4777-be7d-d9bcb21d020e)

Pada halaman tersebut terdapat tombol edit dan delete

## 4. EDIT
Jika tombol 'EDIT' diklik, user diarahkan ke halaman ProdukForm yang mengambil data berdasarkan ID. Pada saat halaman ini dibuka, method initState() akan dipanggil, menjalankan fungsi isUpdate() yang memeriksa apakah objek produk yang diterima tidak null. 
Inputan user akan dimuat ke dalam kontrol teks yang sesuai menggunakan TextEditingController. Jika tombol 'UBAH' diklik, akan dilakukan validasi semua kolom harus diisi.

![image](https://github.com/user-attachments/assets/051cab88-bbb3-44b9-ad11-978656fbf969)

Jika validasi berhasil, fungsi ubah() akan dijalankan. Dalam fungsi ini, objek produk baru akan dibuat menggunakan ID produk tersebut dan inputan user dari kontrol teks tadi akan diambil untuk diperbarui. ProdukBloc.updateProduk() akan diambil untuk mengirim permintaan update ke backend (API). Jika berhasil, user akan kembali ke halaman List Produk.

![image](https://github.com/user-attachments/assets/13423ada-43eb-4ea7-b229-3fd599850032)

## 5. DELETE
Jika tombol 'DELETE' diklik, fungsi confirmHapus() berjalan dan memunculkan PopUp konfirmasi hapus seperti berikut

![Konfirmasi Hapus](https://github.com/user-attachments/assets/4643a736-7526-4b87-b224-293362985d5b)

Jika user klik 'YA', maka ProdukBloc.deleteProduk() akan dipanggil untuk menghapus produk dari backend. Jika proses delete berhasil, user akan kembali ke halaman List Produk.

![Data Berhasil Dihapus](https://github.com/user-attachments/assets/40804103-3894-4f09-9346-a5079909bcbe)
