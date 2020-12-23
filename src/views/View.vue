<template>
  <b-container fluid class="ahmed">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
    <!-- User Interface controls -->
    <b-row>
      <b-col lg="2" class="my-1">
        <div class=" ">Sort:</div>
      </b-col>
      <b-col lg="10" class="my-1">
        <b-form-select
          v-model="selected"
          :options="options"
          id="Sort"
          @change="sort()"
        ></b-form-select>
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="2" class="my-1">
        <b-form-group class="filter" v-slot="{ ariaDescribedby }">
          <b-form-radio
            v-model="selected"
            :aria-describedby="ariaDescribedby"
            name="some-radios"
            value="A"
            >Sender</b-form-radio
          >
          <b-form-radio
            v-model="selected"
            :aria-describedby="ariaDescribedby"
            name="some-radios"
            value="B"
            >Subject</b-form-radio
          >
        </b-form-group>
      </b-col>
      <b-col lg="8" class="my-1">
        <input type="text" id="filter" class="filter" />
      </b-col>
      <b-col lg="2" class="my-1">
        <b-button
          type="submit"
          variant="primary"
          size="md"
          class="filter"
          @click="Filter()"
          >Filter</b-button
        >
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="2" class="my-1">
        <div class=" ">Search:</div>
      </b-col>
      <b-col lg="7" class="my-1">
        <input type="text" id="Search" class="filter" @keydown="search()" />
      </b-col>
      <b-col lg="3" class="my-1">
        <b-form-select v-model="selected2" :options="options2"></b-form-select>
      </b-col>
    </b-row>
    <b-row>
      <b-col sm="2" md="2" class="my-1">
        <b-button size="lg" @click="toggle_on">
          Compose
        </b-button>
      </b-col>
      <b-col sm="10" md="10" class="my-1">
        <b-pagination
          v-model="currentPage"
          :total-rows="totalRows"
          :per-page="perPage"
          align="fill"
          size="lg"
          class="my-0"
        ></b-pagination>
      </b-col>
      <b-container v-show="this.on" class="form">
        <form>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>TO:</div>
            </b-col>
            <b-col lg="10" class="my-1">
              <input type="text" class="filter" id="To" />
            </b-col>
          </b-row>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>Subject:</div>
            </b-col>
            <b-col lg="10" class="my-1">
              <input type="text" class="filter" id="Subject" />
            </b-col>
          </b-row>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>Importance:</div>
            </b-col>
            <b-col lg="10" class="my-1">
              <b-form-select
                id="priority"
                v-model="selected3"
                :options="options3"
              ></b-form-select>
            </b-col>
          </b-row>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>Body:</div>
            </b-col>
            <b-col lg="12" class="my-1">
              <b-form-textarea
                id="textarea"
                v-model="text"
                placeholder="Enter something..."
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-col>
          </b-row>
          <b-row>
            <div id="attachments"></div>
          </b-row>
          <b-row>
            <b-col sm="10" md="10" class="my-1">
              <div></div>
            </b-col>
            <b-col sm="2" md="2" class="my-1">
              <b-button
                size="lg"
                variant="primary"
                class="filter"
                @click="sendOrDraft('Send')"
              >
                Send
              </b-button>
            </b-col>
          </b-row>
        </form>
      </b-container>
    </b-row>
    <b-row>
      <b-col lg="12" class="my-1">
        <!-- Main table element -->
        <div>
          <b-card no-body>
            <b-tabs pills card vertical>
              <b-tab title="Inbox" active @click="setTargetFolder('Inbox')">
                <b-table
                  :items="items"
                  :fields="fields"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(actions)="row">
                    <b-button size="sm" @click="row.toggleDetails">
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                    <button
                      class="delete"
                      @click="deletee((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-trash"></i>
                    </button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul>
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                    </b-card>
                  </template>
                </b-table>
              </b-tab>
              <b-tab title="Send" @click="setTargetFolder('Send')">
                <b-table
                  :items="items"
                  :fields="fields"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(actions)="row">
                    <b-button size="sm" @click="row.toggleDetails">
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                    <button
                      class="delete"
                      @click="deletee((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-trash"></i>
                    </button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul>
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                    </b-card>
                  </template>
                </b-table>
              </b-tab>
              <b-tab title="Draft" @click="setTargetFolder('Draft')">
                <b-table
                  :items="items"
                  :fields="fields"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(actions)="row">
                    <button class="delete rewrite">
                      <i class="fa fa-edit"></i>
                    </button>
                    <b-button size="sm" @click="row.toggleDetails">
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                    <button
                      class="delete"
                      @click="deletee((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-trash"></i>
                    </button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul>
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                    </b-card>
                  </template>
                </b-table>
              </b-tab>
              <b-tab title="Trash" @click="setTargetFolder('Trash')">
                <b-table
                  :items="items"
                  :fields="fields"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(actions)="row">
                    <button
                      class="delete restore"
                      @click="restore((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-undo"></i>
                    </button>
                    <b-button size="sm" @click="row.toggleDetails">
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                    <button
                      class="delete"
                      @click="deletee((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-trash"></i>
                    </button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul>
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                    </b-card>
                  </template>
                </b-table>
              </b-tab>
              <b-tab title="Contacts" @click="setTargetFolder('Contacts')">
                <b-table
                  :items="items"
                  :fields="fields2"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(name)="row">
                    {{ row.value.first }} {{ row.value.last }}
                  </template>

                  <template #cell(actions)="row">
                    <b-button
                      size="sm"
                      @click="info(row.item, row.index, $event.target)"
                      class="mr-1"
                    >
                      Info modal
                    </b-button>
                    <b-button size="sm" @click="row.toggleDetails">
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul>
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                    </b-card>
                  </template>
                </b-table>

                <!-- Info modal -->
                <b-modal
                  :id="infoModal.id"
                  :title="infoModal.title"
                  ok-only
                  @hide="resetInfoModal"
                >
                  <pre>{{ infoModal.content }}</pre>
                </b-modal>
              </b-tab>
            </b-tabs>
          </b-card>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      targetFolder: "Inbox",
      selected: "A",
      options: [
        { value: "Sender", text: "Sender" },
        { value: "Receiver", text: "Receiver" },
        { value: "Subject", text: "Subject" },
        { value: "Body", text: "Body" },
        { value: "Date", text: "Date" },
        { value: "Importance", text: "Importance" }
      ],
      selected2: "all",
      options2: [
        { value: "sender", text: "Sender" },
        { value: "receiver", text: "Receiver" },
        { value: "subject", text: "Subject" },
        { value: "body", text: "Body" },
        { value: "Date", text: "Date" },
        { value: "piriority", text: "Importance" },
        { value: "all", text: "all" }
      ],
      selected3: 1,
      options3: [
        { value: 1, text: "1" },
        { value: 2, text: "2" },
        { value: 3, text: "3" },
        { value: 4, text: "4" }
      ],
      items: [],
      fields: [
        { key: "from", label: "Sender" },
        { key: "to", label: "Receiver" },
        { key: "subject", label: "Subject" },
        { key: "date", label: "Date" },
        { key: "actions", label: "Actions" }
      ],
      fields2: [
        { key: "user", label: "User Name" },
        { key: "email", label: "Email Address" }
      ],
      on: false,
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      infoModal: {
        id: "info-modal",
        title: "",
        content: ""
      }
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter(f => f.sortable)
        .map(f => {
          return { text: f.label, value: f.key };
        });
    }
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
    this.targetFolder = "Inbox";
    this.selected = "A";
    this.getEmails();
  },
  methods: {
    toggle_on() {
      this.on = !this.on;
      if (this.on == false) {
        this.sendOrDraft("Draft");
        alert("Message Saved to Draft Folder");
      }
    },
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`;
      this.infoModal.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal.id, button);
    },
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
    setTargetFolder(v) {
      console.log(v);
      this.targetFolder = v;
      if (v == "Inbox" || v == "Draft" || v == "Send" || v == "Trash") {
        console.log(this.targetFolder);
        this.getEmails();
      } else {
        console.log(v);
        //here we load contacts
      }
    },
    getEmails() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log("get Emails");
      let a = {
        email: email + "@fray.com",
        targetFolder: this.targetFolder
      };
      fetch("http://localhost:8085//GetEmails", {
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
    sendOrDraft(v) {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      let a = {
        from: email + "@fray.com",
        to: document.getElementById("To").value,
        priority: document.getElementById("priority").value,
        body: document.getElementById("textarea").value,
        subject: document.getElementById("Subject").value,
        name: document.getElementById("Subject").value,
        date: "14/2/2000",
        folder: "Inbox"
      };
      document.getElementById("To").value = "";
      document.getElementById("priority").value = 1;
      document.getElementById("textarea").value = "";
      document.getElementById("Subject").value = "";
      this.on = false;
      fetch("http://localhost:8085//" + v, {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          //alert(data);
          if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }
        });
    },
    sort() {
      console.log("i'm in sort");
      this.selected = "A";
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      console.log(document.getElementById("Sort").value + "\ni'm in sort");
      let a = {
        email: email + "@fray.com",
        SortType: document.getElementById("Sort").value,
        targetFolder: this.targetFolder
      };
      fetch("http://localhost:8085//Sort", {
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
          /*if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }*/
        });
    },
    Filter() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      if (this.selected == "A") {
        var type = "Sender";
      } else {
        type = "Subject";
      }
      if (document.getElementById("filter").value == "") {
        alert("Enter the Word you want to filter at Filter text box");
      }
      console.log(email);
      let a = {
        filterType: type,
        email: email + "@fray.com",
        targetFolder: this.targetFolder,
        Word: document.getElementById("filter").value
      };
      let b = [];
      b.push({ email: this.items[0] }, { khalo: "ahmed (ana baba yala)" });
      //a.push({khalo :"ahmed (ana baba yala)"});
      console.log("\n\n" + JSON.stringify(b));
      fetch("http://localhost:8085//Filter", {
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
          /*if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }*/
        });
    },
    search() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      console.log("i'm in search");
      let a = {
        str : document.getElementById("Search").value,
        region : this.targetFolder,
        emailPart: "all",
        Useremail: email + "@fray.com"
      };
      fetch("http://localhost:8085//Search", {
        method: "GET",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          /*if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }*/
        });
    },
    deletee(v) {
      console.log(v);
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      let a = [];
      a.push({ mail: this.items[v] });
      a.push({ email: email + "@fray.com" });
      a.push({ targetFolder: this.targetFolder });
      //this.on = false;
      fetch("http://localhost:8085//Delete", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }
        });
    },
    restore(v) {
      console.log(v);
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      let a = [];
      a.push({ mail: this.items[v] });
      a.push({ email: email + "@fray.com" });
      //this.on = false;
      fetch("http://localhost:8085//Restore", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }
        });
    },
    handle(a) {
      let arr = [];
      var obj = JSON.parse(a);
      for (let i = obj.length - 1; i >= 0; i--) {
        arr.push(obj[i]);
      }
      this.items = arr;
    }
  }
};
</script>
<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap.scss";
@import "node_modules/bootstrap-vue/src/index.scss";
</style>
<style lang="scss">
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
