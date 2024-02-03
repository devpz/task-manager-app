<template>
  <v-app>
    <v-main>
      
  <v-card max-width="996" class="mx-auto" color="grey-lighten-3">
    <v-layout>
      <v-app-bar
        color="teal-darken-4"
        image="https://picsum.photos/1920/1080?random"
      >
        <template v-slot:image>
          <v-img
            gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
          ></v-img>
        </template>

        <template v-slot:prepend>
          <v-app-bar-nav-icon></v-app-bar-nav-icon>
        </template>

        <v-app-bar-title>Task manager app</v-app-bar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-heart</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </v-app-bar>
      

      <v-main>
        <v-container fluid>
          
          <v-row dense>

            
  <v-text-field id="newtasks" label="New task" v-model="newtasks"> 
    <template v-slot:append>
      <v-btn @click="addNewtasks()">
        Add
      </v-btn>
    </template>
   
  </v-text-field>

            <v-col
              v-for="note in tasks"
              :key="note.id"
              cols="12"
            >
            
              <v-card class="d-flex" v-slot:append
                :title="`${note.description}`"><v-btn  v-slot:append density="compact"  icon="mdi-check" @click="deletetasks(note.id)"></v-btn>
          
    
              </v-card>
              
              
            </v-col>
          </v-row>
        </v-container>
      </v-main>
    </v-layout>
  </v-card>
    </v-main>  
  </v-app>
  
</template>

<script>

import { ref, onMounted } from 'vue';
import { getFirestore, collection, addDoc, deleteDoc, doc, getDocs } from 'firebase/firestore';
import { initializeApp } from 'firebase/app';
import HelloWorld from './components/HelloWorld.vue'

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
