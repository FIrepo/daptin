<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <q-page>
    <div class="q-pa-md q-gutter-sm">
      <q-breadcrumbs separator="---" class="text-orange" active-color="secondary">
        <q-breadcrumbs-el label="Users" icon="fas fa-user"/>
        <q-breadcrumbs-el label="Accounts" icon="fas fa-list"/>
      </q-breadcrumbs>
    </div>

    <div class="q-pa-md q-gutter-sm">
      <q-table
        title="User accounts"
        :data="users"
        :rows-per-page-options="[50]"
        :columns="columns"
        row-key="index"
      />

    </div>
  </q-page>
</template>

<script>
  import {mapActions, mapGetters, mapState} from 'vuex';

  export default {
    name: 'TablePage',
    methods: {
      ...mapActions(['load', 'loadData', 'getTableSchema']),
      refresh() {
        var tableName = "user_account";
        const that = this;
        this.getTableSchema(tableName).then(function (res) {
          that.tableSchema = res;
          console.log("Schema", that.tableSchema)
        });

        this.loadData({tableName: tableName}).then(function (data) {
          console.log("Loaded data", data);
          that.users = data.data;
        })
      }
    },
    data() {
      return {
        text: '',
        users: [],
        columns: [
          {
            name: 'email',
            field: 'email',
            label: 'Email',
            align: 'left',
            sortable: true,
          }, {
            name: 'name',
            field: 'name',
            label: 'Name',
            align: 'left',
          },
        ],
        ...mapState([])
      }
    },
    mounted() {
      this.refresh();
    },
    computed: {
      ...mapGetters(['selectedTable']),
      ...mapState([])
    },

    watch: {}
  }
</script>