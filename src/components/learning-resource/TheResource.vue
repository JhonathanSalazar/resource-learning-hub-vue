 <template>
  <base-card>
    <base-button 
      @click="setSelectedTab('stored-resource')" 
      :mode="storedResButtonMode"
      >
        Stored Resource
      </base-button>
    <base-button 
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
      >
        Add Resource
      </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResource from './StoredResource.vue'
import AddResource from './AddResource.vue'

export default {
  components: {
    StoredResource,
    AddResource
  },


  data () {
    return {
      selectedTab: '',
      storedResources: [
        { 
          id: 'official-guide', 
          title: 'Official Guide',
          description: 'The official Vue.js docs',
          link: 'https://vuejs.org'
        },
        { 
          id: 'google', 
          title: 'Google',
          description: 'Google source',
          link: 'https://google.com'
        }
      ]
    }
  },

  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resource' ? null : 'flat'
    },

    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat'
    }
  },

  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResources,
      deleteResource: this.removeResource
    }
  },

  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab
    },

    addResources(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url
      }

      this.storedResources.unshift(newResource)
      this.selectedTab = 'stored-resource'
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId)
      this.storedResources.splice(resIndex,1)
    }
  }
}
</script>