<template>
  <a-layout id="components-layout-demo-side" style="min-height: 100vh">
    <a-layout-sider v-model="collapsed" :trigger="null" collapsible collapsed-width="0" style="width: 500px">
      <a-menu theme="dark" :default-selected-keys="['1']" mode="inline">
        <a-menu-item >
        </a-menu-item>
        <a-menu-item key="1">
          <nuxt-link to="/">
            <a-icon type="gift" />
            <span>Mascotas</span>
          </nuxt-link>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header style="background: #7d9ccc; padding: 1">
        <a-icon
          class="trigger"
          :type="collapsed ? 'menu-unfold' : 'menu-fold'"
          @click="() => (collapsed = !collapsed)"
        />
        <div :class="['global-header-right']">
        </div>
      </a-layout-header>
      <a-layout-content class="app-content">
        <main>
          <div class="content-nuxt">
            <nuxt />
          </div>
        </main>
      </a-layout-content>
      <a-layout-footer style="text-align: center">
        UNIVERSIDAD CENTRAL DEL ECUADOR Aplicativo de Minería de Datos ©2023
      </a-layout-footer>
    </a-layout>
  </a-layout>
</template>

<script>
export default {
  data () {
    return {
      collapsed: true
    }
  },
  methods: {
    async logout () {
      await this.$store.dispatch('session/executiveLogout')
      await this.$router.push({ path: '/executive/login' })
    }
  }
}
</script>
