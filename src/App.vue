<template>
  <div class="container">
    <div class="d-flex align-items-center p-3">
      <h3 class="m-0">取引一覧</h3>
      <button class="btn btn-primary ms-auto" @click="inputform = true">追加</button>      
    </div>

    <div class="p-1 shadow rounded">
      <table class="table table-striped mt-3">
        <thead>
          <tr>
            <th>名前</th>
            <th>カテゴリ</th>
            <th>金額</th>
            <th>操作</th>            
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(item, index) in transactions"
            :key="index"
          >
            <td>{{ item.name }}</td>
            <td>{{ item.category }}</td>
            <td>{{ item.amount }}</td>
            <td>
              <button 
                @click="removeItem(index)"
                class="btn btn-sm btn-outline-danger"
              >
                削除
              </button>
            </td>
          </tr>
        </tbody>
      </table>        
    </div>
  
  </div>

  <div 
    v-if="inputform"
    class="position-absolute top-0 start-0 vw-100 vh-100 d-flex align-items-center justify-content-center"
    style="background-color: rgba(0, 0, 0, 0.5);"
  >
    <div class="container bg-white rounded w-75 px-3 py-4">

      <div class="row d-flex align-items-center mb-3">
        <label class="form-label col-3 m-0">名前</label>
        <div class="col-9">
        <input
          v-model="newItem.name"
          placeholder="お肉"
          class="form-control"
        >
        </div>
      </div>

      <div class="row d-flex align-items-center mb-3">
        <label class="form-label col-3 m-0">カテゴリ</label>
        <div class="col-9">
        <input
          v-model="newItem.category"
          class="form-control"
        >
        </div>
      </div>

      <div class="row d-flex align-items-center mb-3">
        <label class="form-label col-3 m-0">金額</label>
        <div class="col-9">
        <input
          v-model="newItem.amount"
          type="number"
          class="form-control"
        >
        </div>
      </div>

      <div class="mt-4">
        <button class="btn btn-outline-secondary" @click="inputform = false">キャンセル</button>
        <button class="btn btn-outline-primary ms-3" @click="addItem">追加する</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      inputform: false,
      newItem: {
        name: '',
        category: '',
        amount: 0
      },
      transactions: []
    }
  },
  methods: {
    addItem() {
      this.transactions.push({ ...this.newItem })
      this.saveItems()
      this.newItem = {
        name: '',
        category: '',
        amount: 0
      }
      this.inputform = false
    },
    removeItem(index) {
      this.transactions.splice(index, 1)
      this.saveItems()
    },
    saveItems() {
      localStorage.setItem('transaction', JSON.stringify(this.transactions))
    }
  },
  created() {
    const savedTransactions = localStorage.getItem('transaction')
    if (savedTransactions) {
      this.transactions = JSON.parse(savedTransactions)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
