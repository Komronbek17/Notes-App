<template>
    <div class="center">
        <!-- Input Note -->
        <div class="form" v-if="showClass">
            <form @submit.prevent="submitNote">
                <label for="input">Enter Note:</label>
                <input id="input" v-model="noteText" >
                <button type="submit" id="submit">Submit</button>
            </form>
        </div>
        <!-- Show Note -->
        <div class="hasNote" v-if="hasNote">
            <p>{{showNote}}</p>
            <div class="btn-edit">
                <button class="edit" @click="changeText">Change</button>
                <button class="edit" @click="deleteText">Delete</button>
            </div>
        </div>
        <h1 class="title">Save <span class="inner">the necessary</span> days</h1>
        <div class="container">
            <div v-for="month in months" :key="month">
            <h3 class="yilOyi">{{month}}</h3>
            <div class="month">
                <span v-for="weekday in weekdays" :key="weekday" class="inlineBlock">
                    {{weekday}}
                </span>
                <span></span>
                <span></span>
                <span></span>
                <span></span>
                <span></span>
                <span v-for="day in wholeYear()" :key="day.getTime()">
                    <span 
                        class="circle" 
                        v-if="months[day.getMonth()] == month"
                        @click="saveNote(day)"
                        :class="{numberActive:getActive(day)}"
                    >
                        {{day.getDate()}}
                    </span>
                </span>
            </div>
        </div>
        </div> 
    </div>
</template>

<script>
    export default {
        data() {
            return {
                weekdays:[
                    'Sunday',
                    'Monday',
                    'Tuesday',
                    'Wednesday',
                    'Thursday',
                    'Friday',
                    'Saturday'
                ],
                months:[
                    'January',
                    'February',
                    'March',
                    'April',
                    'May',
                    'June',
                    'July',
                    'August',
                    'Septepber',
                    'October',
                    'November',
                    'December'
                ],
                showClass:false,
                noteText:'',
                notes:[],
                id:0,
                hasNote:false,
                showNote:''
            }
        },
        created() {
            console.log(window.innerWidth);
        },
        methods:{
            wholeYear(){
                const currentYear=(new Date()).getFullYear();
                const firstDay=new Date(`January 1 ${currentYear} `);
                const lastDay=new Date(`December 31 ${currentYear}`);
                
                let lastDayInArr=firstDay;

                const wholeDay=[firstDay];
                
                while((lastDayInArr.getTime() !== lastDay.getTime())){
                    wholeDay.push(this.getDay(lastDayInArr));
                    lastDayInArr=wholeDay[wholeDay.length-1];
                }
                return wholeDay;
            },
            getDay(last){
                let next=new Date(last);
                next.setDate(last.getDate()+1);
                return next;
            },
            spaces(month){
                let findMonth;
                for(let i=0; i<this.months.length; i++){
                    if(month === this.months[i]){
                        findMonth = i ;
                        break;
                    }
                }
                const current=new Date();
                current.setMonth(findMonth,1);
                
                let spaceCount=current.getDay(); 

                return spaceCount || 0;
            },
            saveNote(day){
                let position = false;
                document.body.style.overflowY='hidden';
                
                this.notes.forEach(note => {
                    if(note.id == day.getTime()){
                        this.hasNote=true;
                        this.showNote=note.message;
                        position=true;
                        this.showClass=false;
                        this.id=note.id;
                    }
                });

                if(!position){
                    this.showClass=true;
                    this.id=day.getTime();
                }
            },
            submitNote(){
                if(this.noteText !=='' && this.id != 0){
                    this.notes.push({
                        id:this.id,
                        message:this.noteText
                    });
                }
                this.noteText='';
                this.showClass=false;
                document.body.style.overflowY='';
            },
            changeText(event){
                console.log(event.target);
                this.showClass=true;
                this.hasNote=false;
            },
            deleteText(){
                this.notes.forEach((note,index) => {
                    if(note.id == this.id){
                        this.notes.splice(index,1);
                        this.hasNote=false;
                    }
                })
                document.body.style.overflowY='';
            },
            getActive(day){
                let logic=false;
                if(this.notes){
                    this.notes.forEach(note => {
                        if( note.id === day.getTime() && note.message !== ''){
                            logic = true;
                        }
                    })
                }
                
                return logic;
            }
            

        },
    }
</script>

<style  scoped>
*{
    margin:0;
    padding:0;
}
.container{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 1rem;
    margin: 1.5rem;
    width: 60vw;
    text-align: center;
}
.container>div{
    border: 1px solid black;
    border-radius:20px;
    padding:0.1rem;
    background: #F4DB7D;
}
.month{
 display: grid;
 grid-template-columns: repeat(7,1fr);    
}
.inlineBlock{
 color:black;
 text-align: center;
 font-size: 1.1rem;
}
.circle{
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0.1rem;
    width:20px;
    height: 20px;
    background: #1FC58E;
    color:#191414;
    border-radius:50%;
    padding:10px;
    text-align: center;
    cursor:pointer;
}
.circle:hover{
    background: black;
    color:#1FC58E;
}
.hasNote{
    position: fixed;
    width:100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.9);
    font-family: 'Times New Roman', Times, serif;
    font-size: 1.3rem;
    color:#1FC58E;
    letter-spacing: 2px;
}
.title{
    font-family: 'Times New Roman', Times, serif;
    letter-spacing: 2px;
    color:crimson;
    text-align: center;
    margin-bottom: 1rem;
}
.inner{
    color:#1D2228;
}
.yilOyi{
    text-align: center;
    color:#161F6D;
    font-family: cursive;
    margin-bottom: 0.5rem;
}
/* Input field */
.form{
    position: fixed;
    width:100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.9);
}
.form label{
    color:#1FC58E;
    font-size: 1.5rem;
    font-family: 'Times New Roman', Times, serif;
    margin-right:0.5rem;
}
#input{
    width:30vw;
    outline: none;
    border: none;
    padding: 0.5rem;
    border-radius:30px;
    font-size: 1.1rem;
    font-weight: 600;
    font-family: 'Times New Roman', Times, serif;
}
#input::placeholder{
    background: #191414;
}
#submit,
.edit{
    border:none;
    outline: none;
    margin-left: 0.5rem;
    padding:0.5rem;
    font-family: 'Times New Roman', Times, serif;
    font-size: 1.1rem;
    border-radius: 20px;
    color:#1FC58E;
    background:black;
    cursor:pointer;

}
.edit{
    background: #f3f3f3;
    color: black;
}
#submit:hover,
.edit:hover{
    color:black;
    background: #1FC58E;
}
#submit:active,
.edit:active{
    transform: scale(0.9);
}
.btn-edit{
    position: absolute;
    top:10%;
    right:10%;
}
.numberActive{
    background: #361999;
    color:#f3f3f3;
}
@media screen and (max-width:1700px) {
    .container{
        grid-template-columns: 1fr;
    }
}
@media screen and (max-width:1000px) {
   .inlineBlock{
       font-size: 10px;
   }
}
</style>