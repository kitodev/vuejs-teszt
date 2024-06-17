<script lang="ts">
import type { User } from '@/types/user'
import { defineComponent } from 'vue'
import userData from '../../assets/users.json'
import PopUp from '../PopUp/PopUp.vue'
import TableUser from './TableUser.vue'
import TableEdit from './TableUserEdit.vue'
import bin from '../../assets/bin.svg'
import down from '../../assets/down.svg'

export default defineComponent({
  name: 'TableComponent',
  components: {
    TableEdit,
    TableUser,
    PopUp
  },
  data() {
    return {
      userData,
      checkedUsers: [1, 2, 3] as number[],
      pagedUserData: [] as typeof userData,
      selectAll: false,
      isNewUser: false,
      selectedUser: 0 as number | false,
      pop: {
        status: false,
        text: '',
        id: 0 as number | string
      },
      currentPage: 1,
      pageSize: 6,
      sortField: '',
      sortDirection: 'asc' as 'asc' | 'desc',
      bin,
      down
    }
  },
  methods: {
    paginateData() {
      const startIndex = (this.currentPage - 1) * this.pageSize
      this.pagedUserData = this.userData.slice(startIndex, startIndex + this.pageSize)
    },
    changePage(page: number) {
      this.currentPage = page
      this.paginateData()
    },
    //selectelt idk
    checkedHandler(id: number) {
      if (this.checkedUsers.includes(id)) {
        this.checkedUsers = this.checkedUsers.filter((el) => el !== id)
      } else {
        this.checkedUsers.push(id)
        this.selectAll = this.checkedUsers.length === this.userData.length
      }
    },
    //összes useer jelölése
    selectAllHandler() {
      if (this.selectAll) {
        this.checkedUsers = this.userData.map((user) => user.id)
      } else {
        this.checkedUsers = []
      }
    },
    //régi adat módosítása
    saveOldUser(newUserData: User) {
      if (!newUserData) {
        console.error('Az új felhasználói adatoknak tartalmaznia kell az "id" tulajdonságot.')
        return
      }
      this.userData = this.userData.map((el) => {
        if (el.id === this.selectedUser) {
          if (newUserData.name.length > 0 && newUserData.email.length > 0) {
            return {
              id: this.selectedUser,
              name: newUserData.name,
              email: newUserData.email,
              permission: newUserData.permission
            }
          } else {
            return el
          }
        } else {
          return el
        }
      })
      this.paginateData()
      this.selectedUser = false
    },
    //új user hozzáadása
    addNewUser(user: User) {
      if (user.name.includes(' ')) {
        if (user.name.length != 0 && user.email.length != 0) {
          const id = this.userData.length + 1
          this.userData.push({
            ...user,
            id: id
          })
          this.isNewUser = false
          if (this.selectAll) {
            this.checkedUsers.push(id)
          }
          this.selectAll = this.checkedUsers.length === this.userData.length
        }
      } else {
        console.log('Nincs space a name értékbe')
      }
    },
    //módosításra állítás
    setEdit(user: User) {
      this.isNewUser = false
      if (this.selectedUser != user.id) {
        this.selectedUser = user.id
      } else {
        this.selectedUser = false
      }
    },
    //Deletenél notify megjelenítése(1ember)
    setQuestion(user: { name: string; id: number }) {
      this.pop = {
        status: true,
        text: user.name,
        id: user.id
      }
    },
    //user törlése
    deleteElement() {
      if (this.pop.id == 'all') {
        this.pop.status = false
        this.userData = this.userData.filter((el) => !this.checkedUsers.includes(el.id))
        this.checkedUsers = []
        this.paginateData()
      } else {
        this.pop.status = false
        this.userData = this.userData.filter((el) => el.id != this.pop.id)
        this.checkedUsers = this.checkedUsers.filter((el) => el != this.pop.id)
        this.paginateData()
      }
    },
    //Az összesre kérdés(Összes ember)
    questionAll() {
      this.pop.text = this.checkedUsers.length + ' Selected users'
      this.pop.id = 'all'
      this.pop.status = true
    },
    sortBy(field: keyof User) {
      if (this.sortField === field) {
        this.pagedUserData.reverse()
        this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc'
      } else {
        this.sortField = field
        this.sortDirection = 'asc'
        this.pagedUserData.sort((a, b) => {
          if (a[field] < b[field]) return -1
          if (a[field] > b[field]) return 1
          return 0
        })
      }
    }
  },
  watch: {
    checkedUsers() {
      this.selectAll = this.checkedUsers.length === this.userData.length
    },
    currentPage() {
      this.paginateData()
    }
  },
  computed: {
    totalPages(): number {
      return Math.ceil(this.userData.length / this.pageSize)
    }
  },
  mounted() {
    this.paginateData()
  }
})
</script>

