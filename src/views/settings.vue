<template>
  <b-container fluid class="ahmed">
      <b-row>
          <b-col lg="12" class="my-1">
          <div class="title"> FRAY MAILS</div>
          </b-col>
      </b-row>
      <b-row>
      <b-col lg="2" class="my-1">
        <div style="text-align:center;">Useremail:</div>
      </b-col>
      <b-col lg="10" class="my-1">
        <input type="text" id="email" class="filter"  disabled />
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="2" class="my-1">
        <div style="text-align:center;">Username:</div>
      </b-col>
      <b-col lg="10" class="my-1">
        <input type="text" id="name" class="filter"/>
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="2" class="my-1">
        <div style="text-align:center;">Passward:</div>
      </b-col>
      <b-col lg="10" class="my-1">
        <input type="text" id="password" class="filter"/>
      </b-col>
    </b-row>
    <b-row>
        <b-col lg="2" class="my-1">
        <b-button variant="primary" class="filter" @click="toggle_on">Add Contact</b-button>
        </b-col>
        <b-col lg="8" class="my-1">
        </b-col>
        <b-col lg="2" class="my-1">
        <b-button variant="success" class="filter" @click="modify">OK</b-button>
        </b-col>
        <b-col lg="12" class="my-1">
        <b-container v-show="this.on" class="form">
            <b-row>
            <b-col lg="12" class="my-1">
                <input type="text" id="friend" class="filter"/>
            </b-col>
            </b-row>
            <b-row>
            <b-col sm="10" md="10" class="my-1">
              <div></div>
            </b-col>
            <b-col sm="2" md="2" class="my-1">
              <b-button
                size="lg"
                variant="dark"
                class="filter"
                @click="addContact"
              >
                Add
              </b-button>
            </b-col>
            </b-row>
        </b-container>
        </b-col>
    </b-row>
    <b-row>

    </b-row>
    <b-row>
        <b-col lg="10" class="my-1">
        </b-col>
        <b-col lg="2" class="my-1">
        <b-button variant="danger" class="filter" @click="log_out">log out</b-button>
        </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
        on: false
    }
  },
   mounted() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const emaill = urlParams.get("email");
      let a = {
          email:emaill+ "@fray.com",
      }
      this.fetching(a);
  },
   methods: {
    toggle_on() {
      this.on = !this.on;
    },
    addContact(){
      this.on = !this.on;
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      let a = {
        Useremail: email + "@fray.com",
        FriendEmail:document.getElementById("friend").value
      };
      fetch("http://localhost:8085//AddFriend", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          if (data == "true") {
             console.log("add contact");
          } else {
            alert(data);
          }
        });
    },
    fetching(a){
        fetch("http://localhost:8085//GetContact", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          this.handle(data);
        });
    },
    handle(v){
        var obj = JSON.parse(v);
        document.getElementById("email").value=obj.email;
        document.getElementById("name").value=obj.name;
        document.getElementById("password").value=obj.password;
    },
    modify(){
        let a={
            email:document.getElementById("email").value,
            name:document.getElementById("name").value,
            password: document.getElementById("password").value
        };
        fetch("http://localhost:8085//ModifyContact", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
            data;
         const queryString = window.location.search;
         const urlParams = new URLSearchParams(queryString);
         const email = urlParams.get("email");
         this.$router.push("/view?email=" + email);
        });
    },
    log_out(){
        this.$router.push("/");
    }

}
}


</script>


<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap.scss";
@import "node_modules/bootstrap-vue/src/index.scss";
</style>
<style lang="scss">
.ahmed {
  width: 1200px;
  height: 100%;
}
.title{
    text-align: center;
    color:white;
    background-color:RoyalBlue;
    font-family: "Lucida Console", "Courier New", monospace;
    font-size: 30px;
}
.sorting {
  padding: 10px 20px 10px 20px;
  color: rgb(0, 0, 0);
  width: 100%;
  font-size: 25px;
  text-align: center;
}
.pils:hover {
  background: rgba(1, 191, 248, 0.3);
}
.filter {
  width: 100%;
  height: 100%;
}
.form {
  border: 2px solid red;
  border-radius: 5px;
}
.ahmed {
  width: 1200px;
  height: 100%;
}
.delete {
  width: 35px;
  height: 30px;
  margin: 10px 20px 10px 20px;
  background-color: red;
  color: white;
  font-size: 15px;
  border-radius: 8px;
  border: 2px solid black;
}
.delete2:hover {
  box-shadow: 0 12px 16px 0 rgba(0, 0, 0, 0.24),
    0 17px 50px 0 rgba(0, 0, 0, 0.19);
}
.restore {
  background-color: green;
}
.rewrite {
  background-color: blue;
}
</style>