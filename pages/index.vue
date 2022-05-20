<template>
  <div>
    <!-- Page Heading -->
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
      <h1 class="h3 mb-0 text-gray-800">Главная</h1>
      <a href="#" class="d-none d-sm-inline-block btn btn-sm btn-primary shadow-sm">
        <i class="fas fa-download fa-sm text-white"></i> Создать отчет
      </a>
    </div>

    <!-- Content Row -->

    <div class="row">
      <!-- Area Chart -->
      <LineChart :height="200" style="padding-bottom: 20px" />

      <!-- Pie Chart -->
      <div class="col-xl-4 col-lg-5">
        <div class="card shadow mb-4">
          <!-- Card Header - Dropdown -->
          <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
            <h6 class="m-0 font-weight-bold text-primary">Платежи</h6>
            <div class="dropdown no-arrow">
              <a
                class="dropdown-toggle"
                href="#"
                role="button"
                id="dropdownMenuLink"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              >
                <i class="fas fa-ellipsis-v fa-sm fa-fw text-gray-400"></i>
              </a>
            </div>
          </div>
          <!-- Card Body -->
          <div class="card-body">
            <ChartLine />
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-lg-6 mb-4">
        <div class="card shadow mb-4">
          <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">Оценка БП</h6>
          </div>
          <div class="card-body">
            <h4 class="small font-weight-bold">
              Очень низкое
              <span class="float-right">20%</span>
            </h4>
            <div class="progress mb-4">
              <div
                class="progress-bar bg-danger"
                role="progressbar"
                style="width: 20%"
                aria-valuenow="20"
                aria-valuemin="0"
                aria-valuemax="100"
              ></div>
            </div>
            <h4 class="small font-weight-bold">
              Низкое
              <span class="float-right">40%</span>
            </h4>
            <div class="progress mb-4">
              <div
                class="progress-bar bg-warning"
                role="progressbar"
                style="width: 40%"
                aria-valuenow="40"
                aria-valuemin="0"
                aria-valuemax="100"
              ></div>
            </div>
            <h4 class="small font-weight-bold">
              Среднее
              <span class="float-right">60%</span>
            </h4>
            <div class="progress mb-4">
              <div
                class="progress-bar"
                role="progressbar"
                style="width: 60%"
                aria-valuenow="60"
                aria-valuemin="0"
                aria-valuemax="100"
              ></div>
            </div>
            <h4 class="small font-weight-bold">
              Высокое
              <span class="float-right">80%</span>
            </h4>
            <div class="progress mb-4">
              <div
                class="progress-bar bg-info"
                role="progressbar"
                style="width: 80%"
                aria-valuenow="80"
                aria-valuemin="0"
                aria-valuemax="100"
              ></div>
            </div>
            <h4 class="small font-weight-bold">
              Очень высокое
              <span class="float-right">100%</span>
            </h4>
            <div class="progress">
              <div
                class="progress-bar bg-success"
                role="progressbar"
                style="width: 100%"
                aria-valuenow="100"
                aria-valuemin="0"
                aria-valuemax="100"
              ></div>
            </div>
          </div>
        </div>
      </div>

      <div class="col-lg-6 mb-4">
        <div class="card shadow mb-4">
          <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">Определение оценки</h6>
          </div>
          <div class="card-body">
            <div class="small">
              <b-badge class="circle text-danger" variant="danger">.</b-badge>
              <b>Очень низкое -</b>
              <span>
                функционирование не результативное, степень не допустимая-
                Требуется разработка системы и вмешательства руководства
                <br />
                <br />
              </span>
            </div>
            <div class="small">
              <b-badge class="circle text-warning" variant="warning">.</b-badge>
              <b>Низкое -</b>
              <span>
                функционирование не результативное, степень не допустимая-
                Требуется разработка значительных КД
                <br />
                <br />
              </span>
            </div>
            <div class="small">
              <b-badge class="circle text-primary" variant="primary">.</b-badge>
              <b>Среднее -</b>
              <span>
                функционирование результативное, степень допустимая-Требуется
                разработка КД
                <br />
                <br />
              </span>
            </div>
            <div class="small">
              <b-badge class="circle text-info" variant="info">.</b-badge>
              <b>Высокое -</b>
              <span>
                функционирование результативное, степень достаточная- Требуется
                разработка некоторых КД
                <br />
                <br />
              </span>
            </div>
            <div class="small">
              <b-badge class="circle text-success" variant="success">.</b-badge>
              <b>Очень высокое -</b>
              <span>
                функционирование результативное, степень высокая-Требуется
                пересмотр системы и разработка ПД
                <br />
                <br />
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from '../components/Sidebar.vue'

export default {
  components: {
    Sidebar,
  },
  name: 'IndexPage',
}
</script>

<style scoped>
.circle {
  width: 14px;
  border-radius: 50%;
}

.circle::selection {
  background: none;
}
</style>
