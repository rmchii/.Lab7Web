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
