<template>
    <tbody>
        <tr>
            <td v-bind:id="'name'+ insert._id" contenteditable>{{ insert.name }}</td>
            <td v-bind:id="'uni'+ insert._id" contenteditable>{{ insert.uni }}</td>
            <td v-bind:id="'orient'+ insert._id" contenteditable>{{ insert.orient }}</td>
            <td v-bind:id="'points'+ insert._id" contenteditable>{{ insert.points }}</td>

            <td @click="$emit('updateInsert')"><img class="icon" src="./refresh.png"></td>
            <td @click="$emit('deleteInsert')"><img class="icon" src="./delete.png"></td>
        </tr>
    </tbody>
</template>

<script> 
    export default {
       
        props: {
            insert: Object
        },
        emits: ["deleteInsert", "updateInsert"]
        
    
    }
    </script>

    <style scoped>
    .icon {
        cursor: pointer;
        margin-left: 30px;
    }

    td {
        border-bottom: 1px solid rgb(105, 180, 180);
    }
</style>