<template>
  <div class="">
    <div class="box female">
      <div class="person" @click="openModalInfoPerson">
        <div class="avatar">
          <div v-if="person.personImage != null" class="imgSrc">
            <img :src="person.personImage" alt="" />
          </div>
          <svg
            v-else
            class="iconCss_i1b8s8h6"
            role="img"
            viewBox="0 0 24 24"
            width="88"
            height="88"
            fill="currentColor"
          >
            <path
              class="avatarBgCss_a1kg80op"
              fill="#ffd2d2"
              d="M0 0h24v24H0z"
            ></path>
            <path
              class="avatarFaceCss_a13ovmoh"
              d="M17.09 5.63L7.46 5s1.1 13.85 1 13.78a5.89 5.89 0 01-1 .26l.94 2.23 7.5-.16z"
              fill="#dd2762"
            ></path>
            <path
              class="avatarShadowCss_a47si0s"
              d="M15.34 18.74l-.71-4-.48.72a5.32 5.32 0 01-1.1 1.22 2.15 2.15 0 01-1.74-.05 7.51 7.51 0 01-1-.87c-.18-.2-.58-.76-.87-1.07 0 .22.34 1.64.34 1.73a5.91 5.91 0 005.56 2.32z"
              fill="#cb1f57"
            ></path>
            <path
              class="avatarHairCss_a1p46hu9"
              d="M19.45 11.44A3.18 3.18 0 0119 9.25c-.34-1.27-.86-3.48-2.23-4.6-.64-.5-1.22-1.63-2.24-1.76a4.19 4.19 0 00-1 .09s-1.77-.85-2.86-.45C9.73 2.62 4.45 6.59 6 8c0 0-.32.1-.44-.53a1.29 1.29 0 000 1.18s-1.72 1.89-1.24 2.86a5.19 5.19 0 00.31.5c-.3.33-1 1.17-.71 1.85.1.26-.09.44 0 .59a1.07 1.07 0 00.64.54l.3.88c.62 1.92 2.48 2.35 4.2 3.22-.25-.84.32-.79.55-1.45a5.94 5.94 0 00.27-2.29 1.3 1.3 0 00-.2-.49A4.38 4.38 0 018.33 12c.05-.5-.19-.7-.18-1.29s.38.13.43-.11c.09-.46.13-1.31.37-1.53.79-.74.46-.2 1.36-1C11.53 7 10.62 8 11.53 7a6.67 6.67 0 011.74-1.29A3 3 0 0015.1 7c.39.21.12 1.45.1 2.05 0 .44 0 1.22.44 1.22.76-.75 1 .36.95 1.13a1.85 1.85 0 01-.07.41 1.42 1.42 0 01-.14.36c-.11.34-.81.18-.78.37a2 2 0 010 .78 5.53 5.53 0 01-1 1.48c-.25.35-.68 2.45-.06 3.15a2.54 2.54 0 00.73.73 3.69 3.69 0 001.87.44 2.62 2.62 0 01.35-1.6 6.38 6.38 0 011-.8s.78-.83.38-1 .23-.41.23-.41l-.49-.31s.67-.15.44-.48.68-1.41.68-1.41.4-.52-.28-1.67z"
              fill="#6e0d33"
            ></path>
            <path
              class="avatarShirtCss_a17p2za8"
              d="M20.28 24a6.45 6.45 0 00.39-2.22c-.58-1.6-3.76-2.45-4.1-2.59s-1.09-.37-1.09-.37l-.11.21s-.68 2-3.86 2a4.29 4.29 0 01-3.44-2.13c-.72.2-4.74.58-4.74 2.86a6.87 6.87 0 00.4 2.24z"
              fill="#a0084c"
            ></path>
          </svg>
        </div>
        <div class="infor">
          <div class="person-name mt-2">
            <h6 style="font-size: 12px; color: #202121; margin-bottom: 0">
              {{ person.name }}
            </h6>
          </div>
          <div class="identify">
            <span style="color: #000; font-size: 14px">
              <button>{{ infoPersonFamily[personId].data.vocative }}</button>
            </span>
          </div>
        </div>
      </div>
      <div v-if="actionJoin === 1" class="add btn">
        <b-dropdown
          dropright
          class="add-icon icon-plus"
          no-caret
          variant="#202121"
        >
          <div
            v-if="
              person.parentsId === null ||
              person.fatherId === null ||
              person.motherId === null
            "
          >
            <b-dropdown-item
              v-if="person.parentsId === null"
              @click="openModalParent"
              >Thêm bố/mẹ</b-dropdown-item
            >

            <!-- Nếu có fatherId hoặc motherId thì hiển thị drop-item Thêm bố hoặc Thêm mẹ -->
            <div v-if="person.fatherId || person.motherId">
              <b-dropdown-item v-if="person.fatherId" @click="openModalMother">
                Thêm mẹ
              </b-dropdown-item>
              <b-dropdown-item v-if="person.motherId" @click="openModalFather">
                Thêm bố
              </b-dropdown-item>
            </div>
          </div>

          <b-dropdown-item @click="openModalHusband"
            >Thêm chồng</b-dropdown-item
          >

          <!-- Hiển thị drop-item Thêm anh/chị/em nếu parentId là null -->
          <b-dropdown-item
            id="item-sibling"
            :disabled="isParentIdNull"
            @click="openModalSibling"
          >
            Thêm anh/chị/em
          </b-dropdown-item>

          <b-dropdown-item @click="openModalChild"
            >Thêm con cái</b-dropdown-item
          >
        </b-dropdown>
      </div>

      <div
        v-if="
          (infoPersonFamily[personId].fatherId ||
            infoPersonFamily[personId].motherId) &&
          infoPersonFamily[personId].data.side != ''
        "
        class="extra-tree"
        @click="sendSideToLocal(infoPersonFamily[personId].data.side)"
      >
        <svg
          viewBox="0 0 16 16"
          width="24px"
          height="24px"
          focusable="false"
          role="img"
          aria-label="chevron up"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="bi-chevron-up mx-auto b-icon bi"
          data-v-41be6633=""
        >
          <g data-v-41be6633="">
            <path
              fill-rule="evenodd"
              d="M7.646 4.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1-.708.708L8 5.707l-5.646 5.647a.5.5 0 0 1-.708-.708l6-6z"
            ></path>
          </g>
        </svg>
      </div>
    </div>
    <b-modal
      v-model="modalVisibleParent"
      :title="'Thêm bố hoặc mẹ của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-parent
        :id="personId"
        :item="person"
        @personCreated="hideModal"
      />
    </b-modal>
    <b-modal
      v-model="modalVisibleFather"
      :title="'Thêm bố của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-father
        :id="personId"
        :item="person"
        @personCreated="hideModal"
      />
    </b-modal>
    <b-modal
      v-model="modalVisibleMother"
      :title="'Thêm mẹ của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-mother
        :id="personId"
        :item="person"
        @personCreated="hideModal"
      />
    </b-modal>
    <b-modal
      v-model="modalVisibleHusband"
      :title="'Thêm chồng của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-husband
        :id="personId"
        :item="person"
        @personCreated="hideModal"
      />
    </b-modal>
    <b-modal
      v-model="modalVisibleSibling"
      :title="'Thêm anh/chị/em của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-sibling :id="personId" :item="person" />
    </b-modal>
    <b-modal
      v-model="modalVisibleChild"
      :title="'Thêm con cái của ' + person.name"
      hide-footer
      class="modal_add"
      size="lg"
      centered
    >
      <form-infor-child
        :id="personId"
        :item="person"
        @personCreated="hideModal"
      />
    </b-modal>
    <b-modal v-model="modalVisibleInfoPerson" hide-footer>
      <sidebar-person
        :personid="personId"
        :action-join="actionJoin"
        :first-id="firstId"
      />
    </b-modal>
  </div>
