## LAPORAN PRAKTIKUM 1 - 10
NAMA : ALI GUNAWAN | KELAS : I241C | NIM : 312410400

## Praktikum 1: VueJS
## Langkah-langkah Praktikum 1
VueJS merupakan sebuah framework JavaScript yang dirancang khusus untuk membangun antarmuka pengguna (UI) website yang interaktif dan dinamis. VueJS dapat digunakan untuk membangun aplikasi berbasis user interface, seperti halaman web, aplikasi mobile, dan aplikasi desktop.
Framework ini menawarkan keseimbangan dan kemudahan penggunaan dan performa yang kuat. 

### Step 1 | Menambahkan library Vuejs dan Axios
Selanjutnya, Persiapkan library Vuejs dan Axiountuk call API REST. Menggunakan CDN untuk memuat library Vue.
```<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>```.
```<script src="https://unpkg.com/axios/dist/axios.min.js"></script>```

Struktur direktori:

<img width="226" height="124" alt="Screenshot 2026-06-16 232836" src="https://github.com/user-attachments/assets/805a519f-f6b8-4785-945f-c94b9d4ae209" />

### Step 2 | Membuat file index.html pada direktori lab8_vuejs.
kode sebagai berikut:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Frontend Vuejs</title>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>
    <div id="app">
        <h1>Daftar Artikel</h1>

        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Judul</th>
                    <th>Status</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(row, index) in artikel">
                    <td class="center-text">{{ row.id }}</td>
                    <td>{{ row.judul }}</td>
                    <td>{{ statusText(row.status) }}</td>
                    <td class="center-text">
                        <a href="#" @click="edit(row)">Edit</a>
                        <a href="#" @click="hapus(index, row.id)">Hapus</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <script src="assets/js/app.js"></script>
</body>
Penjelasan: View ini akan menampilkan data artikel yg ada di praktikum sebelumnya, yakni ci4.

### Step 3 | Membuat file app.js.
kode:
```JavaScript
const { createApp } = Vue

// tentukan lokasi API REST End Point
const apiUrl = 'http://localhost/lab11_ci/ci4/public'

createApp({

    data() {

        return {

            artikel: [],

            formData: {
                id: null,
                judul: '',
                isi: '',
                status: 0
            },

            showForm: false,

            formTitle: 'Tambah Data',

            statusOptions: [
                { text: 'Draft', value: 0 },
                { text: 'Publish', value: 1 }
            ]
        }
    },

    mounted() {
        this.loadData()
    },

    methods: {

        // Load data artikel
        loadData() {

            axios.get(apiUrl + '/post')

            .then(response => {
                this.artikel = response.data.artikel
            })

            .catch(error => {
                console.log(error)
            })
        },

        // Tambah data
        tambah() {

            this.showForm = true

            this.formTitle = 'Tambah Data'

            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            }
        },

        // Edit data
        edit(data) {

            this.showForm = true

            this.formTitle = 'Ubah Data'

            this.formData = {
                id: data.id,
                judul: data.judul,
                isi: data.isi,
                status: data.status
            }
        },

        // Hapus data
        hapus(index, id) {

            if (confirm('Yakin menghapus data?')) {

                axios.delete(apiUrl + '/post/' + id)

                .then(response => {
                    this.artikel.splice(index, 1)
                })

                .catch(error => {
                    console.log(error)
                })
            }
        },

        // Simpan data
        saveData() {

            // UPDATE
            if (this.formData.id) {

                axios.put(
                    apiUrl + '/post/' + this.formData.id,
                    this.formData
                )

                .then(response => {
                    this.loadData()
                })

                .catch(error => {
                    console.log(error)
                })

            }

            // INSERT
            else {

                axios.post(
                    apiUrl + '/post',
                    this.formData
                )

                .then(response => {
                    this.loadData()
                })

                .catch(error => {
                    console.log(error)
                })
            }

            // Reset form
            this.formData = {
                id: null,
                judul: '',
                isi: '',
                status: 0
            }

            this.showForm = false
        },

        // Status artikel
        statusText(status) {

            if (!status) {
                return 'Draft'
            }

            return status == 1
                ? 'Publish'
                : 'Draft'
        }
    }

}).mount('#app')
```
Fungsinya: 
- loadData
- CREATE Data
- UPDATE Data
- DELETE Data
</html>
```
