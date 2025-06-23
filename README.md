# .Lab7Web
hasil dari mengakses alamat url http://localhost:8080/about
![Screenshot (591)](https://github.com/user-attachments/assets/90e3a930-07c5-4c46-b3cc-0a8da68afc99)
![Screenshot (592)](https://github.com/user-attachments/assets/362e8a20-1bf8-48c5-b829-13a59b8ac025)

Membuat Menu Admin Buat method baru pada Controller Artikel dengan nama admin_index()

public function admin_index() 
 {
 $title = 'Daftar Artikel';
 $model = new ArtikelModel();
 $artikel = $model->findAll();
 return view('artikel/admin_index', compact('artikel', 'title'));
 }
 
 buat view untuk tampilan admin dengan nama admin_index.php
 <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title><?= $title; ?></title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f7f7f7;
    }

    header {
      padding: 20px;
    }

    h1 {
      margin-left: 30px;
      color: #444;
    }

    .navbar {
      background-color: #1d74d0;
      overflow: hidden;
    }

    .navbar a {
      display: inline-block;
      color: white;
      padding: 14px 20px;
      text-decoration: none;
      font-weight: bold;
    }

    .navbar a.active,
    .navbar a:hover {
      background-color: #155a9c;
    }

    table {
      width: 90%;
      margin: 20px auto;
      border-collapse: collapse;
      background-color: white;
    }

    table, th, td {
      border: 1px solid #ccc;
    }

    th {
      background-color: #4a90e2;
      color: white;
      padding: 10px;
      text-align: left;
    }

    td {
      padding: 10px;
      vertical-align: top;
    }

    tfoot th {
      background-color: #4a90e2;
      color: white;
    }

    .btn-ubah {
      background-color: #999;
      color: white;
      padding: 6px 12px;
      text-decoration: none;
      border-radius: 4px;
      margin-right: 5px;
      display: inline-block;
    }

    .btn-hapus {
      background-color: #e74c3c;
      color: white;
      padding: 6px 12px;
      text-decoration: none;
      border-radius: 4px;
      display: inline-block;
    }

    footer {
      background-color: #222;
      color: white;
      text-align: left;
      padding: 15px 30px;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <header>
    <h1>Admin Portal Berita</h1>
  </header>

  <nav class="navbar">
    <a href="<?= base_url('/admin'); ?>" class="active">Dashboard</a>
    <a href="<?= base_url('/admin/artikel'); ?>">Artikel</a>
    <a href="<?= base_url('/admin/artikel/create'); ?>">Tambah Artikel</a>
  </nav>

  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Judul</th>
        <th>Status</th>
        <th>AKsi</th>
      </tr>
    </thead>
    <tbody>
      <?php foreach ($artikel as $row): ?>
      <tr>
        <td><?= $row['id']; ?></td>
        <td>
          <strong><?= $row['judul']; ?></strong><br>
          <?= substr($row['isi'], 0, 50); ?>
        </td>
        <td>0</td>
        <td>
          <a class="btn-ubah" href="<?= base_url('/admin/artikel/edit/' . $row['id']); ?>">Ubah</a>
          <a class="btn-hapus" href="<?= base_url('/admin/artikel/delete/' . $row['id']); ?>" onclick="return confirm('Hapus artikel ini?')">Hapus</a>
        </td>
      </tr>
      <?php endforeach; ?>
    </tbody>
    <tfoot>
      <tr>
        <th>ID</th>
        <th>Judul</th>
        <th>Status</th>
        <th>AKsi</th>
      </tr>
    </tfoot>
  </table>

  <footer>
    <p>&copy; 2021 - Universitas Pelita Bangsa</p>
  </footer>

</body>
</html>
Akses menu admin dengan url http://localhost:8080/admin/artikel

![Screenshot (811)](https://github.com/user-attachments/assets/a882109e-6e75-4329-8a3c-474e8b0901b8)

Menambah Data Artikel
Tambahkan fungsi/method baru pada Controller Artikel dengan nama add()
public function add() 
    {
        // Validasi data
        $validation = \Config\Services::validation();
        $validation->setRules([
            'judul' => 'required'
        ]);
    
        $isDataValid = $validation->withRequest($this->request)->run();
    
        if ($isDataValid) {
            $artikel = new ArtikelModel();
            $artikel->insert([
                'judul' => $this->request->getPost('judul'),
                'isi' => $this->request->getPost('isi'),
                'slug' => url_title($this->request->getPost('judul')),
            ]);
    
            return redirect('admin/artikel');
        }
    
        $title = "Tambah Artikel";
        return view('artikel/form_add', compact('title'));
    }
    
  Kemudian buat view untuk form tambah dengan nama form_add.php
  <?= $this->include('template/admin_header'); ?>

<style>
    form {
        max-width: 800px;
        margin-top: 20px;
    }

    input[type="text"],
    textarea {
        width: 100%;
        padding: 10px;
        margin-bottom: 15px;
        border: 1px solid #ccc;
        border-radius: 4px;
        font-size: 16px;
    }

    input[type="submit"] {
        background-color: #3b82f6; /* Biru */
        color: white;
        padding: 10px 20px;
        font-size: 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    input[type="submit"]:hover {
        background-color: #2563eb;
    }

    h2 {
        margin-top: 20px;
    }
</style>

<h2><?= $title; ?></h2>

<form action="" method="post">
    <p>
        <input type="text" name="judul" placeholder="Judul Artikel">
    </p>
    <p>
        <textarea name="isi" cols="50" rows="10" placeholder="Isi Artikel"></textarea>
    </p>
    <p>
        <input type="submit" value="Kirim">
    </p>
</form>

<?= $this->include('template/admin_footer'); ?>

![Screenshot (812)](https://github.com/user-attachments/assets/3b006368-5976-4205-9642-27dd6f1624a7)

Mengubah Data
Tambahkan fungsi/method baru pada Controller Artikel dengan nama edit()
public function edit($id)
{
    $artikel = new ArtikelModel();

    // Validasi data
    $validation = \Config\Services::validation();
    $validation->setRules([
        'judul' => 'required'
    ]);

    $isDataValid = $validation->withRequest($this->request)->run();

    if ($isDataValid) {
        // Jika valid, update data artikel
        $artikel->update($id, [
            'judul' => $this->request->getPost('judul'),
            'isi'   => $this->request->getPost('isi'),
        ]);

        return redirect()->to('admin/artikel');
    }

    // Ambil data lama jika belum valid atau baru pertama kali akses
    $data  = $artikel->where('id', $id)->first();
    $title = "Edit Artikel";

    return view('artikel/form_edit', compact('title', 'data'));
}
Kemudian buat view untuk form tambah dengan nama form_edit.php
<?= $this->include('template/admin_header'); ?>
<h2><?= $title; ?></h2>
<form action="" method="post">
 <p>
 <input type="text" name="judul" value="<?= $data['judul'];?>" >
 </p>
 <p>
 <textarea name="isi" cols="50" rows="10"><?=
$data['isi'];?></textarea>
 </p>
 <p><input type="submit" value="Kirim" class="btn btn-large"></p>
</form>
<?= $this->include('template/admin_footer'); ?>

![Screenshot (813)](https://github.com/user-attachments/assets/6c447f86-54d4-4c0b-b257-0c132f62f5ff)
Menghapus Data
Tambahkan fungsi/method baru pada Controller Artikel dengan nama delete()
public function delete($id) 
 {
 $artikel = new ArtikelModel();
 $artikel->delete($id);
 return redirect('admin/artikel');
 }
 
PRAKTIKUM 3
   <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTP-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Box Element</title>
</head>
<body>
    <header>
        <h1>Box Element</h1>
        <link rel="stylesheet" type="text/css" href="style.css">
    </header>
    <section>
        <div class="div1">Div 1</div>
        <div class="div2">Div 2</div>
        <div class="div3">Div 3</div>
    </section>
    
</body>
</html>
  css float property
  div {
    float: left;
    padding: 10px;
 }
 .div1 {
    background: red;
 }
 .div2 {
    background: yellow;
 }
 .div3 {
    background: green;
 }
 ![Screenshot (249)](https://github.com/user-attachments/assets/b7e69670-7bee-4be2-b4f7-ee1378819294)

2. Mengatur Clearfix element
  <section>
        <div class="div1">Div 1</div>
        <div class="div2">Div 2</div>
        <div class="div3">Div 3</div>
        <div class="div4">Div 4</div>
    </section>
   css 
   .div4 {
   background-color: blue;
   clear: left;
   float: none;
 }
![Screenshot (250)](https://github.com/user-attachments/assets/b8b0ff35-3990-4947-8560-cc02b05cc465)

 3. Membuat layout sederhana
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTP-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>layout Sederhana</title>
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    <div id="container">

    </div>
    <header>
        <h1>layout Sederhana</h1>
    </header>
    <nav>
        <a href="home.html" class="active">home</a>
        <a href="artkel.html">Artikel</a>
        <a href="about.html">About</a>
        <a href="kontak.html">Kontak</a>
    </nav>
    <section id="hero" ></section>
    <section id="wrapper">
        <section id="main"></section>
        <aside id="sidebar"></aside>
    </section>
    <footer>
        <p>&copy; 2021 - Universiras Pelita Bangsa</p>
    </footer>
</body>
CSS

@import 
url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300;0,400;0,600;0,700;0,800;1,300;1,400;1,600;1,700;1,800&display=swap');
@import 
url('https://fonts.googleapis.com/css2?family=Open+Sans+Condensed:ital,wght@0,300;0,700;1,300&display=swap');

* {
  margin: 0;
  padding: 0;
}

body {
  line-height: 1;
  font-size: 100%;
  font-family: 'Open Sans', sans-serif;
  color: #5a5a5a;
}
#container {
  width: 980px;
  margin: 0 auto;
  box-shadow: 0 0 1em #cccccc;
}
header {
  padding: 20px;

}
header h1 {
  margin: 20px 10px;
  color: #b5b5b5;
}
![Screenshot (251)](https://github.com/user-attachments/assets/8ced60c9-2098-49fd-a38e-f8e895e1a833)

Membuat navigasi
nav {
    display: block;
    background-color: #1f5faa;
}
nav a {
    padding: 15px 30px;
    display: inline-block;
    color: #ffffff;
    font-size: 14px;
    text-decoration: none;
    font-weight: bold;
}
nav a.active,
nav a:hover {
    background-color: #2b83ea;
}
![Screenshot (253)](https://github.com/user-attachments/assets/46af9e30-0675-4eaf-ba1d-6bab46ccb70b)

4. membuat hero panel
<section id="hero" >
        <h1>Hello World!</h1>
        <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Vestibulum lorem elit, iaculis volutpat, malesuada tincidunt arcu. proin in leo fringilla, Vestibulum mi porta, faucibus felis. integer pharetra est nunc, nec pretium nunc pretium ac.</p>
        <a href="home.html" class="btn btn-large">Learn more &raquo;</a>
    </section>
    CSS 

    #hero {
    background-color: #e4e4e5;
    padding: 50px 20px;
    margin-bottom: 20px;
}
#hero h1 {
    margin-bottom: 20px;
    font-size: 35px;
}

#hero p{
    margin-bottom: 20px;
    font-size: 18px;
    line-height: 25px;
}
![Screenshot (254)](https://github.com/user-attachments/assets/d5c1b05e-3426-4272-ab52-f4c24673b952)

5. Membuat Sidebar Widget
   <div class="sidebar">
            <div class="widget">
                <div class="widget-header">Widget Header</div>
                <div class="widget-content">
                    <a href="#" class="widget-link">Widget Link</a>
                    <a href="#" class="widget-link">Widget Link</a>
                    <a href="#" class="widget-link">Widget Link</a>
                    <a href="#" class="widget-link">Widget Link</a>
                    <a href="#" class="widget-link">Widget Link</a>
                </div>
            </div>
            <div class="widget">
                <div class="widget-header">Widget Text</div>
                <div class="widget-content">
                    <p class="widget-text">Vestibulum lorem elit, iaculis in nisl volutpat, malesuada tincidunt arcu. Proin in leo fringilla, vestibulum mi porta, faucibus felis. Integer pharetra est nunc, nec pretium nunc pretium ac.</p>
                </div>
            </div>
   CSS 
   .sidebar {
  width: 250px;
  float: right;
  margin-top: 20px;
}

.widget {
  border: 1px solid #ccc;
  margin-bottom: 20px;
}

.widget-header {
  background-color: #0078d7;
  color: #fff;
  padding: 10px;
  font-weight: bold;
  font-size: 16px;
  text-align: center;
}

.widget-content {
  padding: 10px;
}

.widget-link {
  display: block;
  padding: 8px 10px;
  color: #0078d7;
  text-decoration: none;
  border-bottom: 1px solid #ccc;
}

.widget-link:last-child {
  border-bottom: none;
}

.widget-link:hover {
  background-color: #e6f3ff;
}

.widget-text {
  color: #333;
  line-height: 1.5;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
Mengatur footer
footer {
  clear: both;
  background-color: #1d1d1d;
  padding: 20px;
  color: #eee;
}
![Screenshot (257)](https://github.com/user-attachments/assets/d17e641c-8750-4e63-835f-3975d9e9f545)

6. Menambahkan Elemen lainnya pada Main Content
   <section id="main">
            <div class="row">
                <div class="box">
                    <img src="https://dummyimage.com/120/db7d25/fff.png" alt=""
        class="image-circle">
        <h3>Heading</h3>
        <p>Donec sed odio dui. Etiam porta sem malesuada magna mollis euismod.</p>
        <a href="#" class="btn btn-default">View detail</a>
        </div>
            <div class="box">
                <img src="https://dummyimage.com/120/3e73e6/fff.png" alt=""class="image-circle">
                <h3>Heading</h3>
                <p>Donec sed odio dui. Etiam porta sem malesuada magna molliseuismod.</p>
                <a href="#" class="btn btn-default">View detail</a>
                </div>
                <div class="box">
                <img src="https://dummyimage.com/120/71e6d4/fff.png" alt=""
                class="image-circle">
                <h3>Heading</h3>
                <p>Donec sed odio dui. Etiam porta sem malesuada magna mollis
                euismod.</p>
                <a href="#" class="btn btn-default">View detail</a>
                </div>
                </div>
        </section>
   CSS
   .box {
  display:block;
  float:left;
  width:33.333333%;
  box-sizing:border-box;
  -moz-box-sizing:border-box;
  -webkit-box-sizing:border-box;
  padding:0 10px;
text-align:center;
}
.box h3 {
margin: 15px 0;
}
.box p {
line-height: 20px;
font-size: 14px;
margin-bottom: 15px;
}
box img {
border: 0;
vertical-align: middle;
}
.image-circle {
border-radius: 50%;
}
.row {
margin: 0 -10px;
box-sizing: border-box;
-moz-box-sizing: border-box;
-webkit-box-sizing: border-box;
}
.row:after, .row:before,
.entry:after, .entry:before {
content:'';
display:table;
}
.row:after,
.entry:after {
clear:both;
}
![Screenshot (269)](https://github.com/user-attachments/assets/7fed3e29-b371-45db-a8d2-7fd2dee0d201)

7. Menambahkan Content Artikel
   <hr class="divider" />
        <article class="entry">
            <h2>First featurette heading.</h2>
            <img src="https://dummyimage.com/150/7b8a70/fff.png" alt="">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum lorem elit, iaculis in nisl volutpat, malesuada tincidunt arcu. Proin in leo fringilla, vestibulum mi porta, faucibus felis. Integer pharetra est nunc, nec pretium nunc pretium ac.</p>
        </article>
        <hr class="divider" />
        <article class="entry">
            <h2>First featurette heading.</h2>
            <img src="https://dummyimage.com/150/7b8a70/fff.png" alt=""
            class="right-img">
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum lorem elit, iaculis in nisl volutpat, malesuada tincidunt arcu. Proin in leo fringilla, vestibulum mi porta, faucibus felis. Integer pharetra est nunc, nec pretium nunc pretium ac.</p>
        </article>
   CSS
   .divider {
  border:0;
  border-top:1px solid #eeeeee;
  margin:40px 0;
  }
  .entry {
  margin: 15px 0;
  }
  .entry h2 {
  margin-bottom: 20px;
  }
  .entry p {
    line-height: 25px;
    }
    .entry img {
    float: left;
    border-radius: 5px;
    margin-right: 15px;
    }
    .entry .right-img {
    float: right;
    }
   ![Screenshot (270)](https://github.com/user-attachments/assets/1597025c-358d-413e-807a-4fe448536290)

Pertanyaan dan Tugas
1. Tambahkan Layout untuk menu About
=> buat single layout yang berisi deskripsi, portfolio, dll
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About</title>
    <link rel="stylesheet" type="text/css" href="layout.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>About</h1>
        </header>
        <section class="about-description">
            <h2>Our Story</h2>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla nec eros vitae lacus ultricies sodales. Curabitur ac felis non lorem cursus suscipit ut a libero.</p>
        </section>
        <section class="portfolio">
            <h2>Portfolio</h2>
            <div class="portfolio-items">
                <div class="portfolio-item">
                    <img src="https://dummyimage.com/200x150/db7d25/fff.png" alt="Project 1">
                    <h3>Project 1</h3>
                    <p>Description of Project 1</p>
                </div>
                <div class="portfolio-item">
                    <img src="https://dummyimage.com/200x150/3e73e6/fff.png" alt="Project 2">
                    <h3>Project 2</h3>
                    <p>Description of Project 2</p>
                </div>
                <div class="portfolio-item">
                    <img src="https://dummyimage.com/200x150/71e6d4/fff.png" alt="Project 3">
                    <h3>Project 3</h3>
                    <p>Description of Project 3</p>
                </div>
            </div>
        </section>
    </div>
</body>
</html>
CSS
.container {
      width: 80%;
      margin: 0 auto;
  }
  
  .about-description, .portfolio {
      margin-bottom: 30px;
  }
  
  .portfolio-items {
      display: flex;
      gap: 20px;
  }
  
  .portfolio-item {
      width: 30%;
      text-align: center;
  }

  ![Screenshot (264)](https://github.com/user-attachments/assets/ae8119a5-7cbf-4324-858c-8f4a5811fb4d)

3. Tambahkan layout untuk menu Contact
=> yang berisi form isian: nama, email, message, dll
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kontak</title>
    <link rel="stylesheet" type="text/css" href="layout.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>Kontak</h1>
        </header>
        
        <section class="contact-form">
            <h2>Get in Touch</h2>
            <form action="submit_contact.php" method="POST">
                <label for="name">Nama:</label>
                <input type="text" id="name" name="name" required>
                <label for="email">Email:</label>
                <input type="email" id="email" name="email" required>

                <label for="message">Message:</label>
                <textarea id="message" name="message" rows="5" required></textarea>

                <button type="submit">Send Message</button>
            </form>
        </section>
    </div>
</body>
</html>
CSS 
.contact-form {
    width: 50%;
    margin: 0 auto;
}

.contact-form label {
    display: block;
    margin-top: 15px;
    font-weight: bold;
}

.contact-form input, .contact-form textarea {
    width: 100%;
    padding: 10px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.contact-form button {
    margin-top: 15px;
    padding: 10px 20px;
    background-color: #0078d7;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.contact-form button:hover {
    background-color: #005fa3;
}

![Screenshot (263)](https://github.com/user-attachments/assets/54d74b08-6afd-4c4a-b2bb-c2f77a512527)

PRAKTIKUM 4
Menbuat Tabel User
CREATE TABLE user (
 id INT(11) auto_increment,
 username VARCHAR(200) NOT NULL,
 useremail VARCHAR(200),
 userpassword VARCHAR(200),
 PRIMARY KEY(id)
);
Buat file baru pada direktori app/Models dengan nama UserModel.php
<?php
namespace App\Models;
use CodeIgniter\Model;
class UserModel extends Model
{
 protected $table = 'user';
 protected $primaryKey = 'id';
 protected $useAutoIncrement = true;
 protected $allowedFields = ['username', 'useremail', 'userpassword'];
}
Buat Controller baru dengan nama User.php pada direktori app/Controllers. Kemudian tambahkan method index() untuk menampilkan daftar user, dan method login() untuk proses login
<?php
namespace App\Controllers;
use App\Models\UserModel;

class User extends BaseController
{
    public function index() 
    {
        $title = 'Daftar User';
        $model = new UserModel();
        $users = $model->findAll();
        return view('user/index', compact('users', 'title'));
    }

    public function login()
    {
        helper(['form']);
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        if (!$email)
        {
            return view('user/login');
        }

        $session = session();
        $model = new UserModel();
        $login = $model->where('useremail', $email)->first();

        if ($login)
        {
            $pass = $login['userpassword'];
            if (password_verify($password, $pass))
            {
                $login_data = [
                    'user_id' => $login['id'],
                    'user_name' => $login['username'],
                    'user_email' => $login['useremail'],
                    'logged_in' => TRUE,
                ];
                $session->set($login_data);
                return redirect('admin/artikel');
            }
            else
            {
                $session->setFlashdata("flash_msg", "Password salah.");
                return redirect()->to('/user/login');
            }
        }
        else
        {
            $session->setFlashdata("flash_msg", "Email tidak terdaftar.");
            return redirect()->to('/user/login');
        }
    }

    public function logout()
    {
        session()->destroy();
        return redirect()->to('/user/login');
    }
}
Buat direktori baru dengan nama user pada direktori app/views, kemudian buat file baru dengan nama login.php
<!DOCTYPE html>
<html lang="en">
<head>
 <meta charset="UTF-8">
 <title>Login</title>
 <style>
   body {
     font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
     background: #f1f4f9;
     display: flex;
     justify-content: center;
     align-items: center;
     height: 100vh;
     margin: 0;
   }

   #login-wrapper {
     background: white;
     padding: 40px;
     border-radius: 12px;
     box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
     width: 360px;
   }

   h1 {
     text-align: center;
     margin-bottom: 30px;
     color: #333;
   }

   .form-label {
     display: block;
     margin-bottom: 6px;
     color: #555;
     font-weight: bold;
   }

   .form-control {
     width: 100%;
     padding: 10px;
     margin-bottom: 20px;
     border: 1px solid #ccc;
     border-radius: 6px;
     box-sizing: border-box;
     font-size: 14px;
   }

   .btn {
     width: 100%;
     padding: 10px;
     background-color: #0069d9;
     color: white;
     border: none;
     border-radius: 6px;
     cursor: pointer;
     font-size: 16px;
     transition: background 0.2s ease;
   }

   .btn:hover {
     background-color: #0053b3;
   }

   .alert {
     padding: 10px;
     background-color: #f44336;
     color: white;
     border-radius: 6px;
     margin-bottom: 20px;
     font-size: 14px;
   }
 </style>
</head>
<body>
 <div id="login-wrapper">
   <h1>Sign In</h1>

   <?php if(session()->getFlashdata('flash_msg')): ?>
     <div class="alert">
       <?= session()->getFlashdata('flash_msg') ?>
     </div>
   <?php endif; ?>

   <form action="" method="post">
     <div class="mb-3">
       <label for="InputForEmail" class="form-label">Email address</label>
       <input type="email" name="email" class="form-control"
       id="InputForEmail" value="<?= set_value('email') ?>">
     </div>

     <div class="mb-3">
       <label for="InputForPassword" class="form-label">Password</label>
       <input type="password" name="password" class="form-control" id="InputForPassword">
     </div>

     <button type="submit" class="btn">Login</button>
   </form>
 </div>
</body>
</html>
Membuat database Seeder 
php spark make:seeder UserSeeder
buka file UserSeeder.php yang berada di lokasi direktori /app/Database/Seeds/UserSeeder.php kemudian isi dengan kode berikut
<?php

namespace App\Database\Seeds;

use CodeIgniter\Database\Seeder;

class UserSeeder extends Seeder
{
    public function run()
    {
        $model = model('UserModel');
        $model->insert([
            'username' => 'admin',
            'useremail' => 'admin@email.com',
            'userpassword' => password_hash('admin123', PASSWORD_DEFAULT),
        ]);
    }
}
Uji Coba Login
Selanjutnya buka url http://localhost:8080/user/login seperti berikut
![Screenshot (824)](https://github.com/user-attachments/assets/4add5f5b-0274-466a-bb84-6c4099b7dfdf)

PRAKTIKUM 5
Untuk membuat pagination, buka Kembali Controller Artikel, kemudian modifikasi kode 
pada method admin_index seperti berikut
public function admin_index() 
    {
        $title = 'Daftar Artikel';
        $model = new ArtikelModel();

        $data = [
            'title' => $title,
            'artikel' => $model->paginate(10), // tampilkan 10 artikel per halaman
            'pager' => $model->pager, // pagination
        ];

        return view('artikel/admin_index', $data);
    }
Buka file views/artikel/admin_index.php dan tambahkan kode berikut 
dibawah deklarasi tabel data
<?= $pager->links(); ?>
![Screenshot (826)](https://github.com/user-attachments/assets/f5f8ad48-ea99-44c8-bdf4-96a631793cc4)

PRAKTIKUM 6
Upload Gambar pada Artikel
Menambahkan fungsi unggah gambar pada tambah artikel. 
Buka kembali Controller Artikel pada project sebelumnya, sesuaikan kode pada method add seperti berikut
public function add() 
    {
        // Validasi data
        $validation = \Config\Services::validation();
        $validation->setRules([
            'judul' => 'required'
        ]);
    
        $isDataValid = $validation->withRequest($this->request)->run();
    
        if ($isDataValid) {
            $file = $this->request->getFile('gambar');
            $file->move(ROOTPATH . 'public/gambar');

            $artikel = new ArtikelModel();
            $artikel->insert([
                'judul' => $this->request->getPost('judul'),
                'isi' => $this->request->getPost('isi'),
                'slug' => url_title($this->request->getPost('judul')),
                'gambar' => $file->getName(),
            ]);
    
            return redirect('admin/artikel');
        }
    
        $title = "Tambah Artikel";
        return view('artikel/form_add', compact('title'));
    }
Kemudian pada file views/artikel/form_add.php tambahkan field input file seperti berikut
<p>
 <input type="file" name="gambar">
</p>

Dan sesuaikan tag form dengan menambahkan ecrypt type seperti berikut
Ujicoba file upload dengan mengakses menu tambah artikel.
![image](https://github.com/user-attachments/assets/7ae51da0-344d-400b-86dd-513eeefd7658)
