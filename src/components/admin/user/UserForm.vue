<template>
    <div>
        <form @submit.prevent="submitForm">
            <table>
                <tbody>
                <tr>
                    <td>ID Pengguna</td>
                    <td>
                        <input type="text" v-model="form.id" id="id" :disabled="isEdit" required/>
                    </td>
                </tr>
                <tr>
                    <td>Nama Pengguna</td>
                    <td>
                        <input type="text" v-model="form.name" id="name" required />
                    </td>
                </tr>
                <tr>
                    <td>Email</td>
                    <td>
                        <input type="email" v-model="form.email" id="email" required />
                    </td>
                </tr>
                <tr>
                    <td>Role</td>
                    <td>
                        <select v-model="form.role" required>
                            <option value="" disabled>Pilih role</option>
                            <option value="PH_OPERATOR">PH Operator</option> <!-- value revised from ADMIN to PH_Operator-->
                            <option value="WH_Operator">WH Operator</option> <!-- value revised from USER to WH_Operator-->
                        </select>
                    </td>
                </tr>
                
                <tr>
                    <td></td>
                    <td>
                        <button type="submit">{{ isEdit ? "Simpan Perubahan" : "Tambah Pengguna" }}</button>
                    </td>
                </tr>
                </tbody>
            </table>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        user: {
            type: Object,
            default: () => ({}),
        },
        isEdit: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            form: {
                id: "",
                name: "",
                email: "",
                password: "",
                role: "",
            },
        };
    },
    watch: {
        user: {
            immediate: true,
            handler(newUser) {
                if (this.isEdit) {
                    this.form = { ...newUser };
                } else {
                    this.form = {
                        id: "",
                        name: "",
                        email: "",
                        password: "",
                        role: "",
                    };
                }
            },
        },
    },
    methods: {
        submitForm() {
            if (
                this.form.id &&
                this.form.name &&
                this.form.email &&
                this.form.role
            ) {
                this.$emit("submit", this.form);
            }
        },
    },
    emits: ["submit"],
};
</script>

<style scoped>
table {
    width: 100%;
    border-collapse: collapse;
}
td {
    padding: 10px;
    border: 1px solid #ddd;
}
input[type="text"],
input[type="email"],
input[type="password"] {
    width: 100%;
    padding: 8px;
    box-sizing: border-box;
}
button[type="submit"] {
    background-color: #4caf50;
    color: white;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}
button[type="submit"]:hover {
    background-color: #45a049;
}
</style>
