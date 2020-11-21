<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-purple-6">
      <q-toolbar>
        <q-icon class="icon" name="ti-notepad"/>
        

        <q-toolbar-title>
          To Do List
        </q-toolbar-title>

        <q-icon class="icon" name="ti-pencil"/>
      </q-toolbar>
    </q-header>

    

    <q-page-container>
      <router-view />
    </q-page-container>
    <q-footer elevated class="bg-purple-5">
      <q-toolbar class="footer">
        <q-toolbar-title>
          
            <div class="title"><q-icon class="icon1" name="ti-notepad"/>
           To Do List </div>
          <div class="copyright"> ©Copyright 2020</div>
        </q-toolbar-title>
      </q-toolbar>
      
    </q-footer>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data () {
    return {
      leftDrawerOpen: false
    }
    
  }
}
</script>
<style lang="scss" scoped>
.icon{
  font-size: 1.8em;

}
.icon1{
  font-size: 1.5em;

}

.footer{
  display: flex;
  padding: 10px;
}

.copyright{
  text-align: center;
  font-size: 12px;
}

.title{
    font-size: 1.5em;
}


</style>
