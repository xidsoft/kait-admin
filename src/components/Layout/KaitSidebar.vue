<template>
  <div class='sidebar-wrapper'>
      <perfect-scrollbar>
        <div class="" id="sidebar">
            <div class="sidebar-header">
                <div class="logo">
                <img src="@/assets/images/logo/type.svg" alt="">
                </div>
            </div>
            <div class="sidebar-menu">
                <ul>
                    <li  v-for="(sidebarItem,index) in sidebarItems" @click="toggleSubmenu(index)" :class="{'sidebar-item':true,'sidebar-title':  (sidebarItem.type == 'title'), 'has-sub': sidebarItem.submenu, 'active': isGroupActive(index) || $route.fullPath == sidebarItem.url}" :key="sidebarItem.text" >
                        <span v-if="sidebarItem.type == 'title'">{{ sidebarItem.text }}</span>
                        <template v-else>
                            <a href="#" v-if="sidebarItem.submenu" ><i :class='sidebarItem.icon'></i> <span>{{ sidebarItem.text }}</span></a>
                            <router-link :to="sidebarItem.url" v-else   ><i :class='sidebarItem.icon'></i><span>{{ sidebarItem.text }}</span></router-link>
                            <transition name="fadeHeight">
                                <div class="submenu-wrapper" v-if="sidebarItem.submenu" v-show="sidebarItem.active || isGroupActive(index)" >
                                    <ul :class="{'submenu':true}"  >
                                        <li v-for="submenu in sidebarItem.submenu" :key="submenu.url">
                                            <router-link :to="submenu.url">{{ submenu.text }}</router-link>
                                        </li>
                                    </ul>
                                </div>
                            </transition>
                        </template>
                    </li>
                </ul>
            </div>
        </div>
      </perfect-scrollbar>
  </div>
</template>
<script>
import { PerfectScrollbar } from 'vue2-perfect-scrollbar';
import 'vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css'
export default {
    name: 'KaitSidebar',
    components: {
        PerfectScrollbar
    },
    data() {
        return {
            showSubmenu: '',
            sidebarItems: this.$store.state.sidebarItems,
            settings: { //
                wheelSpeed: 1,      
                swipeEasing: true,
                wheelPropagation: false,
            }
        }
    },
    created() {
        this.sidebarItems = this.sidebarItems.map(sidebarItem => ({...sidebarItem, active: false}))
        // this.sidebarItems.forEach((sidebarItem) => {

        // })
    },
    methods: {
        toggleSubmenu(index) {
            this.$set(this.sidebarItems[index], 'active',!this.sidebarItems[index].active)
            console.log(this.sidebarItems[index].text)
        }
    },
    computed: {
        isSidebarActive: {
            get() {
            return this.$store.state.isSidebarActive;
            },
            set(val) {
            this.$store.commit('TOGGLE_IS_SIDEBAR_ACTIVE', val)
            }
        },
        isGroupActive() {
            return (index) => {

                const path = this.$route.fullPath;
                let open = false;
                let sidebarItem = this.sidebarItems[index];
                let func = function(sidebarItem) {
                    if (sidebarItem.submenu) {
                        sidebarItem.submenu.forEach((item) => {
                            if (path == item.url) {
                                open = true
                            } 
                        })
                    }
                }
                func(sidebarItem)
                return open
            }
        },
    }
}
</script>
<style>

.ps {
  height: 100vh;
}
</style>