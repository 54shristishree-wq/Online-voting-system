# Online-voting-system
Secure authentication and voting 
<!DOCTYPE html>
<html>
<head>
  <title>Simple Online Voting System</title>

  <style>
    body{
      font-family: Arial;
      background:#f4f6ff;
      padding:40px;
    }

    .card{
      background:white;
      padding:25px;
      width:420px;
      border-radius:10px;
      box-shadow:0 0 10px rgba(0,0,0,0.15);
    }

    button{
      padding:8px 15px;
      border:none;
      border-radius:5px;
      cursor:pointer;
      font-size:15px;
    }

    .vote-btn{
      background:#4CAF50;
      color:white;
    }

    .reset-btn{
      background:#e91e63;
      color:white;
      margin-top:10px;
    }
  </style>

</head>

<body>

<div class="card">
  <h2>Online Voting System</h2>

  <p><b>Select your candidate:</b></p>

  <input type="radio" name="candidate" value="Candidate A"> Candidate A<br>
  <input type="radio" name="candidate" value="Candidate B"> Candidate B<br>
  <input type="radio" name="candidate" value="Candidate C"> Candidate C<br><br>

  <button class="vote-btn" onclick="castVote()">Submit Vote</button>

  <h3>Results</h3>
  <p id="resultA">Candidate A: 0</p>
  <p id="resultB">Candidate B: 0</p>
  <p id="resultC">Candidate C: 0</p>

  <button class="reset-btn" onclick="resetVotes()">Reset All Votes</button>

</div>

<script>
// initialize votes if not exist
if(!localStorage.getItem("votes")){
  let initialVotes = {A:0, B:0, C:0};
  localStorage.setItem("votes", JSON.stringify(initialVotes));
}

// display vote counts
function displayResults(){
  let votes = JSON.parse(localStorage.getItem("votes"));
  document.getElementById("resultA").innerText = "Candidate A: " + votes.A;
  document.getElementById("resultB").innerText = "Candidate B: " + votes.B;
  document.getElementById("resultC").innerText = "Candidate C: " + votes.C;
}

// voting function
function castVote(){
  // block double voting
  if(localStorage.getItem("hasVoted")){
    alert("You have already voted!");
    return;
  }

  // get selected candidate
  let options = document.getElementsByName("candidate");
  let selected = null;

  for(let choice of options){
    if(choice.checked){
      selected = choice.value;
      break;
    }
  }

  if(selected === null){
    alert("Please select a candidate first!");
    return;
  }

  let votes = JSON.parse(localStorage.getItem("votes"));

  if(selected === "Candidate A") votes.A++;
  if(selected === "Candidate B") votes.B++;
  if(selected === "Candidate C") votes.C++;

  localStorage.setItem("votes", JSON.stringify(votes));
  localStorage.setItem("hasVoted", true);

  alert("Vote cast successfully!");
  displayResults();
}

// reset vote counts
function resetVotes(){
  let confirmReset = confirm("Reset all votes?");
  if(!confirmReset) return;

  let initialVotes = {A:0, B:0, C:0};
  localStorage.setItem("votes", JSON.stringify(initialVotes));
  localStorage.removeItem("hasVoted");
  displayResults();
}

// load results when page opens
displayResults();
</script>

</body>
</html>
