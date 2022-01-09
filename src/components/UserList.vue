<template>
  <div class="content">
    <div class="row">
      <button class="btn btn-primary addUser" @click="addUserData(respData)">
        Add User
      </button>
    </div>
    <div class="row">
      <!-- div to display the data in cards -->
      <div class="column" v-for="(user, index) in respData" :key="user.id">
        <div class="card">
          <img :src="getImgUrl(index)" />
          <h4>{{ user.name }}</h4>
          <p>
            <i class="far fa-envelope"></i>&nbsp;
            <a :href="'mailto:' + user.email">{{ user.email }}</a>
          </p>
          <p><i class="fas fa-phone"></i>&nbsp;{{ user.phone }}</p>
          <p>
            <i class="fas fa-globe"></i>&nbsp;
            <a :href="user.website">{{ user.website }}</a>
          </p>
          <div class="card-footer">
            <button @click="notFav" v-show="notfav">
              <i class="far fa-heart"></i>
            </button>
            <button @click="isfavSelected" v-show="fav">
              <i class="fas fa-heart"></i>
            </button>
            <button @click="editUserData(user)">
              <i class="far fa-edit"></i>
            </button>
            <button @click="deleteUser(user)">
              <i class="fas fa-trash-alt"></i>
            </button>
          </div>
        </div>
      </div>
      <!-- Cards div end -->
      <div
        v-if="
          respData &&
          Object.keys(respData).length === 0 &&
          Object.getPrototypeOf(respData) === Object.prototype
        "
      >
        <h1>No data</h1>
      </div>
    </div>
    <Modal
      :modalActive="modalActive"
      :userData="userData"
      :validityflag="validityflag"
      @toggleModal="toggleModal($event)"
    >
    </Modal>
    <AddUserModal
      :addusermodalActive="addusermodalActive"
      :userData="userData"
      :validityflag="validityflag"
      @toggleAddModal="toggleAddModal($event)"
    >
    </AddUserModal>
    <DeleteModal
      :deletemodalActive="deletemodalActive"
      :userData="userData"
      @toggleDeleteModal="toggleDeleteModal($event)"
    >
    </DeleteModal>
  </div>
</template>
<script>
import Modal from "../components/Modal.vue";
import DeleteModal from "../components/DeleteModal.vue";
import AddUserModal from "../components/AddUserModal.vue";
export default {
  name: "UserList",
  components: {
    Modal,
    DeleteModal,
    AddUserModal,
  },
  data() {
    return {
      respData: {},
      modalActive: false,
      deletemodalActive: false,
      addusermodalActive: false,
      fav: false,
      notfav: true,
      datalength: 0,
      validityflag: false,
      userData: {
        id: "",
        name: "",
        username: "",
        email: "",
        phone: "",
        website: "",
        company: {
          name: "",
          catchPhrase: "",
          bs: "",
        },

        address: {
          street: "",
          suite: "",
          city: "",
          zipcode: "",
          geo: {
            lat: "",
            lon: "",
          },
        },
      },
    };
  },
  beforeMount() {
    this.loadData();
  },
  methods: {
    // notFav() {
    //   this.notfav = false;
    //   this.fav = true;
    // },
    // isfavSelected() {
    //   this.fav = false;
    //   this.notfav = true;
    // },
    getImgUrl(idx) {
      return require("../assets/" + "avatar_" + idx + ".jpg");
    },

    editUserData(user) {
      this.userData.id = user.id;
      this.userData.name = user.name;
      this.userData.username = user.username;
      this.userData.email = user.email;
      this.userData.phone = user.phone;
      this.userData.website = user.website;
      this.modalActive = !this.modalActive;
    },
    deleteUser(user) {
      console.log(user);
      this.userData.id = user.id;
      this.userData.name = user.name;
      this.deletemodalActive = !this.deletemodalActive;
    },
    addUserData(user) {
      for (const key in this.userData) {
        delete this.userData[key];
      }
      this.userData.id = user.length + 1;
      this.addusermodalActive = !this.addusermodalActive;
    },
    toggleModal() {
      //for update modal
      this.modalActive = !this.modalActive;
      this.validityflag = false;
      this.loadData();
    },
    toggleAddModal() {
      // for Add new user modal
      this.addusermodalActive = !this.addusermodalActive;
      this.validityflag = false;
      this.loadData();
    },
    toggleDeleteModal() {
      // for delete user modal
      this.deletemodalActive = !this.deletemodalActive;
      this.loadData();
    },
    async loadData() {
      // to fetch the data from the api
      return await fetch("http://localhost:3000/data/")
        .then((res) => {
          // a non-200 response code
          if (!res.ok) {
            // create error instance with HTTP status text
            const error = new Error(res.statusText);
            error.json = res.json();
            throw error;
          }

          return res.json();
        })
        .then((json) => {
          // set the response data
          this.validityflag = false;
          this.respData = json;
        })
        .catch((err) => {
          this.error.value = err;
          // In case a custom JSON error response was provided
          if (err.json) {
            return err.json.then((json) => {
              // set the JSON response message
              this.error.value.message = json.message;
            });
          }
        });
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

/* body {
  font-family: Arial, Helvetica, sans-serif;
} */
.content {
  margin-top: -10px;
}

/* Float four columns side by side */
.column {
  float: left;
  width: 25%;
  padding: 15px 20px;
}
.column img {
  width: 100%;
}

.addUser {
  float: right;
  margin-top: 20px;
  margin-right: 20px;
}
/* Remove extra left and right margins, due to padding */
.row {
  margin: 10px 10px;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}
.card {
  /* box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 70); */
  box-shadow: 5px 10px #9e1111;
  padding: 16px;
  text-align: left;
  font-size: 15px;
  background-color: #f1f1f1;
  border-radius: 10px;
}
.card img {
  text-align: center;
  border-radius: 10px;
}
.card-footer {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}
.card-footer button {
  border: none;
  cursor: pointer;
}

/* Responsive columns */
@media screen and (max-width: 600px) {
  .column {
    width: 100%;
    display: block;
    margin-bottom: 20px;
  }
}

/* Style the counter cards */
.card {
  /* box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2); */
  box-shadow: 6px 6px rgba(113, 110, 110, 0.2);
  padding: 16px;
  text-align: left;
  font-size: 15px;
  background-color: #f1f1f1;
}
</style>