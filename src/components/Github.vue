<template>
  <div>
    <h1 class="container my-3">My Github Repos</h1>
    <table
      class="table  table-striped table-bordered container"
      v-for="(item, i) in newData"
      :key="i"
    >
      <thead class="bg-warning">
        <tr>
          <th scope="col">#</th>
          <th scope="col">title</th>
          <th scope="col">description</th>
          <th scope="col">url</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row" width="100">{{ i + 1 }}</th>
          <td width="150">
            <strong>{{ item.name }}</strong>
          </td>
          <td width="800">{{ item.description }}</td>

          <td width="500">
            <a :href="item.svn_url">{{ item.svn_url }}</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

export default {
  data() {
    return {
      Data: [],
      newData: [],
      scrollData: [],
      Num: 0,  
      Counts: 6,
    };
  },
  methods: {
    getData() {
      const vm = this;
      const api = "https://api.github.com/users/because-hung/repos";
      vm.$http.get(api).then((response) => {
        vm.Data = response.data;
        vm.length = response.data.length;

        if (vm.Num == 0) {  //判斷是不是剛載入頁面
          vm.newData = vm.Data.slice(vm.Num, vm.Counts); //初始值 Num = 0 Counts = 6  意思是最開始的畫面只呈現6個資料
        } else {
          vm.scrollData = vm.Data.slice(vm.Num, vm.Counts);

          vm.newData.push(...vm.scrollData); //當畫面滑到最底下 增加6筆資料
        //   console.log(vm.newData);
          vm.scrollData = []; //清空
        }

        vm.Num += 6;    //之後當畫面滑到最底下 每次增加6筆資料
        vm.Counts += 6;
      });
    },
    Scroll() {
      const clientHeight = document.documentElement.clientHeight;  //網頁內容的高度 
      const pageHeight = document.documentElement.scrollHeight;   //整個頁面的高度
      const scrollY = window.scrollY;  //垂直捲軸滾動的高度
      if (scrollY + clientHeight >= pageHeight) {  //垂直捲軸滾動的高度 + 網頁內容的高度 > 整個頁面的高度 就增加資料
        this.getData();  
      }
    },
  },
  created() {
    this.getData();
    window.addEventListener("scroll", () => {  //監聽畫面有沒有滑到最下面
      this.Scroll();
    });
  },
};
</script>
