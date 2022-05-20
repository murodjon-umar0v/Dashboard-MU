<template>
  <div class="row">
    <div class="card w-100 shadow mb-4">
      <div class="card-header py-3">
        <div
          class="m-0 font-weight-bold text-primary d-flex justify-content-between align-items-center"
        >Бизнес процесс</div>
      </div>
      <div class="card-body">
        <vue-tree
          style="width: 100%; height: 600px;"
          :dataset="treeData"
          :config="treeConfig"
          linkStyle="straight"
        >
          <template v-slot:node="{ node, collapsed }">
            <div class="rich-media-node" :style="{ border: collapsed ? '2px solid grey' : '' }">
              <div class="font-weight-bold text-center">{{ node.name }}</div>
              <span>Lavozimlar:</span>
              <ul v-if="node.employee_positions && node.employee_positions.length">
                <li v-for="p in node.employee_positions" :key="p.id">
                  {{ p.name }}
                  <ul>
                    <li
                      v-for="e in p.employees"
                      :key="e.id"
                      @click="$router.push(`/employees`)"
                    >{{ e.full_name }}</li>
                  </ul>
                </li>
              </ul>
              <ul v-else>
                <li>-</li>
              </ul>
            </div>
          </template>
        </vue-tree>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  ssr: false,
  components: {
    VueTree: () => import('@ssthouse/vue-tree-chart'),
  },
  props: {
    treeData: {
      default: () => {},
    },
  },
  name: 'treemap',
  data() {
    return {
      richMediaData: {},
      treeConfig: {
        nodeWidth: 320,
        nodeHeight: 80,
        levelHeight: 200,
      },
    }
  },
  methods: {
    getEmployees(positions) {
      let employees = []
      if (positions) {
        positions.forEach((p) => {
          if (p.employees) {
            employees = employees.concat(p.employees)
          }
        })
      }
      return employees
    },
  },
}
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.rich-media-node {
  width: 300px;
  padding: 8px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: white;
  background-color: #f7c616;
  border-radius: 4px;
}
</style>