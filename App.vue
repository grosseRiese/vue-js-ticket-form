<template>
  <main id="App">
    <section class="booking" id="form">
			<h1 class="col-2">Åka Tåg</h1>
			<aside class="col-2">
				<h2> {{ destinations.from }}</h2>
				<h2 class="direction" 
          v-on:click="toggleCity" > &RightArrow; </h2>
				<h2> {{ destinations.to }}</h2>
			</aside>
			<label class="col-1 mrgbottom">
				<input 
          type="radio"
          name="klass" 
          value="2:a klass"
          v-model="userData.radioClassPicked" />
				2:a klass
			</label>
			<label class="col-1 mrgbottom">
				<input 
          type="radio"
          name="klass"
          value="1:a klass"
          v-model="userData.radioClassPicked" />
				1:a klass
			</label>
			<label class="col-1" for="tickets">
				Antal biljetter <b v-show="!userData.isAcceptedTerms" >*</b>
			</label>
			<label class="col-1" for="title">
				Title <b v-show="!userData.isAcceptedTerms" >*</b>
			</label>
			<select 
          name="tickets" 
          class="col-1" 
          v-model="userData.selectedTicket"
          :disabled="!userData.isAcceptedTerms">
            <option  
              v-for="(numberOfTicket,index) in userData.selectNumberOfTickets"
              v-bind:key="index"
              :value="numberOfTicket.text" > {{numberOfTicket.text}} 
            </option>
        <!--<option value="1">1</option>
				<option value="2">2</option>
				<option value="3">3</option>
				<option value="4">4</option>
				<option value="5">5</option>-->
			</select>
		
			<select 
          name="title" 
          class="col-1" 
          v-model="userData.selectedTitle" 
          :disabled="!userData.isAcceptedTerms">
          <option 
            v-for="(selectTitle,index) in userData.selectTitles"
            v-bind:key="index"
            :value="selectTitle.text"> {{selectTitle.text}}
          </option>
				<!-- <option value="mr">Mr.</option>
				<option value="mrs">Mrs.</option>
				<option value="ms">Ms.</option>
				<option value="dr">Dr.</option>-->
			</select>

			<label class="col-2" for="name">
			Namn <b v-show="!userData.isAcceptedTerms" >*</b>
     <span 
          class="validate-class"
          v-show="msg.name" 
          v-if="!userData.name" > 
          {{msg.name}} 
     </span>
     <!--   -->
      
			</label>
			<input 
          type="text" 
          name="name" 
          class="col-2" 
          v-model="userData.name" 
          :disabled="!userData.isAcceptedTerms"
          />

			<label class="col-2" for="email">
			Epost <b v-show="!userData.isAcceptedTerms" >*</b>
       <span 
          class="validate-class"
          v-show="msg.email" 
          v-if="!userData.email"> 
          {{msg.email}} 
     </span>
      
			</label>
			<input      
          type="email" 
          name="email" 
          class="col-2" 
          v-model="userData.email" 
          :disabled="!userData.isAcceptedTerms"/>
			<label class="col-2" for="terms">
				<input 
          type="checkbox" 
          name="terms" 
          v-model="userData.isAcceptedTerms"
            /> Godkänner villkoren 
			</label>
			<button 
          class="col-2"  
          @click="validateTheForm"
          :disabled="!userData.isAcceptedTerms"
         > Boka biljetter!
      </button>
		</section>
  </main>
</template>

<script>

export default {
  name: 'App',
	data() {
    return{
        destinations: {
          from: 'Stockholm',
          to: 'Göteborg'
        },
        userData: {
              radioClassPicked:'2:a klass',
              selectedTicket:'',
              selectNumberOfTickets:[
                    { value: 1, text: '1' },
                    { value: 2, text: '2' },
                    { value: 3, text: '3' },
                    { value: 4, text: '4' },
                    { value: 5, text: '5' },
                ],
              name: null,
              email:null,
              selectedTitle:'',
              selectTitles:[
                    { value: 'mr', text: 'Mr.' },
                    { value: 'mrs', text: 'Mrs.' },
                    { value: 'ms', text: 'Ms.' },
                    { value: 'dr', text: 'Dr.' },
                ],
              isAcceptedTerms:false,
          },
        isSubmitted: false,
         msg:[],
    }
  },
   methods: {
        submitted() {
            this.isSubmitted = true;
              console.log( this.userData, `Is submited!: ${this.isSubmitted}` );
              console.log( this.destinations );
        },
        validateTheForm(){

           if(this.userData.name && this.userData.email){
              this.msg =[];
              this.userData.name = null;
              this.userData.email = null;
              this.submitted();
          }

          if(!this.userData.name ){
              this.isSubmitted = false;
              console.log('Name Required field ');
              return this.msg.name = 'Name is required field ';
          }

          if(!this.userData.email)
          {
            this.isSubmitted = false;
            console.log('E-mail required field');
            return this.msg.email = 'E-mail is required field ';
          }
        },
        toggleCity(){
          if(this.destinations.from==='Stockholm'){
              this.destinations.from='Göteborg';
              this.destinations.to='Stockholm';

          }else{
              this.destinations.from='Stockholm';
              this.destinations.to='Göteborg';
          }
        },
      }
   
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=PT+Sans:400,700&display=swap');

  main {
	font-family: 'PT Sans', sans-serif;
	height: 100vh;
	background: #eee;
	display: flex;
}

.booking {
	background: white;
	max-width: 320px;
	width: 100%;
	margin: auto;
	display: grid;
	gap: 0 1.6rem;
	padding: 1rem;
	box-shadow: 0 0 2rem rgba(0, 0, 0, .1);
	border-radius: .4rem;
}

.booking h1 {
	grid-column: auto / span 2;
	margin: 0;
	padding: 1rem;
	font-size: 2.4rem;
	text-align: center;
	text-transform: uppercase;
}


.col-1 {
	grid-column: auto / span 1;
}

.col-2 {
	grid-column: auto / span 2;
}

label {
	font-size: .8rem;
	margin: 0;
	padding: 0 0 .125rem 0;
}

aside {
	height: 2rem;
	font-size: 1.4rem;
	display: flex;
	margin: 0 0 2rem 0;
}

aside h2 {
	flex: 1;
	text-align: center;
	font-size: 1.4rem;
}

.mrgbottom {
	margin: 0 0 1rem 0;
}

input[type="text"], input[type="email"], select {
	appearance: none;
	border: 1px solid rgba(0, 0, 0, .6);
	height: 2rem;
	border-radius: .25rem;
	margin: 0 0 1rem 0;
	padding: .125rem .5rem;
	font-size: 1rem;
	outline: none;
	transition: all .2s ease;
}

input[type="text"]:focus, input[type="email"]:focus, select:focus {
	border-color: black;
	outline: none;
}


button {
	appearance: none;
	-webkit-appearance: none;
	-moz-appearance: none;
	background: black;
	border-radius: .25rem;
	color: white;
	font-size: 1.2rem;
	font-weight: 600;
	padding: 1rem 0;
	margin: 1rem 0 0 0;
	outline: none;
}

button:active {
	background: #222;
}
button:disabled{
  background: rgba(158, 146, 146, 0.452);
}
input[type="text"]:disabled, input[type="email"]:disabled,select:disabled {
  border-color: rgba(233, 104, 104, 0.65);/**/
}

b{
   visibility: visible;
   color:red;
   font-size: 0.7rem;
}
span.validate-class{
   visibility: visible;
   color:red;
   font-size: 0.7rem;
}

</style>
