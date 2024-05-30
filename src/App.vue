<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-toolbar-title>ARTIKEL</q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page class="q-pa-md">
        <q-card class="q-mb-md">
          <q-card-section>
            <div class="text-h6">Create / Edit Artikel</div>
          </q-card-section>
          <q-separator />
          <q-card-section>
            <q-form @submit.prevent="save">
              <q-input v-model="form.title" label="Title" outlined class="q-mb-md" />
              <q-input v-model="form.content" label="Content" type="textarea" outlined class="q-mb-md" />
              <q-space />
              <q-btn type="submit" color="primary" label="Save" />
              <q-btn type="reset" color="secondary" label="Reset" @click="resetForm" class="q-ml-md" />
            </q-form>
          </q-card-section>
        </q-card>

        <q-list bordered separator>
          <q-item v-for="article in articles" :key="article.id" clickable>
            <q-item-section>
              <q-item-label>{{ article.title }}</q-item-label>
              <q-item-label caption>{{ article.content }}</q-item-label>
            </q-item-section>
            <q-item-section side>
              <q-btn flat icon="edit" @click="edit(article)" />
              <q-btn flat icon="delete" color="negative" @click="remove(article.id)" />
            </q-item-section>
          </q-item>
        </q-list>

        <q-btn @click="load" color="secondary" label="Load" class="q-mt-md" />
      </q-page>
    </q-page-container>

    <q-footer class="q-pa-md q-mt-xl text-center" >
      <q-space />
      <div>
        <p class="text-caption">Contact Person: muhammaddhandhyputerairvantie@student.uir.ac.id</p>
      </div>
    </q-footer>
  </q-layout>
</template>

<script setup>
import { reactive, ref, onMounted } from 'vue';
import axios from 'axios';
import { QLayout, QHeader, QToolbar, QToolbarTitle, QPage, QPageContainer, QForm, QInput, QBtn, QCard, QCardSection, QSeparator, QSpace, QList, QItem, QItemSection, QItemLabel, QFooter, QAvatar } from 'quasar';

const form = reactive({
  id: null,
  title: '',
  content: '',
});

const articles = ref([]);

async function load() {
  try {
    const response = await axios.get('http://localhost:3000/articles');
    articles.value = response.data;
  } catch (error) {
    console.error('Error loading articles', error);
  }
}

async function save() {
  try {
    if (form.id) {
      await axios.put(`http://localhost:3000/articles/${form.id}`, form);
    } else {
      await axios.post('http://localhost:3000/articles', form);
    }
    load();
    resetForm();
  } catch (error) {
    console.error('Error saving article', error);
  }
}

function edit(article) {
  form.id = article.id;
  form.title = article.title;
  form.content = article.content;
}

async function remove(id) {
  try {
    await axios.delete(`http://localhost:3000/articles/${id}`);
    load();
  } catch (error) {
    console.error('Error deleting article', error);
  }
}

function resetForm() {
  form.id = null;
  form.title = '';
  form.content = '';
}

onMounted(load);
</script>

<style>
.container {
  max-width: 600px;
  margin: auto;
  padding: 16px;
}

.q-footer {
  padding: 16px;  
  background-color: rgb(3, 70, 138);
  color: #fff;
  font-size: medium;  
}

.q-footer p {
  margin: 6px 0; 
}

.q-footer .contact-info {
  font-size: 12px;
}

.q-footer .contact-info strong {
  font-weight: bolder;
}
</style>