</template>

<script>
import formInforParent from '../form_add/form_add_parent/formInfor.vue'
import formInforFather from '../form_add/form_add_parent/formInforFather.vue'
import formInforMother from '../form_add/form_add_parent/formInforMother.vue'
import formInforSibling from '../form_add/form_add_sibling/formInfor.vue'
import formInforChild from '../form_add/form_add_child/formInfor.vue'
import FormInforHusband from '../form_add/form_add_spouse/formInforHusband.vue'
export default {
  components: {
    formInforParent,
    formInforFather,
    formInforMother,
    formInforSibling,
    formInforChild,
    FormInforHusband,
  },

  props: {
    personId: {
      type: Number,
      default: null,
    },

    person: {
      type: Object,
      default: () => ({}),
    },

    infoPersonFamily: {
      type: Object,
      required: true,
    },

    actionJoin: {
      type: Number,
      default: 0,
    },

    firstId: {
      type: Number,
      default: 0,
    },
  },

  data() {
    return {
      modalVisibleParent: false,
      modalVisibleFather: false,
      modalVisibleMother: false,
      modalVisibleHusband: false,
      modalVisibleSibling: false,
      modalVisibleChild: false,
      modalVisibleInfoPerson: false,
    }
  },

  computed: {
    isParentIdNull() {
      // Kiểm tra nếu parentId là null thì trả về true, ngược lại trả về false
      return this.person.parentsId === null
    },
  },

  methods: {
    openModalParent() {
      this.modalVisibleParent = true
    },
    openModalFather() {
      this.modalVisibleFather = true
    },
    openModalMother() {
      this.modalVisibleMother = true
    },
    openModalHusband() {
      this.modalVisibleHusband = true
    },
    openModalSibling() {
      this.modalVisibleSibling = true
    },
    openModalChild() {
      this.modalVisibleChild = true
    },
    openModalInfoPerson() {
      this.modalVisibleInfoPerson = true
    },

    hideModal() {
      // Sử dụng $refs để truy cập modal và ẩn nó đi
      this.modalVisibleParent = false
      this.modalVisibleFather = false
      this.modalVisibleMother = false
      this.modalVisibleHusband = false
      this.modalVisibleSibling = false
      this.modalVisibleChild = false
    },

    sendSideToLocal(side) {
      localStorage.setItem('side', side)
      window.location.reload()
    },
  },
}
</script>