<template>
  <PopUp v-if="pop.status" :text="pop.text" @close="pop.status = false" @delete="deleteElement()" />
  <div class="tableContainer">
    <div
      class="header"
      :style="{ justifyContent: checkedUsers.length > 0 ? 'space-between' : 'flex-end' }"
    >
      <div v-if="checkedUsers.length > 0" class="selectedUsersDelete">
        <p>{{ checkedUsers.length }} user selected</p>
        <button id="deleteUsers" @click="questionAll()">
          <img style="width: 1.8vh; height: auto" :src="bin" alt="" />
          Delete
        </button>
      </div>
      <button
        v-if="!isNewUser"
        id="newUser"
        :style="
          selectedUser ? { background: 'rgba(28, 176, 211, 0.5)' } : { background: '#1CB0D3' }
        "
        @click="
          () => {
            if (!selectedUser) {
              isNewUser = !isNewUser
            }
          }
        "
      >
        + Add New user
      </button>
    </div>
    <div class="table">
      <div class="tableHeader">
        <div class="tableRow">
          <div class="tableCell" style="width: 4%">
            <input type="checkbox" v-model="selectAll" @change="selectAllHandler" />
          </div>
          <div class="tableCell" style="width: 50%" @click="sortBy('name')">Users</div>
          <div class="tableCell" style="width: 25%" @click="sortBy('permission')">
            Permission
            <img :src="down" style="width: 1.5vh; height: auto" />
          </div>
        </div>
      </div>
      <div class="tableBody">
        <TableEdit
          v-if="isNewUser"
          :user="{ name: '', email: '', permission: 'agent' }"
          @save="addNewUser($event)"
          @cancel="isNewUser = false"
          :newUser="true"
        />
        <div v-for="(user, index) in pagedUserData" :key="index">
          <TableUser
            :user="user"
            :userIndex="index"
            :checkedUsers="checkedUsers"
            :selectedUser="selectedUser"
            @check="checkedHandler"
            @edit="setEdit($event)"
            @showPop="setQuestion($event)"
          />
          <TableEdit
            v-if="selectedUser == user.id"
            :user="user"
            @save="saveOldUser($event)"
            @cancel="selectedUser = false"
            :newUser="false"
          />
        </div>
      </div>
      <div class="pagination">
        <button @click="changePage(currentPage - 1)" :disabled="currentPage == 1 ? true : false">
          <svg
            width="10"
            height="14"
            viewBox="0 0 10 14"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            style="width: 1vh; height: 1vh"
          >
            <path
              d="M8.5 1L2 7.5L8.5 13"
              :stroke="currentPage == 1 ? '#D9D9D9' : '#7E7E7E'"
              stroke-width="2"
              stroke-linecap="round"
            />
          </svg>
        </button>
        <!-- <span>{{ currentPage }} / {{ totalPages }}</span> -->
        <button
          class="pages"
          v-for="page in totalPages"
          :key="page"
          :style="
            page == currentPage
              ? { background: '#475DE5', color: 'white' }
              : { background: 'transparent', color: '#7E7E7E' }
          "
          @click="
            () => {
              currentPage = page
            }
          "
        >
          {{ page }}
        </button>
        <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages">
          <svg
            width="9"
            height="14"
            viewBox="0 0 9 14"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
            style="width: 1vh; height: 1vh"
          >
            <path
              d="M1 13L7.5 6.5L1 0.999999"
              :stroke="currentPage === totalPages ? '#D9D9D9' : '#7E7E7E'"
              stroke-width="2"
              stroke-linecap="round"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  display: flex;
  background-color: rgb(235, 235, 235);
}
.tableContainer {
  width: 83%;
  height: 100vh;
  background-color: white;
  border-radius: 3vh 0 0 3vh;
  position: relative;
  overflow-y: scroll;
  z-index: 99;
  .header {
    width: 100%;
    height: 10vh;
    margin-top: 25px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    #newUser {
      outline: none;
      border: none;
      width: 189px;
      height: 50px;
      margin-right: 20vh;
      font-weight: 300;
      font-size: 18px;
      border-radius: 1vh;
      background-color: #1cb0d3;
      color: white;
      transition: transform 0.3s;
      &:hover {
        cursor: pointer;
      }
    }
    div {
      width: auto;
      display: flex;
      align-items: center;
      margin-left: 10.5vh;
      gap: 3vh;
      #deleteUsers {
        outline: none;
        height: 4vh;
        border: 0.1vh solid rgb(163, 163, 163);
        color: rgb(163, 163, 163);
        font-weight: 700;
        border-radius: 1vh;
        display: flex;
        align-items: center;
        img {
          margin-right: 1vh;
          margin-bottom: 0.2vh;
        }
        i {
          margin-right: 0.5vh;
        }
        &:hover {
          cursor: pointer;
        }
      }
    }
  }

  .table {
    width: 79%;
    margin-top: 4vh;
    margin-left: 10vh;
    margin-bottom: 25px;
    min-height: 90vh;
    position: relative;
    input[type='checkbox'] {
      width: 24px;
      height: 24px;
      outline: none;
      border: none;
      border-radius: 8px;
    }
    .tableHeader {
      .tableRow {
        display: flex;
        margin-bottom: 3vh;
        padding: 1vh;
        align-items: center;
        gap: 14px;
        .tableCell {
          color: #7e7e7e;
          font-size: 20px;
          font-weight: 400;
        }
      }
    }
  }

  .pagination {
    height: 3vh;
    display: flex;
    align-items: center;
    position: absolute;
    bottom: 1vh;
    button {
      border: none;
      outline: none;
      background: transparent;
    }
    .pages {
      width: 30px;
      height: 27px;

      background: #475de5;
      border-radius: 10px;

      color: white;

      margin: 5px;
      &:hover {
        cursor: pointer;
      }
    }
  }
}
</style>
