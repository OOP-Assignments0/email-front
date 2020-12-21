<template class="ahmed" >
  <div class="container-fluid"> 
  <div class="row ">
    <div class="col-sm-2">
      <div class="row">
        <div class="col-sm-12">
          <h1 class="logo">iMails</h1>
        </div>
      </div>
    </div>
    <div class="col-sm-10"><br>
       <div class="row" >
        <div class="col-sm-1">
          <a href="#"><i class="bar  fas fa-arrow-left"></i></a>
        </div>
        <div class="col-sm-9">
          <input class="bar" type="text" placeholder="Search mail" id="SearchBox" onkeyup="SearchMailFunction()">
        </div>
        <div class="col-sm-2">
          <i class="fas fa-user-circle" data-toggle="dropdown"></i>
            <div class="dropdown-menu">
              <a class="dropdown-item"><i class="fas fa-user"></i>&nbsp;&nbsp; </a>
              <a class="dropdown-item"><i class="fas fa-envelope-open"></i>&nbsp;&nbsp; </a>
              <a class="dropdown-item" href="../settings"><i class="fas fa-wrench"></i> &nbsp;&nbsp;Settings</a>
              <a class="dropdown-item" href="../forms/logout.php"><i class="fas fa-sign-out-alt"></i>&nbsp;&nbsp; Logout</a>
            </div>
        </div>
      </div><br>
    </div>
  </div>
  <div class="row">
    <div class="col-sm-2" >
      <center>
        <div class="compose_btn" data-toggle="modal" data-target="#myModal">
          <i class="fas fa-plus"></i>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Compose
        </div>
      </center><br>
                    <!-- The Modal -->
                      <div class="modal fade" id="myModal">
                        <div class="modal-dialog">
                          <div class="modal-content">
                            <!-- Modal Header -->
                            <div class="modal-header">
                              <h4 class="modal-title">Compose</h4>
                              <button type="button" class="close" data-dismiss="modal">&times;</button>
                            </div>
                            
                            
                            <div class="modal-body">
                              <div class="box">
                                <form method="post" enctype="multipart/form-data"><br>
                                  <div class="row">
                                    <div class="col-sm-12">
                                      <div class="input-box">
                                        <input type="text" name="m_to" required autocomplete="off"/>
                                        <label>To *</label>
                                      </div>
                                    </div>
                                  </div><br><br>
                                  <div class="row">
                                    <div class="col-sm-12">
                                      <div class="input-box">
                                        <input type="text" name="m_sub" autocomplete="off"/>
                                        <label>Sub *</label>
                                      </div>
                                    </div>
                                  </div><br><br>
                                  <div class="row">
                                    <div class="col-sm-12">
                                      <div class="input-box">
                                        <textarea name="m_msg"  rows="4" autocomplete="off"></textarea>
                                        <label>Message *</label>
                                      </div>
                                    </div>
                                  </div><br><br>
                                  <div class="row">
                                    <div class="col-sm-12">
                                      <center><div class="next-button">
                                          <button type="submit" name="submit" id="submit" value="submit" class="btn btn-primary">Next</button>
                                      </div></center>
                                    </div>
                                  </div>
                                </form>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
      <div class="row">
          <div class="pils" onclick="inbox()">
            <i class="fas fa-inbox"></i>&nbsp;&nbsp;Inbox
            <spam class="mCount"></spam>
          </div>
      </div>
      <div class="row">
          <div class="pils" onclick="sent()">
            <i class="fab fa-telegram-plane"></i>&nbsp;&nbsp;&nbsp;Sent
            <spam class="mCount"></spam>
          </div>
      </div>
      <div class="row">
          <div class="pils" onclick="star()">
            <i class="far fa-star"></i>&nbsp;&nbsp;Starred
            <spam class="mCount"></spam>
          </div>
      </div>
      <div class="row">
          <div class="pils" onclick="draft()">
            <i class="fas fa-file"></i>&nbsp;&nbsp;&nbsp;Drafts
            <spam class="mCount"></spam>
          </div>
      </div>
      <div class="row">
          <div class="pils" onclick="trash()">
            <i class="far fa-trash-alt"></i>&nbsp;&nbsp;&nbsp;Trash
            <spam class="mCount"></spam>
          </div>
      </div>
    </div>
    <div class="col-sm-10" id="demo">
    </div>
</div> 
</div>
</template>
<style scoped lang="scss">
       .box .input-box{
           position: relative;
       }
       .box .input-box input,textarea,select{
           width: 100%;
           padding: 10px,0;    
           font-size: 16px;
           margin-bottom: 10px;
           border: none;
           outline: none;
           border-bottom: 1px solid #09BCFF;
       }
       .box .input-box label{
           position: absolute;
           top: 0;
           left: 0;
           color: #909090;
           padding: 10px,0;
           font-size: 16px;
           pointer-events: none;
           transition: .5s;
       }

       .box .input-box input:focus, textarea:focus{
           border-bottom: 2px solid #09BCFF;
       }

       .box .input-box input:focus ~ label, .box .input-box textarea:focus ~ label,
       .box .input-box input:valid ~ label, .box .input-box textarea:valid ~ label{
           top: -18px;
           left: 0;
           color: #09BCFF;
           font-size: 13px;
       }    
/* Other ............ */
      body{
          background-image: url("https://www.shutterstock.com/image-photo/mountains-under-mist-morning-amazing-nature-1725825019") ;
          background-repeat: no-repeat;
          background-position: center center;
          background-size: cover;
          background-attachment: fixed;
          background-attachment: fixed;
          padding: 0;
          margin: 0;
          font-family: sans-serif;
        }
        .logo{
          color: #fff;
          text-align: center;
          padding: 18px;
        }
