<template>
  <b-container fluid class="ahmed">
    <!-- User Interface controls -->
    <b-row>
      <b-col lg="2" class="my-1">
        <div class=" ">Sort:</div>
      </b-col>
      <b-col lg="10" class="my-1">
        <b-form-select v-model="selected" :options="options"></b-form-select>
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
        <b-button type="submit" variant="primary" size="md" class="filter"
          >Filter</b-button
        >
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="2" class="my-1">
        <div class=" ">Search:</div>
      </b-col>
      <b-col lg="7" class="my-1">
        <input type="text" id="Search" class="filter" />
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
              <input type="text" class="filter" />
            </b-col>
          </b-row>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>Subject:</div>
            </b-col>
            <b-col lg="10" class="my-1">
              <input type="text" class="filter" />
            </b-col>
          </b-row>
          <b-row>
            <b-col lg="2" class="my-1">
              <div>Importance:</div>
            </b-col>
            <b-col lg="10" class="my-1">
              <b-form-select
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
            <b-col sm="10" md="10" class="my-1">
              <div></div>
            </b-col>
            <b-col sm="2" md="2" class="my-1">
              <b-button size="lg" variant="primary" class="filter">
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
              <b-tab title="Inbox" active>
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
              <b-tab title="Send">
                <b-table
                  :items="items"
                  :fields="fields"
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
              <b-tab title="Draft">
                <b-table
                  :items="items"
                  :fields="fields"
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
              <b-tab title="Trash">
                <b-table
                  :items="items"
                  :fields="fields"
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
              <b-tab title="Contacts">
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
      selected: "",
      options: [
        { value: "sender", text: "Sender" },
        { value: "receiver", text: "Receiver" },
        { value: "subject", text: "Subject" },
        { value: "body", text: "Body" },
        { value: "Date", text: "Date" },
        { value: "piriority", text: "Importance" }
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
      items: [
        {
          from: "ahmed@fray.com",
          to: "ali@fray.com",
          subject: "football",
          date: "24/2/2010",
          body: "hi ahmed i love football"
        },
        {
          from: "saad@fray.com",
          to: "ali@fray.com",
          subject: "volleyball",
          date: "24/4/2010"
        },
        {
          from: "ttt@fray.com",
          to: "ali@fray.com",
          subject: "tennis",
          date: "12/2/2010"
        },
        {
          from: "ahmed@fray.com",
          to: "ali@fray.com",
          subject: "football",
          date: "24/2/2010",
          body: "hi ahmed i love football"
        },
        {
          from: "saad@fray.com",
          to: "ali@fray.com",
          subject: "volleyball",
          date: "24/4/2010"
        },
        {
          from: "ttt@fray.com",
          to: "ali@fray.com",
          subject: "tennis",
          date: "12/2/2010"
        },
        {
          from: "ahmed@fray.com",
          to: "ali@fray.com",
          subject: "football",
          date: "24/2/2010",
          body: "hi ahmed i love football"
        },
        {
          from: "saad@fray.com",
          to: "ali@fray.com",
          subject: "volleyball",
          date: "24/4/2010"
        },
        {
          from: "ttt@fray.com",
          to: "ali@fray.com",
          subject: "tennis",
          date: "12/2/2010"
        },
        {
          from: "ahmed@fray.com",
          to: "ali@fray.com",
          subject: "football",
          date: "24/2/2010",
          body: "hi ahmed i love football"
        },
        {
          from: "saad@fray.com",
          to: "ali@fray.com",
          subject: "volleyball",
          date: "24/4/2010"
        },
        {
          from: "ttt@fray.com",
          to: "ali@fray.com",
          subject: "tennis",
          date: "12/2/2010"
        }
      ],
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
  },
  methods: {
    toggle_on() {
      this.on = !this.on;
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
</style>