<style scoped>
.box {
  position: relative;
  padding: 20px 10px;
  height: 220px;
  width: 130px;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.2);
}
.male {
  border-top: 4px solid #1898b8;
}
.female {
  border-top: 4px solid #d7406d;
}

.person {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.avatar {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.avatar > .imgSrc {
  height: 70px;
  width: 70px;
  margin-bottom: 8px;
}

.avatar > .imgSrc > img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
}

.avatar > svg {
  height: 70px;
  width: 70px;
  border-radius: 500px;
}

.infor {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100%;
}

.infor > .identify {
  padding: 2px;
  width: 100%;
  text-align: center;
}

.infor > .identify > span > button {
  border: none;
  padding: 2px 4px;
  border-radius: 8px;
  background-color: #fff;
}

.infor > .identify > span > button:hover {
  background-color: #e1dcdc;
}

/* .box > .add {
      position: absolute;
      top: 0;
      right: 0;
      width: 10px;
      height: 10px;
    } */

.male > .add > .add-icon {
  position: absolute;
  top: 0;
  right: 0;
  color: #1898b8;
  border-radius: 100%;
  padding: 8px 4px;
}
.female > .add > .add-icon {
  position: absolute;
  top: 0;
  right: 0;
  color: #d7406d;
  border-radius: 100%;
  padding: 8px 4px;
}

.male > .add > .add-icon:hover {
  background-color: rgba(0, 177, 218, 0.294);
}
.female > .add > .add-icon:hover {
  background-color: #eac5d0;
}

.icon-plus::before {
  content: '+';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 4px;
  font-size: 30px;
}
.female:hover {
  background: #fbebf0;
}

.extra-tree {
  position: absolute;
  top: -10%;
  left: 50%;
  transform: translateX(-50%);
  border: 1px solid #ccc;
  border-radius: 50%;
  font-size: 16px;
  font-weight: bold;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fff;
  cursor: pointer;
}

.extra-tree:hover {
  background: #fbebf0;
}
</style>
