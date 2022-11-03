<template>
  <div class="container">
    <my-select name="Выберите город" v-model="city" :options="citySortOption" />
    <my-select
      name="Выберите цех"
      v-model="department"
      :options="departmentSortOption"
      :disabled="!city"
    />
    <my-select
      name="Выберите сотрудника"
      v-model="worker"
      :options="workersSortOption"
      :disabled="!department || !city"
    />
  </div>
</template>

<script>
import MySelect from './components/MySelect.vue';
import { mockData } from './mockData';

export default {
  components: {
    MySelect,
  },
  data() {
    return {
      city: '',
      department: '',
      worker: '',
      citySortOption: [],
      departmentSortOption: [],
      workersSortOption: [],
    };
  },
  methods: {
    setCookie() {
      const cookieData = {
        city: this.city,
        department: this.department,
        worker: this.worker,
      };
      document.cookie = 'cookieData=' + JSON.stringify(cookieData);
    },
    getCookie() {
      const cookieValue = document.cookie
        .split('; ')
        .find((row) => row.startsWith('cookieData='))
        ?.split('=')[1];
      if (cookieValue) {
        const cookieData = JSON.parse(cookieValue);
        this.city = cookieData.city;
        this.department = cookieData.department;
        this.worker = cookieData.worker;
      }
    },
  },
  mounted() {
    for (let item in mockData) {
      this.citySortOption.push({ name: item, value: item });
    }
    this.getCookie();
  },
  watch: {
    city: function () {
      if (this.city) {
        this.departmentSortOption = [];
        for (let item in mockData[this.city]) {
          this.departmentSortOption.push({ name: item, value: item });
        }
      } else {
        this.department = '';
        this.worker = '';
        this.departmentSortOption = [];
        this.workersSortOption = [];
      }
      this.setCookie();
    },
    department: function () {
      if (this.department) {
        this.workersSortOption = [];
        let key = mockData[this.city][this.department];
        for (let i = 0; i < key.length; i++) {
          this.workersSortOption.push({ name: key[i], value: key[i] });
        }
      } else {
        this.worker = '';
        this.workersSortOption = [];
      }
      this.setCookie();
    },
    worker: function () {
      this.setCookie();
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
