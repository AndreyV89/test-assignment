<template>
  <main>
      <div class="overlay" :class="{ active: showPopup }" @click="closePopup"></div>
      <div class="popup" :class="{ active: showPopup }">
          <div class="popup-header mb-4">
              <h2>Откликнуться</h2>
              <button @click="closePopup">&times;</button>
          </div>
          <div class="popup-body">
              <input type="text" placeholder="Фамилия" v-model="formData.lastName">
              <input type="text" placeholder="Имя" v-model="formData.firstName">
              <input type="text" placeholder="Отчество" v-model="formData.middleName">
              <input type="tel" placeholder="Номер телефона" v-model="formData.phone">
              <div class="d-flex">
                  <div class="me-4 mb-4">
                      <input type="checkbox" v-model="formData.agree" id="agree">
                  </div>
                  <label for="agree">Я даю согласие на обработку своих персональных данных</label>
              </div>
          </div>
          <div class="popup-footer">
              <button class="button" @click="submitForm">Откликнуться!</button>
          </div>
      </div>
    <div class="d-flex col-12">
      <div class="col-3">
        <h2>Поиск по категориям</h2>
        <div class="px-4 container">
          <div>
            <FilterComponent :filter-options="cityOptions" @select-option="getFilters()" :filter-name="'Регион'" />
          </div>
          <div>
            <FilterComponent @selectItem="selectItem" :filter-name="'Город'" />
          </div>
          <div>
            <FilterComponent @selectItem="selectItem" :filter-name="'Организация'" />
          </div>
          <button class="button">Применить!</button>
        </div>
      </div>
      <div class="col" v-if="currentVacancies.length">
        <VacanciesComponent :vacancies="currentVacancies"  @select="openPopup" />
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";
import FilterComponent from "@/components/FilterComponent.vue";
import VacanciesComponent from "@/components/VacanciesComponent.vue";

const currentVacancies = ref([]);
const selectedItem = ref(null);
const cityOptions = ref(null);
const regionOptions = ref(null);
const organisationOptions = ref(null);
const showPopup = ref(false);
const formData = ref({
    lastName: '',
    firstName: '',
    middleName: '',
    phone: '',
    agree: false
});

const openPopup = () => {
  showPopup.value = true;
};

const closePopup = () => {
  showPopup.value = false;
};

async function getVacancies() {
  const response = await fetch(
    "https://gsr-rabota.ru/api/v2/Vacancies/All/List",
    {
      method: "GET",
      headers: {
        Accept: "application/json",
      },
    }
  );
  if (!response.ok) {
    throw new Error("Error " + response.statusText);
  }
  currentVacancies.value = await response.json();
  currentVacancies.value = currentVacancies.value.slice(0, 30);
    getFilters('placetitle', cityOptions)
}

function getFilters(key, options) {
  const cities = currentVacancies.value.map(item => item[key]);
  options.value = [...new Set(cities)];
}

function submitForm() {
    if (formData.value.agree) {
        //Логика валидации и отправки формы
        closePopup();
    }
};



const selectItem = (event) => {
  selectedItem.value = event.target.value;
};

onBeforeMount(getVacancies);

</script>

<style scoped>
.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 20px;
  background: white;
  border: 1px solid black;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  display: none;
  /* Изначально скрыт */
}

.popup.active {
  display: block;
}

.logo {
  width: 130px;
}

.container {
  display: flex;
  flex-direction: column;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  padding: 2rem;
  gap: 4rem;
}

nav {
  display: flex;
  justify-content: space-between;
  width: 100%;
  align-items: center;
}

main {
  padding: 2rem;
  background-color: #f8f9fa;
}

.button {
  border: none;
  padding: 5px;
  width: 100%;
  background: #eb8000;
  text-align: center;
  color: #fff;
  border-radius: 6px;
}

.popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    z-index: 1000;
    display: none;
    border-radius: 10px;
    width: 400px;
}
.popup.active {
    display: block;
}
.popup-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.popup-header h2 {
    margin: 0;
}
.popup-body input {
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 20px;
}
.popup-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 10;
    display: none;
}
.overlay.active {
    display: block;
}
</style>
