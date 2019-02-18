<template>
  <div>
    <header class="fixed-top pt-0 pt-lg-4">
      <div class="container">

        <div class="row d-none d-lg-block">
          <div class="col-12">
            <div class="clearfix">
              <div class="float-left">
                <div class="lead text-muted">Todos.id</div>
              </div>
              <div class="float-right">

              </div>
            </div>
          </div>
        </div>

        <div class="row">
          <div class="col-12 col-lg-6 mx-auto">
            <div class="form-group mt-3 mt-lg-1">
              <div class="input-group mb-3">
                <input
                  type="search"
                  v-model="newTodo"
                  @keyup.enter="add"
                  :class="{'form-control': true, 'bg-dark': darkMode, 'text-light': darkMode, 'text-dark': !darkMode, 'bg-light': !darkMode}"
                  placeholder="Add something ..."
                  autofocus
                  autocomplete="off"
                >
                <div class="input-group-append">
                  <button :class="{'btn': true, 'bg-dark': darkMode, 'text-light': darkMode, 'text-dark': !darkMode, 'bg-light': !darkMode}" type="button" @click="add">
                    <i class="fa far fa-plus"></i>
                  </button>
                </div>
              </div>
            </div>

          </div>
        </div>

      </div>

    </header>

    <section role="main" class="container pt-lg-5 mt-lg-5">

      <div class="row mt-5 pt-4 mt-lg-0 pt-lg-0">
        <div class="col-12 col-lg-6 mx-auto">
          <dl v-if="todos.length" class="mt-3">
            <dd v-for="(todo, index) in todos" :key="index" class="py-2 px-3 rounded">
              <div class="clearfix">
                <div class="float-left">
                  <a class="draggable pr-2">&equiv;</a>
                  <div class="pretty p-icon p-round p-jelly">
                    <input type="checkbox" v-model="todo.finishedAt" @change="update(todo)" :false-value="null">
                    <div class="state p-success">
                      <i class="icon fa fa-check"></i>
                      <label>{{ todo.name }}</label>
                    </div>
                  </div>
                </div>
                <div class="float-right">
                  <span @click="remove(todo)" class="pl-3 options"><i class="fas fa-trash"></i></span>
                  <span class="pl-3 options">&middot;&middot;&middot;</span>
                </div>
              </div>
            </dd>
          </dl>

          <div v-if="!todos.length" class="text-center text-muted my-5">
            <i class="far fa-2x fa-grin-tears"></i>
            <div class="lead mt-2">You don't have any todos.</div>
          </div>
        </div>
      </div>

    </section>

    <footer class="p-4 small">
      <div class="container">
        <div class="row">
          <div class="col-12 col-lg-6">

            <div class="pretty p-switch p-fill">
              <input type="checkbox" v-model="darkMode">
              <div class="state">
                <label>Dark mode</label>
              </div>
            </div>

          </div>
          <div class="col-12 col-lg-6 text-center text-lg-right">
            &copy;2019 mul14
          </div>
        </div>

      </div>
    </footer>

  </div>
</template>

<script>
  import { mapState, mapMutations } from 'vuex'

  export default {
    name: 'App',

    mounted () {
      this.$store.commit('darkMode', this.$store.state.darkMode)
    },

    computed: {
      ...mapState(['todos']),

      completed() {
        return this.todos.map((todo) => {
          if (!todo.finishedAt) return todo
        })
      },

      newTodo: {
        get () {return this.$store.state.newTodo},
        set (value) {return this.$store.commit('newTodo', value)},
      },

      darkMode: {
        get () {return this.$store.state.darkMode},
        set () {
          return this.$store.commit('darkMode', !this.$store.state.darkMode)
        },
      },

    },

    methods: {
      ...mapMutations(['add', 'remove', 'update']),
    },
  }
</script>

<style lang="scss" scoped>
  @import '~bootstrap/scss/bootstrap';
  @import '~pretty-checkbox/src/pretty-checkbox';

  input {
    border: none !important;
  }

  a:focus, a:active,
  button:focus, button:active,
  input:focus, input:active,
  textarea:focus, textarea:active {
    outline: 0 !important;
  }

  dd:hover {
    background-color: #dfe4ea;
  }

  .dark dd:hover {
    background-color: #34495e;
  }

  .draggable {
    visibility: hidden;
    cursor: grab;
  }

  .draggable:active {
    cursor: grabbing;
  }

  dd:hover .draggable {
    visibility: visible;
  }

  .options {
    cursor: pointer;
    visibility: hidden;
  }

  dd:hover .options {
    visibility: visible;
  }
</style>
