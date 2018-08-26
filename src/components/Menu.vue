<template>
  <nav id="panel" v-bind:class="{collapsed: isCollapsed}">
    <div class="logo">
      <a href="#">
        <img v-bind:src="logo" alt="Radix wallet">
      </a>
    </div>
    <ul class="menu">
      <li v-for="section in sections"
          v-on:click="activeSection = section.title"
          v-bind:class="{active: activeSection == section.title}">
        <span>{{!isCollapsed ? section.title : section.title.substring(0, 1)}}</span>
      </li>
    </ul>
    <p v-on:click="isCollapsed = !isCollapsed" class="menu__switch">
      <i class="material-icons">{{`keyboard_arrow_${isCollapsed ? 'right' : 'left'}`}}</i>
    </p>
  </nav>
</template>

<script>
  export default {
    name: 'panel',
    data() {
      return {
        activeSection: 'Wallet',
        isCollapsed: false,
        logoImg: '../src/assets/img/logo.png',
        logoIcon: '../src/assets/img/icon_logo.png',
        sections: [
          {
            title: "Dashboard",
          },
          {
            title: "Wallet",
          },
          {
            title: "Contacts",
          },
          {
            title: "Messages",
          },
        ]
      }
    },
    computed: {
      logo() {
        const data = this.$data
        return data.isCollapsed ? data.logoIcon : data.logoImg
      }
    },
    mounted() {
      // TODO: this should be improved storing if the user already switched the menu status
      // & respecting the user decission
      this.$nextTick(() => {
        window.addEventListener('resize', () => {
          if (window.innerWidth < 920 && !this.$data.isCollapsed) {
            this.$data.isCollapsed = true;
          }
          else if(window.innerWidth > 920 && this.$data.isCollapsed) {
            this.$data.isCollapsed = false;
          }
        });
      })
    },
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/menu.scss';
</style>
