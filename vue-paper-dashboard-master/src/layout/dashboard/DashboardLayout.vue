<template>
  <div class="wrapper">
    <side-bar>
      <template slot="links">
        <sidebar-link to="/dashboard" name="Dashboard" icon="ti-panel" />
        <sidebar-link to="/stats" name="Criar Atividade" icon="ti-pencil-alt2" />
        <sidebar-link to="/notifications" name="Atividades" icon="ti-bell" />
        <sidebar-link
          to="/table-list"
          name="Utilizadores"
          icon="ti-user"
        />
        <sidebar-link to="/icons" name="Funcionários" icon="ti-user" />
        <sidebar-link to="/typography" name="Marcações" icon="ti-view-list-alt" />
        
        
        
        <sidebar-link to="/maps" name="Sede" icon="ti-map" />
      </template>
      <mobile-menu>
        <li class="nav-item">
         
        </li>
        
        
        <li class="divider"></li>
      </mobile-menu>
    </side-bar>
    <div class="main-panel">
      <top-navbar></top-navbar>

      <dashboard-content @click.native="toggleSidebar"> </dashboard-content>

      <content-footer></content-footer>
    </div>
  </div>
</template>
<style lang="scss"></style>
<script>
import TopNavbar from "./TopNavbar.vue";
import ContentFooter from "./ContentFooter.vue";
import DashboardContent from "./Content.vue";
import MobileMenu from "./MobileMenu";
export default {
  components: {
    TopNavbar,
    ContentFooter,
    DashboardContent,
    MobileMenu,
  },
  methods: {
    toggleSidebar() {
      if (this.$sidebar.showSidebar) {
        this.$sidebar.displaySidebar(false);
      }
    },
  },
};
</script>
