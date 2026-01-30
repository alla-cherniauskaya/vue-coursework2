<template>
  <div class="container column">
    <form class="card card-w30">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="blockType">
          <option value="title" selected> Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="formValue"></textarea>
        <small v-if="error">{{error}}</small>
      </div>

      <app-button
      :disabledButton="isDisabled"
      color="primary" 
      action="add-new" 
      @add-new="addNewValue">Добавить</app-button>
    </form>

  <app-user-resume :userData="userData"></app-user-resume> 
    
  </div>


  <div class="container">
    <p>
      <app-button 
        color="primary" 
        action="upload-comments"
        @upload-comments="uploadCommentsList">
        Загрузить комментарии
      </app-button>
    </p>
    <app-comments :comments="commentsList"></app-comments>
    <app-loader v-if="loadingComments"></app-loader>
  </div>
</template>

<script>
import AppUserResume from './AppUserResume.vue';
import AppComments from './AppComments.vue';
import AppButton from './AppButton.vue';
import AppLoader from './AppLoader.vue';
import axios from 'axios';

export default {
  data() {
    return {
      userData: [], //{id: '', blockType: '', value: ''}
      commentsList: [],
      loadingComments: false,
      blockType: 'title',
      formValue: '',
      error: '',
    }
  },
  methods: {
    addNewValue() {
      this.error = '';
      const isUrl = string => {
        try { return Boolean(new URL(string)); }
        catch(e){ return false; };
      }

      if (this.blockType === 'avatar') {
        if(isUrl(this.formValue)) {
          this.userData.push({
            id: Math.random(),
            blockType: this.blockType,
            value: this.formValue,
          })  
        } else {
          this.error = 'Please enter valid URL';
        }
        
      } else {
        this.userData.push({
          id: Math.random(),
          blockType: this.blockType,
          value: this.formValue,
        })
      }
    
      this.formValue = null;
      this.blockType = 'title';
    },
    async uploadCommentsList() {
      this.loadingComments = true;
      try {
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.commentsList = data;
        this.loadingComments = false;
        
      } catch(e) {
        this.loadingComments = false;
        console.error('upload comments error:', e);

      }
 
    }
  },
  computed: {
    isDisabled () {
      return !this.formValue || this.formValue.length < 4 || !this.formValue?.length;
    }
  },
  components: {
    AppUserResume, AppComments, AppButton, AppLoader
  }

}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>