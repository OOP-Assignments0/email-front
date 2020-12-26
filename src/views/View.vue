<template>
  <b-container fluid class="ahmed" id = "cont">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
    <!-- User Interface controls -->
    <b-row>
      <b-col lg="11" class="my-1">
        <div class="title">FRAY MAILS</div>
      </b-col>
      <b-col lg="1" class="my-1">
        <b-button class="filter" @click="open_settings">
          <i class="fa fa-cogs"></i>
        </b-button>
      </b-col>
    </b-row>
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
        <input type="text" id="Search" class="filter" @keyup="search()" />
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
            <input type="file" id="file" />
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
                    <b-button
                      size="sm"
                      @click="
                        row.toggleDetails();
                        arraying(row.item);
                      "
                    >
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
                      <ul v-show="false">
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                      <b-container class="form">
                        <form>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>From:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.from"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>TO:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.to"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Subject:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.subject"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Importance:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.priority"
                                disabled
                              />
                            </b-col>
                          </b-row>
                            <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Date:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.date"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Body:</div>
                            </b-col>
                            <b-col lg="12" class="my-1">
                              <b-form-textarea
                                v-model="row.item.body"
                                placeholder="Enter something..."
                                rows="3"
                                max-rows="6"
                                disabled
                              ></b-form-textarea>
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="12" class="my-1">
                              <b-table
                                :items="items3"
                                :fields="fields3"
                                :current-page="currentPage"
                                :per-page="perPage"
                                stacked="md"
                                show-empty
                                small
                              >
                                <template #cell(actions)="row">
                                  <b-button
                                    class="filter"
                                    variant="primary"
                                    @click="download(path, row.item.name)"
                                  >
                                    Dowload
                                  </b-button>
                                </template>
                              </b-table>
                            </b-col>
                          </b-row>
                        </form>
                      </b-container>
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
                    <b-button
                      size="sm"
                      @click="
                        row.toggleDetails();
                        arraying(row.item);
                      "
                    >
                      {{ row.detailsShowing ? "Hide" : "Show" }} Details
                    </b-button>
                    <b-button
                      class="delete"
                      @click="deletee((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-trash"></i>
                    </b-button>
                  </template>

                  <template #row-details="row">
                    <b-card>
                      <ul v-show="false">
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                      <b-container class="form">
                        <form>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>From:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.from"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>TO:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.to"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Subject:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.subject"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Importance:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.priority"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Date:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.date"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Body:</div>
                            </b-col>
                            <b-col lg="12" class="my-1">
                              <b-form-textarea
                                v-model="row.item.body"
                                placeholder="Enter something..."
                                rows="3"
                                max-rows="6"
                                disabled
                              ></b-form-textarea>
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="12" class="my-1">
                              <b-table
                                :items="items3"
                                :fields="fields3"
                                :current-page="currentPage"
                                :per-page="perPage"
                                stacked="md"
                                show-empty
                                small
                              >
                                <template #cell(actions)="row">
                                  <b-button
                                    class="filter"
                                    variant="primary"
                                    @click="download(path, row.item.name)"
                                  >
                                    Dowload
                                  </b-button>
                                </template>
                              </b-table>
                            </b-col>
                          </b-row>
                        </form>
                      </b-container>
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
                    <button
                      class="delete rewrite"
                      @click="rewiteMessage((currentPage - 1) * 10 + row.index)"
                    >
                      <i class="fa fa-edit"></i>
                    </button>
                    <b-button
                      size="sm"
                      @click="
                        row.toggleDetails();
                        arraying(row.item);
                      "
                    >
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
                      <ul v-show="false">
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                      <b-container class="form">
                        <form>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>From:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.from"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>TO:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.to"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Subject:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.subject"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Importance:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.priority"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Date:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.date"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Body:</div>
                            </b-col>
                            <b-col lg="12" class="my-1">
                              <b-form-textarea
                                v-model="row.item.body"
                                placeholder="Enter something..."
                                rows="3"
                                max-rows="6"
                                disabled
                              ></b-form-textarea>
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="12" class="my-1">
                              <b-table
                                :items="items3"
                                :fields="fields3"
                                :current-page="currentPage"
                                :per-page="perPage"
                                stacked="md"
                                show-empty
                                small
                              >
                                <template #cell(actions)="row">
                                  <b-button
                                    class="filter"
                                    variant="primary"
                                    @click="download(path, row.item.name)"
                                  >
                                    Dowload
                                  </b-button>
                                </template>
                              </b-table>
                            </b-col>
                          </b-row>
                        </form>
                      </b-container>
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
                    <b-button
                      size="sm"
                      @click="
                        row.toggleDetails();
                        arraying(row.item);
                      "
                    >
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
                      <ul v-show="false">
                        <li v-for="(value, key) in row.item" :key="key">
                          {{ key }}: {{ value }}
                        </li>
                      </ul>
                      <b-container class="form">
                        <form>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>From:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                id="from"
                                :value="row.item.from"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>TO:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                               
                                :value="row.item.to"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Subject:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                
                                :value="row.item.subject"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Importance:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                               
                                :value="row.item.priority"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Date:</div>
                            </b-col>
                            <b-col lg="10" class="my-1">
                              <input
                                type="text"
                                class="filter"
                                :value="row.item.date"
                                disabled
                              />
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="2" class="my-1">
                              <div>Body:</div>
                            </b-col>
                            <b-col lg="12" class="my-1">
                              <b-form-textarea
                                v-model="row.item.body"
                                placeholder="Enter something..."
                                rows="3"
                                max-rows="6"
                                disabled
                              ></b-form-textarea>
                            </b-col>
                          </b-row>
                          <b-row>
                            <b-col lg="12" class="my-1">
                              <b-table
                                :items="items3"
                                :fields="fields3"
                                :current-page="currentPage"
                                :per-page="perPage"
                                stacked="md"
                                show-empty
                                small
                              >
                                <template #cell(actions)="row">
                                  <b-button
                                    class="filter"
                                    variant="primary"
                                    @click="download(path, row.item.name)"
                                  >
                                    Dowload
                                  </b-button>
                                </template>
                              </b-table>
                            </b-col>
                          </b-row>
                        </form>
                      </b-container>
                    </b-card>
                  </template>
                </b-table>
              </b-tab>
              <b-tab title="Contacts" @click="getContacts">
                <b-table
                  :items="items"
                  :fields="fields2"
                  :current-page="currentPage"
                  :per-page="perPage"
                  stacked="md"
                  show-empty
                  small
                >
                  <template #cell(actions)="row">
                    <b-button
                      size="sm"
                      class="mr-1  chat"
                      variant="success"
                      @click="chatting((currentPage - 1) * 10 + row.index)"
                    >
                      Chat
                    </b-button>
                    <button
                      class="delete"
                      @click="deleteee((currentPage - 1) * 10 + row.index)"
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
        { key: "name", label: "User Name" },
        { key: "email", label: "Email Address" },
        { key: "actions", label: "Actions" }
      ],
      fields3: [
        { key: "name", label: "Attachments name" },
        { key: "actions", label: "Actions" }
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
      },
      attachments: null,
      attachments_number: 0,
      items3: [],
      path: "",
      email: ""
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
    const th = this;
    this.attachments = new FormData();
    document.getElementById("file").addEventListener("change", this.select);
    document
      .getElementById("attachments")
      .addEventListener("click", function(e) {
        if (e.target && e.target.nodeName == "BUTTON") {
          th.deleteAttach(e.target.id);
        }
      });
  },
  methods: {
    arraying(v) {
      let a = [];
      var i;
      for (i = 1; i < v.attachments.length; i++) {
        let b = {
          name: v.attachments[i]
        };
        a.push(b);
      }
      this.items3 = a;
      if (v.attachments.length > 0) {
        this.path = v.attachments[0];
      }
      console.log("ahmed");
      console.log(this.items3);
    },
    open_settings() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      this.$router.push("/settings?email=" + email);
    },
    select(event) {
      const f = event.target.files;
      if (!this.attachments.has(f[0].name)) {
        this.attachments.set(f[0].name, f[0]);
        this.attachments_number++;
        this.addAttachToHTML(f[0]);
      } else alert("File already excist");
      document.getElementById("file").value = null;
    },
    addAttachToHTML(file) {
      var div = document.createElement("div");
      div.setAttribute("id", file.name);
      var name = document.createElement("input");
      name.setAttribute("id", "attachName");
      name.setAttribute("value", "name: " + file.name);
      name.disabled = true;
      var type = document.createElement("input");
      type.setAttribute("id", "attachType");
      type.setAttribute("value", "type: " + file.type);
      type.disabled = true;
      var size = document.createElement("input");
      size.setAttribute("id", "attachSize");
      size.setAttribute("value", "size: " + file.size + " bytes");
      size.disabled = true;
      var del = document.createElement("button");
      del.setAttribute("id", file.name);
      var text = document.createTextNode("delete");
      del.appendChild(text);
      div.appendChild(name);
      div.appendChild(type);
      div.appendChild(size);
      div.appendChild(del);
      document.getElementById("attachments").appendChild(div);
    },
    deleteAttach(fileName) {
      if (this.attachments.has(fileName)) {
        this.attachments.delete(fileName);
        this.attachments_number--;
        var item = document.getElementById(fileName);
        item.parentNode.removeChild(item);
      } else alert("Trying to delete nonexistent file");
    },
    download(Path, name) {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      this.email = email;
      console.log(this.email);
      console.log(Path);
      console.log(name);
      var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/json");
      let request = new Request("http://localhost:8085/download/" + name, {
        method: "POST",
        headers: myHeaders,
        body: JSON.stringify({ path: Path })
      });
      fetch(request)
        .then(response => response.blob())
        .then(blob => {
          var url = window.URL.createObjectURL(blob);
          var a = document.createElement("a");
          a.target = "_blank";
          a.href = url;
          a.download = name;
          document.body.appendChild(a);
          a.click();
          a.remove();
          console.log(this.email);
          //this.$router.push("/view?email=" + this.email);
        });
    },
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
        //console.log(this.targetFolder);
        this.getEmails();
      } else {
        //console.log(v);
        //here we load contacts
      }
    },
    getEmails() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      //console.log("get Emails");
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
          //console.log(data);
          this.handle(data);
        });
    },
    sendOrDraft(v) {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      let a = new FormData();
      a.append("from", email + "@fray.com");
      a.append("to", document.getElementById("To").value);
      a.append("priority", document.getElementById("priority").value);
      a.append("body", document.getElementById("textarea").value);
      a.append("subject", document.getElementById("Subject").value);
      a.append("name", document.getElementById("Subject").value);
      a.append("date", "14/2/2000");
      a.append("folder", "Inbox");
      for (var pair of this.attachments.entries()) {
        a.append("file", pair[1]);
      }
      document.getElementById("To").value = "";
      document.getElementById("priority").value = 1;
      document.getElementById("textarea").value = "";
      document.getElementById("Subject").value = "";
      var e = document.getElementById("attachments");
      var child = e.lastElementChild;
      while (child) {
        e.removeChild(child);
        child = e.lastElementChild;
      }
      this.attachments = new FormData();
      this.attachments_number = 0;
      this.on = false;
      fetch("http://localhost:8085//" + v, {
        method: "POST",
        body: a
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
      console.log(this.email);
      /*
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
      //let b = [];
      //b.push({ email: this.items[0] }, { khalo: "ahmed (ana baba yala)" });
      //a.push({khalo :"ahmed (ana baba yala)"});
      //console.log("\n\n" + JSON.stringify(b));
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
      /* });*/
    },
    search() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      let a = {
        str: document.getElementById("Search").value,
        region: this.targetFolder,
        emailPart: "all",
        Useremail: email + "@fray.com"
      };
      fetch("http://localhost:8085//Search", {
        method: "POST",
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
          this.handle(data);
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
    },
    chatting(row) {
      this.on = true;
      document.getElementById("To").value = this.items[row].email;
    },
    rewiteMessage(v) {
      this.on = true;
      document.getElementById("To").value = this.items[v].to;
      document.getElementById("priority").value = this.items[v].priority;
      document.getElementById("textarea").value = this.items[v].body;
      document.getElementById("Subject").value = this.items[v].subject;
      this.removeFromDraft(v);
    },
    deleteee(row) {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      console.log(email);
      let a = {
        Useremail: email + "@fray.com",
        FriendEmail: this.items[row].email
      };
      fetch("http://localhost:8085//DeleteFriend", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          console.log(data);
          this.getContacts();
        });
    },
    getContacts() {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      let a = {
        email: email + "@fray.com"
      };
      fetch("http://localhost:8085//GetFriends", {
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
    removeFromDraft(v) {
      const queryString = window.location.search;
      const urlParams = new URLSearchParams(queryString);
      const email = urlParams.get("email");
      let a = [];
      a.push({ mail: this.items[v] });
      a.push({ email: email + "@fray.com" });
      //console.log("\n\n\n\n\n\n"+JSON.stringify(a));
      fetch("http://localhost:8085//RemoveDraft", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response.text())
        .then(data => {
          //console.log(data);
          if (data == "true") {
            this.getEmails();
          } else {
            alert(data);
          }
        });
    }
  }
};
</script>
<style lang="scss">
@import "node_modules/bootstrap/scss/bootstrap.scss";
@import "node_modules/bootstrap-vue/src/index.scss";
</style>
<style lang="scss">
.title {
  text-align: center;
  color: white;
  background-color: RoyalBlue;
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
.chat {
  width: 100px;
}
</style>
