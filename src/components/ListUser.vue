<template>
  <div class="container pt-4">
    <label class="is-size-5 has-text-weight-bold is-pulled-left"
      >List User</label
    >
    <div>
      <DataTable
        :columns="columns"
        :data="data"
        class="table is-hoverable is-fullwidth"
        width="100%"
      >
        <thead>
          <tr>
            <th>ID</th>
            <th>Avatar</th>
            <th>Email</th>
            <th>FirstName</th>
            <th>LastName</th>
            <th></th>
            <th></th>
          </tr>
        </thead>
      </DataTable>
    </div>
    <div>
      <div v-if="openEditModal" class="modal is-active">
        <div class="modal-background"></div>
        <div class="modal-card">
          <header class="modal-card-head">
            <p class="modal-card-title">Update User</p>
            <button
              class="delete"
              aria-label="close"
              v-on:click="openEditModal = !openEditModal"
            ></button>
          </header>
          <section class="modal-card-body has-text-left">
            <div class="control">
              <label class="title is-6">FirstName:</label>&nbsp;&nbsp;
              <input
                class="input is-small"
                type="text"
                placeholder="FirstName"
                v-model="firstName"
              />
            </div>
            <br />
            <div class="control">
              <label class="title is-6">LastName:</label>&nbsp;&nbsp;
              <input
                class="input is-small"
                type="text"
                placeholder="LastName"
                v-model="lastName"
              />
            </div>
          </section>
          <footer class="modal-card-foot">
            <button class="button is-success" v-on:click="updateUser">
              Save changes
            </button>
            <button class="button" v-on:click="openEditModal = !openEditModal">
              Cancel
            </button>
          </footer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DataTable from "datatables.net-vue3";

const baseURL = "https://reqres.in/api";

export default {
  name: "ListUser",
  components: {
    DataTable,
  },
  setup() {
    const columns = [
      { data: "id" },
      {
        data: null,
        orderable: false,
        render: (data) => {
          return '<figure class="image is-48x48"><img class="is-rounded" src="'+data.avatar+'"></figure>';
        },
      },
      { data: "email" },
      { data: "first_name" },
      { data: "last_name" },
      {
        data: null,
        orderable: false,
        render: (data) => {
          return (
            '<button class="button is-warning is-small" onclick="window.parent.onOpenEditModal(' +
            data.id +
            ')">Edit</button>'
          );
        },
      },
      {
        data: null,
        orderable: false,
        render: (data) => {
          return (
            '<button class="button is-danger is-small" onclick="window.parent.confirmDeleteUser(' +
            data.id +
            ')">Delete</button>'
          );
        },
      },
    ];
    return {
      columns,
    };
  },
  data() {
    return {
      data: [],
      id: "",
      firstName: "",
      lastName: "",
      openEditModal: false,
    };
  },
  methods: {
    async getUsers() {
      try {
        const res = await fetch(`${baseURL}/users`);
        if (!res.ok) {
          const message = `An error has occured: ${res.status} - ${res.statusText}`;
          throw new Error(message);
        }
        const data = await res.json();
        this.data = data.data;
      } catch (e) {
        throw new Error(e.message);
      }
    },
    async updateUser() {
      try {
        const user = {
          first_name: this.firstName,
          last_name: this.lastName,
        };
        const res = await fetch(`${baseURL}/users/${this.id}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(user),
        });

        if (!res.ok) {
          const message = `An error has occured: ${res.status} - ${res.statusText}`
          throw new Error(message);
        }

        const data = await res.json();
        this.data.find((e) => e.id == this.id).first_name = data.first_name
        this.data.find((e) => e.id == this.id).last_name = data.last_name
        alert("Update user success!")
        this.openEditModal = !this.openEditModal
      } catch (e) {
        throw new Error(e.message);
      }
    },
    async deleteUser() {
      try {
        const res = await fetch(`${baseURL}/users/${this.id}`, {
          method: "DELETE",
        });

        if (!res.ok) {
          const message = `An error has occured: ${res.status} - ${res.statusText}`;
          throw new Error(message);
        }

        const userIndex = this.data.findIndex((e) => e.id == this.id);
        this.data.splice(userIndex, 1)
        alert("Delete user success!")
      } catch (e) {
        throw new Error(e.message);
      }
    },
  },
  mounted() {
    this.getUsers();

    document.addEventListener("DOMContentLoaded", function () {
      let inputs = document.querySelectorAll("input[type=search]")
      inputs.forEach(function (input) {
        input.setAttribute("class", "input is-small")
      });
    });

    window.onOpenEditModal = (id) => {
      this.openEditModal = true;
      this.id = id;
      this.firstName = this.data.find((e) => e.id == id).first_name
      this.lastName = this.data.find((e) => e.id == id).last_name
    };

    window.confirmDeleteUser = (id) => {
      this.id = id;
      if (confirm("Do you want to delete this user?")) {
        this.deleteUser();
      }
    };
  },
};
</script>

<style>
.dataTables_length {
  display: none;
}

.dataTables_filter {
  margin-top: 20px;
  margin-bottom: 20px;
  text-align: right;
}

.input {
  width: 200px !important;
}

table.dataTable thead .sorting,
table.dataTable thead .sorting_asc,
table.dataTable thead .sorting_desc,
table.dataTable thead .sorting_asc_disabled,
table.dataTable thead .sorting_desc_disabled {
  cursor: pointer;
  *cursor: hand;
  background-repeat: no-repeat;
  background-position: center right;
}
table.dataTable thead .sorting {
  background-image: url("../assets/images/sort_both.png");
}
table.dataTable thead .sorting_asc {
  background-image: url("../assets/images/sort_asc.png") !important;
}
table.dataTable thead .sorting_desc {
  background-image: url("../assets/images/sort_desc.png") !important;
}
table.dataTable thead .sorting_asc_disabled {
  background-image: url("../assets/images/sort_asc_disabled.png");
}
table.dataTable thead .sorting_desc_disabled {
  background-image: url("../assets/images/sort_desc_disabled.png");
}

.dataTables_wrapper .dataTables_paginate {
  float: right;
  text-align: right;
  padding-top: 0.25em;
}
.dataTables_wrapper .dataTables_paginate .paginate_button {
  box-sizing: border-box;
  display: inline-block;
  min-width: 1.5em;
  padding: 0.5em 1em;
  margin-left: 2px;
  text-align: center;
  text-decoration: none !important;
  cursor: pointer;
  *cursor: hand;
  color: #333 !important;
  border: 1px solid transparent;
  border-radius: 2px;
}
.dataTables_wrapper .dataTables_paginate .paginate_button.current, .dataTables_wrapper .dataTables_paginate .paginate_button.current:hover {
  color: white !important;
  border: 1px solid #42b883;
  background-color: #42b883;
}
.dataTables_wrapper .dataTables_paginate .paginate_button.disabled, .dataTables_wrapper .dataTables_paginate .paginate_button.disabled:hover, .dataTables_wrapper .dataTables_paginate .paginate_button.disabled:active {
  cursor: default;
  color: #666 !important;
  border: 1px solid #979797;
  background: white;
  box-shadow: none;
}
.dataTables_wrapper .dataTables_paginate .paginate_button:hover {
  color: white !important;
  border: 1px solid #42b883;
  background-color: #42b883;
}
.dataTables_wrapper .dataTables_paginate .paginate_button:active {
  outline: none;
  background-color: #42b883;
}
.dataTables_wrapper .dataTables_paginate .ellipsis {
  padding: 0 1em;
}

</style>
