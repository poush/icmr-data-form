<template>
  <div id="app">
    <!-- <login></login> -->
    <el-row>
      <el-col :span="24">
      <men></men>
      </el-col>
    </el-row>
    <el-row style="margin-top:50px">
    <el-col :span="12" :offset="6">
        <formy></formy>
    </el-col>
    </el-row>
  </div>
</template>
<script>

  import Formy from './components/Form.vue';
  import Men from './components/Menu.vue';

  export default {
    name: 'app',
    components: {
      Formy, Men
    }
  };
</script>
