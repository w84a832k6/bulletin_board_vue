<template>
  <div>
    <h3>BulletinBoard</h3>
    <b-card class="mx-1">
      <div class="d-flex flex-row-reverse">
        <b-button v-b-modal.addNew>add new</b-button>
      </div>
      <b-alert
        class="mt-2"
        :show="dismissCountDown"
        dismissible
        fade
        variant="danger"
        @dismiss-count-down="countDownChanged"
      >
        {{ message }}
      </b-alert>
      <b-table class="mt-2" striped hover :items="items" :fields="fields">
        <template #cell(operate)="row">
          <b-button
            size="sm"
            class="mr-2"
            v-b-modal.edit
            @click="setModal(row.item)"
            >edit</b-button
          >
          <b-button size="sm" class="mr-2" @click="deleteItem(row.item.id)">
            delete
          </b-button>
        </template>
      </b-table>
    </b-card>
    <b-modal
      id="addNew"
      title="Add New Bulletin"
      hide-header-close
      @show="resetModal"
      @hidden="resetModal"
      @ok="createItem"
    >
      <form ref="form">
        <b-form-group label="title" label-for="title-input">
          <b-form-input
            id="title-input"
            v-model="bulletin.title"
          ></b-form-input>
        </b-form-group>
        <b-form-group label="content" label-for="content-input">
          <b-form-input
            id="content-input"
            v-model="bulletin.content"
          ></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
    <b-modal
      id="edit"
      title="Edit Bulletin"
      hide-header-close
      @hidden="resetModal"
      @ok="updateItem"
    >
      <form ref="form">
        <b-form-group label="title" label-for="title-input">
          <b-form-input
            id="title-input"
            v-model="bulletin.title"
          ></b-form-input>
        </b-form-group>
        <b-form-group label="content" label-for="content-input">
          <b-form-input
            id="content-input"
            v-model="bulletin.content"
          ></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data () {
    return {
      fields: ['title', 'content', 'created_at', 'updated_at', 'operate'],
      items: [],
      bulletin: {
        id: '',
        title: '',
        content: ''
      },
      message: '',
      dismissCountDown: 0,
      dismissSecs: 5
    }
  },
  mounted: function () {
    this.getItems()
  },
  methods: {
    countDownChanged (dismissCountDown) {
      this.dismissCountDown = dismissCountDown
    },
    getItems () {
      this.axios
        .get(`${process.env.BACKEND_DOMAIN}/api/bulletin`)
        .then(response => {
          this.items = response.data.data
        })
    },
    deleteItem (id) {
      this.axios
        .delete(`${process.env.BACKEND_DOMAIN}/api/bulletin/${id}/delete`)
        .then(response => {
          this.reloadAndAlert(response)
        })
    },
    updateItem () {
      this.axios
        .patch(`${process.env.BACKEND_DOMAIN}/api/bulletin/edit`, this.bulletin)
        .then(response => {
          this.reloadAndAlert(response)
        })
    },
    createItem () {
      this.axios
        .post(
          `${process.env.BACKEND_DOMAIN}/api/bulletin/create`,
          this.bulletin
        )
        .then(response => {
          this.reloadAndAlert(response)
        })
    },
    reloadAndAlert (response) {
      this.getItems()
      this.message = response.data.message
      this.showAlert()
    },
    setModal (row) {
      this.bulletin = { id: row.id, title: row.title, content: row.content }
    },
    resetModal () {
      this.bulletin = { id: '', title: '', content: '' }
    },
    showAlert () {
      this.dismissCountDown = this.dismissSecs
    }
  }
}
</script>
