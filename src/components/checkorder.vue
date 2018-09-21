<template>
  <div>
      <this-dialog :is-show="isShowCheckDialog" @on-close="checkStatus">
          请检查支付状态！
          <div class="button"  @click="checkStatus">
              支付成功
          </div>
          <div class="button"  @click="checkStatus">
              支付失败
          </div>
      </this-dialog>
      <this-dialog :is-show="isShowSuccessDialog" @on-close="toOrderList">
          购买成功
      </this-dialog>
      <this-dialog :is-show="isShowFailDialog" @on-close="toOrderList">
          购买失败
      </this-dialog>
      
  </div>
</template>
<script>
import Dialog from './dialog'
export default {
    components:{
        thisDialog:Dialog
    },
    props:{
        isShowCheckDialog:{
            type:Boolean,
            dafault:false
        },
        orderId:{
            type:[String,Number]
        }
    },
  data(){
      return{
          isShowSuccessDialog:false,
          isShowFailDialog:false
      }
  },
  methods:{
      checkStatus(){
          this.$http.post('/api/checkOrder',{//checkOrder写进db.json
              orderId:this.orderId
          }).then((res)=>{
              this.isShowSuccessDialog=true
              this.$emit('on-close-check-dialog')
          },(err)=>{
              this.isShowFailDialog=true
              this.$emit('on-close-check-dialog')
          })
      },
      close(){
          this.isShowFailDialog=false
      },
       toOrderList () {
      this.$router.push({path: '/orderlist'})
    }
  }
}
</script>
