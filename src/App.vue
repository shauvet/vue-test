<template>
    <div id="app">
        <h1 v-text="title"></h1>
        <input v-model="newItem" v-on:keyup.enter="addNewItem">
        <ul>
            <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
                {{item.label}}
            </li>
        </ul>
    </div>
    <component-a></component-a>
</template>
<script>
import Storage from './storage'
import ComponentA from './components/componentA'

export default {
    data: function () {
        return {
            title: 'this is todolist!',
            items: Storage.fetch(),
            newItem: ''
        }
    },
    components: {ComponentA},
    watch: {
        items: {
            handler: function(items) {
                Storage.save(items);
            },
            deep: true
        }
    },
    methods: {
        toggleFinish: function (item) {
            item.isFinished = !item.isFinished;
        },
        addNewItem: function () {
            this.items.push({
                label: this.newItem,
                isFinished: false
            })
            this.newItem = '';
        }
    }
}
</script>
<style>
.finished {
    color: #ccc;
}
html {
    height: 100%;
}

body {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
}

#app {
    color: #2c3e50;
    margin-top: -100px;
    max-width: 600px;
    font-family: Source Sans Pro, Helvetica, sans-serif;
    text-align: center;
}

#app a {
    color: #42b983;
    text-decoration: none;
}

.logo {
    width: 100px;
    height: 100px
}
</style>
