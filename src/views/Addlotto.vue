<template>
    <b-container fluid>
        <b-row class="text-left">
            <b-col cols="6" >
                เลือกชนิดของหวย
            </b-col>
            <b-col cols="6">
                <b-form-select
                            v-model="lottotype"
                        >
                    <option value="1">สองตัวบน/ล่าง</option>
                    <option value="2">หัว/ท้ายรางวัลที่หนึ่งสามตัว</option>
                </b-form-select>
            </b-col>
            <div class="w-100"></div>
            <b-col cols="6">
                ระบุเลขหวย
            </b-col>
            <b-col cols="6">
                <b-form-input v-model="numlotto" :type="'number'"  :max="'999'" ></b-form-input>
            </b-col>
            <div class="w-100"></div>
            <b-col cols="12"><b-alert show variant="danger" v-if="numlottoLengthStatus==0||numlotto==''">กรุณากรอกจำนวนตัวเลขให้ถูกต้อง</b-alert></b-col>
            <div class="w-100"></div>
            <b-col cols="12">
                <div v-if="lottotype ==1">
                    <b-row>
                        <b-col cols="5">เลือก บน/ล่าง</b-col>
                        <b-col cols="3"></b-col>
                        <b-col cols="4">จำนวนเงิน</b-col>
                        <div class="w-100"></div>
                        <b-col cols="6">
                            <b-form-checkbox
                            v-model="songSwbon"
                            switch
                            >
                            2ตัวบน
                            </b-form-checkbox>
                        </b-col>
                        <b-col cols="2">{{numlottoAddzero}}</b-col>
                        <b-col cols="3"><b-form-input v-model="monnySongbon" :type="'number'" :max="999" :disabled="!songSwbon" ></b-form-input></b-col>
                        <b-col cols="1">฿</b-col>
                     <div class="w-100"></div>
                        <b-col cols="6" v-if="numdoubleSta==false">
                            <b-form-checkbox
                            v-model="songSwbonRw"
                            switch
                            >
                            สลับเลข2ตัวบน
                            </b-form-checkbox>
                        </b-col>
                        <b-col cols="2" v-if="numdoubleSta==false">{{reversedNumlotto}}</b-col>
                        <b-col cols="3" v-if="numdoubleSta==false"><b-form-input v-model="monnySongbonRw" :type="'number'" :max="9999" :disabled="!songSwbonRw"></b-form-input></b-col>
                        <b-col cols="1" v-if="numdoubleSta==false">฿</b-col>
                    <div class="w-100"></div>
                        <b-col cols="6">
                            <b-form-checkbox
                            v-model="songSwlang"
                            switch
                            >
                            2ตัวล่าง
                            </b-form-checkbox>
                        </b-col>
                        <b-col cols="2">{{numlottoAddzero}}</b-col>
                        <b-col cols="3"><b-form-input v-model="monnySonglang" :type="'number'" :max="9999" :disabled="!songSwlang"></b-form-input></b-col>
                        <b-col cols="1">฿</b-col>
                     <div class="w-100"></div>
                        <b-col cols="6" v-if="numdoubleSta==false">
                            <b-form-checkbox
                            v-model="songSwlangRw"
                            switch
                            >
                            สลับเลข2ตัวล่าง
                            </b-form-checkbox>
                        </b-col>
                        <b-col cols="2" v-if="numdoubleSta==false">{{reversedNumlotto}}</b-col>
                        <b-col cols="3" v-if="numdoubleSta==false"><b-form-input v-model="monnySonglangRW" :type="'number'" :max="9999" :disabled="!songSwlangRw"></b-form-input></b-col>
                        <b-col cols="1" v-if="numdoubleSta==false">฿</b-col>
                    </b-row>
                </div>
                <div v-else-if="lottotype ==2">
                     ส่วน3ตัว
                </div>
            </b-col>
            <div class="w-100"></div>
            <b-col cols="6">{{numlottoLengthStatus}}</b-col>
            <b-col cols="6">{{numdoubleSta}}</b-col>
            <div class="w-100"></div>
            <b-col cols="6">{{songSwbon}}</b-col>
            <b-col cols="6"></b-col>
        </b-row>
    </b-container>
