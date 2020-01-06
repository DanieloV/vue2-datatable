<template>
  <a v-if="sortable" href="#" @click.prevent="handleClick" name="HeadSort" :class="order">
    <slot></slot>
    <i :class="iconClass"></i>
  </a>
  <div v-else>
    <slot></slot>
  </div>
</template>
<script>
/**
 * Sorting arrows within <th>
 */
export default {
  name: 'HeadSort',
  props: {
    sortable: { type: Boolean },
    field: { type: String },
    query: { type: Object, required: true }
  },
  data: () => ({
    order: ''
  }),
  computed: {
    iconClass () {
      const { order } = this
      return [
        {
          'fa fa-sort-up': order === 'asc',
          'fa fa-sort-down': order === 'desc'
        }
      ]
    }
  },
  watch: {
    query: {
      handler ({ sort: field, order }) {
        this.order = field === this.field ? order : ''
      },
      deep: true,
      immediate: true
    }
  },
  methods: {
    handleClick () {
      const { query, order } = this
      query.sort = this.field
      query.order = this.order = order === 'desc' ? 'asc' : 'desc'
    }
  }
}
</script>
