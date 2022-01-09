<template>
  <transition name="modal-animation">
    <div v-show="addusermodalActive" class="modal">
      <transition name="modal-animation-inner">
        <div v-show="addusermodalActive" class="modal-inner">
          <i @click="close" class="far fa-times-circle"></i>
          <!-- Modal Content -->
          <form>
            <div class="form-group row">
              <label for="name" class="col-sm-2 col-form-label lbl">Name</label>
              <div class="col-sm-10">
                <input
                  type="name"
                  class="form-control"
                  id="name"
                  v-model="userDataAdded.name"
                />
              </div>
            </div>
            <div class="form-group row">
              <label for="email" class="col-sm-2 col-form-label lbl"
                >Email</label
              >
              <div class="col-sm-10">
                <input
                  type="email"
                  class="form-control"
                  id="email"
                  v-model="userDataAdded.email"
                />
              </div>
            </div>
            <div class="form-group row">
              <label for="phone" class="col-sm-2 col-form-label lbl"
                >Phone</label
              >
              <div class="col-sm-10">
                <input
                  type="text"
                  class="form-control"
                  id="phone"
                  v-model="userDataAdded.phone"
                />
              </div>
            </div>
            <div class="form-group row">
              <label for="website" class="col-sm-2 col-form-label lbl"
                >Website</label
              >
              <div class="col-sm-10">
                <input
                  type="text"
                  class="form-control"
                  id="website"
                  v-model="userDataAdded.website"
                />
              </div>
            </div>
            <p v-if="validityflag_add">Please fill all the feilds</p>
          </form>
          <button @click="addUser" type="button">Add User</button>&nbsp;
          <button @click="close" type="button">Cancel</button>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      userDataAdded: this.userData,
      validityflag_add: this.validityflag,
    };
  },
  props: ["addusermodalActive", "userData", "validityflag"],

  methods: {
    addUser() {
      if (
        !this.userDataAdded.name ||
        !this.userDataAdded.email ||
        !this.userDataAdded.phone ||
        !this.userDataAdded.website
      ) {
        this.validityflag_add = true;
        return;
      }
      fetch("http://localhost:3000/data/", {
        method: "POST",
        body: JSON.stringify(this.userDataAdded),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => console.log("User Added " + JSON.stringify(json)));
      this.close();
      this.validityflag_add = false;
      this.$parent.loadData();
    },

    close() {
      this.$emit("toggleAddModal");
    },
  },
};
</script>

<style scoped>
.lbl {
  margin-top: 6px;
  text-align: end;
}
.modal-animation-enter-active,
.modal-animation-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-animation-enter-from,
.modal-animation-leave-to {
  opacity: 0;
}

.modal-animation-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}

.modal-animation-inner-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-animation-inner-enter-from {
  opacity: 0;
  transform: scale(0.8);
}

.modal-animation-inner-leave-to {
  transform: scale(0.8);
}

.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  background-color: rgba(134, 122, 122, 0.7);
}
.modal-inner {
  position: relative;
  max-width: 640px;
  width: 80%;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06);
  background-color: #fff;
  padding: 64px 16px;
}
i {
  position: absolute;
  top: 15px;
  right: 15px;
  font-size: 20px;
  cursor: pointer;
}

i:hover {
  color: crimson;
}

button {
  padding: 20px 30px;
  border: none;
  font-size: 16px;
  background-color: crimson;
  color: #fff;
  cursor: pointer;
}
</style>