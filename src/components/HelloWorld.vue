<template>
  <div>
     <div class="content">
      <div class="left" id="leftWrap">
        <div v-for="(item,index) in propertyList" :key="item.code">
          <div class="item font" @click="edit(item,index)" @dragenter="dragenter($event,index)">
            <span class="item-name text-ellipsis font">{{item.name}}</span>
            <span class="item-default text-ellipsis">: {{item.defaultValue}}</span>
          </div>
          <span class="item-delete" @click.stop="deletepre(index)"><i
            class="fa fa-trash-o" aria-hidden="true"></i></span>
        </div>
      </div>
      <div class="right">
        <div class="right-head font">属性类型</div>
        <div class="right-tips font">属性类型可拖动到左侧编辑区哦！</div>
        <div class="right-wrap">
          <div class="options font" :draggable="isDraggable" @dragstart="dragstart($event,item)" @dragend="dragend"
               v-for="(item,index) in optionsList " :key="item.type">{{item.name}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
let inner = document.createElement('div')
inner.innerHTML = '文字拖放到这里'
inner.classList.add("inner")

export default {
  name: 'HelloWorld',
  data () {
    return {
      propertyList: [
        {name: 'name', defaultValue: '服务器'},
        {name: 'ip', defaultValue: '127.0.0.1'},
      ],
      optionsList: [
        {name: '单行文本', type: 'singleRowText'},
        {name: '整数', type: 'int'},
        {name: '小数', type: 'double'},
        {name: '日期', type: 'dateTime'},
        {name: '多行文本',type: 'multiRowText'},
        {name: '单选', type: 'singleSel'},
        {name: '多选', type: 'multiSel'},
        {name: '下拉菜单', type: 'listSel'},
        {name: '人员', type: 'richText'},
        {name: '引用', type: 'reference'}
      ],
      isDraggable: true,
      currentType: '',
      number: 0
    }
  },
  mounted() {
    inner.ondragover = (e) => {
      e.preventDefault()
    }

    inner.ondrop = (e) => {
      e.preventDefault()
      alert(this.currentType)
    }
  },
  methods: {
    dragstart (event,itme) {
      this.currentType = itme.type
      return true;
    },
    dragend () {
      const left = document.getElementById('leftWrap')
      for (let i = 0; i < left.children.length; i++) {
        if (left.children[i].className == 'inner') {
          left.removeChild(inner)
        }
      }
    },
    dragenter (event,index) {
      this.number = index + 1
      const target = event.target.tagName == 'SPAN' ? event.target.parentNode : event.target
      target.parentNode.parentNode.insertBefore(inner, target.parentNode.nextElementSibling)
    },
    edit () {

    },
    deletepre () {

    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
.inner {
  height: 30px;
  line-height: 30px;
  border: 1px dashed #ccc;
  margin-bottom: 10px;
}
.content {
  width: 100%;
  display: flex;
  color: #fff;
  .left {
    flex: 1;
    border: 1px solid #0D5990;
    padding: 10px 40px;
    box-sizing: border-box;
    background-color: pink;
    .item {
      height: 30px;
      display: inline-block;
      line-height: 30px;
      margin-bottom: 10px;
      background-color: #0D5990;
      position: relative;
      .item-name {
        display: inline-block;
        width: 100px;
        text-align: right;
        padding-right: 5px;
        box-sizing: border-box;
      }
      .item-default {
        display: inline-block;
        width: 200px;
      }
    }
    .item-delete {
      cursor: pointer;
      color: #fff;
      margin-left: 10px;
      vertical-align: super;
    }

  }
  .right {
    width: 320px;
    padding: 10px;
    box-sizing: border-box;
    .right-head, .right-tips {
      width: 100%;
      padding: 0 15px;
      box-sizing: border-box;
      height: 30px;
      line-height: 30px;
      background-color: #043C68;
      margin-bottom: 10px;
    }
    .right-tips {
      border: 1px dashed #1681C4
    }
    .right-wrap {
      display: flex;
      justify-content: space-around;
      flex-wrap: wrap;
    }
    .options {
      width: 135px;
      height: 28px;
      text-align: center;
      line-height: 28px;
      background-color: #1681C4;
      margin-bottom: 5px;
      cursor: move;
    }
  }
}
</style>
