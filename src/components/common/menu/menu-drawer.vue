<template>
  <mu-appbar :z-depth="0">
    <mu-button icon slot="left" @click="backClick">
      <mu-icon value=":iconfont icon-chevron-left"></mu-icon>
    </mu-button>

    <mu-button icon @click="onChangeVisible">
      <mu-icon class="icon" value=":iconfont icon-menu"></mu-icon>
    </mu-button>

    <mu-drawer :open.sync="open" :docked="false" :right='false'>
      <mu-list>
        <mu-list-item button @click="editBackGround">
          <mu-list-item-action>
            <mu-icon value=":iconfont icon-crop-original"></mu-icon>
          </mu-list-item-action>
          <mu-list-item-title>
            <span>自定义背景</span>
            <input @change="getFile" class="upload" type="file" ref="upload" />
          </mu-list-item-title>
        </mu-list-item>

        <mu-list-item button @click="resetBackGround">
          <mu-list-item-action>
            <mu-icon value=":iconfont icon-update"></mu-icon>
          </mu-list-item-action>
          <mu-list-item-title>
            <span>重置背景</span>
          </mu-list-item-title>
        </mu-list-item>

        <mu-list-item button @click="changeDialogVisible">
          <mu-list-item-action>
            <mu-icon value=":iconfont icon-alternate_email-24px"></mu-icon>
          </mu-list-item-action>
          <mu-list-item-title>关于我们</mu-list-item-title>
        </mu-list-item>
      </mu-list>
    </mu-drawer>
  </mu-appbar>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
export default {
  name: "menu-drawer",
  data() {
    return {
      docked: false,
      open: false,
      dialogVisible: false,
    }
  },

  computed: {
    ...mapState(['originBackGround'])
  },

  methods: {
    ...mapMutations(['getBackgroundImg']),

    onChangeVisible(){
      this.open = true;
    },

    backClick(){
      this.$router.push('/login');
    },

    changeDialogVisible(){
      this.$emit('onDialogVisible');
    },

    editBackGround(){
      this.$refs.upload.click();
    },

    getFile(){
      const file = this.$refs.upload.files[0];
      let reader = new FileReader();
      reader.readAsDataURL(file);
      reader.addEventListener('load', () => {
        localStorage.setItem('backgroundImage', reader.result);
        this.getBackgroundImg(reader.result);
        this.open = false;
      })
    },

    async resetBackGround(){
      let { result } = await this.$confirm('确定要重置吗?', '提示');
      if(result){
        localStorage.removeItem('backgroundImage');
        this.getBackgroundImg(this.originBackGround);
        this.open = false;
      }
    }
  },
}
</script>

<style scoped>
.mu-list{
  padding: 0;
  overflow-x: visible;
}
.mu-appbar, .mu-appbar >>> .mu-appbar-title, .mu-appbar >>> .mu-button{
  padding: 0;
}
.mu-appbar >>> .mu-appbar-title{
  display: flex;
  align-items: center;
  justify-content: flex-end;
  height: 100%;
}
.mu-appbar{
  height: 6vh;
}
.mu-appbar >>> .mu-button{
  height: 6vh;
  width: 6vh;
}
.upload{
  display: none;
}
.mu-icon{
  color: black;
}
.mu-paper{
  background-image: url(~@/assets/img/drawerLogo.jpg);
  background-size: 100vw 100vh;
}
.mu-item-title{
  text-align: center;
  margin-left: -15vw;
}
.mu-list >>> .mu-item{
  color: white;
  height: 6.5vh;
}
.mu-list li{
  margin-top: 1vh;
}
.mu-icon{
  font-size: 1.7rem;
}
.icon{
  font-size: 1.5rem;
}
</style>