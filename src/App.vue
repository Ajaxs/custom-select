<template>
  <div>
    <form action="" id="form">
      <div class="row">
        <div class="column">
          <label>Autos:</label>
          <ax-picklist 
            v-model="auto1" 
            name="auto1"            
            :options="autos"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ auto1 }}</code>
        </div>
      </div>
      <div class="row">
        <div class="column">
          <label>Clearable model:</label>
          <ax-picklist 
            v-model="auto2"
            name="auto2" 
            clearable 
            :options="autos"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ auto2 }}</code>
        </div>
      </div>
      
      <div class="row">
        <div class="column">
          <label>Disabled select:</label>
          <ax-picklist 
            v-model="auto3"
            name="auto3"
            disabled
            :options="autos"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ auto3 }}</code>
        </div>
      </div>
      <div class="row">
        <div class="column">
          <label>Disabled options:</label>
          <ax-picklist 
            v-model="day"
            name="day"
            :options="days"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ day }}</code>
        </div>
      </div>
      <div class="row">
        <div class="column">
          <label>Filterable:</label>
          <ax-picklist
            v-model="country" 
            name="country"
            filterable
            clearable
            :options="countries"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ country }}</code>
        </div>
      </div>
      
      <div class="row">
        <div class="column">
          <label>Multiple:</label>
          <ax-picklist
            v-model="color" 
            name="color"
            multiple
            :options="colors"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ color }}</code>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import AxPicklist from '@/components/AxPicklist.vue';

export default {
  name: 'App',
  components: {
    AxPicklist,
  },
  data() {
    return {
      country: null,
      countries: [
        {
          value: 1,
          label: 'Абхазия',
        },
        {
          value: 2,
          label: 'Австралия',
        },
        {
          value: 3,
          label: 'Австрия',
        },
        {
          value: 4,
          label: 'Азербайджан', 
        },
        {
          value: 5,
          label: 'Албания',
        },
        {
          value: 6,
          label: 'Алжир',
        },
        {
          value: 7,
          label: 'Американское Самоа',
        },
        {
          value: 8,
          label: 'Ангола',
        },
        {
          value: 9,
          label: 'Багамы',
        },
        {
          value: 10,
          label: 'Бангладеш',
        },
        {
          value: 11,
          label: 'Бахрейн',
        },
        {
          value: 12,
          label: 'Германия',
        },
        {
          value: 13,
          label: 'Грузия',
        },
        {
          value: 14,
          label: 'Египет',
        },
        {
          value: 15,
          label: 'Канада',
        },
        {
          value: 16,
          label: 'Соединенные Штаты Америки',
        },
      ],
      auto1: 3,
      auto2: null,
      auto3: 4,
      autos: [
      {
          value: 1,
          label: 'Audi',
        },
        {
          value: 2,
          label: 'Acura',
        },
        {
          value: 3,
          label: 'Alfa Romeo',
        },
        {
          value: 4,
          label: 'Aston Martin',
        },
      ],
      day: 1,
      day2: 1,
      days: [
      {
          value: 1,
          label: 'Понедельник',
        },
        {
          value: 2,
          label: 'Вторник',
        },
        {
          value: 3,
          label: 'Среда',
        },
        {
          value: 4,
          label: 'Четверг',
        },
        {
          value: 5,
          label: 'Пятница',
        },
        {
          value: 6,
          label: 'Суббота',
          disabled: true,
        },
        {
          value: 7,
          label: 'Воскресенье',
          disabled: true,
        },
      ],
      color: [2],
      colors: [
      {
          value: 1,
          label: 'Red',
        },
        {
          value: 2,
          label: 'Green',
        },
        {
          value: 3,
          label: 'Blue',
        },
        {
          value: 4,
          label: 'White',
        },
        {
          value: 5,
          label: 'Black',
        },
      ],
    }
  },
}
</script>

<style lang="less">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

body {
  background-color: #fff; 
  font-family: 'Roboto', sans-serif; 
  font-weight: 400;
  font-size: 13px;
}

form {
  label {
    display: inline-block;
    margin-bottom: 6px;
  }

  .row {
    display: flex;
    gap: 20px;
    justify-content: space-between;
    margin-bottom: 30px;
    padding: 20px;
    border: 1px solid #aaa;
    border-radius: 5px;
    max-width: 600px;

    .column{
      width: 300px;
    }

    .model{
      width: 250px;
      flex-shrink: 0;
    }
  }  
}
</style>
