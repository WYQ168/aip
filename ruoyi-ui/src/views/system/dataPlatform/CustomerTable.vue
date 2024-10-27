<template>
  <div class="customer-table-card">
    <div class="header">
      <h3>New Customers</h3>
      <button class="filter-button">
        <i class="fas fa-filter"></i> Filter
      </button>
    </div>
    <table class="customer-table">
      <thead>
      <tr>
        <th>Customer Name</th>
        <th>Transaction ID</th>
        <th>Date</th>
        <th>Pack</th>
        <th>Amount</th>
        <th>Status</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="customer in customers" :key="customer.id">
        <td class="customer-name">
          <img :src="customer.avatar" alt="avatar" class="avatar" />
          <span>{{ customer.name }}</span>
        </td>
        <td>{{ customer.id }}</td>
        <td>{{ customer.date }}</td>
        <td>{{ customer.pack }}</td>
        <td>\${{ customer.amount.toFixed(2) }}</td>
        <td>
            <span :class="['status', customer.status.toLowerCase()]">
              {{ customer.status }}
            </span>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: ['customers'],
};
</script>

<style scoped>
.customer-table-card {
  border: 1px solid #e5e5e5;
  border-radius: 8px;
  background-color: white;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.filter-button {
  background-color: #f5f5f5;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.filter-button i {
  margin-right: 5px;
}

.customer-table {
  width: 100%;
  border-collapse: collapse;
}

.customer-table th {
  text-align: left;
  padding: 10px;
  color: #888;
  font-weight: 500;
}

.customer-table td {
  padding: 15px 10px;
  border-top: 1px solid #e5e5e5;
  vertical-align: middle;
}

.customer-name {
  display: flex;
  align-items: center;
}

.avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  margin-right: 10px;
}

.status {
  padding: 5px 10px;
  border-radius: 20px;
  font-weight: 600;
  text-align: center;
  display: inline-block;
}

.status.active {
  background-color: #e6f4ea;
  color: #34a853;
}

.status.inactive {
  background-color: #fdecea;
  color: #ea4335;
}
</style>
