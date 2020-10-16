<template>
  <div id="clientform">
    <form class="new-client-form" @submit.prevent="submit">
      <h2>Новый клиент</h2>

      <div class="personal-info">
        <h2 class="section-header">Персональные данные</h2>

        <div class="input-panel">
          <label for="lastName">Фамилия</label>
          <input type="text" id="lastName" maxlength="35" v-model.trim="$v.lastName.$model"/>
          <div class="error" v-if="!$v.lastName.required">Введите Фамилию.</div>
          <div class="error" v-if="!$v.lastName.minLength">Минимум {{$v.lastName.$params.minLength.min}} символа.</div>
        </div>
        <div class="input-panel">
          <label for="firstName">Имя</label>
          <input type="text" id="firstName" maxlength="27" v-model.trim="$v.name.$model"/>
          <div class="error" v-if="!$v.name.required">Введите имя.</div>
          <div class="error" v-if="!$v.name.minLength">Минимум {{$v.name.$params.minLength.min}} символа.</div>

        </div>
        <div class="input-panel">
          <label for="patronymic">Отчество</label>
          <input type="text" id="patronymic" maxlength="29"/>
        </div>
        <div class="input-panel">
          <label for="date">Дата рождения</label>
          <input type="date" id="date" v-model.trim="$v.date.$model"/>
          <div class="error" v-if="!$v.date.required">Введите дату.</div>
        </div>

        <div class="gender-selector">
          <p>Пол</p>
          <div class="radio">
            <label class="container"
              >Женский
              <input type="radio" name="gender" value="other" />
              <span class="checkmark"></span>
            </label>
            <label class="container"
              >Мужской
              <input type="radio" name="gender" value="other" />
              <span class="checkmark"></span>
            </label>
          </div>
        </div>

        <div class="phone">
          <label for="phone">Телефон</label>
          <div class="phone-number">
            <span>+7</span>
            <input
              id="phone"
              type="tel"
              value="111 111 11 11"
              maxlength="10"
              v-model.trim="$v.phone.$model"
            />
          </div>
          <div class="error" v-if="!$v.phone.required">Введите телефон.</div>
          <div class="error" v-if="!$v.phone.minLength">Введите корректный номер.</div>

          <label for="sms" class="container"
            >Не отправлять смс
            <input type="checkbox" name="sms" id="sms" />
            <div class="checkmark"></div>
          </label>
        </div>

        <div class="client-info">
          <div class="select-wrapper">
            <label for="client-group">Группа клиентов</label>
            <select name="clientGroup" id="client-group" v-model.trim="$v.clientGroup.$model">
              <option value="vip">VIP</option>
              <option value="problem-patient">Проблемные</option>
              <option value="insurance">ОМС</option>
            </select>
            <div class="error" v-if="!$v.clientGroup.required">Выберите группу.</div>
          </div>
          <div class="select-wrapper">
            <label for="therapist">Лечащий врач</label>
            <select name="therapist" id="therapist" >
              <option value="1">Иванов</option>
              <option value="2">Захаров</option>
              <option value="3">Чернышева</option>
            </select>
          </div>
        </div>
      </div>

      <div class="address">
        <h2 class="section-header">Адрес</h2>

        <div class="address-input-panel">
          <label>Индекс</label>
          <input type="number" />
        </div>
        <div class="address-input-panel">
          <label>Страна</label>
          <input type="number" />
        </div>
        <div class="address-input-panel">
          <label>Область</label>
          <input type="number" />
        </div>
        <div class="address-input-panel">
          <label>Город</label>
          <input type="text" />
          <div class="error" v-if="!$v.city.required">Укажите город.</div>
        </div>

        <div class="address-input-panel">
          <label>Улица</label>
          <input ctype="text" />
        </div>
        <div class="address-input-panel">
          <label>Дом</label>
          <input type="text" />
        </div>
      </div>

      <div class="documents">
        <h2 class="section-header">Паспортные данные</h2>
        <div class="select-wrapper">
          <label for="document-type">Тип документа</label>
          <select name="documentType" id="document-type">
            <option value="passport">Паспорт</option>
            <option value="birthCertificate">Свидетельство о рождении</option>
            <option value="driversLicense">Вод. удостоверение</option>
          </select>
        </div>

        <div class="doc-input-panel">
          <label>Серия</label>
          <input type="number" />
        </div>
        <div class="doc-input-panel">
          <label>Номер</label>
          <input type="number" />
        </div>
        <div class="doc-input-panel">
          <label>Кем выдан</label>
          <input type="number" />
        </div>
        <div class="doc-input-panel">
          <label>Дата выдачи</label>
          <input type="date" />
        </div>
      </div>

      <div class="submit-wrapper">
        <input class="submit-btn" type="submit" :disabled="submitStatus === 'PENDING'" value="Сохранить">
        <div class="modal" v-if="statusMessageVisible">
          <div class="success">
            <p>Сохранено</p>
            <div class="success-img"></div>
          </div>
        </div>
        <p class="error" v-if="submitStatus === 'ERROR'">Ошибка</p>
        <Loader v-if="submitStatus === 'PENDING'" />
      </div>
    </form>
  </div>
</template>

<script>
import { required, minLength} from 'vuelidate/lib/validators';
import Loader from './Loader'

export default {
  name: "ClientForm",
  components: {
    Loader
  },
  data() {
    return {
      name: '',
      lastName: '',
      date: '',
      phone: 0,
      clientGroup: '',
      submitStatus: null,
      city: '',
      statusMessageVisible: false
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(2)
    },
    lastName: {
      required,
      minLength: minLength(2)
    },
    date: {
      required
    }, 
    phone: {
      required,
      minLength: minLength(10)
    },
    clientGroup: {
      required
    },
    city: {
      required
    }
  },
  methods: {
    submit() {
      console.log('submit!')
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = 'OK'
          this.statusMessageVisible = true
          setTimeout(() => {
            this.statusMessageVisible = false
            location.reload();
          }, 2000)
        }, 2000)
      }
    }
  }
};

</script>