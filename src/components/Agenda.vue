<template>
  <div id="agenda" class="contacts__agenda">
    <p>Your contact list: click to use the address</p>
    <div class="contacts__search">
      <input type="text" v-model="search" placeholder="Search by alias or address.."/>
    </div>
    <div class="contacts__list">
      <a v-for="contact in filteredList" v-on:click="onClick(contact)">
        <span v-bind:class="['contact__alias', contact.alias && contact.alias.length > 20 && 'extended']">{{ contact.alias && contact.alias.substring(0,20)}}{{ contact.alias && contact.alias.length > 20 && '...' }}</span>
        <span class="contact__address">{{ contact.address }}</span>
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'agenda',
    data() {
      return {
        search: '',
      }
    },
    props: {
      contacts: {
        type: Array,
        default: () => []
      },
      onClick: {
        type: Function,
        default: () => null
      }
    },
    methods: {
      // Show full long names on hover: TODO
      showFullAlias(alias, e) {
        const needToShow = alias && alias.length > 20
      },
    },
    computed: {
      // Filter for the contacts search
      filteredList() {
        return this.contacts.filter(contact => {
          return contact.address.includes(this.search) || contact.alias && contact.alias.toLowerCase().includes(this.search.toLowerCase())
        })
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/contacts_agenda.scss';
</style>
