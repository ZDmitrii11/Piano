<template>
    <div class="container">

        <div class="piano" v-for="note in notes" :key="note.id">
            <div class="button-piano waves-effect " @keypress="keySound(note.keyCode)" :id="note.id"
                 @click.prevent="playSound(note.soundPath)">
                <span class="span-notes">
                {{note.name}}
            </span>
                <p>{{note.keyCode.toUpperCase()}}</p>
                <button class="btn-small waves-effect waves-light" @click.stop="trigger(note)">
                    Add
                </button>
                <a v-if="note.keyCode" @click="removeKey(note.id)"  class="btn-floating bottom btn-small waves-effect waves-light red">
                <i class="material-icons">-</i></a>

            </div>
        </div>
        <div class="text-inp" v-if="isActive">
            <h5>Change press key for your note <span style="color: #ff0000">{{currentTarget.name}}</span></h5>
            <input type="text" style='text-transform:uppercase' v-model="textKey" maxlength="1">
            <div class="error" v-if="error.length">{{error}}</div>
            <div class="buttons-input">
                <button class="btn" @click="cancelButton">cancel</button>
                <!-- Modal Trigger -->
                <button data-target="modal1" class="btn modal-trigger">Edit</button>
                <!-- Modal Structure -->
                <div id="modal1" class="modal" ref="openModal">
                    <div class="modal-content">
                        <h5>You Sure You want To Change Voice <span style="color:red">{{this.currentTarget.name}}</span>
                            to KeyPress <span style="color: red">
                         {{this.textKey.toUpperCase()}}
                     </span></h5>
                    </div>
                    <div class="modal-footer">
                        <a href="#!" class="modal-close waves-effect waves-green btn-flat">Disagree</a>
                        <a href="#!" @click="changeKey" class="modal-close waves-effect waves-green btn-flat">Agree</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import M from 'materialize-css/'
    import name from '../notes'

    export default {
        name: "pianino",
        data() {
            return {
                isActive: false,
                modal: null,
                currentTarget: null,
                textKey: '',
                error: '',
                notes: [
                    {
                        id: 1,
                        name: 'Do',
                        soundPath: name.Do,
                        keyCode: ''
                    },
                    {
                        id: 2,
                        name: 'Re',
                        soundPath: name.Re,
                        keyCode: '',
                    },
                    {
                        id: 3,
                        name: 'Mi',
                        soundPath: name.Mi,
                        keyCode: '',
                    },
                    {
                        id: 4,
                        name: 'Fa',
                        soundPath: name.Fa,
                        keyCode: ''
                    },
                    {
                        id: 5,
                        name: 'Soli',
                        soundPath: name.Sol,
                        keyCode: ''
                    },
                    {
                        id: 6,
                        name: 'La',
                        soundPath: name.La,
                        keyCode: '',
                    },
                    {
                        id: 7,
                        name: 'Si',
                        soundPath: name.Si,
                        keyCode: '',
                    },
                    {
                        id: 8,
                        name: 'AllPlay',
                        soundPath: name.allNotes,
                        keyCode: ''
                    },

                ],

            }
        },
        created() {
            window.addEventListener('keypress', this.keySound)
        },
        watch: {
            isActive(newV) {
                if (newV === true) {

                    setTimeout(() => {
                        M.Modal.init(this.$refs.openModal)
                    }, 100)

                }
            }
        },
        methods: {

            playSound(sound) {
                let audio = new Audio(sound);
                audio.play();
            },
            keySound(e) {
                let sorted = this.notes.find(el => el.keyCode === e.key)
                this.playSound(sorted.soundPath)
                let keyNote = document.getElementById(sorted.id)
                keyNote.classList.add('wave2')
                setTimeout(() => {
                    keyNote.classList.remove('wave2')
                }, 1000)
            },
            trigger(ind) {
                this.isActive = !this.isActive
                this.currentTarget = ind
            },
            changeKey() {
                let findKey = this.notes.find(el => el.keyCode === this.textKey)
                if (findKey) {
                    this.error = `Same KeyPress ${this.textKey.toUpperCase()} exist put other`
                } else {
                    this.currentTarget.keyCode = this.textKey
                    localStorage.setItem(`${this.currentTarget.name}`, this.textKey)
                    this.isActive = !this.isActive
                }

            },
            removeKey(item){
                localStorage.removeItem( this.notes[item-1].name)
                this.notes[item-1].keyCode = ''
                console.log(this.notes.keyCode)
            },
            cancelButton() {
                this.isActive = false
                this.textKey = ''
            }
        },
        mounted() {
            this.notes.forEach(el => {
                const name = localStorage.getItem(el.name);
                if (name) {
                    this.notes[el.id - 1].keyCode = name
                }
            })

        }
    }


</script>

<style scoped>
    @import "~materialize-css/dist/css/materialize.min.css";

    .text-inp {
        border: 1px solid black;
        width: 300px;
    }

    .buttons-input {

        display: flex;
        justify-content: space-around;
    }

    .container {
        margin-left: 0px;
        margin-top: 100px;
        height: 250px;
        width: 1200px;
        display: flex;
        flex-direction: row;;
    }

    .piano {
        display: flex;
        width: auto;
        height: 250px;
        flex-direction: row;
    }

    .button-piano {
        width: 80px;
        background-color: burlywood;
        border-radius: 20%;
        border: 1px solid black;
        text-align: center;

    }

    .wave2 {
        -webkit-animation: wave-animation1 6.3s infinite linear; /* Safari 4+ */
        -moz-animation: wave-animation1 6.3s infinite linear; /* Fx 5+ */
        -o-animation: wave-animation1 6.3s infinite linear; /* Opera 12+ */
        animation: wave-animation1 6.3s infinite linear; /* IE 10+ */
    }

    @-webkit-keyframes wave-animation1 {
        0% {
            background-position: 0 0;
        }
        100% {
            background-position: 1601px 0;
        }
    }

    @-moz-keyframes wave-animation1 {
        0% {
            background-position: 0 0;
        }
        100% {
            background-position: 1601px 0;
        }
    }

    @-o-keyframes wave-animation1 {
        0% {
            background-position: 0 0;
        }
        100% {
            background-position: 1601px 0;
        }
    }

    @keyframes wave-animation1 {
        0% {
            background-position: 0 0;
        }
        100% {
            background-position: 1601px 0;
        }
    }

    .wave2 {
        background: url('http://www.templates-preview.com/bootstrap-templates/300111854/images/wave2.png') 0 0 repeat-x;
    }


</style>