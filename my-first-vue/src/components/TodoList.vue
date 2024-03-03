<script lang="ts" setup>
import ListItemVue from './ListItem.vue'
import { computed, onMounted, ref } from 'vue'
import type { Ref } from 'vue'

type Item = {
    title: string;
    checked?: boolean;
}

const initListItems = ():void => {
    if (storageItems.value?.length === 0) {
        const listItems =
  [
    { title: 'Tworzę listę TODO', checked: true },
    { title: 'Tworzę listę TODO2', checked: true },
    { title: 'Tworzę listę TODO3', checked: false },
    { title: 'Tworzę listę TODO4', checked: false },
    { title: 'Tworzę listę TODO5', checked: true },
    { title: 'Tworzę listę TODO6' ,checked: true},
]
setToStorage(listItems)
storageItems.value = listItems
    }
}

const findItemInList = (item: Item): Item | undefined => {
    return storageItems.value.find(
        (itemInList: Item) => itemInList.title === item.title
    )
}
const toggleItemChecked = (item: Item): void => {
    item.checked = !item.checked
}

const updateItem = (item: Item): void => {
    const updateItem = findItemInList(item)
    if (updateItem) {
        toggleItemChecked(updateItem)
        setToStorage(storageItems.value)
    }
}

const storageItems: Ref<Item[]> = ref([])

const setToStorage = (items: Item[]): void => {
    localStorage.setItem('list-items', JSON.stringify(items))
}

const getFromStorage = (): Item[] | [] => {
    const stored = localStorage.getItem('list-items')
    if (stored) {
        return JSON.parse(stored)
    }
    return []
}

const sortedList = computed(() => [
    ...storageItems.value].sort((a,b) => (a.checked ? 1: 0) - (b.checked ? 1: 0))
)

onMounted(() => {
    initListItems()
    storageItems.value = getFromStorage()
})

</script>

<template>
    <ul>
        <li :key="key" v-for="(item, key) in sortedList">
            <ListItemVue :is-checked="item.checked" v-on:click.prevent="updateItem(item)">{{ item.title }}</ListItemVue>
        </li>
    </ul>
</template>