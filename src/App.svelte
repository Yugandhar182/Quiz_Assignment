<script>
	import { onMount } from "svelte";
	import 'bootstrap/dist/css/bootstrap.min.css';
  
	const questions = [
	  {
		question: "Which is the largest land animal?",
		options: ["Lion", "Tiger", "Elephant", "Rhinoceros"],
		answer: "Elephant",
		selected: null
	  },
	  {
		question: "Which group of animals have scales?",
		options: ["Mammals", "Amphibians", "Reptiles", "Dogs"],
		answer: "Reptiles",
		selected: null
	  },
	  {
		question: "Which material from the following has the highest transparency?",
		options: ["Paper", "Wood", "Metal", "Glass"],
		answer: "Glass",
		selected: null
	  },
	  {
		question: "What part of the skeletal system protects the brain?",
		options: ["Spine", "Thigh", "Pelvis", "Skull"],
		answer: "Skull",
		selected: null
	  },
	  {
		question: "Which nutrient plays an essential role in muscle-building?",
		options: ["Protein", "Carbohydrate", "Iron", "Fat"],
		answer: "Protein",
		selected: null
	  }
	];
  
	let score = 0;
	let currentQuestion = 0;
	let isQuizStarted = false;
	let isQuizFinished = false;
  
	let firstName = "";
	let lastName = "";
	let email = "";
	let mobile = "";
  
	function selectOption(option) {
	  questions[currentQuestion].selected = option;
	}
  
	function submitAnswer() {
	  const currentAnswer = questions[currentQuestion].selected;
	  const correctAnswer = questions[currentQuestion].answer;
  
	  if (currentAnswer === correctAnswer) {
		score++;
	  }
  
	  if (currentQuestion < questions.length - 1) {
		currentQuestion++;
	  } else {
		isQuizFinished = true;
	  }
	}
  
	function startQuiz() {
	  if (firstName && lastName && email && mobile) {
		isQuizStarted = true;
	  } else {
		alert("Please fill in all the details.");
	  }
	}
  
	onMount(() => {
	  score = 0;
	  currentQuestion = 0;
	  isQuizStarted = false;
	  isQuizFinished = false;
  
	  questions.forEach((question) => {
		question.selected = null;
	  });
	});
  </script>
  
  <style>
	li {
	  cursor: pointer;
	}
  </style>
  
  <main class="container mt-4">
	{#if !isQuizStarted}
	  {#if !firstName || !lastName || !email || !mobile}
		<h1 style="color:red;">Please fill the form before starting the quiz having 5 questions</h1>
	  {/if}
	  <form on:submit|preventDefault={startQuiz}>
		<div class="mb-3">
		  <label for="firstName" class="form-label">First Name</label>
		  <input type="text" class="form-control" id="firstName" bind:value={firstName} required>
		</div>
		<div class="mb-3">
		  <label for="lastName" class="form-label">Last Name</label>
		  <input type="text" class="form-control" id="lastName" bind:value={lastName} required>
		</div>
		<div class="mb-3">
		  <label for="email" class="form-label">Email</label>
		  <input type="email" class="form-control" id="email" bind:value={email} required>
		</div>
		<div class="mb-3">
		  <label for="mobile" class="form-label">Mobile Number</label>
		  <input type="tel" class="form-control" id="mobile" bind:value={mobile} required>
		</div>
		<button type="submit" class="btn btn-primary">Start Quiz</button>
	  </form>
	{:else if !isQuizFinished}
	  <h1 style="color:red;">Answer the following quiz</h1>
	  <div class="card">
		<div class="card-body">
		  <h1 class="card-title">Question {currentQuestion + 1}</h1>
		  <p class="card-text">{questions[currentQuestion].question}</p>
  
		  <ul class="list-group">
			{#each questions[currentQuestion].options as option}
			  <li class="list-group-item" on:click={() => selectOption(option)}>
				<div class="form-check">
				  <input
					class="form-check-input"
					type="radio"
					name="option"
					value={option}
					checked={questions[currentQuestion].selected === option}
				  />
				  <label class="form-check-label">{option}</label>
				</div>
			  </li>
			{/each}
		  </ul>
  
		  <button class="btn btn-primary mt-3" on:click={submitAnswer}>Next</button>
		</div>
	  </div>
	{:else}
	  <div class="card">
		<div class="card-body">
		  <h1 class="card-title">Quiz Finished</h1>
		  <p class="card-text">Your score: {score} out of {questions.length}</p>
		</div>
	  </div>
	{/if}
  </main>