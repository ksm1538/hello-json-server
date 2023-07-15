<template>
  <div>
    <h2>INSERT</h2>
    <input type="text" placeholder="name" v-model="insertData.name" />
    <button @click="insertClicked()">INSERT</button> 
  </div>
  <hr />
  <div>
    <h2>SELECT</h2>
    <input type="text" placeholder="id" v-model="selectData.id"/>
    <input type="text" placeholder="name" v-model="selectData.name"/>
    <button @click="selectClicked()">SELECT</button>
    <div>
      {{ usersData }}
    </div>
  </div>
  <hr />
  <div>
    <h2>UPDATE</h2>
    <input type="text" placeholder="id" v-model="updateData.id"/>
    <input type="text" placeholder="name" v-model="updateData.name"/>
    <button @click="updateClicked()">UPDATE</button> 
  </div>
  <hr />
  <div>
    <h2>DELETE</h2>
    <input type="text" placeholder="id" v-model="deleteData.id"/>
    <button @click="deleteClicked()">DELETE</button> 
  </div>
</template>

<script>
import { ref } from 'vue';
import axios from 'axios';

export default {
  setup() {
    // INSERT
    let insertData = ref({
      name: ''
    });

    const insertClicked = async () => {
      if (insertData.value.name를 == '') {
          alert('입력할 name를 입력해주세요.');
          return;
        }

      try {
        await axios.post(
          `http://localhost:3000/users`, { name: insertData.value.name }
        );

        insertData.value.name = '';
  
        alert('데이터를 추가하였습니다.');
      } catch (err) {
        console.log(err);
        alert('오류가 발생하였습니다.');
      }    
    };

    // SELECT
    let selectData = ref({
      id:'',
      name: ''
    });

    let usersData = ref([]);

    const selectClicked = async () => {
      try {
        let paramData = {
          id: selectData.value.id,
          name: selectData.value.name
        };

        if (paramData.id == '') {
          delete paramData.id;
        }

        if (paramData.name == '') {
          delete paramData.name;
        }
         
        const res = await axios.get(
          `http://localhost:3000/users`,
          { params : paramData }
        );

        usersData.value = res.data;
      } catch (err) {
        console.log(err);
        alert('오류가 발생하였습니다.');
      }
    };

    // UPDATE
    let updateData = ref({
      id:'',
      name: ''
    });

    const updateClicked = async () => {
      try {
        if (updateData.value.id == '') {
          alert('수정할 데이터의 id를 입력해주세요.');
          return;
        }

        if (updateData.value.name == '') {
          alert('수정할 데이터의 name 입력해주세요.');
          return;
        }
         
        await axios.patch(
          `http://localhost:3000/users/${updateData.value.id}`,
          { name : updateData.value.name }
        );
      
        updateData.value.id = '';
        updateData.value.name = '';

        alert('데이터를 수정하였습니다.');
      } catch (err) {
        console.log(err);
        alert('오류가 발생하였습니다.');
      }    
    };

    // DELETE
    let deleteData = ref({
      id:''
    });

    const deleteClicked = async() => {
      try { 
        if (deleteData.value.id == '') {
          alert('삭제할 데이터의 id를 입력해주세요.');
          return;
        }

        await axios.delete(
          `http://localhost:3000/users/${deleteData.value.id}`
        );
      
        deleteData.value.id = '';

        alert('데이터를 삭제하였습니다.');
      } catch (err) {
        console.log(err);
        alert('오류가 발생하였습니다.');
      }
    }


    return {
      insertData,
      insertClicked,

      selectData,
      usersData,
      selectClicked,

      updateData,
      updateClicked,

      deleteData,
      deleteClicked,
    };
  }
}
</script>

<style>
</style>
