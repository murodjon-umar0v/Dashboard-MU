<template>
  <div class="container-fluid">
    <div class="row">
      <div class="card w-100 shadow mb-4">
        <div class="card-header py-3">
          <div
            class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
          >
            Бизнес правила
            <span>
              <div
                @click="itemUpdate"
                type="button"
                class="btn btn-sm btn-success bg-success btn-icon-split dropdown-item"
              >
                <span class="icon text-white">
                  <i class="text-white fas fa-save"></i>
                </span>
                <span class="text-white px-2 py-1">Сохранить</span>
              </div>
            </span>
          </div>
        </div>
        <div class="card-body">
          <form class="row small" action>
            <label class="input-group col-md-6 input-group-sm mb-3">
              Название
              <input
                v-model="update.name"
                type="text"
                class="form-control w-100"
                placeholder="Название бизнес процесса"
              />
            </label>
            <label class="input-group col-md-6 input-group-sm mb-3">
              Выберите группу
              <select
                v-model="update.classification_group_id"
                class="form-control w-100"
                aria-placeholder="Выберите бизнес процесс"
              >
                <option
                  v-for="(item, code) in groups"
                  :key="code"
                  :value="item.id"
                >{{ item.code }}: {{ item.name }}</option>
              </select>
            </label>
          </form>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="card w-100 shadow mb-4">
        <div class="card-header py-3">
          <div
            class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
          >Бизнес процесс</div>
        </div>
        <div class="card-body">
          <div class="table-responsive border rounded">
            <table class="table" id="dataTable" cellspacing="0">
              <thead>
                <tr class="border-0 small">
                  <th class="border-bottom">Код</th>
                  <th class="border-bottom">Классификации</th>
                  <th class="border-bottom">Результат классификации</th>
                  <th class="border-bottom"></th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="classification in update.classifications"
                  :key="classification.id"
                  class="border-bottom"
                >
                  <td>{{ classification.code }}</td>
                  <td>{{ classification.name }}</td>
                  <td>
                    <select v-model="classification.state" class="form-control">
                      <!--
                            -1 - Низкий
                            0 - Средний
                            1 - Высокий
                      -->
                      <option value="-1">Низкий</option>
                      <option value="0">Средний</option>
                      <option value="1">Высокий</option>
                    </select>
                  </td>
                </tr>
                <tr>
                  <td colspan="2" class="text-right">Результат бизнес-правила:</td>
                  <td>
                    <select class="form-control" v-model="update.result">
                      <option value="rework">Отправить на доработку</option>
                      <option value="payment">Отправить на оплату</option>
                    </select>
                  </td>
                </tr>
              </tbody>
              <tfoot>
                <td
                  class="text-center small text-success"
                  colspan="5"
                >Заполните все поля для добавления нового процесса</td>
              </tfoot>
            </table>
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
      groups: [],
      update: {
        name: '',
        classification_group_id: '',
        classifications: [],
        result: '',
      },
    }
  },
  mounted() {
    this.$axios.$get('/classification-groups').then((response) => {
      this.groups = response.items
    })
    this.$axios.$get(`/rules/${this.$route.params.id}`).then((response) => {
      this.update = response.item
    })
  },
  methods: {
    itemUpdate() {
      this.$axios
        .$put(`/rules/${this.update.id}`, this.update)
        .then((response) => {
          this.$router.push('/rules')
        })
    },
  },
}
</script>

<style scoped>
table tbody td {
  border: none;
}

input:focus {
  outline: none;
}

input[type='checkbox'] {
  appearance: none;
  width: 1.5em;
  height: 1.5em;
  position: relative;
  margin: 0 0.5em 0 0;
  cursor: pointer;
  transform: scale(0.8);
}

input[type='checkbox']:before {
  content: '';
  width: 100%;
  height: 100%;
  background: #ddd;
  color: #fff;
  text-align: center;
  line-height: 1.5;
  border-radius: 0.4em;
  position: absolute;
  top: -4px;
  left: 0;
  z-index: 10;
}

input[type='checkbox']:checked:before {
  font-family: -apple-system !important;
  content: '\2714';
  background: #3e64d3;
}

input[type='checkbox']:after {
  content: '';
  width: 300%;
  height: 300%;
  background-color: #3e64d3;
  border-radius: 100%;
  position: absolute;
  top: 30%;
  left: 50%;
  z-index: 5;
  opacity: 0;
  transform: translate(-50%, -50%);
  animation: none;
}

input[type='checkbox']:checked:after {
  animation: check 0.3s;
}

@keyframes check {
  0% {
    transform: translate(-50%, -50%) scale(0.1);
    opacity: 1;
  }
  40% {
    opacity: 1;
  }
  100% {
    transform: translate(-50%, -50%) scale(1);
    opacity: 0;
  }
}
</style>
