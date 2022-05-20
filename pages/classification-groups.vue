<template>
  <div id="content-wrapper" class="d-flex flex-column">
    <div id="content">
      <div class="container-fluid">
        <div class="row">
          <div class="card w-100 shadow mb-4">
            <div class="card-header py-3">
              <div
                class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
              >
                Группа классификация
                <span>
                  <a
                    @click="createModal = true"
                    class="btn btn-sm btn-primary btn-icon-split"
                    type="button"
                    data-toggle="modal"
                    data-target="#exampleModal"
                  >
                    <span class="icon text-white">
                      <i class="text-white fas fa-plus"></i>
                    </span>
                    <span class="text px-2 py-1">Добавить</span>
                  </a>
                </span>
              </div>
            </div>
            <div class="card-body">
              <div class="table-responsive">
                <table
                  v-if="!showLoader"
                  class="table table-bordered"
                  id="dataTable"
                  cellspacing="0"
                >
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Код</th>
                      <th>Имя</th>
                      <th>Действия</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(item,idx) in items" :key="item.id">
                      <td>{{ item.id }}</td>
                      <td>{{ item.code }}</td>
                      <td>{{ item.name }}</td>
                      <td>
                        <div @click="editItem(idx)" class="btn btn-sm btn-primary btn-icon-split">
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
                        <p class="pt-3">
                          Вы действительно хотите удалить запись?
                        </p>
                      </b-modal>
                    </tr>
                  </tbody>
                </table>
                <Loader v-else-if="showLoader" />
                <b-modal v-model="createModal" title="Группа классификация" @ok="createItem">
                  <div class="form-group">
                    <label for="exampleInputEmail1">Группа классификация</label>
                    <div class="row">
                      <div class="col">
                        <input
                          v-model="create.code"
                          type="text"
                          class="form-control"
                          placeholder="Введите код"
                        />
                      </div>
                      <div class="col">
                        <input
                          v-model="create.name"
                          type="text"
                          class="form-control"
                          placeholder="Введите имя"
                        />
                      </div>
                    </div>
                  </div>
                </b-modal>
                <b-modal v-model="editModal" title="Группа классификация" @ok="updateItem">
                  <div class="form-group">
                    <label for="exampleInputEmail1">Группа классификация</label>
                    <div class="row">
                      <div class="col">
                        <input
                          v-model="update.code"
                          type="text"
                          class="form-control"
                          placeholder="Введите код"
                        />
                      </div>
                      <div class="col">
                        <input
                          v-model="update.name"
                          type="text"
                          class="form-control"
                          placeholder="Введите имя"
                        />
                      </div>
                    </div>
                  </div>
                </b-modal>
              </div>
            </div>
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
        code: '',
        name: '',
      },
      update: {
        code: '',
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
    this.$axios.$get('/classification-groups').then((response) => {
      this.items = response.items
      this.showLoader = false
    })
  },
  methods: {
    createItem() {
      this.$axios
        .$post('/classification-groups', this.create)
        .then((response) => {
          this.items.push(response.item)
          this.create = {
            code: '',
            name: '',
          }
        })
    },
    editItem(index) {
      let item = this.items[index]
      this.update.code = item.code
      this.update.name = item.name
      this.update.id = item.id
      this.editModal = true
    },
    updateItem() {
      this.$axios
        .$put('/classification-groups/' + this.update.id, this.update)
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
      this.$axios.$delete('/classification-groups/' + item.id).then(() => {
        this.items.splice(
          this.items.findIndex((i) => i.id === item.id),
          1
        )
      })
    },
    
  },
}
</script>

