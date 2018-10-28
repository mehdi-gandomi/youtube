<template>
            
            <main class="wrapper">
                <section class="video-input" >
        <div class="description">
                <h3>                آدرس مورد نظر خود را در کادر زیر وارد کرده و روی دکمه دانلود کلیک کنید</h3>

            </div>
            <fieldset >
                    <label class="url-input">
                        <input v-model="url" type="text" name="url" placeholder="آدرس ویدیو مورد نظر">
                        <button  id="download-btn" @click="showModal"><i class="icon-download3"></i></button>
                    </label>
                    
            </fieldset>
          

                    
                </section>
                <sweet-modal ref="info_modal" title="اطلاعات ویدیو">
                    
                        <h3>
                            عنوان ویدیو : 
                            <span>{{video_title}}</span>
                        </h3>
                        <div class="video-thumbnail">
                            <img :src="get_thumbnail_url()" :alt="video_title">
                        </div>
                        <div class="quality-selection">
                           <div class="qualities">
                                <label for="quality">کیفیت ها : </label>
                                <select name="quality" id="quality" v-model="choice" @change="setHref">
                                    <option value="0" >انتخاب کنید</option>
                                    <option v-for="stream in streams" :key="stream.itag" :value="stream.itag">کیفیت : {{stream.resolution}} &nbsp;&nbsp; اندازه فایل : {{stream.filesize}}</option>
                                </select>
                           </div>
                            <div class="download-section">
                                <a class="button" id="download">دانلود</a>
                            </div>
                        </div>

                    
                </sweet-modal>
                <div class="spinner-overlay" v-if="showLoading">
                    <div class="spinner" >
                        <div class="bounce1"></div>
                        <div class="bounce2"></div>
                        <div class="bounce3"></div>
                    </div>
                </div>
                
        </main>
        

</template>

<script>
import { SweetModal} from 'sweet-modal-vue'
export default {
    components:{
        SweetModal
    },
    

    data(){
        return{
            video_title:"test",
            video_thumbnail:"https://via.placeholder.com/300x150",
            url:"",
            showLoading:false,
            streams:[],
            choice:0
        }
    },
    methods: {
  showModal () {
    this.showLoading=true;
    this.$http.get("http://coderguy.pythonanywhere.com/youtube/streams?url="+this.url)
        .then(res=>{
            this.showLoading=false;
            let result=res.data;
            this.video_title=result.info.title;
            this.video_thumbnail=result.info.thumbnail_url;
            this.streams=result.streams;
            this.$refs.info_modal.open();
        })
  },
  hideModal () {
    this.$modal.hide('info-modal');
  },
  get_thumbnail_url(){
      return this.video_thumbnail;
  },
  setHref(){
      let res = this.streams.find(obj => obj.itag == this.choice);
      let button=document.querySelector("#download");
      console.log(button);
      button.setAttribute("href",res.download_link);
      button.setAttribute("download",res.filename);
  }
}
}
</script>
<style lang="scss">

.sweet-modal{
    color: #000 !important;

    .sweet-title{
        text-align: center;
    }
}
.url-input{
    input{
        text-align: center;
    }
}
.video-thumbnail{
        display: flex;
    justify-content: center;
    img{
        width: 80%;
        min-height: 150px;
    }
}
.quality-selection{
    margin-top: 2rem;
    display: flex;
    justify-content: space-around;
    .qualities{
        flex-basis: 70%;
    }
    .download-section{
        padding-top: 0.9rem;
        flex-basis: 20%;
        a{
            padding: 0.7em .9em;
        }
    }

}
</style>
