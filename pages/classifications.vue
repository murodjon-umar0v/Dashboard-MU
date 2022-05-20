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
                Классификации
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
                    <span class="text px-2">Добавить</span>
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
                      <th>Код</th>
                      <th>Имя</th>
                      <th>Мин. балл</th>
                      <th>Макс. балл</th>
                      <th>Высокий показатель</th>
                      <th>Средний показатель</th>
                      <th>Низкий показатель</th>
                      <th>Группа</th>
                      <th>Действия</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(item, idx) in items" :key="item.id">
                      <td>{{ item.code }}</td>
                      <td>{{ item.name }}</td>
                      <td>{{ item.min_rate }}</td>
                      <td>{{ item.max_rate }}</td>
                      <td>{{ item.high_rate }}</td>
                      <td>{{ item.middle_rate }}</td>
                      <td>{{ item.low_rate }}</td>
                      <td>
                        {{
                          getClassificationName(item.classification_group)
                        }}:{{
                          getClassificationCode(item.classification_group)
                        }}
                      </td>
                      <td>
                        <div
                          @click="editItem(idx)"
                          class="btn btn-sm btn-primary btn-icon-split"
                        >
                          <span class="icon text-white">
                            <i class="fas fa-eye"></i>
                          </span>
                        </div>
                        <div
                          @click="deleteModal = true"
                          class="btn btn-sm btn-danger btn-icon-split"
                        >
                          <span class="icon text-white">
                            <i class="fas fa-trash"></i>
                          </span>
                        </div>
                      </td>
                    </tr>
                  </tbody>
                </table>
                <Loader v-else-if="showLoader" />
                <b-modal
                  v-model="createModal"
                  title="Классификации"
                  @ok="createItem"
                >
                  <div class="form-group">
                    <label>Название</label>
                    <input
                      v-model="create.name"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Код</label>
                    <input
                      v-model="create.code"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Мин. балл</label>
                    <input
                      v-model="create.min_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Макс. балл</label>
                    <input
                      v-model="create.max_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Высокий показатель</label>
                    <input
                      v-model="create.high_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Средний показатель</label>
                    <input
                      v-model="create.middle_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Низкий показатель</label>
                    <input
                      v-model="create.low_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Группа</label>
                    <select
                      v-model="create.classification_group_id"
                      class="form-control"
                    >
                      <option
                        v-for="item in groups"
                        :key="item.id"
                        :value="item.id"
                      >
                        {{ item.name }}
                      </option>
                    </select>
                  </div>
                </b-modal>
                <b-modal
                  v-model="editModal"
                  title="Классификации"
                  @ok="updateItem"
                >
                  <div class="form-group">
                    <label>Название</label>
                    <input
                      v-model="update.name"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Код</label>
                    <input
                      v-model="update.code"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Мин. балл</label>
                    <input
                      v-model="update.min_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Макс. балл</label>
                    <input
                      v-model="update.max_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Высокий показатель</label>
                    <input
                      v-model="update.high_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Средний показатель</label>
                    <input
                      v-model="update.middle_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>

                  <div class="form-group">
                    <label>Низкий показатель</label>
                    <input
                      v-model="update.low_rate"
                      type="text"
                      class="form-control"
                      placeholder="Введите правила"
                    />
                  </div>
                  <div class="form-group">
                    <label>Группа</label>
                    <select
                      v-model="update.classification_group_id"
                      class="form-control"
                    >
                      <option
                        v-for="item in groups"
                        :key="item.id"
                        :value="item.id"
                      >
                        {{ item.name }}
                      </option>
                    </select>
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
      groups: [],
      create: {
        name: '',
        code: '',
        min_rate: 0,
        max_rate: 0,
        high_rate: 0,
        middle_rate: 0,
        low_rate: 0,
        classification_group_id: null,
      },
      update: {
        id: '',
        name: '',
        code: '',
        min_rate: 0,
        max_rate: 0,
        high_rate: 0,
        middle_rate: 0,
        low_rate: 0,
        classification_group_id: null,
      },
      createModal: false,
      editModal: false,
      deleteModal: false,
      showLoader: true,
    }
  },
  mounted() {
    this.$axios.$get('/classifications').then((response) => {
      this.items = response.items
      this.showLoader = false
    })
    this.$axios.$get('/classification-groups').then((response) => {
      this.groups = response.items
    })
  },
  methods: {
    getClassificationName(group) {
      // return group name or '-'
      if (group && group.name) {
        return group.name
      } else {
        return '-'
      }
    },
    getClassificationCode(group) {
      // return group code or '-'
      if (group && group.code) {
        return group.code
      } else {
        return '-'
      }
    },
    createItem() {
      this.$axios.$post('/classifications', this.create).then((response) => {
        this.items.push(response.item)
        this.create = {
          name: '',
          code: '',
          min_rate: 0,
          max_rate: 0,
          high_rate: 0,
          middle_rate: 0,
          low_rate: 0,
          classification_group_id: null,
        }
      })
    },
    editItem(index) {
      let item = this.items[index]
      this.update.name = item.name
      this.update.max_rate = item.max_rate
      this.update.min_rate = item.min_rate
      this.update.code = item.code
      this.update.classification_group_id = item.classification_group_id
      this.update.id = item.id
      this.editModal = true
    },
    updateItem() {
      this.$axios
        .$put('/classifications/' + this.update.id, this.update)
        .then((response) => {
          this.items.splice(
            this.items.findIndex((i) => i.id === this.update.id),
            1,
            response.item
          )
        })
    },
    deleteItem(idx) {
      this.deleteModal = true
      this.update.id = this.items[idx].id

      let item = this.items[idx]
      this.$axios.$delete('/classifications/' + item.id).then(() => {
        this.items.splice(this.items.findIndex((i) => i.id === item.id))
      })
    },
  },
}
</script>

<style></style>
