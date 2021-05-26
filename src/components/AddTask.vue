<template>
    <form class="add-form" @submit="onSubmit">
        <div class="form-control">
            <label for="">Tarefa</label>
            <input type="text" v-model="text" name="text" placeholder="Adicionar Tarefa" />
        </div>
        <div class="form-control">
            <label for="">Dia e Hora</label>
            <input type="text" v-model="dia" name="dia" placeholder="Adicionar Dia e Hora" />
        </div>
        <div class="form-control form-control-check">
            <label for="">Adicionar Lembrete</label>
            <input v-model="reminder" type="checkbox" name="reminder" />
        </div>
        <input type="submit"  value="Salvar Tarefa" class="btn btn-block" />
    </form>
</template>

<script>
export default ({
    name: 'AddTask', 
    data() {
        return {
            text: '',
            dia: '',
            reminder: false,
        }
    },
    methods: {
        onSubmit(e) {
            e.preventDefault()

            if(!this.text) {
                alert('Por favor adicione uma tarefa')
                return
            }

            const newTask = {
                text: this.text,
                dia: this.dia,
                reminder: this.reminder
            }

            this.$emit('add-task', newTask)

            this.text = ''
            this.dia = ''
            this.reminder = false
        }
    }
})
</script>

<style scoped>
   input {
       width: 100%;
       padding: 1em 0;
       border-radius: 0.3em;
       margin-right: 1em;
       border: 3px solid rgb(61, 61, 61);
   }

   input:focus {
       border: 3px solid rgb(4, 248, 85);
   }

   .form-control {
       margin-bottom: 1em;
       font-size: 1em;
       font-weight: bold;
       padding-bottom: 1em;
   }

   .form-control-check {
       float: left;
   }
</style>
