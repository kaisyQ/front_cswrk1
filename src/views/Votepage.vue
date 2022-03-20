<template>
    <div>
        <h1>Vote page</h1>
        <div>
            <h2>Comments</h2>
            <table>
                <tr v-for="qst of questions" v-bind:key="qst">
                    <td>
                        {{ qst }}
                    </td>
                    <td>
                        <select>
                            <option v-for="ans of answers" v-bind:key="ans">{{ans}}</option>
                        </select>
                    </td>
                </tr>
            </table>
            <div>
                <div v-for="inf of inputsInfo" v-bind:key="inf">
                    <span>{{inf}}</span>
                    <input type="text">
                </div>
            </div>
        </div>
        <div>
            <button @click.prevent="sendAnswer">vote</button>
        </div>
        <hr />
        <hr />
        <div>
            <table>
            </table>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'VotePage',
  async mounted () {
   const allUsersAnswers = (await axios.get('http://localhost:3000/getAnswers')).data.answers
   const procents = []
   for (ans of allUsersAnswers) {
   }
  },
  data () {
    return {
      answers: [
        'Outstanding / Великолепно',
        'Very Good / Очень хорошо',
        'Good / Хорошо',
        'Adequate / Нормально',
        'Needs Improvement / Необходимо улучшить',
        'Poor / Плохо',
        'Don`t know / Не знаю'
      ],
      questions: [
        'Пожалуйста, оцените самолет, на котором Вы летели с AMONIC Airlines',
        'Как бы Вы оценили наших борт проводников',
        'Как бы Вы оценили развлечения во время полета',
        'Пожалуйста, оцените цену на билет Вашего маршрута'
      ],
      inputsInfo: [
        'firstname',
        'lastname',
        'age'
      ]
    }
  },
  methods: {
    async sendAnswer () {
      const answers = []
      const objectToSend = {}
      const user = {}
      const answerSelect = document.querySelectorAll('select')
      const userInfoArr = document.querySelectorAll('input')

      for (const ans of answerSelect) {
        for (let i = 0; i < this.answers.length; ++i) {
          if (ans.value === this.answers[i]) {
            answers.push(i)
            break
          }
        }
      }
      console.log(answers)
      user.firstname = userInfoArr[0].value
      user.lastname = userInfoArr[0].value
      user.age = userInfoArr[0].value
      objectToSend.user = user
      objectToSend.answers = answers
      const saveAns = await axios.post('http://localhost:3000/sendAnswer', objectToSend)
      console.log(saveAns)
    }
  }
}
</script>

<style scoped>
    h1, h2 {
        text-align: center;
    }
</style>
