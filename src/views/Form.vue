<template>
  <div class="container-mt-2">
    <b-form>
      <b-form-group label="Titulo" label-for="subject">
        <b-form-input
          id="subject"
          v-model="form.subject"
          type="text"
          placehoder="Ex: lavar o carro"
          required
          autocomplete="off"
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Descrição" label-for="description">
        <b-form-textarea
          id="description"
          v-model="form.description"
          type="text"
          placehoder="Ex: preciso levar o carro para lavar"
          required
          autocomplete="off"
        ></b-form-textarea>
        <b-button type="submit" variant="outline-primary" @click="saveTask">Salvar</b-button>
      </b-form-group>
    </b-form>
  </div>
</template>

<script>
import ToastMixin from '@/mixins/toastMixin.js'
import '../styles/form.css'

export default {
  name: "Form",

  mixins: [ToastMixin],
  data() {
    return {
      form: {
        subject: "",
        description: ""
      },
      methodSave: "new"
    }
  },

  created(){
    if(this.$route.params.index === 0 || this.$route.params.index !== undefined) {
      this.methodSave = "update"
      let tasks = JSON.parse(localStorage.getItem("tasks"))
      this.form = tasks[this.$route.params.index]
    }
  },

  methods: {
    // Criando um método onde ao clicar no botão, eu salvo as informações no localStorage
    // Transformando a string que eu recebo em um array de objetos
    saveTask() {
      // Fazendo a verificação para que ao editar a tarefa, não adicione uma nova, só atualize.
      if(this.methodSave === "update") {
        let tasks = JSON.parse(localStorage.getItem("tasks"))
        tasks[this.$route.params.index] = this.form
        localStorage.setItem("tasks", JSON.stringify(tasks))
        this.showToast("success", "Sucesso", "Tarefas atualizada com sucesso")
        this.$route.push({name: "list"})
        return
      }


      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
      tasks.push(this.form) 
      localStorage.setItem("tasks", JSON.stringify(tasks)) // Salvando as informações no localstorage
      this.showToast("success", "Sucesso", "Tarefas criada com sucesso")
      this.$router.push({name: "list"}) // adicionando ele na rota de list e levando ele para essa rota
  },
}
}
</script>