.r{
  color: red;
}
.g{
  color: green;
}

/* Other complete ............ */

/*  compose button ......................*/

.compose_btn{
  padding: 15px 10px;
  background: #fff;
  border-radius: 40px;
  text-align: center;
  font-size: 18px;
  color: #3498DB  ;
}
.compose_btn:hover{
    background: rgba(255, 255, 255, 0.8);
}
/*  compose button complete ......................*/


/* back button  ..............*/
      .fa-ellipsis-h,.fa-arrow-left, .fa-user-circle{
        color: #fff;
        font-size: 20px;
        border-radius: 50%;
        padding: 10px;
      }
      .fa-user-circle{
        font-size: 40px;
        padding: 0;
        padding-right: 20PX;
      }
      .fa-ellipsis-h:hover,.fa-arrow-left:hover{
        background: rgba(255, 255, 255, 0.7);
        height: 40px;
        width: 40px;
      }
/* back button complete ................*/


/* search box .......................... */
      #SearchBox{
        background: rgba(204, 10, 10, 0.5);
        outline: none;
        border-radius: 50px;
        color: #fff;
        width: 100%;
        height: 40px;
        border: 0;
        padding: 20px;
      }

      #SearchBox:focus {
        background: rgba(255, 255, 255, 0.9);
        color: #000;
      }
/* search box complete .......................... */


/* Mail table .......................... */
      table {
        opacity: 0.8; 
        background-color: rgb(255, 255, 255);
      }
      tr:hover{
        opacity: 0.9; 
        box-shadow: 0 1px 2px gray;
      }
      td{
        background-color: #fff;
      }
      .col-1{
        width: 20%;
        font-weight: bold;
        padding-left: 10px;
      }
      .col-2{
        width: 20%;
      }
      .col-3{
        width: 50%;
      }
      .col-4{
        width: 10%;
      }

/* Mail table complete .......................... */
.pils{
  background: transparent;
  padding: 10px 20px 10px 40px;
  color: rgb(255, 255, 255);
  border-top-right-radius: 40px;
  border-bottom-right-radius: 40px;
  width: 100%;
  font-size: 18px;
}
.pils:hover{
  background: rgba(255, 255, 255, 0.3);
}
.mCount{
  float: right;
  font-size: 14px;
  padding-top: 6px; 
}

table .fa-star{
  color: gray;
}
table .fa-star:hover{
  color: #3498DB;
}
.container-fluid{
    background-color: #000;
}
   
 </style>
<script>
  export default {
    data() {
      return {
        items: [
          { isActive: true, age: 40, name: { first: 'Dickerson', last: 'Macdonald' } },
          { isActive: false, age: 21, name: { first: 'Larsen', last: 'Shaw' } },
          {
            isActive: false,
            age: 9,
            name: { first: 'Mini', last: 'Navarro' },
            _rowVariant: 'success'
          },
          { isActive: false, age: 89, name: { first: 'Geneva', last: 'Wilson' } },
          { isActive: true, age: 38, name: { first: 'Jami', last: 'Carney' } },
          { isActive: false, age: 27, name: { first: 'Essie', last: 'Dunlap' } },
          { isActive: true, age: 40, name: { first: 'Thor', last: 'Macdonald' } },
          {
            isActive: true,
            age: 87,
            name: { first: 'Larsen', last: 'Shaw' },
            _cellVariants: { age: 'danger', isActive: 'warning' }
          },
          { isActive: false, age: 26, name: { first: 'Mitzi', last: 'Navarro' } },
          { isActive: false, age: 22, name: { first: 'Genevieve', last: 'Wilson' } },
          { isActive: true, age: 38, name: { first: 'John', last: 'Carney' } },
          { isActive: false, age: 29, name: { first: 'Dick', last: 'Dunlap' } }
        ],
        fields: [
          { key: 'name', label: 'Person full name', sortable: true, sortDirection: 'desc' },
          { key: 'age', label: 'Person age', sortable: true, class: 'text-center' },
          {
            key: 'isActive',
            label: 'Is Active',
            formatter: (value) => {
              return value ? 'Yes' : 'No'
            },
            sortable: true,
            sortByFormatted: true,
            filterByFormatted: true
          },
          { key: 'actions', label: 'Actions' }
        ],
        totalRows: 1,
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
        sortBy: '',
        sortDesc: false,
        sortDirection: 'asc',
        filter: null,
        filterOn: [],
        infoModal: {
          id: 'info-modal',
          title: '',
          content: ''
        }
      }
    },
    computed: {
      sortOptions() {
        // Create an options list from our fields
        return this.fields
          .filter(f => f.sortable)
          .map(f => {
            return { text: f.label, value: f.key }
          })
      }
    },
    mounted() {
      // Set the initial number of items
      this.totalRows = this.items.length
    },
    methods: {
      info(item, index, button) {
        this.infoModal.title = `Row index: ${index}`
        this.infoModal.content = JSON.stringify(item, null, 2)
        this.$root.$emit('bv::show::modal', this.infoModal.id, button)
      },
      resetInfoModal() {
        this.infoModal.title = ''
        this.infoModal.content = ''
      },
      onFiltered(filteredItems) {
        // Trigger pagination to update the number of buttons/pages due to filtering
        this.totalRows = filteredItems.length
        this.currentPage = 1
      }
      
    }
  }
</script>
<style lang="scss">
@import 'node_modules/bootstrap/scss/bootstrap.scss';
@import 'node_modules/bootstrap-vue/src/index.scss';
</style>
