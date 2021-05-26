<template>
<div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>{{ task.text }}
        <i @click="$emit('delete-task', task.id)" class="fas fa-times float-right"></i>
    </h3>
    <h3>{{ task.dia }}</h3>
</div>
</template>

<script>

export default ({
    name: 'Task',
    props: {
        task: Object
    },
    
})
</script>

<style>

.fas {
    text-align: end;
    color: red;
}

.float-left {
    float: right;
}

    .task {
        background: rgb(95, 70, 70);
        margin: 5px;
        padding: 10px 20px;
        cursor: pointer;
        color: white;
    }

    .task.reminder {
        border-left: 10px solid rgb(31, 122, 58);
    }

    .task h3 {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-weight: bold;
    }

</style>