<template>
  <div class="row">
    <div class="col-12">
      <card :title="table1.title" :subTitle="table1.subTitle">
        <div slot="raw-content" class="table-responsive">
          <paper-table :data="table1.data" :columns="table1.columns">
            <template #default="{ row }">
              <div>{{ row.name }}</div>
              <div>{{ row.username }}</div>
              <div>{{ row.email }}</div>
              <div>{{ row.password }}</div>
            </template>
          </paper-table>
        </div>
      </card>
    </div>

    <div class="col-12">
      <card class="card-plain">
        <div class="table-full-width table-responsive">
          <paper-table
            type="hover"
            :title="table2.title"
            :sub-title="table2.subTitle"
            :data="table2.data"
            :columns="table2.columns"
          >
          </paper-table>
        </div>
      </card>
    </div>
  </div>
</template>

<script>
import { PaperTable } from "@/components";
const tableColumns = ["Id", "Name", "Username", "Email", "Password"];

export default {
  components: {
    PaperTable,
  },
  data() {
    return {
      table1: {
        title: "Stripped Table",
        subTitle: "Here is a subtitle for this table",
        columns: [...tableColumns],
        data: [],
      },
      table2: {
        title: "Table on Plain Background",
        subTitle: "Here is a subtitle for this table",
        columns: [...tableColumns],
        data: [],
      },
    };
  },
  mounted() {
    // Retrieve "utilizador" data from local storage
    const storedUtilizador = localStorage.getItem('utilizador');
    if (storedUtilizador) {
      this.table1.data = JSON.parse(storedUtilizador);
      this.table2.data = JSON.parse(storedUtilizador);
    }
  },
};
</script>

<style></style>
