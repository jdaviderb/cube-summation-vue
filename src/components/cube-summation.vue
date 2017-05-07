<template>
  <div class="container">
    <div class="input">
      <h2>Entrada: </h2>
      <textarea v-model="input" class="input-edit" id="" cols="30" rows="10"></textarea>
    </div>

    <div class="input">
      <h2>Salida: </h2>
      <textarea v-model="output.output" disabled class="input-edit " id="" cols="30" rows="10">
      </textarea>
    </div>

    <div class="input">
      <h2>Casos: {{ output.cases || 0 }}</h2>
      <h2>Operaciones: {{ output.querys || 0 }}</h2>
      <small v-if="output.error">ERROR</small>
    </div>

  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      input: 'as'
    }
  },

  methods: {
    makeMatrix (length) {
      let m = []
      for (let x = 0; x < length; x++) {
        m[x] = []
        for (let y = 0; y < length; y++) {
          m[x][y] = []
          for (let z = 0; z < length; z++) {
            m[x][y][z] = 0
          }
        }
      }
      return m
    },
    updateMatrix (matrix, data) {
      data = data.map((cors) => parseInt(cors))
      matrix[data[0] - 1][data[1] - 1][data[2] - 1] = data[3]
    },
    queryMatrix (matrix, data) {
      let i = 0
      data = data.map((cors) => parseInt(cors))
      for (let x = data[0] - 1; x <= data[3] - 1; x++) {
        for (let y = data[1] - 1; y <= data[4] - 1; y++) {
          for (let z = data[2] - 1; z <= data[5] - 1; z++) {
            i += matrix[x][y][z]
          }
        }
      }
      return i
    }
  },

  computed: {
    output () {
      const input = this.input.split('\n')
      const cases = parseInt(input[0])
      const output = {
        output: '',
        cases: cases,
        querys: 0,
        error: false
      }

      try {
        let startQuery = 1
        for (let numberCase = 0; numberCase < cases; numberCase++) {
          const matrixInfo = input[startQuery].split(' ').map((data) => parseInt(data))
          const matrix = this.makeMatrix(matrixInfo[0])
          output.querys += matrixInfo[1]
          startQuery++
          const matrixCase = input.slice(startQuery)
          for (let numberQuery = 0; numberQuery < matrixInfo[1]; numberQuery++) {
            startQuery++
            let query = matrixCase[numberQuery].split(' ')
            if (query[0] === 'UPDATE') {
              this.updateMatrix(matrix, query.slice(1))
            } else if (query[0] === 'QUERY') {
              output.output += `${this.queryMatrix(matrix, query.slice(1))}\n`
            }
          }
        }
      } catch (err) {
        output.error = true
      }
      return output
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .container {
    display: flex;
    justify-content: center;
    flex-direction: row;
    align-items: center;
    min-height: 100vh;
    background: -webkit-linear-gradient(left top, #F2F2F2, #7ABDB5); 

  }

  h2 {
    color: gray;
  }

  .input, .output {
    margin: 2em;
  }

  .input-edit {
    border-radius: 1em;
    outline: 0;
    padding: 1em;
    resize: none;
    border: none;
    transition: all 0.5s;
    border: solid 2px #84C2BA;
    text-align: center;

  }

  .input-edit:focus {
    border: solid 2px #68A19A;
  }

</style>
