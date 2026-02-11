# Housesuperlatives
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Unique Assignment Quiz</title>

<style>
body {
  font-family: Arial, sans-serif;
  padding: 40px;
  max-width: 900px;
  margin: auto;
  background-color: #f9f9f9;
}

h2 {
  text-align: center;
  margin-bottom: 30px;
}

.question {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: white;
}

label {
  display: block;
  margin: 3px 0;
}

button {
  padding: 12px 25px;
  font-size: 16px;
  margin-top: 20px;
  cursor: pointer;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
</head>

<body>

<h2>Assign a Unique Name to Each Category</h2>

<form id="quizForm"></form>

<button type="submit" form="quizForm">Submit</button>

<script>

// ======== EDIT QUESTIONS HERE ========
const questions = [
"Best cook",
"Most likely to be late",
"Best athlete",
"Most competitive",
"Funniest",
"Most organized",
"Biggest gamer",
"Most likely to move away",
"Best dressed",
"Most athletic",
"Most dramatic",
"Most mysterious",
"Best driver",
"Most likely to get rich",
"Most chaotic"
];

// ======== NAMES (15 TOTAL) ========
const names = [
"Abe",
"Mazur",
"Cian",
"Colin",
"Crew",
"Ethan Never Home",
"Ethan Home",
"Hank",
"Ian",
"Jeffrey",
"Kyle",
"Liam",
"Nolan",
"Steven",
"Tyler"
];

const form = document.getElementById("quizForm");

// ======== GENERATE QUESTIONS ========
questions.forEach((questionText, index) => {

  const div = document.createElement("div");
  div.className = "question";

  const title = document.createElement("p");
  title.innerText = questionText;
  div.appendChild(title);

  names.forEach(name => {
    const label = document.createElement("label");

    const input = document.createElement("input");
    input.type = "radio";
    input.name = "q" + (index + 1);
    input.value = name;

    label.appendChild(input);
    label.append(" " + name);

    div.appendChild(label);
  });

  form.appendChild(div);
});


// ======== LIVE DUPLICATE PREVENTION ========
form.addEventListener("change", function() {

  const selectedValues = [];

  document.querySelectorAll("input[type=radio]:checked")
    .forEach(input => selectedValues.push(input.value));

  // Re-enable all first
  document.querySelectorAll("input[type=radio]")
    .forEach(input => input.disabled = false);

  // Disable duplicates
  selectedValues.forEach(value => {
    document.querySelectorAll(`input[value="${CSS.escape(value)}"]:not(:checked)`)
      .forEach(input => input.disabled = true);
  });
});


// ======== SUBMISSION VALIDATION ========
form.addEventListener("submit", function(e) {
  e.preventDefault();

  const answers = Array.from(
    document.querySelectorAll("input[type=radio]:checked")
  ).map(input => input.value);

  if (answers.length !== questions.length) {
    alert("Please answer all 15 questions.");
    return;
  }

  const unique = new Set(answers);
  if (unique.size !== answers.length) {
    alert("Each name can only be used once.");
    return;
  }

  // 🔥 REPLACE THIS WITH YOUR WEB APP URL
  const scriptURL = "https://script.google.com/macros/s/AKfycbwv8Kh7dBqYnTt-YpSbkHZBkAZ9nA0u6WHYiloQPvi9ZRXdUDuAk0vZ_U2s7BBEgi8xxQ/exec";

  fetch(scriptURL, {
    method: "POST",
    body: JSON.stringify({ answers: answers }),
    headers: {
      "Content-Type": "application/json"
    }
  })
  .then(response => response.json())
  .then(data => {
    alert("Submission saved to Google Sheets!");
    form.reset();
  })
  .catch(error => {
    alert("Error saving submission.");
    console.error(error);
  });
});

</script>

</body>
</html>
