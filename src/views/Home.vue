<template>
  <div class="home">
    <h1>This is a table with some important data</h1>
    <b-table :data="tableData" :columns="columns"></b-table>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { TableData } from "@/types/types";

@Component
export default class Home extends Vue {
  tableData: TableData[] = [];
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
  loading = false;

  // mounted works fine if your ide complains about it
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  async mounted(): Promise<void> {
    let initialData: TableData[] = await this.getDataWithTotalsRow();

    this.loading = true;

    let formattedData: TableData[] = await initialData.map(
      (dataItem: TableData) => ({
        ...dataItem,
        randomNumber: Math.random(),
      })
    );

    try {
      await formattedData;
    } catch (error) {
      alert(`Error fetching data:\n\n${error}`);
    } finally {
      this.tableData = await formattedData;
      this.loading = false;
    }
  }

  /**
   * @description This method does the following:
   * - calculates and appends the 'totals' row from the table data
   * - appends the totals row to the table data
   * - returns the table data
   */
  async getDataWithTotalsRow(): Promise<TableData[]> {
    let initialData: TableData[] = await this.getData();
    let totalsRow: TableData = {};

    initialData.forEach((dataRow: TableData) => {
      Object.keys(dataRow).forEach((key) => {
        let value: typeof key = dataRow[key];

        if (isNaN(value)) {
          totalsRow[key] = "";
        } else {
          if (totalsRow[key] != undefined) {
            totalsRow[key] += value;
          } else {
            totalsRow[key] = value;
          }
        }
      });
    });

    totalsRow["name"] = "Total";

    initialData.push(totalsRow);

    return initialData;
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
}
</script>
