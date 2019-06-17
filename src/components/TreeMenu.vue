<template>
  <div class="tree-menu">
      <div v-if="!is_expand">
          <div class="top-menu" v-for="(menuItem,index) in menu_list" :key="index">
            <div class="top-menu-title" @click="toggle_sub(menuItem.id)">
                {{menuItem.name}}
            </div>
            <div :class="'sub-menu sub-menu-title'+menuItem.id" v-for="(menuSubItem,index) in menuItem.menuItem" :key="index">
                <span>{{menuSubItem.name}}</span>
            </div>
        </div>
      </div>
      <div v-if="is_expand">
          <div class="top-menu" v-for="(menuItem,index) in menu_list" :key="index">
            <div class="top-menu-title" @click="prop_sub(menuItem.id,$event)">
                {{menuItem.name}}
            </div>
            <div :class="'sub-menu-box sub-menu-box'+menuItem.id">
                <div :class="'sub-menu sub-menu-title'+menuItem.id" v-for="(menuSubItem,index) in menuItem.menuItem" :key="index">
                    <span>{{menuSubItem.name}}</span>
                </div>
            </div>
        </div>
      </div>
      
  </div>
</template>

<script>
export default {
  name: 'TreeMenu',
  props: {
    menu_list: Array,
    is_expand: Boolean
  },
  methods:{
      toggle_sub(id){
          let subArray = document.getElementsByClassName("sub-menu-title"+id);
          if(subArray && subArray.length>0){
              for(var i = 0;i<subArray.length ; i++){
                  if(subArray[i].style.display == "none"){
                      subArray[i].style.display = "block";
                  }else{
                      subArray[i].style.display = "none";
                  }
              }
          }
      },
      prop_sub(id,e){
        let thisTop = e.target.offsetTop;
        let thisLeft = e.target.offsetLeft;
        let dom_child = document.getElementsByClassName("sub-menu-box"+id)[0];
        dom_child.style.top = thisTop+60+"px";
        dom_child.style.left = thisLeft+40+"px";
        if(dom_child.style.display == "none" || !dom_child.style.display){
            dom_child.style.display = "block";
        }else{
            dom_child.style.display = "none";
        }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sub-menu-box{
    display: none;
    position: fixed ;
    left: 30px;
    border: 1px solid black;
    background: lightseagreen;
    order: 9999;
}
.top-menu-title{
    margin: 10px;
    background: gray;
}
.top-menu-title:hover{
    cursor: pointer;
    background: black;
    color: blanchedalmond;
}
.sub-menu{
    margin: 10px;
    margin-left: 20px;
    height: 20px;
    background: lightblue;
    counter-reset: red;
}
</style>
