<template>
  <a class="button" @click="getCustomerInfo()" type="button" href="/customerForm">
    Add Customer Information</a
  >
  <p></p>

  <table>
    <tr>
      <th>S.No</th>
      <th>Name</th>
      <th>Employee Id</th>
      <th>City</th>
      <th>Gender</th>
      <!-- <th>Action</th> -->
    </tr>
    <tr v-for="info in data" :key="info.id" v-if="data.length > 0">
      <td>{{ Number(info.id) + 1 }}</td>
      <td>
        <a :href="'/customerForm/edit/' + info.id">{{ info.name }}</a>
      </td>
      <td>{{ info.employeeId }}</td>
      <td>{{ info.city }}</td>
      <td>{{ info.gender[0] }}</td>
      <!---- <td style="color: red" @click="deleteInfo(info.id)">Delete</td> -->
    </tr>
    <p v-else class="txt">No Data Available</p>
  </table>
</template>
<script>
export default {
  auth: false,
  layout: 'blank',
  data() {
    return {
      data: localStorage.getItem('customer_info')
        ? JSON.parse(localStorage.getItem('customer_info'))
        : [],
      formData: {
        name: '',
        employeeId: '',
        city: '',
        gender: [],
        id: '0'
      },
      options: [
        { text: 'New York', value: 'New York' },
        { text: 'London', value: 'London' },
        { text: 'Tokyo', value: 'Tokyo' }
      ]
    }
  },
  methods: {
    deleteInfo(id) {
      this.data.splice(id, 1)
      localStorage.setItem('customer_info', JSON.stringify(this.data))
    }
  }
}
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

.button {
  width: 70%;
  padding: 10px 0;
  margin: 10px auto;
  border-radius: 5px;
  border: none;
  background: #8ebf42;
  font-size: 14px;
  font-weight: 600;
  color: #fff;
  text-align: center;
}
.button:hover {
  background: #82b534;
}
.delete {
  background: #8ebf42;
}
</style>
