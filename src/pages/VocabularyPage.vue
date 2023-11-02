<template>
  <q-page padding>
    <div class="row flex-center">
      <div class="col-md-8 q-mb-md">
        <q-pagination
          @update:modelValue="loadNextPage($event)"
          class="flex-center"
          v-model="currentPage"
          :max="totalPages"
          direction-links
          boundary-links
          icon-first="skip_previous"
          icon-last="skip_next"
          icon-prev="fast_rewind"
          icon-next="fast_forward"
        />
      </div>
    </div>

    <div class="row flex-center">
      <div class="col-md-8">
        <q-list>
          <q-item v-for="entry in entries" :key="entry.keyword" clickable v-ripple>
            <q-item-section>
              <q-item-label>{{ entry.keyword }}</q-item-label>
              <q-item-label caption>{{ entry.translations.join(', ') }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import {onMounted, ref} from 'vue';
import axios from 'axios';
import {CollectionEntry} from 'components/models';

let entries = ref<CollectionEntry[]>([]);

let currentPage = ref(1);
let totalPages = ref(0);
let pageSize = ref(10);

const loadNextPage = (event: number) => {
  currentPage.value = event;

  axios.create({
    baseURL: 'http://localhost:5001/api/v1.0',
    headers: {
      'content-type': 'application/json',
      'authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6InRlc3R8YjhiYTQ2ZGIyOGFiZmE1NmJmYWVkODg3IiwiZXhwIjoxNzI3NTMwODQ4LCJpc3MiOiJodHRwczovL3Rlc3QtcGFsYWJyaXRhLm5ldC8iLCJhdWQiOiJodHRwczovL3BhbGFicml0YS5uZXQvYXBpIn0.abGneb0jZKvIFZ2Cf7l3HzTiIqBe_-JYWEVbchkVhZc'
    }
  })
    .get('collections/4f9a5093/entries?pagenumber=' + currentPage.value + '&pagesize=' + pageSize.value)
    .then(response => {
      entries.value = response.data.value;
    })
    .catch(error => {
      console.log(error)
    });
}

// Lifecycle
onMounted(() => {
  console.log('collections/4f9a5093/entries?pagenumber=' + currentPage.value + '&pagesize=' + pageSize.value);

  axios.create({
    baseURL: 'http://localhost:5001/api/v1.0',
    headers: {
      'content-type': 'application/json',
      'authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1laWRlbnRpZmllciI6InRlc3R8YjhiYTQ2ZGIyOGFiZmE1NmJmYWVkODg3IiwiZXhwIjoxNzI3NTMwODQ4LCJpc3MiOiJodHRwczovL3Rlc3QtcGFsYWJyaXRhLm5ldC8iLCJhdWQiOiJodHRwczovL3BhbGFicml0YS5uZXQvYXBpIn0.abGneb0jZKvIFZ2Cf7l3HzTiIqBe_-JYWEVbchkVhZc'
    }
  })
    .get('collections/4f9a5093/entries?pagenumber=' + +currentPage.value + '&pagesize=' + pageSize.value)
    .then(response => {
      totalPages.value = response.data.totalPages
      entries.value = response.data.value;
    })
    .catch(error => {
      console.log(error)
    });
}) // onMounted

</script>
