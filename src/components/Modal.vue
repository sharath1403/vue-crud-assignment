<template>
  <transition name="modal-animation">
    <div v-show="modalActive" class="modal">
      <transition name="modal-animation-inner">
        <div v-show="modalActive" class="modal-inner">
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
                  v-model="userDataUpdated.name"
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
                  v-model="userDataUpdated.email"
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
                  v-model="userDataUpdated.phone"
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
                  v-model="userDataUpdated.website"
                />
              </div>
            </div>

            <p v-if="validityflag_ed">Please fill all the feilds</p>
          </form>
          <button @click="update" type="button">Update</button>
          &nbsp;
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
      userDataUpdated: this.userData,
      validityflag_ed: this.validityflag,
    };
  },
  props: ["modalActive", "userData", "validityflag"],

  methods: {
    update() {
      if (
        !this.userDataUpdated.name ||
        !this.userDataUpdated.email ||
        !this.userDataUpdated.phone ||
        !this.userDataUpdated.website
      ) {
        this.validityflag_ed = true;
        return;
      }
      fetch("http://localhost:3000/data/" + this.userDataUpdated.id, {
        method: "PUT",
        body: JSON.stringify(this.userDataUpdated),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => console.log("Updated " + JSON.stringify(json)));
      this.close();
      this.validityflag_ed = false;
      this.$parent.loadData();
    },
    close() {
      this.$emit("toggleModal");
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