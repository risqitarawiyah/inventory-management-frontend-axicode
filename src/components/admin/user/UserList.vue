<template>
  <div class="user-list">
    <div class="header">
      <h2>List Data Pekerja</h2>
      <button class="add-btn" @click="showAddForm">Tambah Data Pekerja</button>
    </div>
    <div class="user-cards">
      <UserCard
        v-for="user in users"
        :key="user.id"
        :user="user"
        @edit-user="editUser"
        @delete-user="confirmDeleteUser"
      />
    </div>
    <Modal :visible="showForm" @close="cancelEditForm">
      <UserForm
        :user="selectedUser"
        :isEdit="isEdit"
        @submit="handleSubmit"
        @cancel="cancelEditForm"
      />
    </Modal>
  </div>
</template>

<script>
import UserCard from "./UserCard.vue";
import Modal from "@/components/Modal.vue";
import UserForm from "./UserForm.vue";

export default {
  components: {
    UserCard,
    Modal,
    UserForm,
  },
  data() {
    return {
      users: [
        {
          id: 1,
          name: "Amanda",
          email: "Amanda@example.com",
          role: "PH Operator",
        },
        {
          id: 2,
          name: "Risqi",
          email: "Risqi@example.com",
          role: "PH Operator",
        },
        {
          id: 2,
          name: "Novi",
          email: "Amanda@example.com",
          role: "WH Operator",
        },
        {
          id: 3,
          name: "Raynaldi",
          email: "Ray@example.com",
          role: "WH Operator",
        },
        {
          id: 2,
          name: "Fikri",
          email: "Fikri@example.com",
          role: "WH Operator",
        },
      ],
      showForm: false,
      selectedUser: null,
      isEdit: false,
    };
  },
  methods: {
    showAddForm() {
      this.selectedUser = { id: null, name: "", email: "", role: "" };
      this.isEdit = false;
      this.showForm = true;
    },
    editUser(user) {
      this.selectedUser = { ...user };
      this.isEdit = true;
      this.showForm = true;
    },
    handleSubmit(user) {
      // Memastikan name, email, dan role terisi
      if (user.name && user.email && user.role) {
        if (this.isEdit) {
          // Update pengguna yang ada
          const index = this.users.findIndex((u) => u.id === user.id);
          if (index !== -1) {
            this.users[index] = { ...user }; // Mengupdate pengguna yang dipilih
          }
        } else {
          // Tambah pengguna baru
          user.id = this.users.length + 1; // Generate ID baru berdasarkan jumlah pengguna
          this.users.push({ ...user }); // Tambah pengguna baru dengan semua data
        }
      }
      this.showForm = false; // Tutup form setelah submit
    },
    cancelEditForm() {
      this.showForm = false;
    },
    confirmDeleteUser(user) {
      if (confirm(`Apakah Anda yakin ingin menghapus ${user.name}?`)) {
        this.deleteUser(user.id);
      }
    },
    deleteUser(id) {
      this.users = this.users.filter((user) => user.id !== id);
    },
  },
};
</script>

<style scoped>
.user-list {
  padding: 24px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin: 20px 0;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}
h2 {
  color: #4b3f6b;
  font-size: 24px;
}
.add-btn {
  background-color: #754bc5;
  color: white;
  padding: 6px 12px;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  font-size: 14px;
}
.add-btn:hover {
  background-color: #5a37a0;
}
.user-cards {
  display: flex;
  flex-direction: column;
}
</style>
