# Pengelolaan-kontak
 Latihan 3-Muhammad Azhari Nur Pratama-2210010326

## 1. Deskripsi Program:
• Aplikasi menyimpan informasi kontak seperti nama, nomor telepon, dan
kategori kontak ke dalam database SQLite.
• Pengguna dapat menambahkan, mengedit, menghapus kontak tersimpan.
• Kontak dapat dikelompokkan berdasarkan kategori yang dipilih dari
JComboBox, seperti keluarga, teman, atau kerja.

## 2. Komponen GUI: 
JFrame, JPanel, JLabel, JTextField, JButton, JList, JComboBox,JTable, JScrollPane

## 3. Logika Program: 
database SQLite, fitur CRUD (Create, Read, Update, Delete)

## 4. Events:
• ActionListener untuk tombol Tambah, Edit, Hapus, dan Cari Kontak.
# A.Tambah
~~~
private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {                                         
       String nama = jTextField1.getText();
    String nomor = jTextField2.getText();
    String kategori = (String) jComboBox1.getSelectedItem();
    
    if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
        dbHelper.tambahKontak(nama, nomor, kategori);
        loadKontakData(); // Refresh data di JTable
        jTextField1.setText("");
        jTextField2.setText("");
        jComboBox1.setSelectedIndex(0);
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Ditambahkan!");
    }

    }   
~~~
# B.Edit
~~~
 private void jButton5ActionPerformed(java.awt.event.ActionEvent evt) {                                         
      int selectedRow = jTable1.getSelectedRow();
    
    // Pastikan ada baris yang dipilih
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) jTable1.getValueAt(selectedRow, 0)); // Ambil ID dari kolom pertama
        String nama = jTextField1.getText();
        String nomor = jTextField2.getText();
        String kategori = (String) jComboBox1.getSelectedItem();
        
        // Validasi data sebelum diperbarui
        if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
            dbHelper.updateKontak(id, nama, nomor, kategori); // Update kontak di database
            loadKontakData(); // Refresh data di JTable
            JOptionPane.showMessageDialog(this, "Kontak berhasil diperbarui!");

            // Kosongkan input setelah diperbarui
            jTextField1.setText("");
            jTextField2.setText("");
            jComboBox1.setSelectedIndex(0);
        }
        } else {
        JOptionPane.showMessageDialog(this, "Pilih Kontak yang Ingin Diedit!");
    
    }
    } 
~~~
# C.Hapus
~~~
 private void jButton6ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        int selectedRow = jTable1.getSelectedRow();
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) jTable1.getValueAt(selectedRow, 0));
        dbHelper.hapusKontak(id);
        loadKontakData(); // Refresh data di JTable
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Dihapus!");
    }
    }
~~~
# D.Cari Kontak
~~~
 private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        String keyword = jTextField3.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        jTable1.setModel(model);
    }
~~~
• ItemListener untuk JComboBox kategori kontak
~~~
private void jComboBox1ItemStateChanged(java.awt.event.ItemEvent evt) {                                             
    // Periksa apakah item yang dipilih
    if (evt.getStateChange() == java.awt.event.ItemEvent.SELECTED) {
        // Ambil item yang dipilih
        String selectedCategory = jComboBox1.getSelectedItem().toString();
        
        // Lakukan sesuatu berdasarkan kategori yang dipilih
        switch (selectedCategory) {
            case "Keluarga":
                System.out.println("Anda memilih kategori: Keluarga");
                break;
            case "Pasangan":
                System.out.println("Anda memilih kategori: Pasangan");
                break;
            case "Saudara":
                System.out.println("Anda memilih kategori: Saudara");
                break;
            case "Teman":
                System.out.println("Anda memilih kategori: Teman");
                break;
            case "Paman":
                System.out.println("Anda memilih kategori: Paman");
                break;
            case "Tante":
                System.out.println("Anda memilih kategori: Tante");
                break;
            case "Adek":
                System.out.println("Anda memilih kategori: Adek");
                break;
            case "Sepupu":
                System.out.println("Anda memilih kategori: Sepupu");
                break;
            default:
                System.out.println("Kategori tidak dikenal: " + selectedCategory);
        }
    }
}

