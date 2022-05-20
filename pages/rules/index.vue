<template>
  <div class="container-fluid h-auto w-100">
    <!-- Page Heading -->

    <div class="row">
      <div class="card w-100 shadow mb-4">
        <div class="card-header py-3">
          <div
            class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
          >
            Бизнес правила
            <span>
              <nuxt-link
                to="/rules/create"
                class="btn btn-sm btn-primary btn-icon-split"
                type="button"
              >
                <span class="icon text-white">
                  <i class="text-white fas fa-plus"></i>
                </span>
                <span class="text">Добавить</span>
              </nuxt-link>
            </span>
          </div>
        </div>
        <div class="card-body">
          <div class="table-responsive">
            <table v-if="!showLoader" class="table table-bordered" id="dataTable" cellspacing="0">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Бизнес правила</th>
                  <th>Действия</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, idx) in items" :key="item.id">
                  <td>{{ item.id }}</td>
                  <td>{{ item.name }}</td>
                  <td>
                    <div
                      @click="$router.push(`/rules/edit/${item.id}`)"
                      class="btn btn-sm btn-primary btn-icon-split"
                    >
                      <span class="icon text-white">
                        <i class="fas fa-eye"></i>
                      </span>
                    </div>
                    <div @click="deleteModal = true" class="btn btn-sm btn-danger btn-icon-split">
                      <span class="icon text-white">
                        <i class="fas fa-trash"></i>
                      </span>
                    </div>
                  </td>

                  <b-modal
                    v-model="deleteModal"
                    title="Удаление классификации"
                    @ok="deleteItem(idx)"
                  >
                    <p class="pt-3">Вы действительно хотите удалить запись?</p>
                  </b-modal>
                </tr>
              </tbody>
            </table>
            <Loader v-else-if="showLoader" />
            <b-modal v-model="createModal" title="Бизнес правила" @ok="createItem">
              <div class="form-group">
                <label for="exampleInputEmail1">Бизнес правила</label>
                <input
                  v-model="create.name"
                  type="text"
                  class="form-control"
                  placeholder="Введите правила"
                />
              </div>
            </b-modal>
            <b-modal v-model="editModal" title="Тип платежа" @ok="updateItem">
              <div class="form-group">
                <label for="exampleInputEmail1">Тип платежа</label>
                <input
                  v-model="update.name"
                  type="text"
                  class="form-control"
                  placeholder="Введите тип"
                />
              </div>
            </b-modal>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      create: {
        name: '',
      },
      update: {
        name: '',
        id: '',
      },
      createModal: false,
      editModal: false,
      deleteModal: false,
      showLoader: true,
    }
  },
  mounted() {
    this.$axios.$get('/rules').then((response) => {
      this.items = response.items
      this.showLoader = false
    })
  },
  methods: {
    createItem() {
      this.$axios.$post('/rules', this.create).then((response) => {
        this.items.push(response.item)
        this.create = {
          name: '',
        }
      })
    },
    editItem(index) {
      let item = this.items[index]
      this.update.name = item.name
      this.update.id = item.id
      this.editModal = true
    },
    updateItem() {
      this.$axios
        .$put('/rules/' + this.update.id, this.update)
        .then((response) => {
          this.items.splice(
            this.items.findIndex((i) => i.id === this.update.id),
            1,
            response.item
          )
        })
    },
    deleteItem(index) {
      let item = this.items[index]
      this.$axios.$delete('/rules/' + item.id).then(() => {
        this.items.splice(this.items.findIndex((i) => i.id === item.id))
      })
    },
  },
}
</script>

<style></style>
