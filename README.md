<!-- Cascade Putting Course Scorecard-->
<!-- built by DAN -->
<!-- 2024 -->

<!doctype html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<style>
			body,
			html {
				margin: 0;
				padding: 0;
				height: 100%;
				font-family: Arial, sans-serif;
				background-color: #00a3e6;
			}
			.container {
				display: flex;
				flex-direction: column;
				align-items: center;
				text-align: center;
				height: 100%;
			}
			#logo {
				max-width: 100%;
				max-height: 40%;
			}
			.number-of-players {
				color: #00a3e6;
				background-color: white;
				font-size: 30px;
				position: fixed;
				bottom: 300px;
				border-radius: 15px;
				padding: 10px 0px 0px 0px;
				font-weight: bolder;
			}
			.player-buttons {
				font-size: 45px;
			}
			.player-buttons button {
				font-size: 30px;
				border: none;
				color: transparent;
				text-shadow: 0 0 0 #00a3e6;
				vertical-align: 5px;
				background: none;
			}
			#startButton {
				font-size: 60px;
				font-weight: bold;
				background-color: white;
				color: #00a3e6;
				border: solid;
				border-radius: 15px;
				cursor: pointer;
				position: fixed;
				bottom: 180px;
				padding: 5px 1px;
			}
			.bottom-text {
				text-align: center;
				position: fixed;
				bottom: 60px;
				color: #ffffff;
			}
			.bottom-text p {
				margin: 5px;
			}
			#resetButton {
				font-size: 30px;
				font-weight: bold;
				background-color: #00a3e6;
				color: white;
				border: none;
				border-radius: 5px;
				cursor: pointer;
				position: fixed;
				bottom: 15px;
			}
			.scorecard-container {
				overflow-x: auto;
				max-width: 100%;
				position: fixed;
				top: 0;
				background-color: black;
				z-index: 1;
			}
			table {
				border-collapse: collapse;
				width: 100%;
				background-color: black;
			}
			th,
			td {
				text-align: center;
				padding: 10px;
				font-size: 24px;
			}
			th:first-child,
			td:first-child {
				position: sticky;
				left: 0;
				z-index: 1;
			}
			th:last-child,
			td:last-child {
				position: sticky;
				right: 0;
				background-color: black;
				color: white;
				z-index: 1;
			}
			td:first-child {
				background-color: black;
			}
			th:last-child {
				background-color: #00a3e6;
				color: white;
			}
			th {
				background-color: #00a3e6;
				color: white;
			}
			.editable-name {
				width: 80px;
				text-align: center;
				border: none;
				background-color: black;
				color: white;
				font-size: 20px;
				text-transform: capitalize;
			}
			.shot-count {
				display: flex;
				align-items: center;
				justify-content: center;
				padding-left: 10px;
				color: white;
			}
			.shot-count button {
				font-size: 30px;
				padding: 10px;
				background-color: black;
				border: none;
				color: white;
			}
		</style>
	</head>

	<body>
		<div class="container">
			<img
				id="logo"
				src="/images/cascadepicture.jpg"
				alt="Mini Golf Logo" />
			<div class="number-of-players">
				<label for="playerCount">PLAYERS</label>
				<div class="player-buttons">
					<button onclick="decrementPlayerCount()">➖</button>
					<span id="playerCount" class="player-value">1</span>
					<button onclick="incrementPlayerCount()">➕</button>
				</div>
			</div>
			<button id="startButton" onclick="startGame()">⛳ START ⛳</button>
			<button id="resetButton" onclick="resetGame()">NEW GAME</button>
			<div class="bottom-text">
				<p>built by Dan</p>
				<p>Lost a ball? Ask us for a new one!</p>
				<p>For your safety: stay off rocks and waterways.</p>
			</div>
		</div>

		<script>
			document.getElementById("resetButton").style.display = "none";
			let playerCount = 1;

			// Increase player count
			function incrementPlayerCount() {
				if (playerCount < 5) {
					playerCount++;
					document.getElementById("playerCount").innerText =
						playerCount;
				}
			}

			// Decrease player count
			function decrementPlayerCount() {
				if (playerCount > 1) {
					playerCount--;
					document.getElementById("playerCount").innerText =
						playerCount;
				}
			}

			// Start the game and set up the scorecard table
			function startGame() {
				document.getElementById("logo").style.display = "none";
				document.querySelector(".number-of-players").style.display =
					"none";
				document.getElementById("startButton").style.display = "none";
				document.getElementById("resetButton").style.display = "block";
				document.body.style.background = "black";

				const scorecardContainer = document.createElement("div");
				scorecardContainer.classList.add("scorecard-container");

				const table = document.createElement("table");
				const thead = document.createElement("thead");
				const tbody = document.createElement("tbody");

				// Create header row
				const headerRow = document.createElement("tr");
				const nameHeader = document.createElement("th");
				nameHeader.textContent = "HOLE";
				headerRow.appendChild(nameHeader);
				for (let i = 1; i <= 18; i++) {
					const holeHeader = document.createElement("th");
					holeHeader.textContent = `${i}`;
					headerRow.appendChild(holeHeader);
				}
				const totalHeader = document.createElement("th");
				totalHeader.textContent = "TOTAL";
				headerRow.appendChild(totalHeader);
				thead.appendChild(headerRow);

				// Create player rows
				for (let i = 1; i <= playerCount; i++) {
					const playerRow = document.createElement("tr");

					// Create editable player name
					const playerNameCell = document.createElement("td");
					const playerNameInput = document.createElement("input");
					playerNameInput.setAttribute("type", "text");
					playerNameInput.setAttribute("maxlength", "8");
					playerNameInput.classList.add("editable-name");
					playerNameInput.placeholder = `Player ${i}`;
					playerNameInput.addEventListener("input", saveScorecard);
					playerNameCell.appendChild(playerNameInput);
					playerRow.appendChild(playerNameCell);

					// Create shot count cells
					for (let j = 1; j <= 18; j++) {
						const shotCountCell = document.createElement("td");
						const shotCountContainer =
							document.createElement("div");
						shotCountContainer.classList.add("shot-count");

						// Create decrement button
						const decrementButton =
							document.createElement("button");
						decrementButton.textContent = "-";
						decrementButton.addEventListener("click", () => {
							decrementShotCount(shotCountContainer);
							saveScorecard();
						});

						// Create shot count display
						const shotCountDisplay = document.createElement("span");
						shotCountDisplay.textContent = "0";
						shotCountContainer.appendChild(decrementButton);
						shotCountContainer.appendChild(shotCountDisplay);

						// Create increment button
						const incrementButton =
							document.createElement("button");
						incrementButton.textContent = "+";
						incrementButton.addEventListener("click", () => {
							incrementShotCount(shotCountContainer);
							saveScorecard();
						});

						shotCountContainer.appendChild(incrementButton);
						shotCountCell.appendChild(shotCountContainer);
						playerRow.appendChild(shotCountCell);
					}

					// Create total cell
					const totalCell = document.createElement("td");
					totalCell.textContent = "0";
					playerRow.appendChild(totalCell);
					tbody.appendChild(playerRow);
				}

				table.appendChild(thead);
				table.appendChild(tbody);
				scorecardContainer.appendChild(table);
				document.body.appendChild(scorecardContainer);

				// Load scorecard data from localStorage if available
				loadScorecard();
			}

			// Decrement the shot count
			function decrementShotCount(container) {
				const shotCountDisplay = container.querySelector("span");
				let count = parseInt(shotCountDisplay.textContent);
				if (count > 0) {
					count--;
					shotCountDisplay.textContent = count;
					updateTotal(container);
				}
			}

			// Increment the shot count
			function incrementShotCount(container) {
				const shotCountDisplay = container.querySelector("span");
				let count = parseInt(shotCountDisplay.textContent);
				if (count < 6) {
					count++;
					shotCountDisplay.textContent = count;
					updateTotal(container);
				}
			}

			// Update the total score for a player
			function updateTotal(container) {
				const playerRow = container.parentNode.parentNode;
				const shotCountCells =
					playerRow.querySelectorAll(".shot-count span");
				let total = 0;
				shotCountCells.forEach((cell) => {
					total += parseInt(cell.textContent);
				});
				const totalCell = playerRow.querySelector("td:last-child");
				totalCell.textContent = total;
			}

			// Save the scorecard data to localStorage
			function saveScorecard() {
				const scorecardData = [];
				const rows = document.querySelectorAll("tbody tr");

				rows.forEach((row) => {
					const playerData = {
						name: row.querySelector(".editable-name").value,
						scores: Array.from(
							row.querySelectorAll(".shot-count span")
						).map((cell) => parseInt(cell.textContent)),
						total: parseInt(
							row.querySelector("td:last-child").textContent
						)
					};
					scorecardData.push(playerData);
				});

				localStorage.setItem(
					"scorecardData",
					JSON.stringify(scorecardData)
				);
			}

			// Load the scorecard data from localStorage
			function loadScorecard() {
				const scorecardData = JSON.parse(
					localStorage.getItem("scorecardData")
				);
				if (!scorecardData) return;

				const rows = document.querySelectorAll("tbody tr");
				scorecardData.forEach((playerData, i) => {
					const row = rows[i];
					row.querySelector(".editable-name").value = playerData.name;
					const shotCountCells =
						row.querySelectorAll(".shot-count span");
					playerData.scores.forEach((score, j) => {
						shotCountCells[j].textContent = score;
					});
					row.querySelector("td:last-child").textContent =
						playerData.total;
				});
			}

			// Reset the game and clear localStorage
			function resetGame() {
				localStorage.removeItem("scorecardData");
				location.reload();
			}

			// Automatically start the game if scorecard data is available
			window.onload = () => {
				const scorecardData = JSON.parse(
					localStorage.getItem("scorecardData")
				);
				if (scorecardData) {
					playerCount = scorecardData.length;
					document.getElementById("playerCount").innerText =
						playerCount;
					startGame();
				}
			};
		</script>
	</body>
</html>