~~~
## 5. Variasi:
• Tambahkan fitur pencarian kontak berdasarkan nama atau nomor telepon, lalu tampilkan hasilnya di JTable
~~~
 private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {                                         
        String keyword = jTextField3.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        jTable1.setModel(model);
    }
~~~
• Buat validasi input untuk memastikan nomor telepon yang dimasukkan hanya berisi angka dan memiliki panjang yang sesuai
~~~
private void jTextField2KeyTyped(java.awt.event.KeyEvent evt) {                                     
        char c = evt.getKeyChar(); 
    if (!Character.isDigit(c)) { evt.consume();
    JOptionPane.showMessageDialog(null, "Masukkan hanya angka untuk nomor telepon."); } 
    }    
~~~
• Sediakan fitur untuk mengekspor daftar kontak ke file CSV dan mengimpor kontak dari file CSV ke database.
~~~
private void EksporKeCSV() {
    try (FileWriter writer = new FileWriter("kontak.csv")) {
        // Tulis header kolom
        writer.append("NAMA,NOMOR,KATEGORI\n");

        // Ambil model tabel
        DefaultTableModel model = (DefaultTableModel) jTable1.getModel();
        int rowCount = model.getRowCount();

        // Iterasi setiap baris di tabel dan tulis ke CSV
        for (int i = 0; i < rowCount; i++) {
            String nama = (String) model.getValueAt(i, 1); // Kolom Nama
            String nomor = (String) model.getValueAt(i, 2); // Kolom Nomor
            String kategori = (String) model.getValueAt(i, 3); // Kolom Kategori

            // Tulis data ke file CSV
            writer.append(nama).append(",").append(nomor).append(",").append(kategori).append("\n");
        }

        // Baca data dari TextField
        String namaBaru = jTextField1.getText().trim(); // Ambil teks dari TextField Nama
        String nomorBaru = jTextField2.getText().trim(); // Ambil teks dari TextField Nomor
        String kategoriBaru = (String) jList2.getSelectedValue(); // Ambil elemen yang dipilih dari JList


        // Validasi input (opsional)
        if (namaBaru.isEmpty() || nomorBaru.isEmpty() || kategoriBaru.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Semua field harus diisi.", "Peringatan", JOptionPane.WARNING_MESSAGE);
            return; // Keluar jika ada field kosong
        }

        // Tambahkan data baru ke tabel
        model.addRow(new Object[]{rowCount + 1, namaBaru, nomorBaru, kategoriBaru});

        // Simpan data baru ke database
        dbHelper.tambahKontak(namaBaru, nomorBaru, kategoriBaru);

        JOptionPane.showMessageDialog(this, "Data berhasil diekspor ke CSV dan ditambahkan ke tabel.");
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengekspor data: " + e.getMessage());
        e.printStackTrace(); // Log kesalahan
}
}

private void imporDariCSV() {
    try (BufferedReader reader = new BufferedReader(new FileReader("kontak.csv"))) {
        String line;
        boolean header = true;
        
        while ((line = reader.readLine()) != null) {
            if (header) { // Lewati baris header
                header = false;
                continue;
            }
            
            String[] data = line.split(",");
            if (data.length == 4) { // Pastikan ada 4 kolom (ID, Nama, Nomor, Kategori)
                String nama = data[1];
                String nomor = data[2];
                String kategori = data[3];
                
                dbHelper.tambahKontak(nama, nomor, kategori);
            }
        }
        
        JOptionPane.showMessageDialog(this, "Data berhasil diimpor dari kontak.csv!");
        loadKontakData(); // Refresh data di JTable setelah impor
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengimpor data!", "Error", JOptionPane.ERROR_MESSAGE);
    }
}
~~~

## Hasil Setelah Di Run
![Cuplikan layar 2024-11-15 225036](https://github.com/user-attachments/assets/30f2ad24-c13d-43e3-83fc-ef111859da0d)

## Indikator Penilaian:

| No  | Komponen         |  Persentase  |
| :-: | --------------   |   :-----:    |
|  1  | Komponen GUI     |    20    |
|  2  | Logika Program   |    30    |
|  3  |  Events          |    15    |
|  4  | Kesesuaian UI    |    15    |
|  5  | Memenuhi Variasi |    20    |
|     | TOTAL        | 100 |

## Pembuat
Nama  : Muhammad Azhari Nur Pratama
NPM   : 2210010326
