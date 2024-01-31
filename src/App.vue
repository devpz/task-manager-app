<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <input id="newtasks" v-model="newtasks" />&nbsp;
    <button @click="addNewtasks()">Add tasks</button>
    <p v-for="note in tasks" :key="note.id">
      <b>* {{ note.description }}</b>&nbsp;
      <button @click="deletetasks(note.id)">Delete tasks</button>
    </p>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import { getFirestore, collection, addDoc, deleteDoc, doc, getDocs } from 'firebase/firestore';
import { initializeApp } from 'firebase/app';

import { firebaseConfig } from './firebaseConfig'; 

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export default {
  setup() {
    const title = ref('Task manager app');
    const tasks = ref([]);
    const newtasks = ref('');

    const refreshData = async () => {
      tasks.value = [];
      const querySnapshot = await getDocs(collection(db, 'tasks'));
      querySnapshot.forEach((doc) => {
        tasks.value.push({ description: doc.data().description, id: doc.id });
      });
    };

    const addNewtasks = async () => {
      await addDoc(collection(db, 'tasks'), { description: newtasks.value });
      newtasks.value = ''; // Clear the input field
      refreshData();
    };

    const deletetasks = async (id) => {
      await deleteDoc(doc(db, 'tasks', id));
      refreshData();
    };

    onMounted(() => {
      refreshData();
    });

    return { title, tasks, newtasks, refreshData, addNewtasks, deletetasks };
  },
};
</script>


