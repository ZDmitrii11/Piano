<template>
    <div class="container" >

        <div class="piano" v-for="note in notes" :key="note">
          <div class="button-piano" @keypress="keySound(note.keyCode)"  :id="note.id" @click.prevent="playSound(note.soundPath)">
                <span class="span-notes">
                {{note.name}}
            </span>
              <p>{{note.keyCode.toUpperCase()}}</p>

           <button class="str-button" @click.stop="trigger(note)">Click</button>
          </div>
        </div>
        <div class="text-inp" v-if="isActive">
             <h3>Change press key for your note {{currentTarget.name}} </h3>
            <input type="text" style='text-transform:uppercase'  v-model="currentTarget.keyCode" maxlength="1">
            <span class="error" v-if="error.length">{{error}}</span>
            <button @click="isActive = false">cancel</button>
            <button @click="changeKey()">ok</button>


        </div>


    </div>
</template>

<script>

    import name from '../notes'



    export default {
        name: "pianino",
        data() {
            return {
                test:null,
                isActive:false,
                currentTarget:null,
                textKey:'',
                notes: [
                    {
                        id:1,
                        name: 'Do',
                        soundPath: name.Do,
                        keyCode:''
                    },
                    {
                        id:2,
                        name: 'Re',
                        soundPath: name.Re,
                        keyCode:'',
                    },
                    {
                        id:3,
                        name: 'Mi',
                        soundPath: name.Mi,
                        keyCode:'',
                    },
                    {
                        id:4,
                        name: 'Fa',
                        soundPath: name.Fa,
                        keyCode:''
                    },
                    {
                        id:5,
                        name: 'Soli',
                        soundPath: name.Sol,
                        keyCode:''
                    },
                    {
                        id:6,
                        name: 'La',
                        soundPath: name.La,
                        keyCode:'',
                    },
                    {
                        id:7,
                        name: 'Si',
                        soundPath: name.Si,
                        keyCode:'',
                    },
                    {
                        id:8,
                        name: 'AllPlay',
                        soundPath: name.allNotes,
                        keyCode:''
                    },

                ],
                error:'',
                hhh:''
            }
        },
        created() {
            console.log('name',name)
            window.addEventListener('keypress',this.keySound)
        },
        methods:{
            playSound (sound) {
                let audio = new Audio(sound);
                audio.play();


            },

            keySound(e){
               let sorted = this.notes.find(el=>el.keyCode === e.key)
                   this.playSound(sorted.soundPath)
                   let keyNote = document.getElementById(sorted.id)
                    console.log(keyNote)
                keyNote.classList.add('wave2')
                setTimeout(()=>{
                    keyNote.classList.remove('wave2')
                },1000)
                },

            trigger(ind){
              this.isActive = !this.isActive
                this.currentTarget = ind
            },
            changeKey(){
            this.currentTarget = this.notes.id = this.currentTarget.id
            this.isActive = !this.isActive
            },
        },
        destroyed() {
            document.removeEventListener('keydown', this.keySound);
        },
    }



</script>

<style scoped>
.container{
    margin-left: 25%;
    margin-top: 100px;
    height: 250px;
    width: 1200px;
    display: flex;
    flex-direction: row;;
}
    .piano{
        display: flex;
        width: auto;
        height: 250px;
        flex-direction: row;
    }
.button-piano{
    width: 80px;
    background-color: burlywood;
    border-radius: 33%;
    border: 1px solid black;
    text-align: center;

}
.wave2 {
    -webkit-animation: wave-animation1 6.3s infinite linear; /* Safari 4+ */
    -moz-animation:    wave-animation1 6.3s infinite linear; /* Fx 5+ */
    -o-animation:      wave-animation1 6.3s infinite linear; /* Opera 12+ */
    animation:         wave-animation1 6.3s infinite linear; /* IE 10+ */
}

@-webkit-keyframes wave-animation1 {
    0%   { background-position: 0 0; }
    100% { background-position: 1601px 0; }
}
@-moz-keyframes wave-animation1 {
    0%   { background-position: 0 0; }
    100% { background-position: 1601px 0; }
}
@-o-keyframes wave-animation1 {
    0%   { background-position: 0 0; }
    100% { background-position: 1601px 0; }
}
@keyframes wave-animation1 {
    0%   { background-position: 0 0; }
    100% {background-position: 1601px 0;}
}

.wave2 {
    background: url('http://www.templates-preview.com/bootstrap-templates/300111854/images/wave2.png') 0 0 repeat-x;
}


</style>