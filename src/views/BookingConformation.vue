<template>
  <div class="booking__data">
    <div class="booking">
      <div class="booking__header">
        Outbound flight details:
      </div>
      <div class="booking__details">
        <div class="data">
          From: <span class="user__booking__data">{{ $route.params.from }}</span>
        </div>

        <div class="data">
          To: <span class="user__booking__data">{{ $route.params.to }}</span>
        </div>

        <div class="data">
          Cabin Type: <span class="user__booking__data">{{ $route.params.toCabinType }}</span>
        </div>

        <div class="data">
          Date: <span class="user__booking__data">{{ toDateTransformed }}</span>
        </div>

        <div class="data">
          Flight Number: <span class="user__booking__data">{{ $route.params.toFlightNumber }}</span>
        </div>
      </div>
    </div>

    <div class="booking">
      <div class="booking__header">
        Return flight details:
      </div>
      <div class="booking__details">
        <div class="data">
          From: <span class="user__booking__data">{{ $route.params.to }}</span>
        </div>

        <div class="data">
          To: <span class="user__booking__data">{{ $route.params.from }}</span>
        </div>

        <div class="data">
          Cabin Type: <span class="user__booking__data">{{ $route.params.backCabinType }}</span>
        </div>

        <div class="data">
          Date: <span class="user__booking__data">{{ backDateTransformed }}</span>
        </div>

        <div class="data">
          Flight Number: <span class="user__booking__data">{{ $route.params.backFlightNumber }}</span>
        </div>
      </div>
    </div>

    <div class="booking">
      <div class="booking__header">
        Passenger details
      </div>
        <form class="booking__form">
          <div class="data__selectors">
            <div class="data__selectors__item">
              Firstname
              <input class="user__info__input border" type="text" placeholder="Alex" v-model="firstName">
            </div>

            <div class="data__selectors__item">
              Lastname
              <input class="user__info__input border" type="text" placeholder="Nice" v-model="lastName">
            </div>

            <div class="data__selectors__item">
              Birthdate
              <input class="user__info__input" type="date" placeholder="Birthdate" style="border: 1px solid black;margin-left:1rem;" v-model="birthDate">
            </div>
          </div>

          <div class="data__selectors">
            <div class="data__selectors__item">
              Passport Number
              <input class="user__info__input border" type="text" placeholder="1322 2288" v-model="passportNumber">
            </div>

            <div class="data__selectors__item">
                Country
                <select class="user__info__input select border" v-model="country">
                    <option>Select country</option>
                    <option v-for="country in countryData" v-bind:key="country.country_id">{{country.country_name}}</option>
                </select>
            </div>

            <div class="data__selectors__item">
              Phone
              <input class="user__info__input border" type="tel" placeholder="+228-222-333-7" v-model="phone">
            </div>
          </div>

          <div class="data__selectors">
            <div class="data__selectors__item">
              <button class="btn" @click.prevent="addPassenger">Add passenger</button>
            </div>

            <div class="data__selectors__item">
              <button class="btn" @click.prevent="removePassenger">Remove passenger</button>
            </div>
          </div>
        </form>
    </div>

    <div class="booking">
      <div class="manage_table">
        <table class="data__table manage">
          <tr class="table__header">
            <td>Firstname</td>
            <td>Lastname</td>
            <td>Birthdate</td>
            <td>Passport number</td>
            <td>Passport country</td>
            <td>Phone</td>
          </tr>
        </table>

      <div class="table__scroll booking__height">
        <table class="data__table manage">
          <tr class="user_data-table_element" v-for="passenger of passengers" v-bind:key="passenger">
            <td>{{ passenger.firstName }}</td>
            <td>{{ passenger.lastName }}</td>
            <td>{{ passenger.birthDate }}</td>
            <td>{{ passenger.passportNumber }}</td>
            <td>{{ passenger.country }}</td>
            <td>{{ passenger.phone }}</td>
          </tr>
        </table>
      </div>
    </div>

    <div class="booking">
      <form class="booking__form no__border" @submit.prevent>
        <div class="data__selectors">
          <div class="data__selectors__item">
            <button class="btn" @click="back">&orarr;Back to search for flights</button>
          </div>

          <div class="data__selectors__item">
            <button class="btn" @click.prevent="showBookingModal">&#10004;Confirm booking</button>
            <BookingModal v-model:BookModalShow="bookingModalVisible"></BookingModal>
          </div>
        </div>
      </form>
    </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BookingModal from '../components/manageFlight/BookFlightModal.vue'

export default {
  name: 'BookingConformation',
  components: {
    BookingModal
  },
  data () {
    return {
      bookingModalVisible: false,
      toDateTransformed: this.$route.params.toDate.split('-').reverse().join('/'),
      backDateTransformed: this.$route.params.backDate.split('-').reverse().join('/'),
      countryData: null,
      firstName: '',
      lastName: '',
      birthDate: '',
      passportNumber: '',
      country: '',
      phone: '',
      passengers: []
    }
  },
  async mounted () {
    const countriesData = await axios.post('http://localhost:3000/GetCountries', { token: localStorage.getItem('Token') })
    if (countriesData.data.isCountriesTaken) {
      this.countryData = countriesData.data.countries
    } else {
      console.log(countriesData)
    }
  },
  methods: {
    back () {
      this.$router.back()
    },
    showBookingModal () {
      this.bookingModalVisible = true
    },
    addPassenger () {
      if (this.firstName && this.lastName && this.birthDate && this.passportNumber && this.country && this.phone) {
        const passenger = {
          firstName: this.firstName,
          lastName: this.lastName,
          birthDate: this.birthDate.split('-').reverse().join('/'),
          passportNumber: this.passportNumber,
          country: this.country,
          phone: this.phone
        }
        this.passengers.push(passenger)
      }
    },
    removePassenger () {
      if (this.firstName && this.lastName && this.birthDate && this.passportNumber && this.country && this.phone) {
        const passenger = {
          firstName: this.firstName,
          lastName: this.lastName,
          birthDate: this.birthDate.split('-').reverse().join('/'),
          passportNumber: this.passportNumber,
          country: this.country,
          phone: this.phone
        }
        for (let index = 0; index < this.passengers.length; ++index) {
          let boolResult = true
          boolResult &&= this.passengers[index].firstName === passenger.firstName
          boolResult &&= this.passengers[index].lastName === passenger.lastName
          boolResult &&= this.passengers[index].birthDate === passenger.birthDate
          boolResult &&= this.passengers[index].passportNumber === passenger.passportNumber
          boolResult &&= this.passengers[index].country === passenger.country
          boolResult &&= this.passengers[index].phone === passenger.phone
          if (boolResult) {
            this.passengers.splice(index, 1)
            break
          }
        }
      }
    }
  }
}
</script>

<style>
  .booking {
    margin: 1rem 1rem 2rem;
    font-size: 1.2rem;
  }
  .booking__header {
    margin-bottom: 0.5rem;
  }
  .booking__details {
    display: flex;
    justify-content: space-around;
    font-size: 1.2rem;
    padding: 1rem;
    border: 1px solid black;
  }
  .user__booking__data {
    font-weight: bold;
  }
  .booking__form {
    border: 1px solid black;
  }
  .no__border {
    border: none;
  }
  .booking__height {
    min-height: 170px;
    max-height: 300px;
  }
</style>
