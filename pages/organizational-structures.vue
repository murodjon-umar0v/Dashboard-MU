<template>
  <div id="content-wrapper" class="d-flex flex-column">
    <div id="content">
      <div class="container-fluid">
        <no-ssr>
          <OrganizationalStructureDiagram v-model="treeDataLoaded" :treeData="treeData" />
        </no-ssr>
        <div class="row">
          <div class="card w-100 shadow mb-4">
            <div class="card-header py-3">
              <div
                class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
              >
                Организационная структура
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
                      <th>ID</th>
                      <th>Имя</th>
                      <th>Классификационные группы</th>
                      <th>по пункту</th>
                      <th>Действия</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(item, idx) in items" :key="item.id">
                      <td>{{ item.id }}</td>
                      <td>{{ item.name }}</td>
                      <td>
                        <div v-for="(group, idx) in item.classification_groups" :key="group.id">
                          <!-- checked icon of font-awesome -->
                          <span class="fas fa-check-square"></span>
                          {{ group.name }}
                        </div>
                      </td>
                      <td>{{ getParentName(item) }}</td>
                      <td>
                        <div @click="editItem(idx)" class="btn btn-sm btn-primary btn-icon-split">
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
                <b-modal v-model="createModal" title="Организационная структура" @ok="createItem">
                  <div class="form-group">
                    <label for="exampleInputEmail1">Организационная структура</label>
                    <input
                      v-model="create.name"
                      type="text"
                      class="form-control"
                      placeholder="Введите структура"
                    />
                  </div>
                  <div class="form-group">
                    <label for="exampleInputEmail1">по пункту</label>
                    <select
                      v-model="create.parent_id"
                      class="form-control"
                      id="exampleFormControlSelect1"
                    >
                      <option
                        v-for="(item) in items"
                        :key="item.id"
                        :value="item.id"
                      >{{ item.name }}</option>
                    </select>
                  </div>
                  <!-- vue-bootstrap checkbox group -->
                  <b-form-group label="Группа классификация" v-slot="{ ariaDescribedby }">
                    <b-form-checkbox
                      v-model="create.classification_group_ids"
                      v-for="option in classification_groups"
                      :key="option.id"
                      :value="option.id"
                      :aria-describedby="ariaDescribedby"
                      name="flavour-3a"
                    >{{ option.name }}</b-form-checkbox>
                  </b-form-group>
                </b-modal>
                <b-modal v-model="editModal" title="Организационная структура" @ok="updateItem">
                  <div class="form-group">
                    <label for="exampleInputEmail1">Организационная структура</label>
                    <input
                      v-model="update.name"
                      type="text"
                      class="form-control"
                      placeholder="Введите структура"
                    />
                  </div>

                  <div class="form-group">
                    <label for="exampleInputEmail1">по пункту</label>
                    <select
                      v-model="update.parent_id"
                      class="form-control"
                      id="exampleFormControlSelect1"
                    >
                      <option
                        v-for="(item) in items"
                        :key="item.id"
                        :value="item.id"
                      >{{ item.name }}</option>
                    </select>
                  </div>
                  <!-- vue-bootstrap checkbox group -->
                  <b-form-group label="Группа классификация" v-slot="{ ariaDescribedby }">
                    <b-form-checkbox
                      v-model="update.classification_group_ids"
                      v-for="option in classification_groups"
                      :key="option.id"
                      :value="option.id"
                      :aria-describedby="ariaDescribedby"
                      name="flavour-3a"
                    >{{ option.name }}</b-form-checkbox>
                  </b-form-group>
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
      classification_groups: [],
      create: {
        classification_group_ids: [],
        parent_id: null,
        name: '',
      },
      update: {
        name: '',
        parent_id: null,
        classification_group_ids: [],
        id: '',
      },
      createModal: false,
      editModal: false,
      deleteModal: false,
      showLoader: true,
      treeDataLoaded: false,
      treeData: [],
    }
  },
  mounted() {
    this.$axios.$get('/organizational-structures').then((response) => {
      this.items = response.items
      this.showLoader = false
    })

    this.$axios.$get('/classification-groups').then((response) => {
      this.classification_groups = response.items
    })
    this.loadTreeData()
  },
  methods: {
    getParentName(item) {
      return item.parent ? item.parent.name : 'Корень'
    },
    createItem() {
      this.$axios
        .$post('/organizational-structures', this.create)
        .then((response) => {
          this.items.push(response.item)
          this.create = {
            name: '',
          }
          this.loadTreeData()
        })
    },
    editItem(index) {
      let item = this.items[index]
      this.update.name = item.name
      this.update.id = item.id
      this.update.parent_id = item.parent_id
      this.update.classification_group_ids = item.classification_groups.map(
        (item) => item.id
      )
      this.editModal = true
    },
    updateItem() {
      this.$axios
        .$put('/organizational-structures/' + this.update.id, this.update)
        .then((response) => {
          this.items.splice(
            this.items.findIndex((i) => i.id === this.update.id),
            1,
            response.item
          )
          this.loadTreeData()
        })
    },
    deleteItem(idx) {
      let item = this.items[idx]
      this.$axios.$delete('/organizational-structures/' + item.id).then(() => {
        this.items.splice(
          this.items.findIndex((i) => i.id === item.id),
          1
        )
        this.loadTreeData()
      })
    },
    loadTreeData() {
      this.$axios
        .$get('/organizational-structures/tree-view')
        .then((response) => {
          this.treeData = response.items
          this.treeDataLoaded = true
        })
    },
  },
}
</script>

<style></style>
