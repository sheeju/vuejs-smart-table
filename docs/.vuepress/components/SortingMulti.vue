<template>
  <div class="card mt-3">
    <div class="card-body">
      <v-table
        :data="users"
      >
        <thead slot="head">
          <v-th :sortKey="nameLength" defaultSort="desc">Name</v-th>
          <v-th sortKey="age">Age</v-th>
          <v-th sortKey="address.state">State</v-th>
          <v-th sortKey="customOrder" :customSort="customMultiSort">Registered</v-th>
          <v-th sortKey="customOrder" :customSort="customMultiSort">Status</v-th>
        </thead>
        <tbody slot="body" slot-scope="{displayData}">
          <tr v-for="row in displayData" :key="row.guid">
            <td>{{ row.name }}</td>
            <td>{{ row.age }}</td>
            <td>{{ row.address.state }}</td>
            <td>{{ row.registered }}</td>
            <td>{{ row.isActive }}</td>
          </tr>
        </tbody>
      </v-table>
    </div>
  </div>
</template>

<script>
import users from './data.json'
import VTable from '../../../src/VTable.vue'
import VTh from '../../../src/VTh.vue'

export default {
  name: 'SortingMulti',
  components: { VTable, VTh },
  data: () => ({
    users: users.slice(0, 10)
  }),
  methods: {
    nameLength (row) {
      return row.name.length
    },
    dateSort(a, b) {
      let date1 = new Date(a.registered).getTime();
      let date2 = new Date(b.registered).getTime();

      return date1 - date2;
    },
    customMultiSort(a, b, sortOrder) {
      // firstSort - asc
      let aFirstVal = a.isActive;
      let bFirstVal = b.isActive;

      //For boolean
      let firstSorted = (aFirstVal === bFirstVal)? 0 : aFirstVal? 1 : -1;
      //For String
      // let firstSorted = aFirstVal.localeCompare(bFirstVal);

      // secondSort - asc/desc
      let aSecondVal = new Date(a.registered).getTime();
      let bSecondVal = new Date(b.registered).getTime();

      let secondSorted = (aSecondVal - bSecondVal) * sortOrder;
      console.log(sortOrder, firstSorted, secondSorted);

      return firstSorted || secondSorted;
    }
  }
}
</script>

<style>
  @import "~bootstrap/dist/css/bootstrap.css";
</style>
