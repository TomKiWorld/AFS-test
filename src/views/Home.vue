<template>
  <div class="home">
    <h1 class="center-text">This is a table with some important data</h1>
    <AppModal @input="handleModalInput" />
    <Spinner v-if="loading" />
    <div
      class="container"
      v-else
    >
      <b-table
        :data="actuelTableData"
        :columns="columns"
      ></b-table>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { TableData } from "@/types/types";
import Spinner from "@/components/Spinner.vue";
import AppModal from "@/components/AppModal.vue";

@Component({
  name: "Home",
  components: { Spinner, AppModal },
})
export default class Home extends Vue {
  tableData: TableData[] = [];
  actuelTableData: TableData[] = [];
  loading = true;
  totalID = "total-id"
  columns = [
    {
      label: "Security class",
      field: "name",
    },
    {
      label: "Authorized amount",
      field: "authorizedAmount",
    },
    {
      label: "Issued amount",
      field: "issuedAmount",
    },
    {
      label: "Authorized Capital",
      field: "authorizedCapital",
    },
    {
      label: "Issued capital",
      field: "issuedCapital",
    },
  ];

  // mounted works fine if your ide complains about it
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  async mounted() {
    setTimeout(async () => {
      await this.fetchData()
      this.addTableTotal()
    }, 500);
  }

  getRowTotal(data: TableData[], key: keyof TableData): number {
    return data.reduce((p: number, c: TableData) => {
      const value = c[key];
      return typeof value === "number" ? p + value : p;
    }, 0);
  }

  getTotal(data: TableData[]): TableData {
    return {
      id: this.totalID,
      name: "Total",
      nominalValue: this.getRowTotal(data, "nominalValue"),
      authorizedAmount: this.getRowTotal(data, "authorizedAmount"),
      issuedAmount: this.getRowTotal(data, "issuedAmount"),
      authorizedCapital: this.getRowTotal(data, "authorizedCapital"),
      issuedCapital: this.getRowTotal(data, "issuedCapital"),
    }
  }

  addTableTotal(): void {
    this.actuelTableData = [...this.actuelTableData, this.getTotal(this.actuelTableData)];
  }

  async fetchData(): Promise<TableData[] | void>  {
    try {
      this.loading = true;
      
      const data: TableData[] = await this.getData();      
      const modifiedData = data.map((dataItem: TableData) => ({
        ...dataItem,
        randomNumber: Math.random(),
      }));
      
      this.tableData = [...modifiedData];
      this.actuelTableData = [...this.tableData]
      
      this.loading = false;
    } catch (error) {
      console.error(error, "This is not good");
    }
  }

  async getData(): Promise<TableData[]> {
    return [
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series A",
        nominalValue: 5,
        authorizedAmount: 1500,
        issuedAmount: 500,
        authorizedCapital: 7550,
        issuedCapital: 2500,
      },
      {
        id: "42f2462d-49d0-4e91-8fe1-de2e656b0f06",
        name: "Series B",
        nominalValue: 10,
        authorizedAmount: 15000,
        issuedAmount: 5000,
        authorizedCapital: 150000,
        issuedCapital: 50000,
      },
      {
        id: "fd78c11b-e3d2-455a-99b0-49907a75c463",
        name: "Series C",
        nominalValue: 1,
        authorizedAmount: 96876,
        issuedAmount: 61760,
        authorizedCapital: 96876,
        issuedCapital: 61760,
      },
      {
        id: "d8654cb0-8986-4fbc-b969-025e514cb934",
        name: "Series D",
        nominalValue: 1,
        authorizedAmount: 10110,
        issuedAmount: 1100,
        authorizedCapital: 10110,
        issuedCapital: 1100,
      },
    ];
  }

  handleModalInput(value: TableData): void {
    this.actuelTableData.push(value);
    const index = this.actuelTableData.findIndex(item => item.id === this.totalID)
    this.actuelTableData.splice(index, 1)
    this.addTableTotal()
  }
}
</script>

<style lang="scss" scoped>
.app-btn {
  margin: 1rem;
}</style>
