<template>
  <div class="list">
    <b-form class="col-md-12">
      <b-form-group id="searchName" label="Họ và tên" label-for="name">
        <b-form-input id="name" v-model="searchName" placeholder="Nhập tên"></b-form-input>
      </b-form-group>
    </b-form>

    <div class="list-person col-md-12">
      <ul>
        <li v-for="person in listPerson" :key="person.personId" style="list-style: none"
          @click="openModalInfoPerson(person.personId)">
          <div class="wrapper row" style="
              margin-bottom: 8px;
              box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
                rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
              border-radius: 8px;
              cursor: pointer;
            ">
            <div class="avt col-md-4" style="
                display: flex;
                justify-content: center;
                align-items: center;
              ">
              <img style="width: 50%; border-radius: 8px" :src="person.personImage
                  ? person.personImage
                  : 'https://icons.veryicon.com/png/o/internet--web/55-common-web-icons/person-4.png'
                " alt="" />
            </div>
            <div class="content col-md-8">
              <h5 class="name">{{ person.personName }}</h5>
              <h6 class="job">
                Giới tính: {{ getGenderText(person.personGender) }}
              </h6>
              <h6 class="age">Ngày sinh: {{ formatDate(person.personDob) }}</h6>
            </div>
          </div>
        </li>
      </ul>
    </div>

    <b-modal v-model="modalVisibleInfoPerson" hide-footer>
      <sidebar-person :personid="selectedPersonId" />
    </b-modal>
  </div>
</template>

<script>
import SidebarPerson from './sidebarPerson.vue'

export default {
  components: {
    SidebarPerson,
  },

  data() {
    return {
      searchName: '',
      listPerson: [],

      modalVisibleInfoPerson: false,
      selectedPersonId: null,
      idCay: 0,
    }
  },

  watch: {
    searchName: function (newVal, oldVal) {
      // Thực hiện hàm getListPerson khi giá trị của searchName thay đổi
      this.getListPerson()
    },
  },

  created() {
    this.getListPerson()
  },

  methods: {
    async getListPerson() {
      if (
        this.$route.query.code !== undefined &&
        this.$route.query.code !== 'undefined' &&
        this.$route.query.code !== null
      ) {
        const join = await this.$axios.get(
          'http://localhost:8080/getFamilyIdByCode?code=' +
          this.$route.query.code
        )
        this.idCay = join.data.fid
      }
      else {
        this.idCay = this.$route.query.id
      }
      const list = await this.$axios.get(
        'http://localhost:8080/personSearch?familyTreeId=' +
        this.idCay +
        '&keyword=' +
        this.searchName
      )

      this.listPerson = list.data

      // eslint-disable-next-line no-console
      console.log('list: ', list)
    },

    openModalInfoPerson(personId) {
      this.selectedPersonId = personId
      this.modalVisibleInfoPerson = true
    },

    getGenderText(isMale) {
      return isMale ? 'Nam' : 'Nữ'
    },

    formatDate(dateString) {
      if (!dateString) return '' // Check if the date string exists

      const date = new Date(dateString)
      if (isNaN(date.getTime())) return '' // Check if the date is valid

      const options = { year: 'numeric', month: 'numeric', day: 'numeric' }
      return date.toLocaleDateString('en-US', options)
    },
  },
}
</script>

<style scoped>
.list {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.list-person {
  overflow: auto;
  scroll-behavior: smooth;
}

.list-person>ul {
  height: 400px;
  scroll-behavior: smooth;
}

.wrapper:hover {
  background: #f1f1f1;
}
</style>
