<script lang="ts" setup>
import ListItemVue from './ListItem.vue'
import { computed, onMounted, ref } from 'vue'
import type { Ref } from 'vue'


type Item = {
    title: string;
    checked?: boolean;
}

const listItems: Item[] = [
    { title: 'Tworzę listę TODO', checked: true },
    { title: 'Tworzę listę TODO2', checked: true },
    { title: 'Tworzę listę TODO3', checked: false },
    { title: 'Tworzę listę TODO4', checked: false },
    { title: 'Tworzę listę TODO5', checked: true },
    { title: 'Tworzę listę TODO6' },
]

const storageItems: Ref<Item[]> = ref([])

const setToStorage = (items: Item[]): void => {
    localStorage.setItem('list-items', JSON.stringify(items))
}


</script>

<template>
    <ul>
        <li :key="key" v-for="(item, key) in listItems">
            <ListItemVue :is-checked="item.checked">{{ item.title }}</ListItemVue>
        </li>
    </ul>
</template>