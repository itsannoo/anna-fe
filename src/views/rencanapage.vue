<template>
  <div v-if="isLoggedIn" class="container">
    <h1>Tambah Baru</h1>
    <form @submit.prevent="createRencana" class="form-container">
      <input v-model="rencanaData.rencana" type="text" placeholder="NIP" required>
      <input v-model="rencanaData.waktu" type="text" placeholder="Nama" required>
      <input v-model="rencanaData.keterangan" type="text" placeholder="Jabatan" required>
      <button type="submit">Tambah Baru</button>
    </form>
    
    <!-- Form Edit -->
    <div v-if="selectedRencana._id" class="form-container">
      <h1 class="title">Edit</h1>
      <form @submit.prevent="updateRencana">
        <input v-model="selectedRencana.rencana" type="text" placeholder="NIP" required>
        <input v-model="selectedRencana.waktu" type="text" placeholder="Nama" required>
        <input v-model="selectedRencana.keterangan" type="text" placeholder="Jabatan" required>
        <button type="submit">Update</button>
      </form>
    </div>

    <h1 class="title">All Rencana</h1>
    <table class="rencana-table">
      <thead>
        <tr>
          <th>NIP</th>
          <th>Nama</th>
          <th>Jabatan</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="rencana in rencanas" :key="rencana._id">
          <td>{{ rencana.rencana }}</td>
          <td>{{ rencana.waktu }}</td>
          <td>{{ rencana.keterangan }}</td>
          <td>
            <button class="edit-button" @click="getRencanaById(rencana._id)">Edit</button>
            <button class="delete-button" @click="deleteRencana(rencana._id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      rencanaData: {
        rencana: '',
        waktu: '',
        keterangan: '',
      },
      rencanas: [],
      selectedRencana: {
        _id: '',
        rencana: '',
        waktu: '',
        keterangan: '',
      },
    };
  },
  computed: {
    isLoggedIn() {
      return !!localStorage.getItem('token');
    },
  },
  methods: {
    getToken() {
      return localStorage.getItem('token');
    },
    async createRencana() {
      try {
        const response = await axios.post('https://anna-be.vercel.app/rencana', this.rencanaData, {
          headers: { Authorization: `Bearer ${this.getToken()}` },
        });
        this.rencanas.push(response.data);
        this.rencanaData.rencana = '';
        this.rencanaData.waktu = '';
        this.rencanaData.keterangan = '';
      } catch (error) {
        console.error(error);
      }
    },
    async getAllRencana() {
      try {
        const response = await axios.get('https://anna-be.vercel.app/rencana', {
          headers: { Authorization: `Bearer ${this.getToken()}` },
        });
        this.rencanas = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async getRencanaById(id) {
      try {
        const response = await axios.get(`https://anna-be.vercel.app/rencana/${id}`, {
          headers: { Authorization: `Bearer ${this.getToken()}` },
        });
        this.selectedRencana = { ...response.data };
      } catch (error) {
        console.error(error);
      }
    },
    async updateRencana() {
      try {
        const response = await axios.put(
          `https://anna-be.vercel.app/rencana/${this.selectedRencana._id}`,
          this.selectedRencana,
          {
            headers: { Authorization: `Bearer ${this.getToken()}` },
          }
        );
        const index = this.rencanas.findIndex((rencana) => rencana._id === response.data._id);
        if (index !== -1) {
          this.rencanas.splice(index, 1, response.data);
        }
        this.selectedRencana = {
          _id: '',
          rencana: '',
          waktu: '',
          keterangan: '',
        };
      } catch (error) {
        console.error(error);
      }
    },
    async deleteRencana(id) {
      try {
        await axios.delete(`https://anna-be.vercel.app/rencana/${id}`, {
          headers: { Authorization: `Bearer ${this.getToken()}` },
        });
        this.rencanas = this.rencanas.filter((rencana) => rencana._id !== id);
      } catch (error) {
        console.error(error);
      }
    },
  },
  created() {
    this.getAllRencana();
  },
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  color: rgb(86, 86, 86);
  outline-color: rgb(244, 255, 96);
}

.title {
  font-size: 24px;
  margin-bottom: 20px;
}

.rencana-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

.rencana-table th,
.rencana-table td {
  padding: 10px;
  border: 1px solid #000000;
}

.rencana-table th {
  background-color: #021c4d;
  font-weight: bold;
  text-align: left;
}

.rencana-table tbody tr:hover {
  background-color: #fff9f9;
}

/* Styles for Create and Update forms */
.form-container {
  max-width: 400px;
  margin-bottom: 20px;
}

.form-container h1 {
  font-size: 20px;
  margin-bottom: 10px;
}

.form-container form input {
  display: block;
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #080808;
  border-radius: 4px;
}

.form-container form button {
  background-color: #4caf50;
  color: #ffffff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.form-container form button:hover {
  opacity: 0.8;
}

/* Styles for Actions buttons in the table */
.edit-button {
  background-color: #4caf50;
  color: #fff;
  padding: 6px 10px;
  margin-right: 5px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.delete-button {
  background-color: #f44336;
  color: #fff;
  padding: 6px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.edit-button:hover,
.delete-button:hover {
  opacity: 0.8;
}
</style>
