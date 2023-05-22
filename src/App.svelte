<h1 style="color:red;"> Answer the following Quiz</h1>
<script>

    import { onMount } from "svelte";
	import 'bootstrap/dist/css/boostrap.min.css';



const questions = [
      {
	   question:"Which is the largest land animal?",
	   Options:["Lion" ,"Tiger", "Elephant" ,"Rhinoceros"],
	   answer:"Elephant",
	   selected:null

	  },
	  {
       question: "Which group of animals have scales?",
	   Options: ["Mammals" , " Amphibians" ," Reptiles" , "Dogs"],
	   answer:"Reptiles",
	   selected:null
	  },
	  {
		question: " Which material from the following has the highest transparency?",
		options:["Paper" , "Wood" , "Metal" , "Glass"],
		answer:"Glass",
		selected:null
	  },
	  {
		question:"What part of the skeletal system protects the brain?",
		options:["Spine" , "Thigh" , " Pelvis" , " Skull"],
		answer: "Skull" ,
		selected:null
	  },

	  {
        question:" Which nutrient plays an essential role in muscle-building?",
		options: ["Protein" , "Carbohydrate"," Iron" , "Fat"],
		answer:"Protien",
		selected:null

	  }
];
  let score = 0;
  let currentQuestion = 0;
  let isQuizFinished = false;

    function selectedOption (option) {
		question[currentQuestion].selected = option;
	}

	function submitAnswer (){
		const currentAnswer = questions[currentQuestion].selected;
		const correctAnswer = questions[currentQuestion].answer;
	

	if(correctAnswer===correctAnswer){
		score++;
	}
	if(currentQuestion < questions.lenght-1){
		currentQuestion++;
	}
	else{
		isQuizFinished =true;
	}
}

  onMount(() => {
	score =0;
	currentQuestion =0;
	isQuizFinished = false;

	questions.forEach((question)=>{
		question.selected = null;
	});
  });


</script>


<main class = "container mt-4">
	{#if isQuizFinished}
	  <div class="card">
		<div class = "card-body">
		<h1 class = "card-title">Quiz Finished</h1>
		<p class ="card-text">Your score : {score} out of {questions.lenght}</p>

	</div>
	</div>

	{:else}
	<div class = "card">
		<div class="card-body">
			<h1 class = "card-title"> Question{currentQuestion +1}</h1>
			<p class = "card-text"> {questions[currentQuestion].question}</p>


			<ul class = "list-group">
				{#each questions[currentQuestion].options as option}
				 <li class ="list-group-item" on:click = {()=> selectOption(option)}>
	                <div class = "form-check">
						<input
						   class = "form-check-input"
						   type = "radio"
						   name = "option"
						   value={option}
						   checked = {questions[currentQuestion].selected===option}
						   on:change={()=>{}}
						   />
						   <label class = "form-check-label">{option}</label>
                           </div>
						   </li>
						   {/each}
						   </ul>

				
				<button class= "btn btn-primary mt-3" on:click = {submitAnswer}>Next</button>
				</div>
				</div>
				{/if}

</main>

<style>
	li {
		cursor: pointer;
	}
</style>


