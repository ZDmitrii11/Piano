<template>
    <div class="container" >

        <div class="piano" v-for="(note,index) in notes" :key="note">
          <div class="button-piano" @keypress="keySound(note.keyCode)"  :id="index+1" @click.prevent="playSound(note.soundPath)">
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
            <button @click="isActive = false">cancel</button>
            <button @click="changeKey()">ok</button>


        </div>


    </div>
</template>

<script>
const note1 = require('../notes/noty-do.mp3');
const note2 = require('../notes/re.mp3');
const note3 = require('../notes/mi.mp3');
const note4 = require('../notes/fa.mp3');
const note5 = require('../notes/sol.mp3');
const note6 = require('../notes/lja.mp3');
const note7 = require('../notes/si.mp3');
const noteAll = require('../notes/do-re-mi-fa-sol-lja-si.mp3')


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
                        soundPath: note1,
                        keyCode:''
                    },
                    {
                        id:2,
                        name: 'Re',
                        soundPath: note2,
                        keyCode:'',
                    },
                    {
                        id:3,
                        name: 'Mi',
                        soundPath: note3,
                        keyCode:'',
                    },
                    {
                        id:4,
                        name: 'Fa',
                        soundPath: note4,
                        keyCode:''
                    },
                    {
                        id:5,
                        name: 'Soli',
                        soundPath: note5,
                        keyCode:''
                    },
                    {
                        id:6,
                        name: 'La',
                        soundPath: note6,
                        keyCode:'',
                    },
                    {
                        id:7,
                        name: 'Si',
                        soundPath: note7,
                        keyCode:'',
                    },
                    {
                        id:8,
                        name: 'AllPlay',
                        soundPath: noteAll,
                        keyCode:''
                    },

                ]
            }
        },
        created() {
            window.addEventListener('keypress',this.keySound)
        },
        methods:{
            playSound (sound) {
                let audio = new Audio(sound);
                audio.play();
            },

            keySound(e){
               let sorted = this.notes.find(el=>el.keyCode === e.key)
                   return this.playSound(sorted.soundPath)
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
    height: 250px;
    width: 1200px;
    display: flex;
    flex-direction: row;

}
    .piano{
        display: flex;
        width: 150px;
        height: 250px;
        flex-direction: row;

    }
.button-piano{
    width: 120px;
    background-color: burlywood;
    border: 1px solid black;
    text-align: center;
    justify-content: flex-end;
}
.button-piano>span {


}
</style>