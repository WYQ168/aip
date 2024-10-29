<template>
  <div class="customer-table-card">
    <div class="header">
      <h3>月度碳排放强度</h3>
      <button class="filter-button">
        <i class="fas fa-filter"></i> Filter
      </button>
    </div>
    <table class="customer-table">
      <thead>
      <tr>
        <th></th>
        <th>1月</th>
        <th>2月</th>
        <th>3月</th>
        <th>4月</th>
        <th>5月</th>
        <th>6月</th>
        <th>7月</th>
        <th>8月</th>
        <th>9月</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="customer in customers" :key="customer.id">
        <td class="customer-name">
          <span>{{ customer.name }}</span>
        </td>
        <td>{{ customer.id }}</td>
        <td>{{ customer.date }}</td>
        <td>{{ customer.pack }}</td>
        <td>{{ customer.pack1 }}</td>
        <td>{{ customer.pack2 }}</td>
        <td>{{ customer.pack3 }}</td>
        <td>{{ customer.pack4 }}</td>
        <td>{{ customer.amount }}</td>
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
