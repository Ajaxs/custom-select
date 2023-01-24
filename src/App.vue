<template>
  <div>
    <form action="" id="form">
      <div class="row">
        <div class="column">
          <label>Autos:</label>
          <nwx-select 
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
          <nwx-select 
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
          <nwx-select 
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
          <nwx-select 
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
          <nwx-select
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
          <nwx-select
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

      <div class="row">
        <div class="column">
          <label>Multiple filtrable:</label>
          <nwx-select
            v-model="color2" 
            name="color"
            multiple
            filterable
            clearable
            :options="colors"             
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ color2 }}</code>
        </div>
      </div>

      <div class="row">
        <div class="column">
          <label>Multiple limit:</label>
          <nwx-select
            v-model="color3" 
            name="color"
            multiple
            clearable
            limit="3"
            :options="colors"
            placeholder="Max 3 options"         
          />
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ color3 }}</code>
        </div>
      </div>

      <div class="row">
        <div class="column">
          <label>Custom template:</label>
          <nwx-select
            v-slot="props"
            v-model="curency" 
            name="curency"
            :options="curencies"
            filterable             
          >
            <div class="curency-option">
              <span class="label">{{ props.option.label }}</span>
              <span class="gray">{{ props.option.curency }}</span>
            </div>
          </nwx-select>
        </div>
        <div class="model">
          <p>Model:</p>
          <code>{{ curency }}</code>
        </div>
      </div>

    </form>
  </div>
</template>

<script>
import NwxSelect from '@/components/NwxSelect.vue';

export default {
  name: 'App',
  components: {
    NwxSelect,
  },
  data() {
    return {
      country: null,
      countries: [
        {
          value: 1,
          label: 'Australia',
        },
        {
          value: 2,
          label: 'Austria',
        },
        {
          value: 3,
          label: 'Azerbaijan',
        },
        {
          value: 4,
          label: 'Belgium', 
        },
        {
          value: 5,
          label: 'Brazil',
        },
        {
          value: 6,
          label: 'Germany',
        },
        {
          value: 7,
          label: 'Greece',
        },
        {
          value: 8,
          label: 'India',
        },
        {
          value: 9,
          label: 'Indonesia',
        },
        {
          value: 10,
          label: 'Italy',
        },
        {
          value: 11,
          label: 'Kenya',
        },
        {
          value: 12,
          label: 'Laos',
        },
        {
          value: 13,
          label: 'Luxembourg',
        },
        {
          value: 14,
          label: 'Malaysia',
        },
        {
          value: 15,
          label: 'United States',
        },
        {
          value: 16,
          label: 'Vietnam',
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
      days: [
      {
          value: 1,
          label: 'Monday',
        },
        {
          value: 2,
          label: 'Tuesday',
        },
        {
          value: 3,
          label: 'Wednesday',
        },
        {
          value: 4,
          label: 'Wednesday',
        },
        {
          value: 5,
          label: 'Friday',
        },
        {
          value: 6,
          label: 'Saturday',
          disabled: true,
        },
        {
          value: 7,
          label: 'Sunday',
          disabled: true,
        },
      ],
      color: [2],
      color2: [1,3],
      color3: [],
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
          disabled: true,
        },
        {
          value: 6,
          label: 'Pink',
        },
        {
          value: 7,
          label: 'Gray',
        },
      ],
      curency: null,
      curencies: [
        {
          value: 1,
          label: 'United States',
          curency: 'USD',
        },
        {
          value: 2,
          label: 'France',
          curency: 'EUR',
        },
        {
          value: 3,
          label: 'Russian Federation',
          curency: 'RUB',
        },
        {
          value: 4,
          label: 'Armenia',
          curency: 'AMD',
        },
      ]
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
    border: 1px solid #ddd;
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



.curency-option {
  display: flex;
  justify-content: space-between;

  .gray {
    color: #888;
  }
}

.selected {
  .curency-option {
    .label {
      color: #409eff;
    }
  }
}
</style>
