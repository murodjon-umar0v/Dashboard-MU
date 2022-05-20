<template>
  <div class="container-fluid">
    <!-- Page Heading -->
    <div class="row">
      <div class="card w-100 shadow mb-4">
        <div class="card-header py-3">
          <div
            class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
          >
            Сотрудники
            <span>
              <a
                @click="createModal = true"
                type="button"
                data-toggle="modal"
                data-target="#exampleModal"
                class="btn btn-sm btn-primary bg-primary btn-icon-split dropdown-item"
              >
                <span class="icon text-white">
                  <i class="text-white fas fa-plus"></i>
                </span>
                <span class="text-white px-2 py-1">Добавить</span>
              </a>
            </span>
          </div>
        </div>
        <div class="card-body">
          <div class="table-responsive">
            <table v-if="!showLoader" class="table table-bordered" id="dataTable" cellspacing="0">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>ФИО</th>
                  <th>Позиция</th>
                  <th>Дата начала</th>
                  <th>Действия</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, idx) in items" :key="item.id">
                  <td>{{ item.id }}</td>
                  <td>{{ item.full_name }}</td>
                  <td>{{ getOrganizationalStructureName(item.employee_position) }}</td>
                  <td>{{ getEmployeePositionName(item) }}</td>
                  <td>{{ item.created_at.substring(0, 10) }}</td>
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
                    <p class="pt-3">Вы действительно хотите удалить запись?</p>
                  </b-modal>
                </tr>
              </tbody>
            </table>
            <Loader v-else-if="showLoader" />
            <b-modal v-model="createModal" title="Ползователи" @ok="createItem">
              <div class="form-group">
                <label for="exampleInputEmail1">ФИО</label>
                <input
                  v-model="create.full_name"
                  type="text"
                  class="form-control"
                  placeholder="Имя"
                />
              </div>
              <div class="form-group">
                <label for="exampleInputEmail1">Позиция</label>
                <select v-model="create.employee_position_id" class="form-control">
                  <option
                    v-for="position in positions"
                    :key="position.id"
                    :value="position.id"
                  >{{ position.name }}</option>
                </select>
              </div>
            </b-modal>
            <b-modal v-model="editModal" title="Ползователи" @ok="updateItem">
              <div class="form-group">
                <label for="exampleInputEmail1">ФИО</label>
                <input
                  v-model="update.full_name"
                  type="text"
                  class="form-control"
                  placeholder="Имя"
                />
              </div>
              <div class="form-group">
                <label for="exampleInputEmail1">Позиция</label>
                <select v-model="update.employee_position_id" class="form-control">
                  <option
                    v-for="position in positions"
                    :key="position.id"
                    :value="position.id"
                  >{{ position.name }}</option>
                </select>
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
      positions: [],
      organizational_structures: [],
      create: {
        full_name: '',
        employee_position_id: null,
        organizational_structure_id: null,
      },
      update: {
        full_name: '',
        employee_position_id: null,
        organizational_structure_id: null,
        id: '',
      },
      createModal: false,
      editModal: false,
      deleteModal: false,
      showLoader: true,
    }
  },
  mounted() {
    this.$axios.$get('/employee-positions').then((response) => {
      this.positions = response.items
    })
    this.$axios.$get('/employees').then((response) => {
      this.items = response.items
      this.showLoader = false
    })
    this.$axios.$get('/organizational-structures').then((response) => {
      this.organizational_structures = response.items
    })
  },
  methods: {
    getEmployeePositionName(item) {
      return item.employee_position ? item.employee_position.name : '-'
    },
    getOrganizationalStructureName(item) {
      return item.organizational_structure
        ? item.organizational_structure.name
        : '-'
    },
    createItem() {
      this.$axios.$post('/employees', this.create).then((response) => {
        this.items.push(response.item)
        this.create = {
          full_name: '',
          employee_position_id: null,
          organizational_structure_id: null,
        }
      })
    },
    editItem(index) {
      let item = this.items[index]
      this.update.full_name = item.full_name
      this.update.employee_position_id = item.employee_position_id
      this.update.organizational_structure_id = item.organizational_structure_id
      this.update.id = item.id
      this.editModal = true
    },
    updateItem() {
      this.$axios
        .$put('/employees/' + this.update.id, this.update)
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
      this.$axios.$delete('/employees/' + item.id).then(() => {
        this.items.splice(this.items.findIndex((i) => i.id === item.id))
      })
    },
  },
}
</script>

<style></style>
