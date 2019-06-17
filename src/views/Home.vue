<template>
  <div class="home">
    <HomeBox header_height="50" @trigger_toggle_button="changeMenuShow">
      <div slot="left_box">
        <TreeMenu :menu_list="tree_menu" :is_expand="menuIsExpand"/>
      </div>
      <template slot="right_box">
        <div style="height:100%;margin:10px;background:white;">

        </div>
      </template>
    </HomeBox>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeBox from '@/components/HomeBox.vue'
import TreeMenu from '@/components/TreeMenu.vue'

export default {
  name: 'home',
  data(){
    return {
      tree_menu:[
        {
          name:"1-一级菜单",
          id:1,
          menuItem:[
            {
              id:11,
              name:"1-1-二级菜单"
            },
            {
              id:12,
              name:"1-2-二级菜单"
            }
          ]
        },
        {
          name:"2-一级菜单",
          id:2,
          menuItem:[
            {
              id:21,
              name:"2-1-二级菜单（比较长长长）"
            },
            {
              id:22,
              name:"2-2-二级菜单"
            }
          ]
        },
        {
          name:"3-一级菜单",
          id:3,
          menuItem:[
            {
              id:31,
              name:"3-1-二级菜单"
            },
            {
              id:32,
              name:"3-2-二级菜单"
            }
          ]
        }

      ],
      menuIsExpand:true
    }
  },
  methods:{
    changeMenuShow(isExpand){
      this.menuIsExpand = isExpand;
    }
  },
  components: {
    HomeBox,
    TreeMenu
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home{
  height: 100%;
  width:100%;
  background: lightblue;
}
</style>