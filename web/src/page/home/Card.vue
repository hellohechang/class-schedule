<template>
  <div>
    <div class="editBox">
      <i class="icon" @click.stop="removeCard">&times;</i>
      <div class="edit"><span @click.stop="hdEdit">编辑</span></div>
    </div>
    <h1>{{ data.title }}</h1>
    <p>{{ data.describe }}</p>
  </div>
</template>

<script setup>
import { h } from 'vue';
import { useData } from '../../store/modules/data'
import MyMessageBox from './MessageBox'
import MyFormVue from './ScheduleTable/MyForm.vue';
import { delScheduleReq, editScheduleReq } from '../../api';
const store = useData()
const props = defineProps({
  data: Object
})
const removeCard = () => {
  MyMessageBox({
    title: '删除课程安排提示',
    btnCancelText: '取消',
    btnConfirmText: '删除',
    insert: h('p', '确定要删除此课程安排吗'),
    confirm(close) {
      close()
      delScheduleReq({ id: props.data.id }).then(res => {
        if (res.code == 0) {
          store.updateData()
        }
      }).catch(err => { })
    },
    cancel() {
    }
  })
}
let formData = {}
const hdEdit = () => {
  MyMessageBox({
    title: '编辑课程安排提示',
    btnCancelText: '取消',
    btnConfirmText: '提交',
    insert: h(MyFormVue, {
      title: props.data.title,
      des: props.data.describe,
      update(data) {
        formData = data
      }
    }),

    confirm(close) {
      let { title = '', des } = formData
      if (title === '' || title === props.data.title && des === props.data.describe) return
      close()
      editScheduleReq({ id: props.data.id, title, describe: des }).then(res => {
        if (res.code == 0) {
          store.updateData()
        }
      }).catch(err => { })
    },
    cancel() { }
  })

}
</script>

<style lang="less" scoped>
div {
  position: relative;
  width: 100%;
  height: 100%;
  color: #155724;
  background-color: #d4edda;
  flex-flow: column;
  padding: .1rem;
  overflow: hidden;

  &:hover {
    &>.editBox {
      transform: translateY(0);
    }
  }

  .editBox {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transform: translateY(100%);
    transition: .5s;
    overflow: hidden;
    padding: 0;

    .edit {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;

      span {
        cursor: pointer;

        &:hover {
          color: #000;
        }
      }
    }

    .icon {
      width: .3rem;
      line-height: .3rem;
      text-align: center;
      position: absolute;
      top: 0;
      right: 0;
      font-style: normal;
      font-size: .30rem;
      z-index: 2;
      cursor: pointer;

      &:hover {
        font-weight: bold;
      }
    }
  }

  h1 {
    width: 100%;
    border-bottom: 1px solid #ccc;
    padding-bottom: .05rem;
  }

  p {
    margin-top: .05rem;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
    overflow: hidden;
  }
}
</style>