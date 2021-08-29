<template>
  <q-page class="row items-center justify-evenly">
    <div class="row">
      <div class="col-12 col-md-auto">
        <q-input filled v-model="input_Name" label="请输入学生姓名" />
      </div>
      <div class="col-12 col-md-auto">
        <q-input
          filled
          v-model="input_PhoneNumber"
          label="请输入学生/家长手机号"
        />
      </div>
      <div class="col-4">
        <q-select
          filled
          v-model="select_GradeNumber"
          :options="selectOptions_GradeNumber"
          label="学生所在年级"
        />
      </div>
      <div class="col-4">
        <q-select
          filled
          v-model="select_ClassNumber"
          :options="selectOptions_ClassNumber"
          label="学生所在班级"
        />
      </div>
    </div>
    <div class="row">
      <div class="col-grow">
        <q-select
          filled
          @popup-show="functionasd"
          v-model="select_ClassClass"
          :options="selectOptions_ClassClassList"
          label="选择课程类型"
        />
      </div>
      <div class="col-grow">
        <q-select
          filled
          v-model="select_ClassName"
          :options="selectOptions_ClassNameList"
          label="选择具体课程"
        />
      </div>
    </div>
    <q-btn color="white" text-color="black" label="登记" @click="ClickUpload" />
  </q-page>
</template>

<style lang="sass" scoped>
.row > div
  padding: 5px 5px
</style>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/CompositionComponent.vue';
import { defineComponent, ref } from '@vue/composition-api';
import { console } from 'console';

export default defineComponent({
  name: 'PageIndex',
  components: { ExampleComponent },
  setup() {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: 'ct1'
      },
      {
        id: 2,
        content: 'ct2'
      },
      {
        id: 3,
        content: 'ct3'
      },
      {
        id: 4,
        content: 'ct4'
      },
      {
        id: 5,
        content: 'ct5'
      }
    ]);
    const meta = ref<Meta>({
      totalCount: 1200
    });
    return { todos, meta };
  }
});
</script>

<script>
export default {
  data() {
    return {
      input_Name: '',
      input_PhoneNumber: '',
      select_GradeNumber: null,
      selectOptions_GradeNumber: [
        '一年级',
        '二年级',
        '三年级',
        '四年级',
        '五年级',
        '六年级'
      ],
      select_ClassNumber: null,
      selectOptions_ClassNumber: [
        '一班',
        '二班',
        '三班',
        '四班',
        '五班',
        '六班'
      ],
      select_ClassClass: null,
      selectOptions_ClassClassList: [
        '课程类型1',
        '课程类型2',
        '课程类型3',
        '课程类型4'
      ],
      select_ClassName: null,
      selectOptions_ClassNameList: ['sad', 'asdasd'],
      UserData: {
        name: '',
        phoneNumber: '',
        grade: '',
        class: '',
        aspirationFirst: '',
        aspirationSecond: '',
        aspirationThree: ''
      }
    };
  },
  methods: {
    functionasd() {
      var url = 'http://localhost:8081/get';

      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(myJson => {
          //console.log(myJson);
          if (myJson.islist == true) {
            this.selectOptions_ClassClassList = myJson.list;
            //console.log(myJson.list);
          }
        });
    },
    getClass() {
      var url = 'http://localhost:8081/get';
      fetch(url)
        .then(function(response) {
          return response.json();
        })
        .then(function(myJson) {
          console.log(myJson);
        });
      //console.log()
    },
    ClickUpload() {
      console.log('Click Upload');
      //fetch.get('http://localhost:3000/Upload?' + 'name=' + this.input_Name);

      var url = 'http://localhost:8081/post';

        this.UserData.name=this.input_Name,
        this.UserData.phoneNumber= this.input_PhoneNumber,
        this.UserData.grade= this.select_GradeNumber,
        this.UserData.class= this.select_ClassNumber,
        this.UserData.aspirationFirst= '1',
        this.UserData.aspirationSecond= '2',
        this.UserData.aspirationThree= '3'
      
      fetch(url, {
        method: 'POST', // or 'PUT'
        body: JSON.stringify(this.UserData), // data can be `string` or {object}!
        headers: new Headers({
          'Content-Type': 'application/json'
        })
      })
        .then(res => res.json())
        .catch(error => console.error('Error:', error))
        .then(response => console.log('Success:', response));
    }
  }
};
</script>
