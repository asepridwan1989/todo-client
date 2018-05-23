<template>
  <div id="navigasi">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand">ToDo app</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">      
              <router-link 
                v-bind:key="links[0].id"
                :to="`${links[0].page}`"><a>{{links[0].text}}</a>
              </router-link>      
          </li>
          <li class="nav-item">
            <router-link 
              v-bind:key="links[1].id"
              :to="`${links[1].page}`"><a>{{links[1].text}}</a>
            </router-link>
          </li>
          <li class="nav-item dropdown">
              <router-link 
                v-bind:key="links[2].id"
                :to="`${links[2].page}`"><a>{{links[2].text}}</a>
              </router-link>
          </li>
        </ul>        
      </div>
     </nav>
  </div>
</template>

<script>
export default {
  name: 'Navigation',
  data() {
    return {
      links: [
        {
          id: 1,
          text: 'Home',
          page:'/'
        },
        {
          id: 2,
          text: 'About',
          page:'/About'
        },
        {
          id: 3,
          text: 'Contact',
          page:'/Contact'
        }
      ]
    }
  }
}
</script>

<style>
nav{
  margin-top: -60px;
} 
#navigasi{
  position: fixed;
  width: 100%;
} 
a{
  margin-left: 20px;
}
</style>