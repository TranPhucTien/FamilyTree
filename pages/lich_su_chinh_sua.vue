<template>
  <div class="lscs row">
    <!-- lịch sử thêm -->
    <div v-if="showCreatedInfo" class="created col-md-10">
      <div
        class="beforRepair d-flex align-items-center justify-content-center"
        style="flex-direction: column"
      >
        <h2 class="text-center">Thông tin được tạo mới</h2>

        <div
          class="content"
          style="
            width: 500px;
            background: #ddd;
            border-radius: 20px;
            height: 86vh;
          "
        >
          <div class="px-3 py-2">
            <div class="d-flex justify-content-center">
              <b-img
                :src="
                  createdInfo.personImage
                    ? createdInfo.personImage
                    : 'https://icons.veryicon.com/png/o/internet--web/55-common-web-icons/person-4.png'
                "
                style="margin: auto; width: 200px; height: 200px"
                fluid
                thumbnail
              ></b-img>
            </div>
            <br />

            <div class="">
              <h3 class="text-center">Thông tin cá nhân</h3>
              <div class="mt-4" style="height: 450px">
                <h6 style="font-weight: bold">
                  Họ và tên: {{ createdInfo.personName }}
                </h6>
                <h6>Giới tính: {{ formatGender(createdInfo.personGender) }}</h6>
                <h6>
                  Tình trạng: {{ formatStatus(createdInfo.personStatus) }}
                </h6>
                <p>Nghề nghiệp: {{ createdInfo.personJob }}</p>
                <p>Địa chỉ: {{ createdInfo.personAddress }}</p>
                <p>Dân tộc: {{ createdInfo.personEthnic }}</p>
                <p>Tôn giáo: {{ createdInfo.personReligion }}</p>
                <p>Ngày sinh: {{ formatDateCreate(createdInfo.personDob) }}</p>
                <p>Ngày mất: {{ formatDateCreate(createdInfo.personDod) }}</p>
                <span>Mô tả: {{ createdInfo.personDescription }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Lịch sử sửa -->
    <div v-if="showUpdatedData" class="row repair col-md-10">
      <div class="beforRepair col-md-6">
        <h2 class="text-center">Trước khi chỉnh sửa</h2>
        <div class="content">
          <div class="px-3 py-2">
            <div class="d-flex justify-content-center">
              <b-img
                :src="
                  oldData.personImage
                    ? oldData.personImage
                    : 'https://icons.veryicon.com/png/o/internet--web/55-common-web-icons/person-4.png'
                "
                style="margin: auto; width: 200px; height: 200px"
                fluid
                thumbnail
              ></b-img>
            </div>
            <br />

            <div class="">
              <h3 class="text-center">Thông tin cá nhân</h3>
              <div class="mt-4" style="height: 450px">
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personName,
                      newwData.personName,
                      'left'
                    )
                  "
                  style="font-weight: bold"
                >
                  Họ và tên: {{ oldData.personName }}
                </h6>
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personGender,
                      newwData.personGender,
                      'left'
                    )
                  "
                >
                  Giới tính: {{ formatGender(oldData.personGender) }}
                </h6>
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personStatus,
                      newwData.personStatus,
                      'left'
                    )
                  "
                >
                  Tình trạng: {{ formatStatus(oldData.personStatus) }}
                </h6>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personJob,
                      newwData.personJob,
                      'left'
                    )
                  "
                >
                  Nghề nghiệp: {{ oldData.personJob }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personAddress,
                      newwData.personAddress,
                      'left'
                    )
                  "
                >
                  Địa chỉ: {{ oldData.personAddress }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personEthnic,
                      newwData.personEthnic,
                      'left'
                    )
                  "
                >
                  Dân tộc: {{ oldData.personEthnic }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personReligion,
                      newwData.personReligion,
                      'left'
                    )
                  "
                >
                  Tôn giáo: {{ oldData.personReligion }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personDob,
                      newwData.personDob,
                      'left'
                    )
                  "
                >
                  Ngày sinh: {{ formatDateCreate(oldData.personDob) }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personDod,
                      newwData.personDod,
                      'left'
                    )
                  "
                >
                  Ngày mất: {{ formatDateCreate(oldData.personDod) }}
                </p>
                <span
                  :class="
                    getHighlightClass(
                      oldData.personDescription,
                      newwData.personDescription,
                      'left'
                    )
                  "
                  >Mô tả: {{ oldData.personDescription }}</span
                >
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="afterRepair col-md-6">
        <h2 class="text-center">Sau khi chỉnh sửa</h2>
        <div class="content">
          <div class="px-3 py-2">
            <div class="d-flex justify-content-center">
              <b-img
                :src="
                  newwData.personImage
                    ? newwData.personImage
                    : 'https://icons.veryicon.com/png/o/internet--web/55-common-web-icons/person-4.png'
                "
                style="margin: auto; width: 200px; height: 200px"
                fluid
                thumbnail
              ></b-img>
            </div>
            <br />

            <div class="">
              <h3 class="text-center">Thông tin cá nhân</h3>
              <div class="mt-4" style="height: 450px">
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personName,
                      newwData.personName,
                      'right'
                    )
                  "
                  style="font-weight: bold"
                >
                  Họ và tên: {{ newwData.personName }}
                </h6>
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personGender,
                      newwData.personGender,
                      'right'
                    )
                  "
                >
                  Giới tính: {{ formatGender(newwData.personGender) }}
                </h6>
                <h6
                  :class="
                    getHighlightClass(
                      oldData.personStatus,
                      newwData.personStatus,
                      'right'
                    )
                  "
                >
                  Tình trạng: {{ formatStatus(newwData.personStatus) }}
                </h6>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personJob,
                      newwData.personJob,
                      'right'
                    )
                  "
                >
                  Nghề nghiệp: {{ newwData.personJob }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personAddress,
                      newwData.personAddress,
                      'right'
                    )
                  "
                >
                  Địa chỉ: {{ newwData.personAddress }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personEthnic,
                      newwData.personEthnic,
                      'right'
                    )
                  "
                >
                  Dân tộc: {{ newwData.personEthnic }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personReligion,
                      newwData.personReligion,
                      'right'
                    )
                  "
                >
                  Tôn giáo: {{ newwData.personReligion }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personDob,
                      newwData.personDob,
                      'right'
                    )
                  "
                >
                  Ngày sinh: {{ formatDateCreate(newwData.personDob) }}
                </p>
                <p
                  :class="
                    getHighlightClass(
                      oldData.personDod,
                      newwData.personDod,
                      'right'
                    )
                  "
                >
                  Ngày mất: {{ formatDateCreate(newwData.personDod) }}
                </p>
                <span
                  :class="
                    getHighlightClass(
                      oldData.personDescription,
                      newwData.personDescription,
                      'right'
                    )
                  "
                  >Mô tả: {{ newwData.personDescription }}</span
                >
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Lịch sử xóa -->
    <div v-if="showDeletedData" class="created col-md-10">
      <div
        class="beforRepair d-flex align-items-center justify-content-center"
        style="flex-direction: column"
      >
        <h2 class="text-center">Thông tin người bị xóa</h2>

        <div
          class="content"
          style="
            width: 500px;
            background: #ddd;
            border-radius: 20px;
            height: 86vh;
          "
        >
          <div class="px-3 py-2">
            <div class="d-flex justify-content-center">
              <b-img
                :src="
                  deletedInfo.personImage
                    ? deletedInfo.personImage
                    : 'https://icons.veryicon.com/png/o/internet--web/55-common-web-icons/person-4.png'
                "
                style="margin: auto; width: 200px; height: 200px"
                fluid
                thumbnail
              ></b-img>
            </div>
            <br />

            <div class="">
              <h3 class="text-center">Thông tin cá nhân</h3>
              <div class="mt-4" style="height: 450px">
                <h6 style="font-weight: bold">
                  Họ và tên: {{ deletedInfo.personName }}
                </h6>
                <h6>Giới tính: {{ formatGender(deletedInfo.personGender) }}</h6>
                <h6>
                  Tình trạng: {{ formatStatus(deletedInfo.personStatus) }}
                </h6>
                <p>
                  Nghề nghiệp: {{ deletedInfo.personJob }}
                </p>
                <p>Địa chỉ: {{ deletedInfo.personAddress }}</p>
                <p>Dân tộc: {{ deletedInfo.personEthnic }}</p>
                <p>Tôn giáo: {{ deletedInfo.personReligion }}</p>
                <p>Ngày sinh: {{ formatDateCreate(deletedInfo.personDob) }}</p>
                <p>Ngày mất: {{ formatDateCreate(deletedInfo.personDod) }}</p>
                <span>Mô tả: {{ deletedInfo.personDescription }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="slideRepair col-md-2 p-3">
      <h4 class="text-center">Lịch sử chỉnh sửa</h4>
      <div class="listHistory">
        <div
          v-for="item in list"
          :key="item.historyId"
          class="itemHistory"
          @click="showCreatedData(item)"
        >
          <h6 class="time">{{ item.historyAction === 'CREATED' ? formatDate(item.historyCreatedAt) : item.historyAction === 'UPDATED' ? formatDate(item.historyUpdatedAt) : formatDate(item.historyDeletedAt) }}</h6>
          <p v-if="item.historyAction === 'CREATED'" class="status create">
            Thêm mới
          </p>
          <p v-if="item.historyAction === 'UPDATED'" class="status update">
            Sửa
          </p>
          <p v-if="item.historyAction === 'DELETED'" class="status delete">
            Xóa
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],

      showCreatedInfo: false,
      createdInfo: {},

      showUpdatedData: false,
      oldData: {},
      newwData: {},

      showDeletedData: false,
      deletedInfo: {},
    }
  },

  created() {
  },

  mounted() {
    document.querySelector('body').style.overflow = 'hidden'
    // Kiểm tra nếu có accessToken trong localStorage
    if (localStorage.getItem('accessToken')) {
      // Kiểm tra nếu đường dẫn hiện tại là "/account/dang_nhap" hoặc "/account/dang_ki"
      const currentPath = this.$route

      // eslint-disable-next-line no-console
      console.log(currentPath)
      if (
        currentPath === '/account/dang_nhap' ||
        currentPath === '/account/dang_ki' ||
        currentPath === '/account/quen_mat_khau' ||
        currentPath === '/account/xac_nhan_otp' ||
        currentPath === '/account/trang_chao_mung'
      ) {
        this.$router.push('/lich_su_chinh_sua')
      }
      // Ngược lại, giữ nguyên trang
    } else {
      // Chuyển hướng về trang /account/dang_nhap nếu không có accessToken
      this.$router.push('/account/dang_nhap')
    }

    this.getHistory()

  },
  
  methods: {
    getHighlightClass(prevData, currentData, side) {
      if (prevData && !currentData && side === 'left') {
        return 'highlight-red'
      } else if (currentData && prevData !== currentData && side === 'right') {
        return 'highlight-green'
      } else {
        return 'khongcogi'
      }
    },
    async getHistory() {
      const listHistory = await this.$axios.get(
        'http://localhost:8080/history/list?familyTreeId=' +
          this.$route.query.treeId
      )

      const dataSorted = listHistory.data.data.sort((a, b) => {
        return b.historyId - a.historyId;
      })

      this.list = dataSorted
    },

    formatDate(dateString) {
      if (!dateString) return '' // Kiểm tra xem chuỗi ngày tháng có tồn tại không

      const date = new Date(dateString)
      const options = {
        year: 'numeric',
        month: 'numeric',
        day: 'numeric',
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric',
      }
      return date.toLocaleDateString('en-US', options)
    },

    showCreatedData(item) {
      if (item.historyAction === 'CREATED') {
        this.createdInfo = JSON.parse(item.currentData)
        this.showCreatedInfo = true
      } else {
        this.showCreatedInfo = false
      }

      if (item.historyAction === 'UPDATED') {
        this.oldData = JSON.parse(item.oldData)
        this.newwData = JSON.parse(item.currentData)
        this.showUpdatedData = true
      } else {
        this.showUpdatedData = false
      }

      if (item.historyAction === 'DELETED') {
        this.deletedInfo = JSON.parse(item.oldData)
        this.showDeletedData = true
      } else {
        this.showDeletedData = false
      }
    },

    formatGender(gender) {
      return gender ? 'Nam' : 'Nữ'
    },
    formatStatus(status) {
      return status ? 'Còn sống' : 'Từ trần'
    },
    formatDateCreate(dateString) {
      if (!dateString) return '' // Nếu không có ngày tháng, trả về chuỗi rỗng

      const date = new Date(dateString)
      const options = {
        year: 'numeric',
        month: 'numeric',
        day: 'numeric',
      }
      return date.toLocaleDateString('en-US', options)
    },
  },
}
</script>

<style scoped>
.lscs {
  display: flex;
  justify-content: flex-end;
  height: 80vh;
}

.beforRepair,
.afterRepair {
  border-right: 1px #000 solid;
  padding: 10px;
}

.beforRepair {
  background: #999;
}

.afterRepair {
  background: #ccc;
}

.slideRepair {
  padding: 10px;
  background: #ddd;
}

.listHistory {
  height: 86vh;
  overflow: auto;
  scroll-behavior: smooth;
}

.itemHistory {
  margin: 8px;
  padding: 8px;
  background: #fff;
  border-radius: 10px;
  cursor: pointer;
}

.status {
  font-weight: bold;
}

.create {
  color: #28a745;
}

.update {
  color: #ffc107;
}

.delete {
  color: #dc3545;
}

.created,
.repair {
  padding: 0 !important;
}


.highlight-green {
  background-color: rgba(4, 155, 94, 0.4);
}
.highlight-red {
  background-color: rgba(126, 5, 47, 0.4);
}

</style>
