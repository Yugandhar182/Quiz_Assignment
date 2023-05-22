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
	  isQuizStarted = true;
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
  
	.selected {
	  background-color: #007bff;
	  color: #fff;
	}
  
	.correct {
	  color: green;
	}
  
	.incorrect {
	  color: red;
	}
  </style>
  
  <main class="container mt-4">
	{#if !isQuizStarted}
	  <form on:submit|preventDefault={startQuiz}>
		<div class="card">
		  <div class="card-body">
			<h1 class="card-title">Quiz Instructions</h1>
			<p class="card-text">Please read the following instructions before starting the quiz:</p>
			<ol>
			  <li>Read the Questions Carefully: Take your time to understand each question before selecting an answer. Misinterpreting a question can lead to choosing the wrong option.</li>
			  <li>Answer Every Question: Try to answer every question, even if you are unsure. Leaving a question unanswered guarantees it will be incorrect. Make an educated guess if necessary.</li>
			  <li>Use Process of Elimination: If you're unsure about an answer, try eliminating the options that are clearly incorrect. Narrowing down the choices can increase your chances of selecting the correct answer.</li>
			  <li>Manage Your Time: Keep an eye on the time remaining for the quiz. Allocate a suitable amount of time for each question to ensure you can complete the quiz within the given time limit.</li>
			</ol>
			<button type="submit" class="btn btn-primary">Start Quiz</button>
		  </div>
		</div>
	  </form>
	{:else if !isQuizFinished}
	  <div class="card">
		<div class="card-body">
		  <h1 class="card-title">Question {currentQuestion + 1}</h1>
		  <p class="card-text">{questions[currentQuestion].question}</p>
  
		  <ul class="list-group">
			{#each questions[currentQuestion].options as option}
			  <li
				class="list-group-item"
				on:click={() => selectOption(option)}
				class:selected={questions[currentQuestion].selected === option}
			  >
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
  
		  {#if currentQuestion === questions.length - 1}
			<button class="btn btn-primary mt-3" on:click={submitAnswer} disabled={!questions[currentQuestion].selected}>
			  Submit
			</button>
		  {:else}
			<button class="btn btn-primary mt-3" on:click={submitAnswer} disabled={!questions[currentQuestion].selected}>
			  Next
			</button>
		  {/if}
		</div>
	  </div>
	{:else}
	  <div class="card">
		<div class="card-body">
		  <h1 class="card-title">Quiz Finished</h1>
		  <p class="card-text">Your score: {score} out of {questions.length}</p>
		  <h2 class="card-title">Answers</h2>
		  <ul>
			{#each questions as question}
			  <li>
				<strong>{question.question}</strong><br>
				{#if question.selected === question.answer}
				  <span class="correct">Your Answer: {question.selected}</span><br>
				{:else}
				  <span class="incorrect">Your Answer: {question.selected}</span><br>
				{/if}
				<em>Correct Answer: {question.answer}</em>
				<hr>
			  </li>
			{/each}
		  </ul>
		</div>
	  </div>
	{/if}
  </main>
  