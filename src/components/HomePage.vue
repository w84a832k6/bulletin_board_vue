<template>
  <div>
    <h3>公佈欄</h3>
    <b-card class="mx-1">
      <div class="d-flex flex-row-reverse">
        <b-button>新增</b-button>
      </div>
      <b-table striped hover :items="items"></b-table>
    </b-card>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data () {
    return {
      items: [
        { age: 40, first_name: '123123', last_name: 'Macdonald' },
        { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
        { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
        { age: 38, first_name: 'Jami', last_name: 'Carney' }
      ]
    }
  }
}
</script>
