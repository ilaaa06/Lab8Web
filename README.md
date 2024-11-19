# Lab8Web
## Nur Laila (312310149)

## Menjalankan MySQL Server
![image](https://github.com/user-attachments/assets/c18815a7-6548-45a6-99b2-6044b85a497f)   

## Membuat Database
![image](https://github.com/user-attachments/assets/d118d515-4e89-4e14-8a64-c5e203fa9c71)  

## Membuat Table data_barang
![image](https://github.com/user-attachments/assets/a74f8242-1e7b-4365-9cdd-200373b2c544)   

## Menambahkan Data dan Melihat isi Table
![image](https://github.com/user-attachments/assets/35c3d79b-b8ec-40eb-bcda-6ddc6176ede1)   
![image](https://github.com/user-attachments/assets/cf7fc476-7d1e-4041-b395-2ed8918519f8)   

## Membuat Program CRUD 
![image](https://github.com/user-attachments/assets/5cd350a1-05d4-46ab-b85d-971b77844eac)   
![image](https://github.com/user-attachments/assets/6e9c5c16-f498-41fd-be8d-72d0fd0481d8)    

# Membuat file koneksi database
```sh
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
echo "Koneksi ke server gagal.";
die();
} #else echo "Koneksi berhasil";
?>
```
![image](https://github.com/user-attachments/assets/3e93bd23-4950-470a-bfe4-c0bde0745d8b)   
1. **Koneksi Database**: Kode ini mencoba untuk menghubungkan aplikasi PHP ke database MySQL dengan menggunakan `mysqli_connect()`, yang membutuhkan parameter host, username, password, dan nama database.   
2. **Cek Koneksi**: Jika koneksi gagal (misalnya server tidak tersedia atau kredensial salah), pesan "Koneksi ke server gagal." akan ditampilkan dan eksekusi script dihentikan menggunakan `die()`. Jika koneksi berhasil, pesan "Koneksi berhasil" akan ditampilkan.   

Output dari kode ini adalah pesan yang menunjukkan apakah koneksi ke database berhasil atau gagal.   

## Membuat file index untuk menampilkan data (Read) 
```sh
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Barang</title>
    <style>
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        a {
            text-decoration: none;
            color: blue;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <h1>Data Barang</h1>
    <a href="tambah_barang.php">Tambah Barang</a>
    <br><br>
    <table>
        <thead>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Kategori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            <?php
            // Sample data, you should replace it with a database query in a real application
            $data_barang = [
                [
                    "id" => 1,
                    "gambar" => "HP Samsung Android",
                    "nama" => "HP Samsung Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 2000000,
                    "harga_beli" => 2400000,
                    "stok" => 5
                ],
                [
                    "id" => 2,
                    "gambar" => "HP Xiaomi Android",
                    "nama" => "HP Xiaomi Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 1000000,
                    "harga_beli" => 1400000,
                    "stok" => 5
                ],
                [
                    "id" => 3,
                    "gambar" => "HP OPPO Android",
                    "nama" => "HP OPPO Android",
                    "kategori" => "Elektronik",
                    "harga_jual" => 1800000,
                    "harga_beli" => 2300000,
                    "stok" => 5
                ]
            ];

            foreach ($data_barang as $barang) {
                echo "<tr>
                        <td>{$barang['gambar']}</td>
                        <td>{$barang['nama']}</td>
                        <td>{$barang['kategori']}</td>
                        <td>{$barang['harga_jual']}</td>
                        <td>{$barang['harga_beli']}</td>
                        <td>{$barang['stok']}</td>
                        <td>
                            <a href='ubah.php?id={$barang['id']}'>Ubah</a> | 
                            <a href='hapus.php?id={$barang['id']}'>Hapus</a>
                        </td>
                    </tr>";
            }
            ?>
        </tbody>
    </table>
</body>
</html>
```
![image](https://github.com/user-attachments/assets/9037bec4-146a-4743-a61c-d366c4626ef1) 
Kode ini menampilkan tabel data barang dengan kolom untuk gambar, nama, kategori, harga jual, harga beli, stok, dan aksi (ubah/hapus). Data barang disimulasikan dalam array, dan setiap barang memiliki link untuk mengubah atau menghapusnya. Tabel dan link diformat dengan CSS untuk tampilan yang rapi.    

## Menambah Data (Create)
```sh
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
'{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
</head>
<body>
<div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
        <form method="post" action="tambah.php"
enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option value="Komputer">Komputer</option>
                    <option value="Elektronik">Elektronik</option>
                    <option value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" />
            </div>
                <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" />
            </div>
                <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
                <div class="submit">
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```
![image](https://github.com/user-attachments/assets/0ca4d199-734b-41d8-9124-f9349141632b)   
Penjelasan singkat mengenai kode PHP di atas:   

1. **Error Reporting**: `error_reporting(E_ALL);` menampilkan semua jenis error dalam PHP.   
2. **Proses Form Submit**: Ketika tombol submit ditekan, data dari form (nama, kategori, harga jual, harga beli, stok, dan file gambar) diproses.   
3. **Pengolahan File Gambar**:    
   - Jika file gambar diupload (`$_FILES['file_gambar']`), file tersebut dipindahkan ke folder `gambar/`.    
   - Nama file diganti dengan spasi menjadi underscore (`_`) dan disalin ke folder tujuan.   
   - Variabel `$gambar` diisi dengan path gambar yang berhasil diupload.   
4. **Query Insert**: Data yang diterima dari form dimasukkan ke dalam query `INSERT INTO` untuk menyimpan data barang baru ke dalam tabel `data_barang`.   
5. **Redirect**: Setelah data berhasil disimpan, halaman akan diarahkan kembali ke `index.php`.    
6. **Form HTML**: Form untuk menambah data barang dengan input untuk nama, kategori, harga jual, harga beli, stok, dan gambar. Form ini mengirimkan data ke script `tambah.php` dengan metode POST dan tipe `multipart/form-data` untuk menangani upload file.   

Output dari kode ini adalah form untuk menambahkan barang baru dengan input untuk data barang dan gambar. Setelah form disubmit, data barang baru akan disimpan di database dan gambar diupload ke server.   
  
## Mengubah Data (Update)
```sh
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

// Validasi parameter ID
if (!isset($_GET['id']) || empty($_GET['id'])) {
    die('Error: ID tidak ditemukan di URL.');
}
$id = $_GET['id'];

// Query untuk mendapatkan data berdasarkan ID
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);

if (!$result || mysqli_num_rows($result) == 0) {
    die('Error: Data tidak ditemukan di database.');
}

$data = mysqli_fetch_array($result);

// Variabel untuk memuat data
$nama = $data['nama'];
$kategori = $data['kategori'];
$harga_jual = $data['harga_jual'];
$harga_beli = $data['harga_beli'];
$stok = $data['stok'];

function is_select($var, $val) {
    return $var == $val ? 'selected="selected"' : '';
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $nama; ?>" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select('Komputer', $kategori); ?> value="Komputer">Komputer</option>
                    <option <?php echo is_select('Elektronik', $kategori); ?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select('Hand Phone', $kategori); ?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $harga_jual; ?>" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $harga_beli; ?>" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $stok; ?>" />
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="hidden" name="id" value="<?php echo $id; ?>" />
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```
![image](https://github.com/user-attachments/assets/faf8d886-db34-4f73-9ed1-856f1c37ef09)   
Penjelasan singkat mengenai kode PHP di atas:   

1. **Error Reporting**: `error_reporting(E_ALL);` menampilkan semua jenis error dalam PHP.   
2. **Validasi ID**: Memeriksa apakah parameter `id` ada di URL dan tidak kosong. Jika tidak ada, menampilkan pesan error.   
3. **Query Database**: Mengambil data dari tabel `data_barang` berdasarkan `id_barang` yang diterima dari URL. Jika data tidak ditemukan, menampilkan pesan error.   
4. **Menyimpan Data**: Data yang diambil dari database dimasukkan ke dalam variabel seperti `$nama`, `$kategori`, `$harga_jual`, dll.     
5. **Fungsi `is_select`**: Digunakan untuk memilih opsi yang sesuai dalam elemen `<select>`.   
6. **Form HTML**: Menampilkan form untuk mengubah data barang dengan nilai default yang diisi dengan data yang diambil dari database, seperti `nama`, `kategori`, `harga_jual`, dll. Ada juga input untuk memilih file gambar.  
7. **Form Action**: Form mengirimkan data ke `ubah.php` untuk proses selanjutnya (misalnya pembaruan data).   

Output dari kode ini adalah sebuah form untuk mengubah data barang berdasarkan `id` yang ada di URL, dengan nilai default yang sesuai dengan data barang yang ditemukan di database.   

## Menghapus Data (Delete)
```sh
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
![image](https://github.com/user-attachments/assets/b55a150d-c59c-47c8-a072-70817ddaf4b4)   
setelah mengetik url 'hapus.php' website akan beralih ke location index.php sesuai dengan perintahnya.   

![image](https://github.com/user-attachments/assets/cd3b1902-0914-4518-a0c1-20017c7a4295)   
Kode PHP ini menghapus data barang berdasarkan parameter `id` yang diterima dari URL, menjalankan query DELETE untuk menghapus data dari tabel `data_barang`, dan kemudian mengarahkan pengguna kembali ke halaman `index.php` setelah proses penghapusan selesai.   











