<template>
  <div class="container-fluid h-auto w-100">
    <!-- Page Heading -->

    <div class="row">
      <div class="card w-100 shadow mb-4">
        <div class="card-header py-3">
          <div
            class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
          >
            История платежей
            <span>
              <a
                class="btn btn-sm btn-primary btn-icon-split"
                type="button"
                data-toggle="modal"
                data-target="#exampleModal"
              >
                <span class="icon text-white">
                  <i class="text-white fas fa-plus"></i>
                </span>
                <span class="text" @click="createModel = true">Добавить</span>
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
                  <th>Бизнес-процесс</th>
                  <th>Общая сумма</th>
                  <th>Детали платежа</th>
                  <th>Тип платежа</th>
                  <th>Статус</th>
                  <th>Дата</th>
                  <th>Действия</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, idx) in items" :key="item.id">
                  <td>{{ item.id }}</td>
                  <td>
                    <nuxt-link
                      :to="`/business-process/edit/${item.business_process_id}`"
                    >{{ item.business_process.id }}: {{ item.business_process.name }}</nuxt-link>
                  </td>
                  <td>{{ item.amount }}</td>
                  <td>{{ item.payment_detail }}</td>
                  <td>{{ item.payment_type.name }}</td>
                  <td>
                    <span
                      class="badge badge-success"
                    >{{ item.payment_status=='completed'?'Завершенный':'В ожидании' }}</span>
                  </td>
                  <td>{{ item.date }}</td>
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
      showLoader: true,
      items: [],
    }
  },
  mounted() {
    this.$axios.$get('/payment-transactions').then((response) => {
      this.items = response.items
      this.showLoader = false
    })
  },
}
</script>

<style></style>
