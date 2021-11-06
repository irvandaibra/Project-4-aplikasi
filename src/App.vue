
<template>
<div class="container">
<div class="header">
 SILAKAN MENGISI FORM DISINI INI
</div>

<div class="section">
<div class="nav">
  <h3>silahkan isi form disini</h3>
  <div>
    <br>
     <br>
    <form @submit.prevent="add">
        <label for="">Nama Siswa</label>
        <input required placeholder="isi nama anda disini" style="width: 200px;" type="text" v-model="form.nama"/><br><br>
        <label for="">Judul Buku</label>
        <input required placeholder="isi judul buku" style="width: 200px;" type="text" v-model="form.judul"/> <br><br>
        <label for="">Tanggal Pinjam</label>
        <input required placeholder="isi tanggal pinjam" style="width: 200px;" type="text" v-model="form.tpinjam"/> <br><br>
        <label for="">Tanggal Pengembalian</label>
        <input required placeholder="isi tanggal pengembalian" style="width: 200px;" type="text" v-model="form.tpengembalian"/> <br><br>
        <br><br>
        <button id="button" type="submit" v-show="!updateSubmit">Tambah Buku</button>
        <button id="button" type="button" v-show="updateSubmit" @click="update(form)">
          Tambah Data</button> <br>
    </form> 
    <br>
  </div>
</div>
<div class="article">
  <h2>TABEL PINJAMAN BUKU</h2>
  <br>
  <center>
        <table border='1' style="padding: 5px;"  > 
           <thead>
            <tr> 
              <th>No</th> 
              <th>Nama Siswa</th> 
              <th>judul Buku</th> 
              <th>Tanggal Pinjam</th> 
              <th>Tanggal Pengembalian</th> 
              <th>Aksi</th> 
            </tr> 
           </thead>
            <tbody> 
    <tr v-for="(user,index) in users" :key="index"> 
      <th>{{++index}}</th> 
      <th>{{user.nama}}</th> 
      <th>{{user.judul}}</th> 
      <th>{{user.tpinjam}}</th> 
      <th>{{user.tpengembalian}}</th> 
      <th>{{user.action}}<button class="button" @click="edit(user)">ubah</button>|<button class="button" @click="del(user)">hapus</button>
      </th> 
    </tr> 
      </tbody> 
         </table>
  </center>
</div>
</div>
<div class="footer">
  <h2>Terima kasih atas kepercayaan anda</h2>
</div>
</div>
</template>

<script>
/* eslint-disable */
import axios from "axios";
export default {
  data() {
    return {
      form: {
        id: "",
        nama: "",
        judul: "",
        tpinjam: "",
        tpengembalian: "",
      },
      users: "",
      updateSubmit: false,
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/users")
        .then((res) => {
          this.users = res.data; //respon dari rest api dimasukan ke users
        })
        .catch((err) => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/users", this.form).then((res) => {
        this.load();
        this.form.nama = "";
        this.form.judul = "";
        this.form.tpinjam = "";
        this.form.tpengembalian = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.nama = user.nama;
      this.form.judul = user.judul;
      this.form.tpinjam = user.tpinjam;
      this.form.tpengembalian = user.tpengembalian;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, {
          nama: this.form.nama,
          judul: this.form.judul,
          tpinjam: this.form.tpinjam,
          tpengembalian: this.form.tpengembalian,
        })
        .then((res) => {
          this.load();
          this.form.id = "";
          this.form.nama = "";
          this.form.judul = "";
          this.form.tpinjam = "";
          this.form.tpengembalian = "";
          this.updateSubmit = false;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then((res) => {
        this.load();
        let index = this.users.indexOf(form.nama);
        this.users.splice(index, 1);
      });
    },
  },
};
</script>

<style>
  * {
    box-sizing: border-box;
  }
  
  .body {
    font-family: Arial, Helvetica, sans-serif;
  }
  
  /* Style the header */
  .header {
    background-color: rgb(97, 237, 255);
    padding: 30px;
    text-align: center;
    font-size: 35px;
    color: rgb(49, 49, 49);
    font-family: 'Times New Roman', Times, serif;
  }
  
  /* Container for flexboxes */
  .section {
    display: -webkit-flex;
    display: flex;
  }
  
  /* Style the navigation menu */
  .nav {
    -webkit-flex: 1;
    -ms-flex: 1;
    flex: 1;
    background: skyblue;
    padding: 20px;
  }
  
  /* Style the list inside the menu */
  .navul {
    list-style-type: none;
    padding: 0;
  }
  
  /* Style the content */
  .article {
    -webkit-flex: 3;
    -ms-flex: 3;
    flex: 3;
    background-color: #a0fcff;
    padding: 10px;
    text-align: center;
  }
  
  /* Style the footer */
  .footer {
    background-color: #57afbb;
    padding: 10px;
    text-align: center;
    color: white;
  }
  
.container {
  margin: 50px;
}

.button {
  position: relative;
  background-color: #2bc731;
  border: none;
  font-size: 10px;
  color: #FFFFFF;
  padding: 10px;
  height: 50px;
  width: 50px;
  text-align: center;
  transition-duration: 0.5s;
  text-decoration: none;
  overflow: hidden;
  cursor: pointer;
  border-radius: 10px;
}

.button:after {
  content: "";
  background: #f1f1f1;
  display: block;
  position: absolute;
  padding-top: 300%;
  padding-left: 350%;
  margin-left: -20px !important;
  margin-top: -120%;
  opacity: 0;
  transition: all 0.7s;
  
}

.button:active:after {
  padding: 0;
  margin: 0;
  opacity: 1;
  transition: 0s
}

#button {
  display: inline-block;
  border-radius: 4px;
  background-color: #f4511e;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 28px;
  padding: 15px;
  width: 200px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

#button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
}

#button span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

#button:hover span {
  padding-right: 25px;
}

#button:hover span:after {
  opacity: 1;
  right: 0;
}
</style>

</style>


