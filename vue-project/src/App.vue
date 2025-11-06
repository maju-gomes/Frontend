<template>
  <div>
    <button @click="playMusic">Play Music</button>
    <button @click="stopMusic">Stop Music</button>
  <div>
    ~<ul>
      <li v-for="user in users">{{ user.name }}</li>
    </ul>
    <ul>
      <li v-for="user in users"> {{ user.website  }}</li>
    </ul>
  </div>

</div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { Howl } from 'howler';
import  axios from 'axios';
import musicUrl from './assets/O Velho e a Flor - Toquinho (youtube).mp3';

const users = ref([]);

const sound = new Howl({
  src: [musicUrl],
  loop: true,
  volume: 1.0,
});

const isPlaying = ref(false);

function playMusic(){
  try {
    sound.play();
    isPlaying.value = true;
    console.log('playing music')
  } catch (err) {
    console.error('error playing sound', err);
  }
}

function stopMusic(){
  sound.stop();
  isPlaying.value = false;
}


onMounted(async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    users.value = response.data;
  } catch (error) {
    console.error('Error fetching users:', error);
  }
});
</script>