</template>

<script>
export default {
    data(){
        return{
            lottotype:'1',
            numlotto:'',
            numlottoAddzero:'00',
            numlottoLengthStatus:'1',
            numdoubleSta: true,
            songSwbon: true,
            songSwlang: true,
            songSwbonRw: false,
            songSwlangRw: false,
            monnySongbon: '',
            monnySonglang: '',
            monnySongbonRw: '',
            monnySonglangRW: '',


        }
    },//end data
    computed:{
        reversedNumlotto: function(){
            //ฟังชั่นสลับตัวเลขสองตัว
            let i=0
            let lottoCount = []
            let lottoReturn =''
            let numlottoLength = this.numlottoAddzero.length
            for(i = 0; i < numlottoLength; i++ ){
              lottoCount.push(this.numlottoAddzero.slice(i, i+1))
            }
            for(i = numlottoLength; i > 0; i--){
                lottoReturn = lottoReturn+lottoCount.pop()
            }
            return lottoReturn
        },//end reversedNumlotto

    },//end computed 
    watch:{
        numlotto: function(){
            if(this.lottotype==1&&this.numlotto.length==2){
                this.numlottoAddzero = this.numlotto
            }else if(this.lottotype==1&&this.numlotto.length<=1){
                this.numlottoAddzero = '0'+this.numlotto
            }
            //ส่วนเติมเลขหน้าเลข2ตัว
            if(this.lottotype==1&&this.numlotto.length==2){
                //ถ้า lottotype เท่ากับ1 และ ตัวอักษรในnumlotto น้อยกว่า2
                this.numlottoLengthStatus = '1';//เปลี่ยนสถานะ numlottoLengthStatus เป็น1 เพื่อนนำไปกำหนดการแสดงผลของช่องรับ numlotto
            }else if(this.lottotype==2&&this.numlotto.length==3){
                //ถ้า lottotype เท่ากับ2 และ ตัวอักษรในnumlotto 3
                this.numlottoLengthStatus = '1';//เปลี่ยนสถานะ numlottoLengthStatus เป็น1 เพื่อนนำไปกำหนดการแสดงผลของช่องรับ numlotto
            }else{
                this.numlottoLengthStatus = '0';//เปลี่ยนสถานะ numlottoLengthStatus เป็น0 เพื่อนนำไปกำหนดการแสดงผลของช่องรับ numlotto
            }
            //ส่วนตรวจสอบว่าจำนวนเลข2ตัวที่ใส่มาครบตามจำนวนรึยัง 0 มีน้อยกว่า 2 ตัว ,1 มี 2 ตัว หรือ มี3ตัว
            if(this.numlottoAddzero==this.reversedNumlotto){
                this.numdoubleSta=true
            }else{
                this.numdoubleSta=false
            }
            //ส่วนตรวจสอบว่าเป็นเลขเบิ้ลหรือไม่
        },//and numlotto
        lottotype : function(){
            this.numlotto = ''
            this.numlottoAddzero ='00'
            this.songSwbon = true
            this.songSwlang = true
            this.songSwbonRw = false
            this.songSwlangRw = false
            //ส่วนกำหนดค่าให้ตัวแปรหลังจากเปลี่ยนรูปแบบ2ตัว3ตัว
        },//end lottotype
        monnySongbon : function(){
            this.monnySonglang = this.monnySongbon
            this.monnySongbonRw = this.monnySongbon
            this.monnySonglangRW = this.monnySongbon
            //ตั้งค่าให้ใส่จำนวนเงินออโตตามฟิลสองตัวบน
        }//end monnySongbon
    },//and watch
}
</script>